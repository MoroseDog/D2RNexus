# FAQ

<p align="center"><a href="faq.md">繁體中文</a> · <a href="faq.zh-CN.md">简体中文</a> · <b>English</b> · <a href="faq.ko.md">한국어</a></p>

[← Back to the README](../README.en.md)

## Signing in

### Connecting to Battle.net shows "Failed to authenticate"

The game has already opened; Battle.net is rejecting the login, which has nothing to do with multi-launching. Check these in order:

1. The account has an authenticator → switch to "Use token login" and get a token
2. Is the password correct? (If you changed your password, enter it again)
3. Is the login account (email) typed correctly?
4. For accounts using a token: if you re-linked the authenticator, click "Clear" and get a new token
5. Sign in once with the Battle.net launcher, complete the security check, then try again

### Stuck on "Connecting to Battle.net"

The game has opened and multi-launch worked; Battle.net just isn't responding to the login. Try these in order:

1. Close the game, sign in to this account once with the Battle.net launcher, complete any check such as "I'm not a robot", and wait a while before opening it again. If you sign in too many times in a short period or mistype the password a few times, Battle.net asks for verification, and a game opened by Nexus has nowhere to complete it, so it may stay stuck
2. If you're using a VPN or a game booster, turn it off and try again
3. If the game also gets stuck when you open it from the Battle.net launcher, it's a network or server problem and has nothing to do with multi-launching

If it still doesn't work, please report it in Issues with your login method (password or token), which account it starts getting stuck on, and the log.

### Asia works, but Europe or the Americas says it cannot reach the server

This happens with account-and-password login; switching that account to token login fixes it. Asia is not affected.

In the account settings set "Login region" to Europe or the Americas, tick "Use token login", then click "Get token" and approve it once on your phone. **A token belongs to one region**, so changing the region means getting a new one. If the token you get does not match the login region, the activity log says so.

### An account with token login can only play offline

That token is no longer valid. Changing your Battle.net password, re-linking the authenticator or removing it all invalidate it.

In the account settings, next to "Login token", click "Clear" and then "Get token" again, and approve it once more on your phone. For an account without an authenticator you can also uncheck "Use token login" and sign in with the account and password instead.

### An account with token login stops at "Press any key to continue"

That is how the game behaves: when it starts with a token it waits on its title screen, and it only begins signing in once you **press a key in that window**. Accounts that sign in with an account and password do it by themselves.

With several token accounts this step matters even more: Windows only holds one token at a time, so **the next account can only start after the previous one has signed in**. Until you press that key the accounts behind it keep waiting (the activity log says what they are waiting for and for how long). Press a key in each token account's window as it appears and the rest follow on.

"Help with signing in" in the global settings makes this easier: **bringing a token account's game window to the front** is on by default, so you never have to go looking for the window. If you would rather not press the key at all, the same card has **press past the title screen for me**, and Nexus does it for you. That is the program operating the game, so it is off by default, and before it can be turned on you are shown exactly what it does and asked to agree. Weigh the risk for yourself.

If only some of your accounts use it, Nexus **starts the token accounts without the key press last**, so the others do not sit behind one that is waiting for you; the cost is that such an account opens last. The activity log says so when it happens.

### I sign in with Apple, Google, etc. and don't have a Battle.net password

The page that gets the token only accepts an email and password. In the login window, click "Sign in another way" at the top right, sign in the way you usually do, then click "Done signing in". For the steps, see [Signing in with Apple, Google, etc. (Chinese)](apple-login.md).

### Does "Window ready" mean I'm signed in?

Not necessarily. Nexus can only confirm that the window appeared; it can't tell whether you've finished signing in. Check the game screen.

## Game settings

### Will Nexus change the settings I adjusted in the game?

It only writes what you picked under "Game settings" for that account: **Window size**, **Frame cap**, and the **Graphics** and **Sound** groups it is on. Other game settings (automap, key bindings, controls…) are never touched.

Graphics and Sound start on "Current settings", which means the account keeps whatever the game has and nothing is written at all. To pin an account to a particular quality, go to "Global settings → Game settings", click "Manage setting groups…", make a copy of one, then select it for that account.

With "Keep game options the same across accounts" turned on, one more set goes in: your own game options (the numbers on the orbs, the automap and so on), the same ones for every account. That exists because with several games open, something you change in one account is wiped out by another. The switch is off by default and nothing of the kind is touched until you turn it on.

Anything Nexus manages is written again at every launch, so changes you make in the game are overwritten next time. To make the values you tuned in the game the defaults, go to "Global settings → Game settings" and click "Read current game settings" once. For frames, the game uses whichever is higher, the cap or the target frame rate of dynamic resolution scaling, so when that target sits above the cap, Nexus brings it down too.

### The frame cap is set but the frame rate is different

Check **VSync** in the game's display settings first. With it on, the frame rate only lands on whole divisions of the monitor's refresh rate — on a 60 Hz screen that is 60, 30, 20 and 15 — so a cap of 36 or 45 runs at 30. Turn VSync off to follow the number exactly.

The other one is the **target frame rate** of dynamic resolution scaling: the game limits frames at whichever of the two is higher. From v0.4.3 Nexus brings it down together with the cap when it has to; on older versions check that value in the game yourself.

### What does CPU priority do?

When several games are running, they all compete for the CPU. If you set your background accounts to "Low", your main account gets the CPU first and stutters a bit less.

Note that it **doesn't lower CPU usage**; it only decides who goes first. If you don't run many games and your CPU isn't maxed out, you won't notice any difference, so just leave it at "Normal".

### Several games at once stutter, or too many at once will not start

Nexus only starts the games; once they are running it uses no performance of its own, and the stutter comes from several games running together. For the accounts you keep in the background, set all three:

1. **Frame cap** 15 or 30
2. **CPU priority** to "Low", so the account you play gets the CPU first
3. **Window size** to 1280x720

When you start many at once and the next game crowds the one still loading, set "Delay between accounts" to 5–10 seconds under "Global settings → Launch & diagnostics".

### How small can the window size be?

The list contains the window sizes the game supports, and you can also choose "Custom…". The smallest recommended size is 1280x720. Below that, the game doesn't scale down; it just cuts off part of the screen (you may not be able to see your inventory or hotbar).

## The app and your data

### Do hotkeys record what I type?

No. Nexus **registers** the key combinations you choose with Windows, and Windows only notifies it when you press one of those combinations. It receives no other keys at all. This is different from a "keyboard hook", which intercepts every key press; Nexus doesn't use one.

Because the combination is registered with the system, it isn't passed on to other programs while Nexus is running. So always include Ctrl, Alt, or Win when you set one, and don't use single keys that the game itself uses.

### The program is still running after I closed the window?

When you click the close button, you can choose "Minimize to tray". Nexus then keeps running in the background; click its icon in the system tray (bottom right of the taskbar) to bring it back, or right-click it for "Show main window" / "Exit D2R Nexus".

This choice can be remembered. To change it later, go to "Global settings → Window → When the close button is clicked". Minimizing works like in any other program: it stays on the taskbar.

### Why does it need administrator permission?

It's needed to unlock the multi-launch limit, and Nexus asks only once when it opens. If you decline, you can still use it, but you'll be asked again each time. The games will also run as administrator, so OBS and Discord may need to be run as administrator too in order to capture the game screen.

### Where are my login account and password stored?

On your PC, in `%LOCALAPPDATA%\D2RNexus`. Passwords and tokens are saved with Windows' built-in encryption and are never uploaded. When you sign in with a login account and password, the password is passed to the game in its launch options; with token login, it isn't.

### Will closing Nexus close my games?

No. When you reopen Nexus, it still recognizes the games it opened earlier and won't let the same account open twice.

### Will updating lose my settings?

No. Updating means replacing the old `D2RNexus.exe` with the new one. Accounts, groups and display settings live in a separate folder, and the new version picks them up as they are.

To keep a copy of your own, use "Help → Open the settings folder" in the top menu and copy the whole folder. **On another computer or under another Windows user, passwords and login tokens have to be entered again**: those two files are encrypted by Windows for the user who created them and cannot be opened elsewhere.

### How do I remove it completely?

Delete `D2RNexus.exe` and the folder `%LOCALAPPDATA%\D2RNexus`. If you used the window size setting, you can also delete `Settings.json.nexus-backup` in the game's settings folder.
