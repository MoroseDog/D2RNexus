<p align="center">
  <img src="docs/images/icon.png" width="96" alt="D2R Nexus 圖示">
</p>

<h1 align="center">D2R Nexus · 暗黑中樞</h1>

<p align="center">《暗黑破壞神 II：獄火重生》多帳號啟動與管理工具（Windows）</p>

<p align="center">
  <a href="https://github.com/MoroseDog/D2RNexus/releases"><img src="https://img.shields.io/github/v/release/MoroseDog/D2RNexus?include_prereleases&label=download" alt="下載最新版本"></a>
  <img src="https://img.shields.io/badge/Windows-10%20%7C%2011-0078D6" alt="Windows 10 | 11">
</p>

![D2R Nexus 主畫面](docs/images/screenshot.png)

勾選要玩的帳號，按一下「啟動所選」，Nexus 會依序幫你開好每一個遊戲，並自動解除多開限制。有綁手機驗證器（OTP）的帳號也能用。

> 目前是開發測試版，部分情境仍在實機驗證中。

## ✨ 功能

**多開與登入**

- **一鍵多開** —— 勾選帳號，依你排的順序逐一開啟，自動解除多開限制
- **支援 OTP 驗證器** —— 手機核准一次，之後直接啟動
- **群組** —— 常一起玩的帳號一鍵勾選

**每個帳號可以各自設定**

- **登入地區、視窗模式、靜音、MOD、額外參數**
- **畫面與效能** —— 視窗解析度、幀數上限、CPU 優先順序。背景帳號鎖低幀、把優先順序設低，資源留給主力帳號
- **視窗改名** —— 遊戲標題改成帳號名稱，多開時不會認錯

**其他**

- **免安裝** —— 一個 exe，不用另外裝 .NET
- **畫面隱私** —— 帳號自動打碼；密碼與 Token 以 Windows 加密保存在本機
- **右下角圖示** —— 按關閉鈕可以選擇收到工作列右下角，程式繼續執行
- **檢查新版本** —— 有新版會提示，不會自動下載或安裝，可關閉

## ⬇️ 下載

到 [Releases](https://github.com/MoroseDog/D2RNexus/releases) 下載 `D2RNexus.exe`，放在任意資料夾直接執行。[版本紀錄](CHANGELOG.md)

| 需求 | |
|---|---|
| 系統 | Windows 10 / 11（64 位元） |
| 遊戲 | 已安裝《暗黑破壞神 II：獄火重生》 |
| WebView2 | OTP 登入時需要，Windows 10 / 11 通常已內建 |
| 網路 | 登入 Battle.net；第一次多開時下載微軟官方工具；開啟時查詢新版本（可關閉） |

程式沒有購買程式碼簽章，所以會跳警告：Edge 下載時點「刪除」旁的 **∨** →「仍要保留」；執行時按「其他資訊」→「仍要執行」。原因見[安全性說明](SECURITY.md)。

## 🚀 快速開始

1. 開啟 Nexus，Windows 詢問管理員權限時按「是」。
2. 左側「全域設定」→「瀏覽遊戲執行檔…」，選擇 `D2R.exe`。
3. 回到「帳號總覽」，修改示範帳號或按「＋ 新增帳號」，填入帳號、密碼、登入地區。
4. 有綁驗證器的帳號，勾「使用 Token 登入」，按「取得 Token」後在手機核准一次。
5. 勾選要開的帳號，按「啟動所選」。

拖曳帳號左側的 ≡ 調整啟動順序；亮起的「沿用全域」表示該項使用全域設定，點一下改成個別設定。

> 💡 多開時，把背景帳號的**幀數上限**調低、**CPU 優先順序**設成「低」，主力帳號會順一點。

## 🛡️ 不是外掛

Nexus 只是幫你開遊戲的管理工具，**不注入程式、不讀寫遊戲記憶體、不改遊戲檔案、不攔截連線、不自動操作遊戲、不模擬鍵盤滑鼠、不繞過驗證、不上傳你的資料**。每一項做法、需要的權限和連線對象都寫在[安全性說明](SECURITY.md)。

多開與第三方工具是否符合規範，最終以 Blizzard 的使用條款與判定為準，Nexus 無法保證帳號不會受到任何處分。

## ❓ 常見問題

<details>
<summary>連線 Battle.net 顯示「無法認證」</summary>

遊戲已經開啟，是 Battle.net 拒絕登入，和多開無關。依序確認：

1. 帳號有綁驗證器 → 改用「使用 Token 登入」並取得 Token
2. 密碼是否正確（改過密碼要重新輸入）
3. 登入帳號（Email）有沒有打錯
4. 用 Token 的帳號，重綁過驗證器就按「清除」再重新取得
5. 先用 Battle.net 啟動器登入一次，完成安全驗證後再試
</details>

<details>
<summary>用 Apple、Google 等方式登入，沒有 Battle.net 密碼</summary>

取得 Token 的頁面只接受 Email 和密碼。請在登入視窗按右上角的「用其他方式登入」，用平常的方式登入後再按「完成登入」。步驟見[用 Apple、Google 等方式登入](docs/apple-login.md)。
</details>

<details>
<summary>Nexus 會改到我在遊戲裡調的設定嗎？</summary>

只會寫入你在「畫面與效能」設定的**視窗解析度**和**幀數上限**，其他遊戲設定（畫質、音量、自動地圖、按鍵…）完全不碰。

那兩項由 Nexus 決定，所以在遊戲裡改了之後，下次啟動會被帳號的設定覆蓋。想把遊戲裡調好的值變成預設，到「全域設定 → 畫面與效能」按一次「讀取目前遊戲設定」。
</details>

<details>
<summary>CPU 優先順序有什麼用？</summary>

多開時大家一起搶 CPU，把背景帳號設成「低」，主力帳號就會先用到 CPU，卡頓會少一點。

要注意它**不會讓 CPU 使用率變低**，只是決定誰先用。如果你開的數量不多、CPU 沒有吃滿，設了也不會有感覺，維持「正常」就好。
</details>

<details>
<summary>關掉視窗以後程式還在跑？</summary>

按關閉鈕時可以選「收到右下角」，Nexus 就會繼續在背景執行，點工作列右下角的圖示能叫回來，右鍵有「顯示主視窗 / 結束」。

這個選擇可以記住，之後想改到「全域設定 → 視窗 → 按關閉鈕時」。按最小化則和一般程式一樣，留在工作列。
</details>

<details>
<summary>為什麼需要管理員權限？</summary>

解除多開限制需要，開啟時只問一次。拒絕仍可使用，只是每次要再問。遊戲會跟著以管理員權限執行，OBS、Discord 可能也要用管理員身分開啟才能擷取畫面。
</details>

<details>
<summary>帳號密碼存在哪裡？</summary>

在你電腦的 `%LOCALAPPDATA%\D2RNexus`，密碼與 Token 以 Windows 內建加密保存，不會上傳。以帳號密碼登入時，密碼會傳給遊戲的啟動參數；Token 登入則不會。
</details>

<details>
<summary>顯示「視窗就緒」代表登入成功嗎？</summary>

不一定。Nexus 只能確認視窗出現，無法判斷是否登入完成，請以遊戲畫面為準。
</details>

<details>
<summary>視窗解析度為什麼只有這幾個？</summary>

這些是遊戲支援的視窗大小，其他尺寸畫面會被裁切。選「不變更」就沿用遊戲的設定。
</details>

<details>
<summary>關掉 Nexus 會關掉遊戲嗎？</summary>

不會。重開 Nexus 後仍認得之前開的遊戲，不會讓同一個帳號重複開啟。
</details>

<details>
<summary>如何完全移除？</summary>

刪除 `D2RNexus.exe` 與資料夾 `%LOCALAPPDATA%\D2RNexus`。用過視窗解析度的話，遊戲設定資料夾裡的 `Settings.json.nexus-backup` 也可以刪。
</details>

## 🐛 回報問題

到 [Issues](https://github.com/MoroseDog/D2RNexus/issues) 描述狀況，並附上作業系統、遊戲版本、Nexus 版本、登入方式，以及日誌（上方選單「說明 → 開啟日誌資料夾」）。

Issues 是公開的，請不要貼出密碼、Token、完整 Email 或 BattleTag，截圖也請先遮住帳號。

## 📄 授權

Copyright © 2026 J.J. Huang，保留所有權利。可免費供個人使用；未經作者書面同意，不得修改、重新散布或販售。完整條款見 [LICENSE.txt](LICENSE.txt)，第三方元件授權見 [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt)。

D2R Nexus 是非官方工具，與 Blizzard Entertainment 無關，也未獲其認可或支援。Diablo® II: Resurrected 與 Battle.net® 是 Blizzard Entertainment, Inc. 的商標或註冊商標。使用風險由使用者自行承擔。
