# Spec: `choosing-agent-models`

## 目標

一個教主 session「派 subagent 時揀邊個 model」嘅 routing skill。個人版已經寫咗（見現有草稿），呢個 spec 係俾你將佢升級做**開源版** — 任何人裝完都用到，唔止 Kelon。

## 核心原則（保留）

1. Routing 按任務性質，唔係按任務「重唔重要」：
   - **頂級 model（fable/opus 級）** — taste/judgment：設計、critique、架構決策、adversarial verify、多 agent synthesis
   - **中 tier（sonnet 級）** — research、探索、標準實作、內容起草
   - **細 model（haiku 級）** — 機械式 bulk：inventory、格式轉換、大規模抽取/分類
2. 唔肯定就 **omit model param**（繼承 session model）— 錯嘅 override 差過冇 override
3. Workflow script 層面：`agent(prompt, {model, effort})` — finder 平、judge/verify 貴；effort 'low' 俾機械 stage
4. 配套 pattern：用 `~/.claude/agents/*.md` custom agent 將 model 綁死喺 agent type（例：Kelon 嘅 `taste-designer` 綁 fable）

## 原始素材

- 現有草稿 `skills/choosing-agent-models/SKILL.md` — 已過 RED/GREEN 測試。Baseline 最大發現：agent 天然會「research 好複雜所以用 opus」over-spend，呢個 counter 一定要留
- `~/.claude/agents/taste-designer.md` — custom agent + model pinning 嘅實例

## 硬性要求

- 開源版唔可以 hardcode Kelon 嘅個人偏好做規則 — 改成「tier 框架 + 一個 personalization section 教人點加自己嘅 routing rules / custom agents」
- Model 名要處理到唔同環境（人哋未必有 fable — 用「strongest available tier」措辭，具體名做例子）
- Description 只寫 trigger；英文；<450 words 主體
- 改完跟 writing-skills 做一次 GREEN 驗證（同一 scenario：design/scan/research 三任務點 route）

## 開放發揮空間

- 要唔要一個「cost intuition」段（相對成本差距數量級，等人明點解 routing 有着數）
- 要唔要 cover「幾時唔應該派 agent、自己做」（altitude 問題）
- Personalization section 嘅形式：教人寫自己嘅 agents/*.md？定教人 fork 呢個 skill 加 rules？
- 同 testing-with-synthetic-users 嘅互相引用（佢嘅 host/persona 分工正正係一個 routing 實例）
