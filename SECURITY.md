# 安全性說明

**繁體中文** · [简体中文](SECURITY.zh-CN.md) · [English](SECURITY.en.md) · [한국어](SECURITY.ko.md)

這一頁說明 D2R Nexus 需要哪些權限、碰到哪些資料、會連到哪裡，以及為什麼防毒軟體或 Windows 可能跳出警告。

## 需要管理員權限的原因

遊戲啟動後會建立一個「已經開過一個」的系統標記，第二個遊戲看到它就不會啟動。要關掉這個標記需要管理員權限，所以 Nexus 開啟時會詢問一次。

- 拒絕也能使用，只是每次需要解除多開限制時會再問一次。
- 遊戲會跟著以管理員權限執行。錄影或疊加軟體（例如 OBS、Discord）可能也要用管理員身分開啟才能擷取畫面。

## 實際會做的事

| 動作 | 說明 |
|---|---|
| 找到遊戲 | 讀取 Windows 的已安裝程式清單（「應用程式與功能」裡的那一份），找出暗黑 2 安裝在哪裡。只讀不寫 |
| 啟動遊戲 | 用遊戲本身支援的啟動參數執行 `D2R.exe` |
| 解除多開限制 | 用微軟官方工具 Sysinternals Handle，關閉遊戲用來檢查「是否已經開過一個」的系統標記 |
| 視窗改名 | 用 Windows 標準功能修改遊戲視窗的標題文字 |
| 切換遊戲視窗 | 用 Windows 標準功能把指定的遊戲視窗叫到前面。只是切換視窗，不會送任何按鍵或滑鼠動作給遊戲 |
| 快捷鍵 | 向 Windows **註冊**你指定的組合鍵。只有按下那一組時 Windows 才通知 Nexus，其他按鍵完全收不到，也不會記錄。這不是鍵盤掛鉤 |
| 迷你視窗的狀態 | 迷你視窗展開時，每兩秒查詢 Nexus 自己啟動的那幾個遊戲有沒有回應、用了多少 CPU 與記憶體。查的是 Windows 已有的資訊，不讀遊戲內容 |
| 畫面設定 | 只改寫遊戲設定檔 `Settings.json` 裡的視窗大小與幀數上限，其他項目不動，第一次改寫前會備份 |
| CPU 優先順序 | 遊戲啟動後用 Windows 標準功能調整「這個程序」的優先順序，讓多開時主力帳號先用到 CPU；不改遊戲、不改系統設定 |
| 檢查新版本 | 開啟時讀取本專案 Release 的版本號，比目前版本新就提示。不會自動下載或安裝，可在全域設定關閉 |
| OTP 帳號登入 | 開啟 Battle.net 官方登入頁取得登入 Token，寫到 Battle.net 啟動器本身使用的 Windows 登錄位置；遊戲讀取後會自動清空 |
| 認出自己開的遊戲 | 記住程序編號、啟動時間與執行檔路徑，避免同一個帳號被重複開啟 |

## 不會做的事

- 不注入任何程式（例如 DLL）到遊戲裡，也不修改遊戲的程式檔案。
- 不讀取或修改遊戲的記憶體。
- 不攔截或修改遊戲的網路連線。
- 不自動操作遊戲：沒有掛機、自動打怪、自動撿物、巨集，也不模擬鍵盤滑鼠。
- 不繞過 Battle.net 的人機驗證或手機驗證。
- 不記錄鍵盤輸入。快捷鍵是向 Windows 註冊組合鍵，不是鍵盤掛鉤，Nexus 收不到其他按鍵。
- 不收集你的帳號資料，也不上傳到任何伺服器。

## 資料存在哪裡

全部在你自己的電腦，位置是 `%LOCALAPPDATA%\D2RNexus`：

| 檔案 | 內容 |
|---|---|
| `settings.json`、`accounts.json`、`groups.json` | 全域設定、帳號設定、群組。**不含密碼與 Token** |
| `credentials.dat` | 密碼，以 Windows 內建的資料保護加密，只有同一個 Windows 使用者能解開 |
| `tokens.dat` | 登入 Token，同樣加密 |
| `runtime.json` | 認出自己開的遊戲用的程序資訊，不含帳號密碼 |
| `WebView2` | 登入視窗的瀏覽器資料，使用無痕工作階段，不保留登入 Cookie |
| `Tools` | 從微軟下載的 Handle 工具 |
| `logs` | 每日紀錄。帳號已打碼，不會寫入密碼或 Token |

## 會連線到哪裡

只有這幾個對象：

1. **Battle.net 官方登入頁**：只在你按「取得 Token」時開啟。帳號密碼只會自動填入網址為 `battle.net` 或 `*.battle.net` 的頁面；用 Apple 等其他方式登入時，那些頁面 Nexus 一個字都不會填。
2. **微軟下載網站**（`download.sysinternals.com`）：第一次多開、經你同意後下載 Handle 工具。下載後會確認是有效的微軟簽章檔案才使用。
3. **GitHub**（`api.github.com`）：開啟時查本專案最新版本號。送出的只是一個一般的網頁請求，不含任何你的資料；可以在全域設定關閉。
4. **遊戲本身連線 Battle.net**：這是遊戲自己的連線，和 Nexus 無關。

Nexus 沒有任何把你的帳號、密碼、Token 或設定送到其他地方的功能。

## 為什麼會跳出警告

**Windows SmartScreen**：程式沒有購買程式碼簽章，下載人數也少，所以 Windows 會顯示「無法驗證此檔案是否安全」。這不是偵測到病毒。

- Edge 下載時：點「刪除」旁邊的 **∨** →「**仍要保留**」。
- 執行時：按「**其他資訊**」→「**仍要執行**」。

**防毒軟體**：Nexus 會要求管理員權限、啟動其他程式、關閉遊戲程序中的一個系統標記。這些都是正常軟體也會用到的功能，但惡意程式同樣會用，所以有些防毒軟體會標記。被標記不代表一定有毒，沒被標記也不代表一定安全。

**你可以自己確認**：

- 只從本專案的 [Releases](https://github.com/MoroseDog/D2RNexus/releases) 下載。
- 核對 Release 說明或 `SHA256SUMS.txt` 裡的檔案指紋：在檔案所在資料夾開啟 PowerShell，執行 `Get-FileHash .\D2RNexus.exe -Algorithm SHA256`，比對是否一致。
- 看 Release 說明附的 VirusTotal 掃描結果，或自己把檔案上傳到 [VirusTotal](https://www.virustotal.com) 掃描。
- 用微軟的 [TCPView](https://learn.microsoft.com/sysinternals/downloads/tcpview) 看 Nexus 實際連線的對象，用 [Process Monitor](https://learn.microsoft.com/sysinternals/downloads/procmon) 看它讀寫了哪些檔案與登錄檔。
- Nexus 是沒有混淆的 .NET 程式，懂程式的人可以用 ILSpy 之類的工具直接檢視程式邏輯。授權只禁止修改與重新散布，檢視沒有問題。

## 回報安全問題

請到 [Issues](https://github.com/MoroseDog/D2RNexus/issues) 說明。Issues 是公開的，請不要貼出密碼、登入 Token、完整 Email 或 BattleTag。

## 免責

D2R Nexus 是非官方工具，與 Blizzard Entertainment 無關。多開與第三方工具是否符合規範，最終以 Blizzard 的使用條款與判定為準，本程式無法保證帳號不會受到任何處分，請自行評估風險後使用。
