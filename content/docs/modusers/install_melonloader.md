---
weight: 11100
title: "Installing MelonLoader"
description: "A guide on installing the mod loader"
icon: "download"
date: "2025-06-18T20:47:07+02:00"
lastmod: "2025-06-18T20:47:07+02:00"
draft: false
toc: true
---

MelonLoader is a mod loader for Unity games, including Schedule I.

{{< alert context="info" >}}
Official MelonLoader documentation can be found on the <a href="https://melonwiki.xyz/">MelonWiki</a>. This is a simplified guide to installing MelonLoader for Schedule I.
{{< /alert >}}

## Installation Steps

### Automatic Installation

Install a mod manager like [r2modman](https://thunderstore.io/c/schedule-i/p/ebkr/r2modman/), [Gale](https://thunderstore.io/c/schedule-i/p/Kesomannen/GaleModManager/) for Thunderstore mods or [Vortex](https://www.nexusmods.com/site/mods/1) for NexusMods mods. These mod managers should automatically install MelonLoader for you when you install your first mod or ask you to install it, if they can't do that.

### Manual Installation

#### Windows

##### Installing pre-requisites

{{< alert context="warning" >}}
Following software is required to run MelonLoader:

<li><a href="https://aka.ms/vs/16/release/vc_redist.x64.exe">Microsoft Visual C++ 2015-2019 Redistributable 64 Bit</a></li>
<li><a href="https://dotnet.microsoft.com/en-us/download/dotnet/6.0#runtime-desktop-6.0.19">.NET Desktop Runtime 6.0 64 Bit</a></li>
{{< /alert >}}

##### Installing MelonLoader

Download [MelonLoader 0.7.0](https://github.com/LavaGang/MelonLoader/releases/download/v0.7.0/MelonLoader.Installer.exe) and run it.

The installer should automatically detect your Schedule I installation. If it does not, you can manually select the game folder.
Select the game folder of Schedule I, which is usually located at `C:\Program Files (x86)\Steam\steamapps\common\Schedule I`.

Click "Install" to install MelonLoader.

#### Linux

It is assumed that you have a working installation of Schedule I on Linux (using Proton). If you're using Wine, you can follow Windows steps or use `winetricks` in your Wine prefix to install the required dependencies.

##### Installing pre-requisites

Install [protontricks](https://github.com/Matoking/protontricks) for easy installation of the required dependencies.

This allows you to install .NET Desktop 6.0 automatically, via `protontricks 3164500 dotnet6`. You also might need to install the Visual C++ 2015-2019 Redistributable via `protontricks 3164500 vcrun2015`.

You can also do that with GUI (e.g. on Steam Deck) by opening the Discover app, searching for "protontricks", installing it, and then launching it. From there select Schedule I, `Select the default wineprefix` and then `Install a Windows DLL or component`, and select `dotnet6` and `vcrun2015` from the list to install them.

##### Installing MelonLoader

Download [MelonLoader 0.7.0](https://github.com/LavaGang/MelonLoader/releases/download/v0.7.0/MelonLoader.x64.zip) and extract it to the game's folder.

Your game folder should contain the `MelonLoader` folder and `version.dll` alongside game files like `Schedule I.exe`.

To make sure MelonLoader starts with the game, add `WINEDLLOVERRIDES="version=n,b" %command%` to the launch options of Schedule I in Steam. Alternatively, if you are using Wine, you can handle the override in `winecfg` for your Wine prefix.

Example on Proton with GUI Protontricks:

Select `Run winecfg` from the Protontricks menu, select `Libraries` tab, navigate to `New override for library`, click the dropdown arrow and select `version`. Validate that the last position in the list below is `version (native, builtin)`. Click OK, OK and close the window.

#### MacOS

There are [reports](https://www.applegamingwiki.com/wiki/Schedule_I) of Schedule I running on MacOS using Wine, CrossOver or other Wine-based solutions like Sikarugir. 

For the best chances of success, use one of listed solutions to run a Windows version of Steam and install Schedule I through it. This will be the way you run the game on MacOS.

Before proceeding with MelonLoader installation, make sure that you have a working installation of Schedule I on MacOS.

##### Installing pre-requisites

Install them in the same way as on Windows with CrossOver, or use `winetricks` in your Wine prefix to install the required dependencies (see Linux section).

If you're using Sikarugir, you can open the `Configure.app` by clicking `Show package contents` on your Sikarugir-managed Steam installation, navigating to `Contents` and running `Configure.app`. `winecfg` can be found under the `Tools` tab and `winetricks` in the bottom bar.

##### Installing MelonLoader

Follow the same steps as on Linux.

You also need to make sure that the overrides are set in Steam launch options. Alternatively, you can set the overrides in your Wine prefix with `winecfg`, or click "Wine Configuration" in CrossOver and set the overrides in libraries. In Sikarugir use the `winecfg` in `Configure.app` and/or set the `Unix Commands` in the `Configuration` tab of `Configure.app` to `export WINEDLLOVERRIDES="version=n,b"`.

## Running Schedule I with MelonLoader

Run the game through Steam as usual. First startup after installing MelonLoader or after updating the game may take a bit longer.

{{< alert context="info" >}}
If you have installed MelonLoader correctly, the moment you start the game, a console window should appear. Once the MelonLoader initializes, the game window should open.
{{< /alert >}}
