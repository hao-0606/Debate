## 具體操作流程教學

這整個流程的目的是：「複製一份 Dify 寫好的網頁外觀程式碼，放到你自己的 Vercel 帳號上跑，然後用密碼（API Key）讓這個網頁去連線你的 Dify 機器人。」

請依照以下步驟操作：

### 第一步：準備工作
1.  註冊一個 **GitHub** 帳號（如果沒有）。
2.  註冊一個 **Vercel** 帳號（建議直接用 GitHub 帳號登入 Vercel）。
3.  在 Dify 後台，找到你的 App，點擊「API 訪問」或「API Access」，**複製你的 API Key**（這是一串亂碼，等一下會用到）。

### 第二步：Fork 程式碼（複製一份到你的倉庫）
1.  點擊 Dify 提示中的 **「Dify-WebClient」** 連結（這會帶你到 GitHub）。
2.  在 GitHub 頁面右上角，找到 **「Fork」** 按鈕並點擊。
3.  點擊後確認 Create Fork。
    *   *這一步的意思是：把 Dify 官方的程式碼，影印一份放到你自己的 GitHub 帳號裡，這樣你才有權限修改或部署。*

### 第三步：在 Vercel 中部署（把網站架起來）
1.  登入 Vercel Dashboard。
2.  點擊 **"Add New..."** -> **"Project"**。
3.  在左側你會看到 "Import Git Repository"，你應該會看到剛剛 Fork 的那個專案（名字通常也叫 `dify-web-client` 或類似的）。
4.  點擊該專案旁邊的 **"Import"** 按鈕。

### 第四步：配置環境變數（最關鍵的一步！）
在 Vercel 的設定頁面中（Configure Project），你會看到一個叫 **"Environment Variables"** 的區塊。這裡是用來填寫「通關密語」的地方，讓你的網頁知道要去哪裡抓 Dify 的資料。

你需要依序新增三組變數（依照你貼文中的資訊）：

*   **第 1 組：**
    *   Name (Key): `NEXT_PUBLIC_APP_ID`
    *   Value: `36e217e7-42dc-4296-9f8d-ea3cbe4b2adf` (這是你貼文中提供的 ID)
    *   *點擊 Add*

*   **第 2 組：**
    *   Name (Key): `NEXT_PUBLIC_APP_KEY`
    *   Value: **(這裡請貼上你在 Dify 後台生成的 API Key)**
    *   *注意：不要保留 `< >` 這些符號，直接貼上代碼即可。*
    *   *點擊 Add*

*   **第 3 組：**
    *   Name (Key): `NEXT_PUBLIC_API_URL`
    *   Value: `https://api.dify.ai/v1`
    *   *點擊 Add*

### 第五步：完成部署
1.  確認三個變數都加入後，點擊下面的 **"Deploy"** 按鈕。
2.  等待約 1-2 分鐘，Vercel 會跑一些安裝程序。
3.  當畫面出現滿滿的彩帶或 "Congratulations!" 時，點擊預覽圖或 **"Visit"** 按鈕。

恭喜！你現在擁有一個獨立網址的 AI Web App 了。

---

## 常見問題

**Q: 我如果要改網頁上的文字或 Logo 怎麼辦？**
A: 因為你已經 Fork (複製) 程式碼到你的 GitHub 了。你需要回到 **GitHub** 上你自己的那個倉庫，直接修改程式碼檔案（通常在 `config` 資料夾或是 `public` 資料夾換圖片）。你一修改並儲存 (Commit)，Vercel 就會偵測到變更，自動重新部署更新網站。

**Q: 為什麼不能直接用 Dify 給的連結就好？**
A: Dify 原生的連結通常長得像 `dify.ai/app/xxx`，你不能改它的 Logo 或版面。透過這個方法，你可以把網頁架在自己的網域，甚至把介面改成你自己公司的風格。

希望這樣解釋有幫助到你！如果在填寫環境變數那邊卡住，通常是 Key 複製錯誤，檢查一下即可。
