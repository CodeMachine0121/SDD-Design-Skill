# SDD Design Skills

[English](README.md) · **繁體中文**

一組**設計導向的 Spec-Driven Design (SDD)** 技能，把「一個模糊的設計需求」一路帶到「可實際體驗的畫面」。四個技能各司其職、依序交棒，每一步都產出一份存放在 `.sdd/` 的文件，成為下一步唯一的輸入來源。

核心理念：**需求與驗收條件只有一個家、UI 的事只在 UI 階段決定、實作階段不再重新做設計決策。**

而且 pipeline **不是單行道**：當下游發現缺陷其實屬於上游（例如 `design-build` 蓋到一半發現某情境根本沒對應畫面），會透過**回饋閉環**把問題退回擁有者技能修正，再往下重同步——每份文件永遠只有一個技能能編輯。

> 📊 **互動式流程圖**：想快速看懂整條 pipeline 與回饋閉環，開啟 [`docs/flow.zh-TW.html`](docs/flow.zh-TW.html)（英文版：[`docs/flow.html`](docs/flow.html)）。內含可切換流向的示意圖，以及「一個退件情境如何閉環」的逐步動畫。

---

## 安裝（透過 Claude Code 指令）

在 Claude Code 中，只要兩步就能裝好這個 plugin。

**1. 加入 marketplace**（把這個 repo 註冊為 plugin 來源）：

```
/plugin marketplace add CodeMachine0121/SDD-Design-Skill
```

**2. 安裝 plugin**：

```
/plugin install sdd-design-skills@SDD-Design-Skill
```

> `sdd-design-skills` 是 plugin 名稱，`SDD-Design-Skill` 是上一步加入的 marketplace 名稱。

安裝後重啟 Claude Code（或執行 `/plugin` 確認狀態），四個技能的觸發詞（如 `/ux-spec`、`/ui-spec`、`/design-build`）即可使用。

**管理與更新**：

```
/plugin                                   # 開啟 plugin 管理介面，啟用／停用／檢視
/plugin marketplace update SDD-Design-Skill   # 更新到 marketplace 最新版
/plugin uninstall sdd-design-skills@SDD-Design-Skill
```

---

## Pipeline 總覽

```
                 交棒 ──▶（下游以上游文件為唯一輸入）
   ux-spec              ui-spec                (optional)              design-build
 ┌──────────┐        ┌──────────┐          ┌────────────────────┐   ┌──────────────┐
 │  需求 +   │  ───▶  │  UI 設計  │  ───▶    │ ubiquitous-        │   │  真實畫面 +   │
 │  流程 +   │        │ + 視覺數值 │          │ language-mapping   │──▶│  可播放流程   │
 │  AC 情境  │        │（依 guide）│          │（共用語彙，可略）    │   │ (HTML/Figma) │
 └──────────┘        └──────────┘          └────────────────────┘   └──────────────┘
 DESIGN-BRIEF.md      UI-SPEC.md              UL-MAP.md                 build/
      ▲                    ▲                                                │
      │                    └──────── 退件：畫面/狀態/視覺值缺失或不可實作 ──────┤
      └────────────────────────────── 退件：需求/流程/情境(AC) 有誤或不可行 ──────┘
                 ◀── Handback（退回擁有者修正）＋ 修正後往下 Re-sync（增量重同步）
```

| 階段 | 技能 | 產出 | 一句話定位 |
|---|---|---|---|
| 1 | **ux-spec** | `DESIGN-BRIEF.md` | 用**體驗語言**談清楚需求、流程，並以情境（Specification by Example）鎖定 —— 這些情境**就是驗收條件 (AC)** |
| 2 | **ui-spec** | `UI-SPEC.md` | 純粹談 **UI 的事**：畫面、元件、階層、狀態、互動，以及**這個功能真正需要**的視覺數值（依 design guideline） |
| 3 | **ubiquitous-language-mapping** | `UL-MAP.md` | **選用**。在 ui-spec 之後，把設計實際落地的語彙收攏成一份共用字典 |
| 4 | **design-build** | `build/`（HTML 或 Figma） | **純實作**：照 UI-SPEC 把畫面做出來，並以 DESIGN-BRIEF 的 AC 為驗收準則 |

---

## 各技能說明

### 1. `ux-spec` — 需求與體驗探索
- **做什麼**：與使用者達成 100% 意圖共識 —— 使用者意圖、底層需求、目標體驗、端到端流程；再用**具體情境**（happy path／邊界／例外）把意圖釘死。
- **關鍵原則**：
  - 全程停留在**體驗語言**，不碰 UI 元件、版位、視覺數值。
  - 情境 = **驗收條件**。這份 brief 是所有**需求範例與 AC 的唯一來源**，下游只引用、絕不重寫。
- **輸出**：`.sdd/{yyyy-MM-dd}-{feature-slug}/DESIGN-BRIEF.md`
- **觸發**：`ux-spec`、`探討需求`、`設計探索`、`/ux-spec`

### 2. `ui-spec` — UI 設計
- **做什麼**（FEATURE 模式）：拿確認過的 brief，跟使用者把**每個畫面**的元件、位置、階層、狀態、互動談清楚。因為 UI 是在這裡決定的，所以**可以、也應該**落到具體視覺數值（色彩用法、尺寸、間距、圓角、字體、邊框），但**只做這個功能真正需要的深度** —— 依情況問對應的問題，而不是機械式地問每個元件的圓角/色碼。
- **design guideline**：ui-spec 是吃 guideline 的地方。使用者若提供 design tokens／design system，落定的數值就對應到它；guideline 沒有的就標 `assumed`，還不能決定的標 `TBD`。
- **不做**：**完全沒有** scenario、沒有 Given/When/Then —— AC 一律留在 brief，這裡只引用做覆蓋確認。
- **另有 FOUNDATIONS 模式**：`/ui-spec foundations`，分析 UL-MAP 與既有設計，產出 `.sdd/DESIGN-FOUNDATIONS.md`（版面系統、元件清單、互動與無障礙規範）。
- **輸出**：`.sdd/{yyyy-MM-dd}-{feature-slug}/UI-SPEC.md`
- **觸發**：`ui-spec`、`UI 設計`、`畫面規格`、`/ui-spec`、`/ui-spec foundations`

### 3. `ubiquitous-language-mapping` — 共用設計語彙（選用）
- **做什麼**：維護一份字典，讓「使用者用語 ↔ 元件/樣式名稱 ↔ UI 標籤 ↔ design token」指向同一個概念。
- **時機**：**選用**，且建議在 **ui-spec 之後**執行 —— 等 UI 設計把畫面、元件、標籤、數值都定下來，再把實際落地的語彙收攏起來。跨功能要重用語彙時很值得做；一次性功能可略過。
- **輸出**：`.sdd/UL-MAP.md`
- **觸發**：`ubiquitous language`、`UL map`、`設計語言`、`/ul-init`、`/ul-update`

### 4. `design-build` — 產生真實設計
- **做什麼**：把 UI-SPEC 變成能操作的東西。**純實作**，不重新做視覺決策：照 spec 已定的數值做，spec 刻意留白的細節則在建置時依 guideline/foundations 補上合理值並列出來。
- **驗收準則 (oracle)**：**DESIGN-BRIEF 的情境（AC）** —— 逐條走過並確認在成品中可觀察。
- **兩種產出**：
  - **HTML**：**單一自包含 `index.html`**，內嵌 CSS + JS，用 mock data 驅動各種條件畫面／狀態，讓 reviewer 在一個檔案裡走完整段流程。
  - **Figma**：除了畫面，還要**串好完整可播放的 prototype** —— 設計師按下 Play 就能體驗整段 user flow（含轉場動畫）。
- **輸出**：`.sdd/{yyyy-MM-dd}-{feature-slug}/build/`
- **觸發**：`design-build`、`build the design`、`生成設計`、`/design-build`

---

## `.sdd/` 產出結構

```
.sdd/
├── UL-MAP.md                          # 跨功能共用語彙（選用）
├── DESIGN-FOUNDATIONS.md              # 跨功能設計基礎（選用，ui-spec foundations 產出）
└── {yyyy-MM-dd}-{feature-slug}/       # 一個功能一個資料夾
    ├── DESIGN-BRIEF.md                # ux-spec：需求 + 流程 + AC 情境
    ├── UI-SPEC.md                     # ui-spec：UI 設計 + 視覺數值
    └── build/                         # design-build：HTML 單檔 或 Figma 連結
```

---

## 典型流程

1. **`ux-spec`** — 談清楚要解決什麼、使用者怎麼走、各種情境（= AC）。得到 `DESIGN-BRIEF.md`。
2. **`ui-spec`** — 把畫面與 UI 細節設計出來（有 design guideline 就一起帶入）。得到 `UI-SPEC.md`。
3. *(選用)* **`ubiquitous-language-mapping`** — 收攏這次設計的語彙。得到／更新 `UL-MAP.md`。
4. **`design-build`** — 選 HTML 或 Figma，把設計做成可體驗的成品，並對照 brief 的 AC 驗收。

---

## 回饋閉環（Handback & Re-sync）

Pipeline 是雙向的。當下游技能發現缺陷其實屬於上游，不會自行硬補、也不會偷改上游文件，而是：

- **向上退件（Handback）**：下游停下來，產出一個結構化退件區塊（缺陷是什麼、對應到哪份文件的哪個情境/元素、建議怎麼改），**路由給擁有那份文件的技能**去修。
- **向下重同步（Re-sync）**：擁有者技能修訂後，下游偵測到自己的產出已過期，以**增量修訂**（只改受影響的部分）而非重做的方式對齊。

**退件路由表：**

| 缺陷本質 | 退回哪個技能 | 修哪份文件 |
|---|---|---|
| 需求／流程／情境(AC) 有誤、缺漏，或**作為體驗根本不可行** | `ux-spec` | `DESIGN-BRIEF.md` |
| 畫面／元件／狀態／互動／視覺值 缺漏、矛盾，或**做不出來** | `ui-spec` | `UI-SPEC.md` |
| 語彙漂移／新出現的名詞要收攏 | `ubiquitous-language-mapping` | `UL-MAP.md` |

**兩條保命規則**：

- **擁有權唯一**：每份文件只有一個技能能編輯（brief→ux-spec、UI-SPEC→ui-spec、UL-MAP→ul-mapping、build→design-build）。回饋靠「退件 + 重同步」達成，不靠越權改檔——這正是「AC 只有一個家」得以成立的原因。
- **收斂優先**：能就地用低影響方式解決的就別退件（例如 design-build 對 spec 留白的細節補合理值即可）；只有**結構性／影響 AC** 的缺陷才升級，且退件批次一次給完，避免一來一回。

---

## 設計原則（跨技能一致）

- **AC 只有一個家**：需求範例與驗收條件全都在 `DESIGN-BRIEF.md`；下游引用、不複寫。
- **各階段語言分層**：ux-spec 用體驗語言；ui-spec 才談 UI 與視覺數值；design-build 只實作。
- **依判斷提問，不跑固定清單**：ui-spec 只針對這個功能真正需要的 UI 細節提問。
- **實作不重做設計**：design-build 照 spec 做，留白處補合理值並標記為假設。
- **雙向回饋、擁有權唯一**：下游缺陷經 Handback 退回擁有者修正、再向下 Re-sync；沒有任何技能編輯不屬於自己的文件。
- **選擇題提問**：需要向使用者提問時，一律給 ≥3 個具體選項 + 「Other」。
