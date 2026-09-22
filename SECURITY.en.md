# Security notes

[繁體中文](SECURITY.md) · [简体中文](SECURITY.zh-CN.md) · **English** · [한국어](SECURITY.ko.md)

This page explains what permissions D2R Nexus needs, what data it touches, where it connects, and why antivirus software or Windows may show a warning.

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
| Display settings | Only rewrites the window size and frame cap in the game's settings file `Settings.json`, leaves everything else alone, and makes a backup before the first change |
| CPU priority | After the game starts, uses a standard Windows feature to adjust the priority of that one process, so your main account gets the CPU first when running several games. It doesn't change the game or any system settings |
| Check for new versions | At startup, reads the version number from this project's Releases and lets you know if it's newer than the current one. It never downloads or installs anything automatically, and can be turned off in Global settings |
| OTP account login | Opens the official Battle.net login page to get a login token, and writes it to the Windows registry location that the Battle.net launcher itself uses; the game clears it automatically after reading it |
| Recognize the games it opened | Remembers the process ID, start time, and executable path, so the same account isn't opened twice |

## What it doesn't do

- It doesn't inject any code (such as a DLL) into the game, and doesn't modify the game's program files.
- It doesn't read or modify the game's memory.
- It doesn't intercept or modify the game's network traffic.
- It doesn't automate the game: no AFK botting, auto-fighting, auto-looting, or macros, and it doesn't simulate keyboard or mouse input.
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
