# sensei-harness

一個**可重複用於任何學習目標**的 AI 教練引擎。

> 核心設計:**課程是資料,引擎不動。** 換一門課只換課綱檔,教練協議一行不改。

## 這是什麼

大多數「AI 家教」把教學內容和教學方法綁死在一起,換個主題就得整包重寫。`sensei-harness` 把兩者拆開:

- **引擎(`CLAUDE.md`)** — 技能無關的教練協議:怎麼提示、怎麼 review、怎麼評分、什麼時候該喊停。這套規則對任何學科都成立。
- **資料(可抽換)** — 三種檔案:
  - `LEARNER.md` — 學習者是誰、怎麼學、有哪些要主動防禦的行為模式。
  - `courses/<slug>/SYLLABUS.md` — 這門課教什麼(模組化課綱)。
  - 狀態檔(進度 / 成績 / 停車場)— 引擎在教學過程中維護。

開一門新課 = 寫一份 `SYLLABUS.md` 丟進 `courses/`,開場宣告 slug 即可上課。

## 開始使用

需要:[Claude Code](https://claude.com/claude-code),以及用 `git clone` 取得本 repo(下載 ZIP 沒有 `.git`,隱私 hook 裝不起來)。

1. **啟用隱私 hook**(clone 後執行一次):
   ```sh
   git config core.hooksPath .githooks
   ```
2. **寫你的學習者檔案**:複製 `LEARNER.example.md` 為 `LEARNER.md`,依模板填寫。
3. **寫課綱**:建立 `courses/<slug>/SYLLABUS.md`。規範與「跟 Claude 一起編課」的提示詞見 [`docs/syllabus-guide.md`](docs/syllabus-guide.md),完整示例見 [`courses/example-course/SYLLABUS.md`](courses/example-course/SYLLABUS.md)。
4. **確認課綱**:看過一遍,把 frontmatter 的 `status` 從 `draft` 改成 `active`。
5. **開始上課**:開一個新的 Claude Code session,說「開始上課,課程是 `<slug>`」。

## 已知限制

以下是引擎目前依賴、但這個 repo 沒有附上的東西。缺了不影響上課,但對應的功能要照下面的方式處理。

- **筆記功能依賴作者的私人環境。** 你要求整理筆記時,引擎會先針對當次教過的內容出一份小考,通過後才整理成筆記。整理好的筆記,協議規定交給一支叫 `learning-note` 的 skill 寫進 Obsidian vault(知識庫)。
  - **`learning-note` 的作用:** 讓寫進知識庫的筆記守同一套規則。它會判斷筆記該放進哪個分類資料夾、檢查檔名不跟既有筆記撞名,並把新筆記先放成草稿,等你確認過分類再移進正式資料夾。引擎自己不直接寫知識庫,就是為了讓這些規則只由一個地方負責。
  - **這支 skill 和 vault 都不在 repo 裡。** 沒有的話,小考照常進行,只是最後一步要你決定筆記去哪:直接告訴教練寫到哪個資料夾,或請它把筆記內容貼在對話裡。
- **狀態檔不一定會自動建立。** 引擎只明確規定 `PROGRESS.md` 不存在時要建立。`GRADEBOOK.md`(成績)和 `PARKING.md`(岔路紀錄)第一次要用到時,如果教練沒有自己建,請它建立,或自己建一個空檔:
  - `GRADEBOOK.md` 每列一筆判定,格式:`日期|模組|題目|PASS/FAIL|一句話註記|重考排程`
  - `PARKING.md` 每列一條岔路,格式:`日期|岔路一句話|來源模組`
- **沒有「編課」模式。** 引擎只為上課設計,在 repo 裡開 Claude Code 時,它可能直接進入上課的開場流程。要編課,請用 [`docs/syllabus-guide.md`](docs/syllabus-guide.md) 最後的提示詞,它的第一行會讓教練跳過開場流程。

## 教學協議重點

- **絕不替學習者寫實作碼。** 教練給方向、給骨架、給測試,不給答案 — 唯一例外是一個帶代價的暗語。
- **提示階梯 L0–L3。** 從反問起,逐層加深,學習者明說才升級。
- **二元評分 + 間隔重考。** PASS / FAIL 不設中間級;沒過的進重考佇列,連過兩次才出列。
- **主動停損。** 依學習者的能量參數強制收尾 — hyperfocus 是風險,不是進度。

## Repo 結構

```
CLAUDE.md                     引擎:教練協議,技能無關
LEARNER.md                    學習者檔案            (gitignored)
LEARNER.example.md            匿名模板              (committed)
.gitignore
.githooks/pre-commit          私有檔案進 staging 的二次攔截
.claude/skills/oral-exam/     口試考官 skill(CC 與 Chat 共用)
docs/
  syllabus-guide.md           課綱撰寫指南:規範、檢查清單、編課提示詞
courses/
  example-course/
    SYLLABUS.md               示例課綱:展示課綱 schema  (committed)
  <你的課程>/                  真實課程整包               (gitignored)
    SYLLABUS.md / PROGRESS.md / GRADEBOOK.md / PARKING.md
```

## 隱私模型

本 repo 為 public。個人學習資料(`LEARNER.md` 與真實課程目錄整包 —— 課綱、進度、成績、停車場)一律 `.gitignore`,並由 `.githooks/pre-commit` 二次攔截,永不上網。公開的只有引擎與示例:匿名結構看 `LEARNER.example.md`,課綱 schema 看 `courses/example-course/SYLLABUS.md`。

啟用方式見上方「開始使用」第 1 步。
