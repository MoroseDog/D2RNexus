# 安全说明

[繁體中文](SECURITY.md) · **简体中文** · [English](SECURITY.en.md) · [한국어](SECURITY.ko.md)

本页说明 D2R Nexus 需要哪些权限、会接触哪些数据、会连接到哪里，以及为什么杀毒软件或 Windows 可能弹出警告。

## 需要管理员权限的原因

游戏启动后会创建一个“已经打开了一个”的系统标记，第二个游戏看到它就不会启动。要关闭这个标记需要管理员权限，所以 Nexus 打开时会请求一次。

- 拒绝也能使用，只是每次需要解除多开限制时会再请求一次。
- 游戏会随之以管理员权限运行。录屏或叠加层软件（例如 OBS、Discord）可能也要以管理员身份运行才能捕获画面。

## 实际会做的事

| 操作 | 说明 |
|---|---|
| 找到游戏 | 读取 Windows 的已安装程序列表（“应用和功能”里的那一份），找出暗黑2安装在哪里。只读不写 |
| 启动游戏 | 用游戏本身支持的启动参数运行 `D2R.exe` |
| 解除多开限制 | 用微软官方工具 Sysinternals Handle，关闭游戏用来检查“是否已经打开了一个”的系统标记 |
| 窗口改名 | 用 Windows 标准功能修改游戏窗口的标题文字 |
| 切换游戏窗口 | 用 Windows 标准功能把指定的游戏窗口切到前台。只是切换窗口，不会向游戏发送任何按键或鼠标操作 |
| 快捷键 | 向 Windows **注册**你指定的组合键。只有按下那一组时 Windows 才通知 Nexus，其他按键完全收不到，也不会记录。这不是键盘钩子 |
| 迷你窗口的状态 | 迷你窗口展开时，每两秒查询一次 Nexus 自己启动的那几个游戏有没有响应、用了多少 CPU 与内存。查的是 Windows 已有的信息，不读取游戏内容 |
| 画面设置 | 只改写游戏设置文件 `Settings.json` 里的窗口大小与帧率上限，其他项目不动，第一次改写前会备份 |
| CPU 优先级 | 游戏启动后用 Windows 标准功能调整“这个进程”的优先级，让多开时主力账号先用到 CPU；不改游戏、不改系统设置 |
| 检查新版本 | 打开时读取本项目 Release 的版本号，比当前版本新就提示。不会自动下载或安装，可在全局设置里关闭 |
| OTP 账号登录 | 打开 Battle.net 官方登录页获取登录 Token，写到 Battle.net 启动器本身使用的 Windows 注册表位置；游戏读取后会自动清空 |
| 识别自己打开的游戏 | 记住进程 ID、启动时间与可执行文件路径，避免同一个账号被重复启动 |

## 不会做的事

- 不向游戏注入任何程序（例如 DLL），也不修改游戏的程序文件。
- 不读取或修改游戏的内存。
- 不拦截或修改游戏的网络连接。
- 不自动操作游戏：没有挂机、自动打怪、自动捡物、宏，也不模拟键盘鼠标。
- 不绕过 Battle.net 的人机验证或手机验证。
- 不记录键盘输入。快捷键是向 Windows 注册组合键，不是键盘钩子，Nexus 收不到其他按键。
- 不收集你的账号数据，也不上传到任何服务器。

## 数据存在哪里

全部在你自己的电脑上，位置是 `%LOCALAPPDATA%\D2RNexus`：

| 文件 | 内容 |
|---|---|
| `settings.json`、`accounts.json`、`groups.json` | 全局设置、账号设置、分组。**不含密码与 Token** |
| `credentials.dat` | 密码，用 Windows 内置的数据保护加密，只有同一个 Windows 用户能解开 |
| `tokens.dat` | 登录 Token，同样加密 |
| `runtime.json` | 用来识别自己打开的游戏的进程信息，不含账号密码 |
| `WebView2` | 登录窗口的浏览器数据，使用无痕会话，不保留登录 Cookie |
| `Tools` | 从微软下载的 Handle 工具 |
| `logs` | 每日日志。账号已打码，不会写入密码或 Token |

## 会连接到哪里

只有这几个对象：

1. **Battle.net 官方登录页**：只在你点击“获取 Token”时打开。账号密码只会自动填入网址为 `battle.net` 或 `*.battle.net` 的页面；用 Apple 等其他方式登录时，那些页面 Nexus 一个字都不会填。
2. **微软下载网站**（`download.sysinternals.com`）：第一次多开、经你同意后下载 Handle 工具。下载后会确认是有效的微软数字签名文件才使用。
3. **GitHub**（`api.github.com`）：打开时查询本项目的最新版本号。发送的只是一个普通的网页请求，不含任何你的数据；可以在全局设置里关闭。
4. **游戏本身连接 Battle.net**：这是游戏自己的连接，和 Nexus 无关。

Nexus 没有任何把你的账号、密码、Token 或设置发送到其他地方的功能。

## 为什么会弹出警告

**Windows SmartScreen**：程序没有购买数字签名证书，下载人数也少，所以 Windows 会显示“无法验证此文件是否安全”。这不是检测到了病毒。

- Edge 下载时：点击“删除”旁边的 **∨** →“**仍然保留**”。
- 运行时：点击“**更多信息**”→“**仍要运行**”。

**杀毒软件**：Nexus 会请求管理员权限、启动其他程序、关闭游戏进程中的一个系统标记。这些都是正常软件也会用到的功能，但恶意程序同样会用，所以有些杀毒软件会将它标记为可疑。被标记不代表一定有毒，没被标记也不代表一定安全。

**你可以自己确认**：

- 只从本项目的 [Releases](https://github.com/MoroseDog/D2RNexus/releases) 下载。
- 核对 Release 说明或 `SHA256SUMS.txt` 里的文件哈希值：在文件所在的文件夹打开 PowerShell，运行 `Get-FileHash .\D2RNexus.exe -Algorithm SHA256`，比对是否一致。
- 查看 Release 说明附带的 VirusTotal 扫描结果，或自己把文件上传到 [VirusTotal](https://www.virustotal.com) 扫描。
- 用微软的 [TCPView](https://learn.microsoft.com/sysinternals/downloads/tcpview) 查看 Nexus 实际连接的对象，用 [Process Monitor](https://learn.microsoft.com/sysinternals/downloads/procmon) 查看它读写了哪些文件与注册表。
- Nexus 是未经混淆的 .NET 程序，懂编程的人可以用 ILSpy 之类的工具直接查看程序逻辑。授权只禁止修改与重新分发，查看没有问题。

## 报告安全问题

请到 [Issues](https://github.com/MoroseDog/D2RNexus/issues) 说明。Issues 是公开的，请不要贴出密码、登录 Token、完整 Email 或 BattleTag。

## 免责声明

D2R Nexus 是非官方工具，与 Blizzard Entertainment 无关。多开与第三方工具是否符合规定，最终以 Blizzard 的使用条款与判定为准，本程序无法保证账号不会受到任何处罚，请自行评估风险后使用。
