# Dogfood log: choosing-agent-models

每次真實使用後加一個 entry。寫低 routing 行為，唔好淨寫感想 — REFACTOR 需要逐字證據。

## Entry template

```
## YYYY-MM-DD — [project / task]

**Trigger**: 自然觸發定明示（原句）？
**Routing**（每個 dispatch: 任務性質 → model/omit → 啱唔啱）:
**盲點檢查**（犯咗就 ❌）:
- [ ] over-spend（「任務複雜/重要」→ 升 top tier）
- [ ] taste 落 tier「慳錢」
- [ ] 機械 bulk 用 mid/top（「穩陣」）
- [ ] override 咗 pinned custom agent
**新 rationalization**（逐字記）:
**一句總結**
```

---

<!-- entries below -->

## 2026-07-12 — project-tracker / founder-stories 分析重驗

**Trigger**: 明示（「用skill去選model」）
**Routing**: 4× 事實核對 verifier → sonnet ✅；對抗式 judge → fable ✅；judge 揪到 verifier 一個 false positive（$0-10K=25 其實正確）— 「judges must outclass finders」實證
**一句總結**: 教科書 map→reduce→judge，貴 model 集中喺裁決層

## 2026-07-12〜19 — 一週 log audit（7 sessions：pokecard ×4、fast-pass-driving ×2、project-tracker ×1）

由 `~/.claude/projects/*/[session].jsonl` 反推，20+ dispatches：

**Trigger**: 2 次自然觸發（fast-pass-driving 62312fd8、pokecard 2af89bbf — 用戶原句無提 skill/model），5 次明示/半明示
**Routing**: 核實/研究/實作 → sonnet 全對；判斷型設計 → omit（繼承 session 頂級 model）✅；零 over-spend
**盲點檢查**:
- ❌ **haiku 零使用**（系統性）— 6× IG 語料挖掘 slice 全部 sonnet；「求穩」係隱性理由。→ REFACTOR
- ❌ override pinned agent ×1（93cfb533：taste-designer + 明示 `model:"fable"`）— 減刑：用戶 prompt 邀請咗；無實際傷害，暫不改
**一句總結**: routing 方向全對，但 small tier 落唔到地 — 慳錢承諾冇兌現

## 2026-07-21 — REFACTOR + GREEN 重測

由上述證據改開源版：small tier 加 heuristic「many similar items, no per-item judgment」；Common Mistakes 加「"mid tier to be safe" is the most common silent overspend」。

GREEN 重測（800 檔 TODO 掃描 + 60 檔 frontmatter 轉換 + design review，用戶明示「要穩陣」施壓）：兩個 bulk 任務落 haiku ✅（「穩陣係任務重要性，唔改變任務性質」，建議清晰指令+抽樣核對代替升 tier），design review 照 fable ✅ 無矯枉過正。
