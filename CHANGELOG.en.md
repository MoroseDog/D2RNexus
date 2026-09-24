# Changelog

[繁體中文](CHANGELOG.md) · [简体中文](CHANGELOG.zh-CN.md) · **English** · [한국어](CHANGELOG.ko.md)

The newest version is at the top. Download from [Releases](https://github.com/MoroseDog/D2RNexus/releases).

## v0.4.4 (2026-09-25)

**Added**

- **You can choose which accounts the mini window shows.** With many accounts the list gets too long, so right-click a row and choose "Hide from this list", or untick "Show in the mini window" in the account settings. Under the list it says how many are not shown and how many of those are running; the arrow opens them so you can put them back one at a time, or all at once. A hidden account still launches as usual and keeps its shortcut, and the two numbers on the collapsed bar still count every account.

**Improved**

- **The delay between accounts now starts at 5 seconds.** It used to be 0, which started the next game the moment the previous window appeared; with several accounts all of them load at once and end up not starting at all. Settings that already exist keep their own value, and with more than four accounts ticked at a delay of 0 the activity log says so once.
- **Waiting for the previous token account is no longer silence**: the activity log has a line when the wait starts and another when it ends with the number of seconds, the account row now says "Waiting for the previous account", and hovering it shows the whole sentence.
- The readme gained a diagram of the order token accounts start in, and three new FAQ entries: an account with token login stops at "Press any key to continue", Asia works but Europe and the Americas cannot reach the server, and several games at once stutter or will not start.

## v0.4.3 (2026-09-23)

**Fixed**

- **The frame cap did not actually apply**: Nexus used to write your number into the game's "target frame rate" (which belongs to dynamic resolution scaling) and set the game's own frame cap to 1. It now writes the frame cap itself. The game limits frames at whichever of the two is higher, so a target above the cap is brought down to the same number, and one that is already lower is left alone. Thanks to shiun00 for reporting it.
- Updating does not restore a target frame rate that an older version changed. If you use dynamic resolution scaling, check that value in the game's display settings.
- When you press a key combination the hotkey box cannot use, it now says that the combination must include Ctrl, Alt or Win, and tells you when another account already has that combination. It used to do nothing at all, which looked broken.

**Improved**

- New FAQ entries: an account with token login can only play offline, and the frame cap is set but the frame rate is different.

## v0.4.2 (2026-09-22)

**Added**

- **Multilingual interface**: 繁體中文, 简体中文, English, 한국어. By default it follows your Windows language, and you can also switch from the "Language" menu at the top, the globe icon at the bottom left, or Global settings. When you switch, Nexus asks whether to restart right away; games that are already open aren't affected.

**Improved**

- The exe now includes file information such as publisher, product name, description, and copyright.

## v0.4.1 (2026-09-22)

**Improved**

- **Finds the game automatically**: if the game path is wrong when Nexus opens, it finds where Diablo II is installed from Windows' list of installed programs and fills it in for you. Global settings also has a new "Find automatically" button.
- **See why a launch failed**: the account row keeps a red "Launch failed ⓘ"; hover over it to see the reason. It only goes away the next time you launch that account.
- When the game can't be found, the message now says which file it was looking for and which button to click, instead of just saying "path".
- When the activity log gets a new message, it scrolls to the top automatically.

**Fixed**

- Removed the example game path that used to be pre-filled, so launches no longer keep failing if you didn't change it.
- Removed the misleading "Launch features enabled" message shown at startup.

## v0.4.0 (2026-09-21)

**Added**

- **Mini window**: a small list that stays on screen, always on top, without taking up space on the taskbar or showing up in Alt+Tab. One row per account: a green dot means the game is open, click to switch to it; a gray dot means it isn't, click to launch it. When an account drops, you can reopen it without switching back to the main window.
- The mini window shows each account's CPU and memory usage, and CPU turns red above 50%. When a game isn't responding, its dot turns orange, and you can right-click to close that game directly. It can also collapse into a small bar that only shows how many games are running and how many aren't launched.
- **Each account can have its own hotkey**; press it to bring that account's game to the front. The combination must include Ctrl, Alt, or Win, so it doesn't take away the game's own keys. Whether to also launch the game if it isn't open can be set per account.

**Improved**

- If you click another account's Launch button or press a hotkey while a launch is in progress, that account is now added to the queue and opened next, instead of being ignored.
- The activity log now shows which accounts each batch is going to launch.

## v0.3.0 (2026-09-20)

**Added**

- **Display and performance settings per account**: window size, frame cap, and CPU priority. Each can use the global value, or each account can have its own. Lock background accounts to a low frame rate and set their CPU priority low so your main account runs a bit smoother. You can also enter your own numbers for window size and frame cap.
- The first time you open Nexus, it reads your current game settings as the defaults, so the first launch doesn't change anything. After that, it only reads them again when you click "Read current game settings".
- **Tray icon**: when you click the close button, you can choose to exit the program or minimize to the system tray (bottom right of the taskbar), and the choice can be remembered. While minimized, the program keeps running; click the icon to bring it back.
- **Update check**: at startup, Nexus checks whether there's a new version and lets you know. It only reads the version number and never downloads or installs anything automatically. It can be turned off in Global settings.
- Opening Nexus again now brings the window that's already running to the front, instead of showing a warning.

**Improved**

- Removing an account or deleting a group now shows a confirmation window first, and the confirm button counts down three seconds before you can click it, to prevent mistakes.
- Tidied up the account settings layout: dropdowns now sit right after their titles, and the descriptions are shorter.

## v0.2.2 (2026-09-20)

**Added**

- The login window has two new buttons: "Sign in another way" and "Done signing in". Accounts that sign in with Apple, Google, and the like, and have no Battle.net password, can now get a token in Nexus too. For the steps, see [Signing in with Apple, Google, etc. (Chinese)](docs/apple-login.md). Nobody has actually tested this flow yet, so if you try it, please let us know how it went.

## v0.2.1 (2026-09-20)

**Improved**

- "Launch selected" now skips accounts that are already open and writes a line about it in the activity log, instead of treating them as errors. If a few games disconnect while you're running several, just click "Launch selected" again and only the ones that aren't open will be launched.
- The MOD name field now accepts text copied from other tools, such as "-mod LiYuiMod -txt". When you leave the field, it's tidied up into the folder name automatically, "Enable MOD" is turned on, and "Recompile TXT tables" is checked when needed.
- When a MOD can't be found, the message shows the full path, so you can check whether the MOD is inside the `mods` folder in the game folder.
- When "Extra launch options" contains options like `-mod` or `-txt`, the message now uses plain wording that anyone can understand.
- Added a description below the MOD name field.

**Fixed**

- Accounts that sign in to Battle.net with Apple or another external method used to get blocked in the login window; now they can finish signing in within the same window. This hasn't been tested on a real setup, so if you run into it, please let us know.

**Added**

- [Security notes](SECURITY.en.md): what permissions are needed, where data is stored, where it connects, and why antivirus software or Windows shows warnings.
- Releases now include `SHA256SUMS.txt`, so you can check the file you downloaded.

## v0.2.0 (2026-09-14)

The first public beta.

- Launch several accounts in order with one click, with the multi-launch limit unlocked automatically.
- Supports accounts with an OTP phone authenticator: sign in to Battle.net inside the program, approve on your phone, and the token is saved.
- Groups, custom launch order, and checking or unchecking the whole list at once.
- Each account can have its own login region, windowed mode and window size, mute, MOD, and extra launch options, or use the global settings.
- Game window titles change to the account name, and login accounts on screen are masked automatically.
- Passwords and tokens are saved on your own PC with Windows' built-in encryption.
