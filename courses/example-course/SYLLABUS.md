---
course: example-course
status: draft
total_modules: 2
hours_budget: 4h/週
phases:
  - {name: Phase 1, modules: 1-2, note: 示例}
excluded_globally: [正規表達式引擎(NFA/DFA)實作]
---

# example-course — 示例課綱(schema 展示)

> 本檔是課綱 schema 的示例,不是真實課程。真實課程放 `courses/<slug>/`,整包 gitignored。
> 覆蓋不變式:每個「概念」條至少對應一題「驗收」——沒有驗收鉤子的概念進不了 retest queue,等於沒學。

## 通則(先於模組)

- 每模組固定六欄:目標 / 概念 / 實作 / 驗收 / 錨點 / 不學。
- 「錨點」把新概念掛回 `LEARNER.md` 的已知領域,寧缺勿硬掛;「不學」是該模組的硬邊界,全域硬邊界收在 frontmatter `excluded_globally`。
- 能量/停損的個人硬數字不在本檔,由引擎讀 `LEARNER.md`。

---

### M1|正規表達式:基礎與心智模型

目標:能讀懂並手寫中等複雜度的 pattern。

概念:
- literal、character class、quantifier(`* + ? {n,m}`)
- anchor(`^ $ \b`)與 escape
- greedy vs lazy 匹配

實作:
- 手寫 10 題常見 pattern(日期、email、log 行擷取),先預測結果再驗證
- 用任一 regex playground 對照 match 過程

驗收:
- [ ] `[a-z]+?` 和 `[a-z]+` 對同一輸入的結果差在哪?為什麼?
- [ ] `^` 和 `\b` 各在什麼時候用?舉一個非用 `\b` 不可的例子
- [ ] 挑一題實作題,逐段解釋 pattern 每一段在做什麼

錨點:
- (示例:若學習者熟 shell,glob ↔ regex 的差異是自然錨點;找不到自然錨點就留白,不硬掛。)

不學:
- lookahead / lookbehind 深水區 —— 基礎課先不碰,遇到需求記進 PARKING

### M2|正規表達式:實戰與陷阱

目標:能在真實文本處理任務安全使用 regex,並知道什麼時候不該用。

概念:
- capture group 與 replace
- catastrophic backtracking(為什麼某些 pattern 會把 CPU 燒滿)
- 「不該用 regex」的判斷(巢狀結構、HTML)

實作:
- 對一份真實 log 檔寫擷取 + 改寫腳本
- 造一個會 backtracking 爆炸的 pattern,實測並修好它

驗收:
- [ ] capture group 的編號規則是什麼?named group 什麼時候值得用?
- [ ] 你造的 backtracking 爆炸案例,根因是什麼?怎麼改寫?
- [ ] 什麼輸入結構不該用 regex 處理?為什麼?

錨點:
- backtracking 爆炸 ↔ 學習者既有的「輸入特定形狀時演算法退化」經驗

不學:
- regex 引擎內部(NFA/DFA)—— 概念一句話帶過即可(全域硬邊界)

---

## Changelog

- 初版:示例課綱,僅為展示 schema。
