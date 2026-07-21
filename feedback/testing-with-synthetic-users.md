# Dogfood log: testing-with-synthetic-users

每次真實使用後加一個 entry。寫低行為，唔好淨寫感想 — REFACTOR 需要逐字證據。

## Entry template

```
## YYYY-MM-DD — [project / feature]

**Trigger**: skill 有冇自動 fire？定要手動叫？你講咗咩原句？
**盲點檢查**（犯咗就 ❌ + 引 agent 原話）:
- [ ] one-and-done（出咗報告就停，冇 fix→retest）
- [ ] QA-dimension persona（"mobile tester" 呢類）
- [ ] 冇輪換 persona 角度
- [ ] 跌返去 P0-P3 純 bug 分級
- [ ] 冇 clean final round 就收工
**新 rationalization**（agent 偏離 skill 時講咩理由，逐字記）:
**指引唔清晰位**（agent 喺邊段猶豫/問你/做錯）:
**Persona 質素**：agent 自己 derive 嘅 persona 揪唔揪到嘢？邊個 archetype 冇用？
**Report**：有冇自動出 artifact？跟唔跟 template？
**一句總結**：呢次 skill 幫到咩 / 阻住咩
```

---

<!-- entries below -->

## 2026-07-12 — pokecard / 精靈圖鑑總覽頁 + Grid 中文名（6 輪）

*（由 report 反推：`pokecard/docs/synthetic-test-pokemon-index-2026-07-12.md`；trigger 原句 + agent 逐字 rationalization 冇記錄）*

**盲點檢查**：五項全過 ✅
- 預定 5 輪，R5 有 🟡 finding → **自動順延到 R6 先 sign-off** — 「exit condition 大過輪數」條rule完美執行
- Persona 每輪輪換，有 returning persona re-score（阿晴 3→8/10）、R2 有 adversarial（挑機佬）
- 🔴🟡🟢 + 主觀評分齊；backlog table + commit chain 齊
**新發現嘅 failure mode**：R2 出咗一個 false positive（401「發現」）— 源頭係 host 自己 debug 殘留喺共用 browser console，即**測試污染**。Skill 目前冇講 persona 環境隔離/污染檢查。
**一句總結**：教科書式執行，仲觸發咗 skill 最核心嗰條「found problems ≠ last round」rule。
**補充（Kelon 2026-07-12 總結）**：全程 11 個 persona；R3 sitemap 1000-cap 係 host 自己 curl 自查抓到（host 都做 checker 係好 pattern）；繁簡污染係手機 persona R5 先眼利見到 — 遲輪 persona 真係揪到早輪盲點；教訓已記 memory（1000-cap 地雷 + 共用 browser console 污染）。

## 2026-07-12 — pokecard / 價格走勢圖雙源整合（2 輪）

*（由 report 反推：`pokecard/05-pokedex/docs/synthetic-test-price-trend-2026-07-12.md`）*

**盲點檢查**：五項全過 ✅（R1 搵嘢→修→deploy→R2 rotate 角度回歸→clean sign-off）
**偏離（全部有明示，冇靜雞雞）**：
- Playwright profile 被其他 session 鎖住（跟咗「唔准 kill」規矩）→ host 親自揸 claude-in-chrome 順序行 persona，**唔係獨立 persona subagent** — report 有 ⚠️ 方法透明度聲明
- Desktop Chrome 最小窗寬 clamp ~500px → 375px 手機 layout 驗唔到 — 誠實記做 🟢 backlog「工具限制」
**指引含糊位（兩次 run 對照先見到）**：「default 5 rounds」被兩個 agent 解讀成兩樣嘢 — index run 當佢係「起碼咁上下」（伸到 6），price-trend run 當佢係「上限」（R2 clean 即收工）。Flowchart 容許 clean round 即走，所以理論上 R1 全綠可以零輪換就 sign-off — 咁 rotation 條 rule 就冇機會執行。
**一句總結**：degrade 行為理想（偏離全部申報），但「輪數下限 vs clean-round 即走」係真實含糊位。

## 2026-07-13/14 — pokecard iOS / 交易 listing 生命週期 + Profile 系統（⚠️ 疑似冇跟 skill）

*（由 commit 反推：`443b5758`「合成用戶測試 15/15 通過（開單/編輯/重新上架/收檔/RLS 授權隔離，打真 kado.hk API）」+ `08864133`「e2e 合成用戶驗證 4 大件全通過」）*

**盲點檢查**：⚠️ 多項疑似回退（但無法確認 skill 有冇 fire — 見下）
- ❌ **冇 report artifact** — docs/ 搵唔到報告，findings 只剩 commit 一句「15/15 通過」
- ❌ **疑似 one-pass checklist** — 「15/15」「4 大件全通過」係驗收清單口吻，睇唔到 round table / triage / rotation / clean final round
- ❌ 冇 🔴🟡🟢 分級、冇主觀評分、冇 backlog
- ✅ 打真 production API（kado.hk）— iOS UI 冇 browser automation，API-level walk 係啱嘅 degrade 方向
**關鍵未知（要 Kelon answer）**：嗰兩個 iOS session 到底有冇 invoke 個 skill？「合成用戶測試」可能只係沿用咗詞彙、憑 repo history 印象做，skill 根本冇 load。
**Discovery 疑點**：本 repo 早期草稿 description 有中文 trigger（「合成測試」「會唔會出事」），開源重寫時剷走咗 — 而 Kelon 啲 session 全程廣東話。如果 iOS 嗰兩次係 discovery miss，呢個就係 root cause 候選。
**一句總結**：web 上兩次教科書式，轉到 iOS（新平台 + 廣東話 trigger）就疑似斷咗 — 到底係 discovery 問題定 agent 有 skill 都照簡化，係下一個要驗證嘅嘢。

## REFACTOR 候選

1. ~~輪數語義~~ ✅ 已修（2026-07-12，commit 8fd7a38）
2. ~~測試污染~~ ✅ 已修（同上）
3. ~~方法透明度入 template~~ ✅ 已修（同上）
4. ~~中文 trigger keywords~~ ✅ 已修（2026-07-21，commit c0c4589）— root cause 未確認，但兩個候因（discovery miss / rationalization）都平價，一次過修埋
5. ~~mobile-app degrade path~~ ✅ 已修（同上）— 「degrade transport, never the method」+ common mistakes 加 native-app row；GREEN：廣東話 iOS 情境 subagent 拒絕 15/15 checklist、出足 loop + report plan
