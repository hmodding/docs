---
description: >-
  This Tutorial aims to show you how to install RaftModLoader on other OSes (not
  Windows).
---

# How to install RaftModLoader on Linux

## How to install RaftModLoader on Linux

Prerequisites:
- Steam
- winetricks
- wine.x86_64
- wine.i686

### 1. Get Raft

Install Steam, preferably in a new Wine prefix. E.g.
`env WINEPREFIX=/home/user/.wine-raft WINEARCH=win64 winetricks steam`
You might have to start Steam the first time like `Steam.exe -noreactlogin` to be able to log in.
Then install Raft as normal in this new Wine environment.

### 2. Install .NET 4.6.2 into Raft's wine prefix

Install dotnet462 by running:
`env WINEPREFIX=/home/user/.wine-raft WINEARCH=win64 winetricks dotnet462`
Sometimes the installation hangs, but just restarting the winetricks command and go through the process once more usually fixes it. You have to choose "Repair" if presented with the option of either removing or repairing the .NET-installation, of course.

### 3. Download the Launcher

The same as you would do on Windows. Go to the [download page](https://www.raftmodding.com/download) and place it where you want.
![](../../.gitbook/assets/download.png)

### 4. Add a library override for winhttp so RaftModLoader is able to use doorstop

You have to make sure that the builtin library "winhttp" is marked as native in the wine overlay, or else the doorstop code used for injecting the mod loader will not work.
Run `env WINEPREFIX=/home/user/.wine-raft WINEARCH=win64 winecfg`, go to the Libraries-tab, and then write in "*winhttp" with the asterisk (but without the quotation marks) in the "New override for library:" box and then press "Add". Then exit winecfg.

### 5. Run RaftModLoader

Run RMLLauncher via Wine: `env WINEPREFIX=/home/user/.wine-raft WINEARCH=win64 wine RMLLauncher.exe`. It'll find the Raft folder automatically since you have Steam installed in the prefix, but if it for some reason doesn't, then browse to it manually. Then just use RaftModLoader as you would on Windows.

### Addendum
The easiest way to install mods are just downloading the .rmod files and just place them directly in the Mods-subfolder in Raft which RMLLauncher.exe should've created for you. It'll be in /home/user/.wine-raft/drive_c/your_steamapps_path/common/Raft/Mods.

(The install-mod-through-browser method usually doesn't work without you adding a handler for rmllauncher:// URLs, but the only thing that the installing procedure does is placing .rmod files in Raft's Mods-folder.)
