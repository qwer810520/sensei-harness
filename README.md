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
courses/
  example-course/
    SYLLABUS.md               示例課綱:展示課綱 schema  (committed)
  <你的課程>/                  真實課程整包               (gitignored)
    SYLLABUS.md / PROGRESS.md / GRADEBOOK.md / PARKING.md
```

## 隱私模型

本 repo 為 public。個人學習資料(`LEARNER.md` 與真實課程目錄整包 —— 課綱、進度、成績、停車場)一律 `.gitignore`,並由 `.githooks/pre-commit` 二次攔截,永不上網。公開的只有引擎與示例:匿名結構看 `LEARNER.example.md`,課綱 schema 看 `courses/example-course/SYLLABUS.md`。

啟用 hook(clone 後執行一次):

```sh
git config core.hooksPath .githooks
```
