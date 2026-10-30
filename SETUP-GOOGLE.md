# 開啟 Google 雲端同步（約 10 分鐘，只要做一次）

沒做這一步，遊戲照樣能玩，畫面會顯示「📱 未設定雲端，只存在這台裝置」。做完之後，每台裝置按「☁️ 用 Google 登入同步」，紀錄就會合併到你自己雲端硬碟的 `知識島學習紀錄.json`。

## 1. 建立 Google Cloud 專案
1. 打開 https://console.cloud.google.com/ ，用你的 Google 帳號登入。
2. 點上方的專案選單，選「新增專案」，名稱填「知識島」，然後按「建立」。
3. 進入「API 和服務 → 程式庫」，搜尋 **Google Drive API**，按「啟用」。

## 2. OAuth 同意畫面（Google Auth Platform）
1. 進入「API 和服務 → OAuth 同意畫面」（新版介面叫「Google Auth Platform → 品牌塑造／目標對象」）。
2. 應用程式名稱填「知識島」，支援電子郵件填你自己的信箱。
3. 使用者類型選 **外部（External）**。
4. 「資料存取／範圍」加入 `https://www.googleapis.com/auth/drive.file`。這是**非機密（non-sensitive）**範圍，只能存取本程式自己建立的檔案，不需要 Google 審查。
5. 發布狀態有兩種：
   - **測試中（Testing）**：只有你在「測試使用者」裡加入的帳號能登入，最多 100 個。把家裡會用的 Google 帳號都加進去。注意：測試中的授權大約 7 天會過期，到時重新登入一次即可。
   - **正式版（In production）**：按「發布應用程式」。因為只用非機密範圍，不需要審查，任何帳號都能登入。第一次登入可能會看到「Google 尚未驗證這個應用程式」，點「進階 → 前往知識島」即可。
   - 家用建議直接選「正式版」，就不用每 7 天重新授權。

## 3. 建立 OAuth 用戶端 ID
1. 「API 和服務 → 憑證 → 建立憑證 → OAuth 用戶端 ID」（或 Google Auth Platform → 用戶端 → 建立用戶端）。
2. 應用程式類型選 **網頁應用程式**。
3. 「已授權的 JavaScript 來源」加入 `https://caroelvis.github.io`。只填網域，不要加 `/knowledge-island`，結尾也不要加 `/`。
4. 「已授權的重新導向 URI」留空，這個程式不需要。
5. 按「建立」，複製用戶端 ID（看起來像 `1234567890-abc...apps.googleusercontent.com`）。用戶端 ID 不是密碼，放在公開網頁上沒問題。**不需要**用戶端密鑰。

## 4. 把用戶端 ID 貼進網站
1. 打開 https://github.com/caroelvis/knowledge-island/blob/main/config.js ，按鉛筆圖示編輯。
2. 改成 `window.LG_GOOGLE_CLIENT_ID = "你的用戶端ID.apps.googleusercontent.com";`
3. 按「Commit changes」，等 1～2 分鐘 GitHub Pages 更新。
4. 重新整理遊戲，按「☁️ 用 Google 登入同步」。

## 從舊版（Apps Script）搬家
舊版在「家長分析 → 備份」下載 JSON，新版到「家長分析 → 還原」選那個檔案即可。建議在新版還沒有紀錄時先做這一步。

## 常見問題
- **每次打開都要按登入嗎？** 登入權杖有效約 1 小時，只存在這個分頁。過期後按一次登入即可，已授權過的帳號通常一按就好。沒登入時紀錄照樣存在本機，登入後會自動合併。
- **出現 `origin_mismatch`**：第 3 步的 JavaScript 來源沒填對。
- **出現 `access_denied`**：同意畫面還在「測試中」，而這個帳號不在測試使用者名單裡。
