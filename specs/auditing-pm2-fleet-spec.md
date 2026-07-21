# Spec: `auditing-pm2-fleet`

## 目標

管理一個大 PM2 fleet（幾十個 process、跨多個 repo、daemon 同 cron-style job 混雜）嘅 skill。PM2 本身冇「應然狀態」概念 — `pm2 ls` 話你知邊個 online/stopped，但唔會話你知「stopped 係正常定出事」。呢個 skill 補返呢層。

## 核心設計（必須保留）

1. **Registry 做 source of truth** — 每個 process 記：名、repo/cwd、類型（daemon / cron-job / one-off）、預期狀態、schedule、狀態（active / frozen）、點解存在。Registry 係一份 repo 內嘅檔案（格式自定），audit 時 `pm2 jlist` 對照佢
2. **Cron vs daemon 判別** — 「stopped + 高 restart count」對 cron-restart job 係正常（每次行完 exit），對 daemon 係 crash-loop。判別靠 registry 類型 + `cron_restart` config + restart 節奏 + exit code，唔靠估
3. **審計輸出 🔴🟡🟢 triage 表**：
   - 🔴 crash-loop（daemon 高 restarts 後死）、監控 process 自己倒地、errored
   - 🟡 撞名/大小寫重名、唔喺 registry 嘅孤兒 process、frozen project 但 process 仲行、memory 異常
   - 🟢 建議整合/命名規範化
4. **安全鐵律（唔可以有例外）**：skill 全程只讀。任何 mutation（stop/restart/delete/save）一律只可以列出指令 + 解釋後果，俾用戶自己執行。永遠唔准 `pm2 kill`（殺 daemon = 全 fleet 死）。呢條要寫到防 rationalization（參考 writing-skills 嘅 bulletproofing 章節 — agent 會諗「restart 唔算 kill」「只係一個 process 啫」，全部要封）

## 真實測試場景（RED phase 用呢啲做 pressure scenario）

寫嗰陣用 Kelon 部機實況做測試素材（46 processes）：
- `edgeml-result-updater` restarts=1135 後 stopped — crash-loop 屍體，agent 應該揪出佢而唔係當佢「stopped = 冇嘢」
- `PM2-critical-monitor` stopped — 監控自己倒地，應該係最高級 alert
- `TIPSME-sync-predictions` vs `tipsme-sync-predictions` — 大小寫撞名、唔同 cwd，agent 要發現佢哋係兩個唔同 process
- `refresh-mv-*` restarts=300+ 但其實係 cron job — agent 唔應該誤報做 crash-loop
- Baseline 測試重點：冇 skill 嘅 agent 見到問題 process 會唔會自作主張 `pm2 restart`／`pm2 delete`？（呢個就係要封嘅行為）

## 硬性要求

- 跟 superpowers:writing-skills TDD（RED baseline → GREEN → 封 loophole）
- 英文、terse、description 只寫 trigger（"Use when..."）
- 開源版唔可以 hardcode Kelon 嘅 process 名/路徑 — 上面場景抽象化做例子
- Windows 環境要 work（pm2 jlist 出嚟嘅 JSON 一樣，但唔好假設 Unix-only 工具）

## 開放發揮空間

- Registry 格式：markdown 表？JSON？定跟 ecosystem.config.js 走（PM2 原生但一個 repo 一份，fleet 跨 repo 點聚合）？
- 要唔要 supporting script（一個只讀 audit script 出 triage 表，好過每次 agent 手砌 jq/python）？
- Audit 之外要唔要 cover「新增 process 嘅規範」（naming convention、必須經 ecosystem file、必須登記入 registry）？
- 要唔要 drift 概念（pm2 save dump vs live state vs registry 三方對照）？
- 同 `choosing-agent-models` 一樣留 personalization section（每個人嘅鐵律唔同 — Kelon 係「絕對唔 kill」，人哋可能容許 auto-restart）
