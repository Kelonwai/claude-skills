# Spec: `testing-with-synthetic-users`

## 目標

將 Kelon 喺 pokecard 用開嘅「合成用戶測試 R1-R9」workflow 寫成一個可以開源俾任何人用嘅 Claude Code skill。呢個係全 repo 嘅旗艦 skill — 質素目標係放上 GitHub 之後人哋會真心覺得「呢個方法論我未見過，即刻想用」。

## 方法論核心（必須保留嘅骨架）

1. **Persona 係有動機嘅人，唔係 QA 維度** — 「唔識英文名嘅新手」會揪到 discoverability 問題，「mobile tester」只會揪到 layout bug
2. **多輪 loop**：派 persona → 🔴🟡🟢 分級 → triage（🔴即修／🟡揀高ROI／🟢backlog）→ 修 → deploy → 下輪換 persona 角度重測
3. **收工條件 = 一整輪全綠回歸**，唔係「冇嘢炸」
4. **報告落地做 artifact**（docs/），唔係淨喺 chat 度講完就算
5. 主持 agent 用強 model，persona agent 用中 tier，行真 production（Playwright/瀏覽器自動化）

## 原始素材（去呢度攞真實例子）

- `pokecard/docs/synthetic-test-pokemon-species-2026-07-11.md` — 5 輪報告範本（R1-R5 逐輪 persona/發現/行動表）
- `pokecard/docs/superpowers/plans/2026-07-11-pokemon-species.md` L743-763 — Task 9 嘅輪次設計
- `pokecard` git log grep「合成測試」— trade 功能嗰次 R1-R5（審核/並發/XSS 角度）可以做第二個例子
- `pokecard/05-pokedex/scripts/synthetic-users.json` — persona bank 實例
- 現有草稿 `skills/testing-with-synthetic-users/SKILL.md` — 已經過一次 RED baseline 驗證（baseline agent 嘅五大盲點：one-and-done、QA-dimension persona、唔輪換、P0-P3 純 bug 分級、冇 clean final round）。你可以完全重寫，當佢係 raw material

## 硬性要求

- 跟 superpowers:writing-skills 嘅 TDD：改完要用 subagent 做 GREEN 驗證（baseline 已做，見上）
- Frontmatter description 只寫 trigger condition（"Use when..."），唔准 summarize workflow
- 英文寫（開源受眾），terse、scannable
- 開源前提：唔可以寫死 Kelon 嘅私人路徑/項目名，pokecard 例子要抽象化或者標明係 case study

## 開放發揮空間（自由決定）

- 結構：單一 SKILL.md 定係加 supporting files（persona bank template？report template？輪次 checklist？）
- 要唔要一個 dot flowchart 表達個 loop（writing-skills 話只有 non-obvious decision 先用）
- Persona 設計指引可以去到幾深（archetype library？按產品類型生成 persona 嘅 heuristic？）
- 點樣 handle 冇 browser automation 嘅環境（degrade 定直接話唔適用？）
- 分級要唔要超出 🔴🟡🟢（例如 persona 主觀評分 1-10 制度化？）
- README 級嘅 marketing copy（開源 repo 個賣相）
