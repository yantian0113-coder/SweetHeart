# SweetHeart · Smart Grid Typography Studio

# SweetHeart · 智慧網格排版工作室

**Design-support tool for layout grids, typographic presets, pixel rulers, and alignment guides — with a bilingual **English / 中文** UI.**  
**面向視覺傳達設計的輔助工具：網格、排版預設、像素標尺對齊線與參考線，並支援中英文雙語介面。**

**Repository · 倉庫** | **Live App · 線上應用** | **YouTube · 演示** | **GitHub Pages · 簡報**

---

**Stack · 技術屬性：** Single-page **web app**（`index.html` + Fabric.js + Tailwind CDN）· **無須** `npm install` 或編譯步驟  
**與 Grid-Seed 差異說明：** Grid-Seed 為 **Adobe Illustrator CEP 擴展**；SweetHeart 為 **瀏覽器內運行** 的版面實驗工具，可在 GitHub Pages 直接向老師／同學展示。

---

## Authors · 作者

| English name | 中文名 | Student ID · 學號 |
| --- | --- | --- |
| *HAO TIAN TIAN* | *郝甜甜* | *MC569103* |
| *ZHANG KE XIN* | *張可欣* | *MC569236* |

---

## Repository layout · 倉庫布局說明

| English | 中文 |
| --- | --- |
| On GitHub **`main`**, the **web app** lives at the **repository root**: `index.html` (main UI), `presentation.html` (design deck), and `presentation-assets/` (CSS for the deck). | 在 GitHub **`main`** 分支，**網頁應用**位於**倉庫根目錄**：`index.html`（主程式）、`presentation.html`（設計簡報）、`presentation-assets/`（簡報樣式）。 |
| There is **no** Illustrator CEP folder (`CSXS/`, `jsx/`). Everything runs in a modern browser. | **沒有** Illustrator CEP 目錄（如 `CSXS/`、`jsx/`）。全部在現代瀏覽器中執行即可。 |
| If you clone to a local folder, keep **`index.html` at repo root** next to `presentation.html`; run `python3 -m http.server` **from the repo root** and open `http://127.0.0.1:PORT/index.html`. | 若在本機複製整個倉庫，請保持 **`index.html` 在根目錄**；在**倉庫根目錄**執行 `python3 -m http.server`，再開啟 `http://127.0.0.1:端口/index.html`。 |

---

## Quick links · 快速連結

| Resource · 資源 | URL |
| --- | --- |
| **GitHub repository · 倉庫** | [https://github.com/yantian0113-coder/SweetHeart](https://github.com/yantian0113-coder/SweetHeart) |
| **App (GitHub Pages) · 線上應用** | [https://yantian0113-coder.github.io/SweetHeart/](https://yantian0113-coder.github.io/SweetHeart/) *(須在 Settings → Pages 啟用，`main` / `(root)`)* |
| **Design deck (GitHub Pages) · 線上簡報** | smart-grid-typography-studio-guides-i18n.html|
| **YouTube demo · 演示影片** | **https://youtu.be/FaBdy5Zuy08?si=-t_AzFj_AGQ7qgeK** |

**Sharing · 分享連結：** 請分享 **GitHub Pages 的 `https://…` 網址**，不要分享本機 `http://localhost`。  
若對方畫面異常，常見原因為：用 **`file://` 雙擊** 開檔、或未在**正確資料夾**啟動本機 HTTP 伺服器。

---

## Project overview · 項目簡介

| English | 中文 |
| --- | --- |
| **SweetHeart** is a **browser-based** layout lab: import images, **typography presets** (e.g. rule of thirds, golden ratio, modular / axis / radial / symmetry grids), **pixel rulers**, subtle **guide lines**, layers, **undo**, merge, and **PNG / JPG** export. | **SweetHeart** 為 **瀏覽器端** 版面實驗室：匯入圖片、**排版預設**（如三分法、黃金比例、模組／軸線／放射／對稱等）、**像素標尺**、**參考線**、圖層、**撤回**、合併、**PNG／JPG** 匯出。 |
| **Bilingual UI:** use the **language button** in the header to switch **English / 中文**. | **雙語介面：** 使用頂欄 **語言按鈕** 切換 **英文／中文**。 |
| **No API keys** are required for core features. | **核心功能不需要外部 API Key。** |

---

## Scope · 倉庫能呈現什麼

| English | 中文 |
| --- | --- |
| The **full workflow** (import → presets → guides → export) runs in a **web browser**. GitHub Pages hosts the same files for online demos. | **完整操作流程**（匯入 → 預設 → 參考線 → 匯出）在 **瀏覽器** 內完成；GitHub Pages 用來線上展示。 |
| **Illustrator / InDesign** are **not** required; this tool is for **early layout exploration** in visual communication courses. | **不需**安裝 Illustrator／InDesign；適合視傳課程中 **前期構圖與網格練習**。 |
| For **very large** assets (long screen recordings, huge PDFs), use **Google Drive** or similar — GitHub has file-size limits. | **大型檔案**（長時螢幕錄影、超大 PDF）請放 **Google Drive** 等 — GitHub 對單檔體積有限制。 |

---

## Google Drive · 網盤（可選）

**EN:** GitHub discourages very large files (rough guideline: trouble above ~50–100 MB). Put long demo videos or archives on Drive if needed.  
**中文：** GitHub 不適合放大檔（約 50 MB 會警告、約 100 MB 常無法推送）。演示長片或完整打包可放雲端硬碟。

👉 *（若你有共用作業資料夾，請在此列連結）*

---

## Video · 視頻演示

👉 **請上傳作業要求的 YouTube 後，將連結更新到本 README 上方「Quick links」列。**

---

## GitHub Pages · 在線訪問

**EN:** Enable **Settings → Pages**. For this **static** site, use **Source: Deploy from a branch** → **`main`** → **`/(root)`**. A log that says **“GitHub Actions”** may appear depending on org defaults; static hosting from `main` is sufficient.

**中文：** 在 **Settings → Pages** 啟用。本倉為**靜態網頁**，選 **Deploy from a branch** → **`main`** → **`/(root)`** 即可。

### Common URLs · 常用地址

| Page · 頁面 | URL |
| --- | --- |
| **App · 主應用** | [https://yantian0113-coder.github.io/SweetHeart/](https://yantian0113-coder.github.io/SweetHeart/) |
| **Design deck · 設計簡報** | [https://yantian0113-coder.github.io/SweetHeart/presentation.html](https://yantian0113-coder.github.io/SweetHeart/presentation.html) |

**EN:** If you see **404**: confirm `main` is pushed, Pages is on, wait **1–3 minutes**.  
**中文：** 若 **404**：確認已推送 `main`、已開啟 Pages，等待 **1–3 分鐘**。

---

## Quick start · 快速開始

### Run in the browser · 在瀏覽器中使用

| Step · 步驟 | English | 中文 |
| --- | --- | --- |
| 1 · 克隆 | `git clone https://github.com/yantian0113-coder/SweetHeart.git` | 克隆倉庫至本機 |
| 2 · 目錄 | `cd SweetHeart` | 進入 **`SweetHeart` 根目錄**（須含 `index.html`） |
| 3 · 伺服器 | `python3 -m http.server 8765` | 執行靜態伺服器（埠號可自訂） |
| 4 · 開啟 | [http://127.0.0.1:8765/index.html](http://127.0.0.1:8765/index.html) | 瀏覽器開啟上述網址 |

**EN:** Avoid relying on **`file://` double-click** as your main workflow (some browsers restrict scripts).  
**中文：** 盡量不要用 **`file://` 雙擊** 當主要方式（部分瀏覽器對本機腳本限制較嚴）。

**Design deck locally · 本機簡報：**

```text
http://127.0.0.1:8765/presentation.html
```

（須與 `presentation-assets/` **同屬根目錄**，與現有結構一致。）

---

## Usage · 使用提示

| English | 中文 |
| --- | --- |
| **Presets:** click a card to apply grid + typographic layout (needs a **selected / reference image**). | **預設：** 點卡片套用網格與文字版面（需有 **參考圖片**）。 |
| **Rulers / guides:** toggle in header; choose mint or gray guide palette. | **標尺／對齊線：** 頂欄開關；可選薄荷綠或淺灰參考線色票。 |
| **Language:** click **中文 / EN** in the header to switch UI strings. | **語言：** 頂欄按鈕切換介面語言。 |

---

## Project structure · 項目結構（GitHub `main`）

### Repo root · 倉庫根目錄

| Path · 路徑 | English | 中文 |
| --- | --- | --- |
| **README.md** | This readme | 本說明文件 |
| **index.html** | Main app (canvas UI) | 主程式（畫布介面） |
| **presentation.html** | Design presentation entry | 設計簡報入口 |
| **presentation-assets/presentation.css** | Deck styles | 簡報樣式 |
| **.gitignore** | Ignore rules | 忽略規則 |

**EN:** No `package.json` at repo root — runtime is **browser + CDN scripts**, not a Node build pipeline.  
**中文：** 根目錄 **無** `package.json`：以 **瀏覽器 + CDN** 運行，不依賴 Node 打包發佈。

---

## Tech stack · 技術棧

**EN:** **HTML / CSS / JavaScript**; **Fabric.js** (canvas); **Tailwind CSS** (Play CDN); bilingual strings in-page.  
**中文：** **HTML / CSS / JavaScript**；**Fabric.js** 畫布；**Tailwind CSS**（Play CDN）；雙語字串寫於頁面腳本中。

---

## Contributing · 貢獻（課程內）

**EN:**

1. Prefer **simple ES5-style** syntax for maximum browser compatibility if you extend the script.
2. Keep **preset IDs** in sync wherever they are referenced (`PRESETS` array vs. `drawGrid` branches).
3. Update **both** `zh` and `en` blocks when adding UI strings.

**中文：**

1. 擴充腳本時盡量維持 **相容度髙的 JS 寫法**。
2. 新增預設時，**ID** 與 `drawGrid`／版型邏輯須一致。
3. 新增介面文案時，**中文與英文**一併維護。

---

## License · 許可證

**EN:** Coursework — follow your school’s rules; **Adobe** terms apply if you reference AI workflows. Add an explicit license (e.g. MIT) only if your team agrees for public open source.  
**中文：** 課程作業請遵守校規；若對外開源請團隊協商後再添加明確授權條款。

---

## Features · 特性摘要

**EN:** Multi-preset **grids**, **rulers**, **alignment guides**, **undo**, **layers**, **merge**, **PNG/JPG export**, **bilingual UI**, **`presentation.html` + `presentation-assets/`** for design narrative.  
**中文：** 多種**網格預設**、**標尺**、**對齊參考線**、**撤回**、**圖層**、**合併**、**PNG／JPG 匯出**、**雙語介面**、**`presentation.html` + `presentation-assets/`** 設計簡報。

---

## Push README to GitHub · 將 README 推送到 GitHub

```bash
cd /path/to/SweetHeart
git add README.md
git commit -m "docs: update README"
git push origin main
```

**EN:** To sync the whole repo: `git add -A`, then commit and push (watch **`.gitignore`** and **large files**).  
**中文：** 同步整倉：`git add -A` 後提交並推送（留意 **`.gitignore`** 與**大檔**）。

---

**❤️ Love comes from visual communication design.· 愛來自視覺傳達設計**
