# AI 自動剪輯實戰：互動課程網頁

整個網站只有一個檔案 `index.html`，裡面包含 HTML、CSS 和 JavaScript。不需要安裝任何東西、不需要 build，也不用連網路就能運作。

## 怎麼打開
- **在自己電腦上**：直接雙擊 `index.html`，用 Chrome、Edge 或 Safari 開啟就可以。
- **GitHub Pages**：把 `index.html` 放進一個 repo，到 Settings → Pages，選擇 `main` branch 的根目錄。網址會是 `https://<帳號>.github.io/<repo>/`。
- **Netlify Drop**：打開 https://app.netlify.com/drop ，把整個 `course-site` 資料夾拖進去，幾秒後就會有公開網址。
- 其他靜態主機（Vercel、Cloudflare Pages、S3、公司內部主機）也都可以：上傳 `index.html` 就好。

網址後面的 `#u1`、`#u3` 這類代號代表頁面，可以直接分享，例如 `.../index.html#u3` 會打開指令產生器。

## 怎麼改內容
課程文字全部集中在 `index.html` 的 `const COURSE = {...}` 和後面的 `COURSE.pages.xxx = {...}` 裡，前面有中文註解標示。下方「以下是程式邏輯」那一段是程式本身，改內容時不用動。

| 想改的東西 | 位置 |
|---|---|
| 課程名稱、總分鐘數 | `COURSE.title / subtitle / totalMinutes` |
| 側欄順序與分組 | `COURSE.navGroups` |
| 各頁內容 | `COURSE.pages.home / prep / u1 / u2 / ub / u3 / u4 / u5 / u6 / u7 / review / teacher` |
| 單元完成條件 | 各頁的 `reqs`（練習用 `req: "id"` 綁定） |
| 路線 A／B 的優點與限制、四件事檢查、修改例句 | `COURSE.shared` |
| 指令產生器的主題、風格、功能文字、品質檢查 | `COURSE.generator` |
| 卡關處理（第 5 單元＋課後「常見錯誤」） | `COURSE.troubles` |
| 修改練習的 12 個問題（第 6 單元＋課後「常用修改句」） | `COURSE.editPractice` |
| 美髮指令範本 | `COURSE.templates` |
| 發布前檢查表 | `COURSE.publishChecks` |

注意：頁面 id（`#ub`、`#u3`…）沿用原課程的網址，所以 `ub` 是第 3 單元、`u3` 是第 4 單元，依此類推。

### 會隨上方切換而改變的文字
用 `v({...})` 包起來：
```js
html: v({ chatgpt: "按「＋」上傳影片", claude: "把影片拖進對話" })
rows: v({ chat: [...], agent: [...] })
title: v({ "gpt-chat": "...", "claude-chat": "...", "gpt-agent": "...", "claude-agent": "..." })
```
比對順序是：AI＋路線組合 → AI → 路線 → `default`。

### 區塊種類（`t` 欄位）
- 文字類：`p`、`small`、`h3`、`list`、`callout`、`banner`、`tagPink`、`table`、`grid2`、`tree`、`code`（含複製鈕）、`quote`、`links`、`sources`、`action`（按鈕）
- 互動類：`toolcards`（工具卡）、`quiz`（單選練習）、`match`（分級配對）、`tabs`（分頁切換）、`toolmap`、`flow`、`layers`、`stratSim`（剪輯策略模擬）、`arollSim`（時間軸模擬）、`gen`（指令產生器）、`prompt`（顯示產生器指令）、`routeCards`、`aiPick`、`steps`（依平台的步驟清單）、`checklist`、`publishCheck`、`trouble`、`troubleAccordion`、`sentence`、`editPractice`、`homework`、`progressCards`、`templates`、`fixList`、`allSteps`、`testList`

最簡單的改法是找一個類似的區塊複製一份再改文字。新增測驗題時，在 `qs` 陣列加一筆 `{ q, opts, a（正解的位置，從 0 算起）, ex }`。

## 進度儲存
勾選、作答和進度存在瀏覽器的 localStorage，key 是 `COURSE.storageKey`。換電腦或換瀏覽器不會跟著走。改掉 `storageKey` 會讓所有人的進度重新開始，大改版時可以這樣做。
