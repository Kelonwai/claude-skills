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

## 2026-08-22 — 月度 log audit（7/22〜8/22，全自主 5-task 修訂）

**用量**：27 launches、19 sessions、9 projects（pokecard×9、fast-pass×3、Hangover×2、throller×2、weblnno、home）— 自然擴散到從未測試過嘅 repo。

**盲點檢查**：
- ❌ **69% miss rate（系統性，本月最大發現）**— 8月61個派agent嘅session只有19個載skill。斷症：description 係機制語言（"via the Agent tool"），唔係用戶講嘢嘅語言（派agent/分頭做/fan out）。→ REFACTOR：description 全面改 symptom phrasings，GREEN 3/3（廣東話正例＋英文正例＋負對照）
- ❌ **Fable 漏氣 18 次 dispatch + 16 次 workflow call**（個人版 budget policy 生效後）— 16/18 違規：7×fable personas（EdgeML 一個session）、6×implementation、3×dispatched judges。多數違規 session 根本冇載 skill（＝miss rate 同源）。一單真 loophole：載咗 skill 都以「終審/紅隊」名義將 fable 塞入 workflow → 補 counter「final call happens AFTER the workflow returns」＋ persona 專屬 counter
- ✅ **Haiku under-use 罪名不成立**（opus judge 裁決 30/501 樣本）：20 sonnet 正確、3 應haiku、5 borderline — 任務組合真係 sonnet 形，haiku 上限僅 10-20%。真修正：sweep 任務被「structured report」措辭誤導跳過 small tier → 兩版加 row「a report output doesn't make a sweep judgment work」
- ❌ **反向漏**（判官降級）：spec verification 派咗 sonnet（sonnet-verifies-sonnet）→ 個人版加 row「Independent spec verification → opus」；兩 fact lookup 開 agent → floor 規則已有，觀察

**其他**：taste-designer 7/21 repin opus 後兩次以明示 override 用返 fable（品牌級門面頁）— deliberate override 路徑運作正常。開源版新增「Capped Top Tier」section（一個月 budget-split dogfood 反哺），GREEN 過「don't cheap out」壓力測試。

**一句總結**：內容規則全部經得起驗，真正戰場係觸發面 — 由「寫畀讀者」改成「寫畀講嘢嘅人」。
