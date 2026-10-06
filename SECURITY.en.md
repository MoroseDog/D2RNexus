# Security notes

[繁體中文](SECURITY.md) · [简体中文](SECURITY.zh-CN.md) · **English** · [한국어](SECURITY.ko.md)

This page explains what permissions D2R Nexus needs, what data it touches, where it connects, and why antivirus software or Windows may show a warning.

<h2 align="center">⚠️ From v0.5.0, Nexus can send a key press to the game</h2>

<p align="center"><b>Off by default. You have to turn it on yourself, read what it does and agree to it.<br>
Once it is on, the program is operating the game for you, and the risk to your account is yours.</b></p>

<p align="center"><b>Each account also decides for itself.<br>
To keep your main account or a mule out of this entirely, leave it off for that account.</b></p>

> [!IMPORTANT]
> **What changed in v0.5.0: Nexus can now press past the game's title screen for you.**
>
> **Up to v0.4.6**, Nexus never sent any key to the game, and said so. **From v0.5.0** there is one feature that does: it presses "Press any key to continue" for you.
>
> - The feature is **off by default**. Leave it off and Nexus behaves exactly as it did before, sending no keys at all.
> - To use it you have to turn it on yourself under "Global settings → Help with signing in". You are first shown what it does, and the agree button only becomes available after a five-second countdown.
> - Once it is on, the program is operating the game for you. **The risk to your account is yours**, so weigh it before you decide.

## Why it needs administrator permission

When the game starts, it creates a system marker that says "one is already open", and a second game that sees it won't start. Closing this marker requires administrator permission, so Nexus asks once when it opens.

- You can still use it if you decline; you'll just be asked again each time the multi-launch limit needs to be unlocked.
- The games will also run as administrator. Recording or overlay software (such as OBS or Discord) may need to be run as administrator too in order to capture the game screen.

## What it actually does

| Action | Details |
|---|---|
| Find the game | Reads Windows' list of installed programs (the one shown in "Apps & features") to find where Diablo II is installed. Read-only, nothing is written |
| Launch the game | Runs `D2R.exe` with launch options that the game itself supports |
| Unlock multi-launch | Uses Sysinternals Handle, an official Microsoft tool, to close the system marker the game uses to check whether one is already open |
| Rename windows | Changes the game window's title text using a standard Windows feature |
| Switch game windows | Brings the chosen game window to the front using a standard Windows feature. It only switches windows and never sends any keys or mouse actions to the game |
| Hotkeys | **Registers** the key combinations you choose with Windows. Windows only notifies Nexus when you press one of those combinations; Nexus receives no other keys at all and records nothing. This is not a keyboard hook |
| Mini window status | While the mini window is expanded, every two seconds it checks whether the games Nexus itself launched are responding and how much CPU and memory they use. This is information Windows already has; nothing inside the game is read |
| Game settings | Only rewrites what you asked for in the game's settings file `Settings.json`: window size, frame cap, and the graphics and sound groups that account is on. Everything else is left alone, and a backup is made before the first change. Graphics and sound start on "Current settings", which writes nothing at all. The game limits frames at whichever is higher, the cap or the target frame rate of dynamic resolution scaling, so a target above the cap is brought down to the same number |
| CPU priority | After the game starts, uses a standard Windows feature to adjust the priority of that one process, so your main account gets the CPU first when running several games. It doesn't change the game or any system settings |
| Check for new versions | At startup, reads the version number from this project's Releases and lets you know if it's newer than the current one. It never downloads or installs anything automatically, and can be turned off in Global settings |
| OTP account login | Opens the official Battle.net login page to get a login token, and writes it to the Windows registry location that the Battle.net launcher itself uses; the game clears it automatically after reading it |
| Recognize the games it opened | Remembers the process ID, start time, and executable path, so the same account isn't opened twice |
| Press past the title screen (off by default) | After you have turned it on and agreed, sends a single space bar to the game window Nexus opened, getting past the game's own "Press any key to continue". Only to that window, only before the sign-in finishes, and it stops when the sign-in is done or the window closes. It is a Windows message to that one window, not simulated keyboard input to the whole machine, so no other program receives it. The time you agreed and the text you agreed to are kept on your own computer. Each account has its own switch as well, so an account can be kept out of it entirely. Every row in the account list is marked "Title screen: auto" in red or "Title screen: manual" in green, so you can see which accounts use it |

## What it doesn't do

- It doesn't inject any code (such as a DLL) into the game, and doesn't modify the game's program files.
- It doesn't read or modify the game's memory.
- It doesn't intercept or modify the game's network traffic.
- It doesn't automate the game: no AFK botting, auto-fighting, auto-looting, or macros. **The one exception** is the last row of the table above, pressing past the title screen: it is off by default, and it sends that single key only after you turn it on and agree.
- It doesn't bypass Battle.net's human verification (such as CAPTCHAs) or phone verification.
- It doesn't log keystrokes. Hotkeys are key combinations registered with Windows, not a keyboard hook, so Nexus can't receive any other keys.
- It doesn't collect your account data or upload it to any server.

## Where your data is stored

Everything stays on your own PC, in `%LOCALAPPDATA%\D2RNexus`:

| File | Contents |
|---|---|
| `settings.json`, `accounts.json`, `groups.json` | Global settings, account settings, and groups. **No passwords or tokens** |
| `credentials.dat` | Passwords, encrypted with Windows' built-in data protection; only the same Windows user can decrypt them |
| `tokens.dat` | Login tokens, encrypted the same way |
| `runtime.json` | Process information used to recognize the games Nexus opened; no login accounts or passwords |
| `WebView2` | Browser data for the login window. It uses a private browsing session and doesn't keep login cookies |
| `Tools` | The Handle tool downloaded from Microsoft |
| `logs` | Daily logs. Accounts are masked, and passwords and tokens are never written |
| `consent.txt` | The time you agreed to the automatic key press and the text you agreed to. It is only added to, and the log retention setting never removes it |

## Where it connects

Only these:

1. **The official Battle.net login page**: opened only when you click "Get token". Your login account and password are filled in automatically only on pages whose address is `battle.net` or `*.battle.net`; when you sign in with Apple or another method, Nexus doesn't fill in a single character on those pages.
2. **Microsoft's download site** (`download.sysinternals.com`): the first time you multi-launch, with your consent, Nexus downloads the Handle tool. After downloading, it checks that the file has a valid Microsoft signature before using it.
3. **GitHub** (`api.github.com`): at startup, Nexus checks the latest version number of this project. This is just an ordinary web request and contains none of your data; you can turn it off in Global settings.
4. **The game's own connection to Battle.net**: this is the game's own connection and has nothing to do with Nexus.

Nexus has no feature that sends your account, password, token, or settings anywhere else.

## Why you see warnings

**Windows SmartScreen**: Nexus doesn't have a paid code-signing certificate and hasn't been downloaded by many people yet, so Windows says it "couldn't verify if this file is safe". This does not mean a virus was detected.

- When downloading in Edge: click the **∨** (or "…") next to "Delete" → "**Keep anyway**".
- When running it: click "**More info**" → "**Run anyway**".

**Antivirus software**: Nexus asks for administrator permission, launches other programs, and closes a system marker inside the game's process. Normal software uses these features too, but so does malware, so some antivirus software flags it. Being flagged doesn't prove a file is malicious, and not being flagged doesn't prove it's safe.

**You can check it yourself**:

- Only download from this project's [Releases](https://github.com/MoroseDog/D2RNexus/releases).
- Compare the file fingerprint in the release notes or in `SHA256SUMS.txt`: open PowerShell in the folder where the file is, run `Get-FileHash .\D2RNexus.exe -Algorithm SHA256`, and check that it matches.
- Look at the VirusTotal scan results included in the release notes, or upload the file to [VirusTotal](https://www.virustotal.com) and scan it yourself.
- Use Microsoft's [TCPView](https://learn.microsoft.com/sysinternals/downloads/tcpview) to see what Nexus actually connects to, and [Process Monitor](https://learn.microsoft.com/sysinternals/downloads/procmon) to see which files and registry entries it reads and writes.
- Nexus is a .NET program without obfuscation, so anyone who knows programming can look at its logic directly with a tool such as ILSpy. The license only forbids modifying and redistributing it; looking at it is fine.

## Reporting security issues

Please describe the issue in [Issues](https://github.com/MoroseDog/D2RNexus/issues). Issues are public, so please don't post passwords, login tokens, your full email, or your BattleTag.

## Disclaimer

D2R Nexus is an unofficial tool and is not affiliated with Blizzard Entertainment. Whether multi-launching and third-party tools comply with the rules is ultimately determined by Blizzard's terms of use and rulings. This program cannot guarantee that your accounts will not be penalized in any way. Please assess the risks yourself before using it.
