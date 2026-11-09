# 《紙妻》互動劇本殺｜GitHub Pages V5.0

這份壓縮檔是**可直接上傳 GitHub Pages 的公開網站前端**。它不包含七位角色的私有劇本、固定問答答案、完整線索或最終真相；這些資料已移至獨立部署的 Cloudflare Worker。

## 本版本包含

- 七位角色立繪、正面頭像、清河陳宅場景及視覺小說式對話介面。
- 序章、七位人物介紹、兩輪公開調查、最後陳述、兇手指認與《陳氏古法》去留投票。
- **免 Token 固定選項模式**：每位角色每輪三個主問題；每個主問題包含三個第一層追問，每個第一層追問再包含三個第二層追問。玩家選擇一條問題分支後，答案由 Worker 逐層提供，不會把所有答案樹放在公開網站。
- **Token 自由對話模式**：可自由輸入問題，由 Groq AI 扮演其他角色；玩家自行提供 API Key。
- 87 條線索分成 44 條第一批與 43 條第二批，第二批必須完成第一輪後才解鎖。
- 個人角色劇本及第二輪任務只由 Worker 回傳給玩家自己的角色；不會把其他角色的私有劇本送到公開前端。

## 重要：部署順序

**這個網站需要 Cloudflare Worker 才能開始遊戲。** 這是為了把角色秘密、固定問答、尚未公開的線索和案件真相留在伺服器端。免 Token 模式仍然不會呼叫 Groq，也不會消耗模型 Token，但仍需 Worker 提供固定內容與遊戲 Session。

### 第一步：部署 Worker

1. 先下載同一版本的 `Paper_Wife_Cloudflare_Worker_V5.0.zip`，解壓縮。
2. 依照 Worker 壓縮檔中的 `README.md` 建立 KV、填入 KV ID，並部署到 Cloudflare Workers。
3. 複製部署後的 `https://....workers.dev` 網址。

### 第二步：上傳 GitHub Pages

1. 解壓縮本壓縮檔。
2. 將解壓縮後**根目錄內的所有檔案與資料夾**上傳到 GitHub Repository 根目錄。確認 `index.html` 直接位於根目錄，不要多包一層資料夾。
3. GitHub Repository → **Settings → Pages**。
4. 在 Build and deployment 選擇 **Deploy from a branch**，Branch 選 `main`，資料夾選 `/ (root)`，儲存。
5. 開啟 GitHub Pages 網址，點右上角齒輪設定，貼上第一步取得的 Cloudflare Worker 網址，按「測試 Worker 連線」後儲存。

### 第三步：測試兩種模式

- 選「免 Token 固定選項模式」：只需要 Worker 網址，不需要 Groq API Key。
- 選「Token 自由對話模式」：除了 Worker 網址，還要輸入自己的 Groq API Key；可先按「測試 Groq API 連線」。API Key 不會寫入 localStorage，也沒有放在網站程式碼中。

## 檔案說明

- `index.html`、`style.css`、`app.js`：網站介面及流程。
- `data/cases.js`：僅含公開案件背景、人物公開介紹及場景/肖像路徑，不含完整線索及角色私密內容。
- `assets/`：人物立繪、頭像與場景圖片。
- `.nojekyll`：讓 GitHub Pages 保留靜態資產路徑。
- 本壓縮檔**刻意不包含** `worker/`、原始 DOCX、完整角色私有資料或真相資料；請勿把 Worker ZIP 解壓後的 `src/private-data.js` 上傳到公開 GitHub。

## Token 精簡設計

Worker 以事先整理的 AI 精簡知識卡為核心，並針對當次提問挑選最多 4 條相關個人時間線、最多 2 條防守規則、最多 3 條已知事實、最多 4 條關係、最多 7 條相關公開線索及最多 7 條相關公開發言。每次呼叫不會重新傳送角色五幕完整劇本。公開紀錄逐漸增加時，輸入 Token 仍可能上升；實際用量以 Groq 回傳值為準。

## 案情一致性

本版以組織手冊與角色時間線交叉整理：惠雯真正死於阿忠 23:05 的行動，23:20 確認死亡；紅豆 22:50 的勒頸行動未完成；東青在杯口塗藥，但杯中水未被飲用；江靜儀 23:45 的注射發生在惠雯死亡之後。孔神婆由假陳聿明（駱午陽）於約 02:00 在涼亭殺害，紅豆後續處理屍體。

## 本地檢查

本版已執行 JavaScript 語法檢查、Worker 模擬端點測試、兩批線索解鎖測試、七位角色固定問答資料結構檢查，以及 AI Prompt 精簡測試。由於沒有使用你的實際 Cloudflare 帳號與 Groq API Key，仍需部署後在你的環境測試實際連線。
