---
weight: 11300
title: "Troubleshooting"
description: "Common issues and their solutions"
icon: "bug_report"
date: "2025-06-18T21:34:35+02:00"
lastmod: "2025-06-18T21:34:35+02:00"
draft: false
toc: true
---

Sometimes things don't work as expected. Here are some common issues and their solutions.

If you encounter an issue not listed here, consider asking for help in the Modding Discord server or report the problem to the specific mod's author.

{{< alert context="warning" >}}
When reporting issues, always include relevant information such as:
<ul>
    <li>Log files (see <a href="/docs/modusers/common_terms/#log-files">Common Terms</a> for log file locations)</li>
    <li>Game version and branch (IL2CPP or Mono)</li>
    <li>MelonLoader version</li>
    <li>List of installed mods/plugins</li>
    <li>Steps to reproduce the issue and/or relevant screenshots</li>
</ul>
<b>Log files are especially important</b>, as they often contain error messages that can help diagnose the problem.
{{< /alert >}}

## Mod not loading
Verify that the mod is in `Mods` folder in the game directory.

If the mod is in the correct folder, check the MelonLoader console/log for any errors related to the mod. If the mod is installed properly, MelonLoader should log a message indicating that the mod has been loaded. If you see an error message, it may indicate that the mod is incompatible with your version of the game or branch.

## IL2CPP/Mono mismatch
If you are trying to load a mod and it fails with an error related to IL2CPP or Mono, it means that the mod is built for a different scripting backend than the one you are using.
The easy way to determine that is to check the mod's description on the mod page. If it doesn't specify, you can check the mod's `.dll` file name. If it contains `IL2CPP`, it is an IL2CPP mod, if it contains `Mono`, it is a Mono mod. If it doesn't contain either, it might be a mod for the main branch of the game, which is IL2CPP.

To check for yourself, look at the MelonLoader console/log when you start the game. Il2Cpp mods will usually error with `... Version=6.0.0` on Mono, while Mono mods will error with `... Version=0.0.0` on IL2CPP.

Examples of those can be found below.

### Could not load file or assembly 'System.Collections, Version=6.0.0.0, Culture=neutral, PublicKeyToken=...' or one of its dependencies.
{{< figure src="ilcpperr.png" alt="Error on Mono while trying to load IL2PP mod" >}}
This error indicates that the mod you're trying to load was built for IL2CPP, but you're running the game on Mono branch. You need to switch to IL2CPP branch in Steam properties or find a Mono version of the mod.

### Could not load type 'ScheduleOne.UI.Phone.Delivery.DeliveryShop' from assembly 'Assembly-CSharp, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null'.
{{< figure src="monomoderr.png" alt="Error on IL2CPP while trying to load Mono mod" >}}
This error indicates that the mod you're trying to load was built for Mono, but you're running the game on IL2CPP branch. You need to switch to Mono branch in Steam properties or find an IL2CPP version of the mod.

## Assembly-CSharp.dll is being used by another process
> [Il2CppAssemblyGenerator] Unhandled exception. System.IO.IOException: The process cannot access the file '<path>\cpp2il_out\Assembly-CSharp.dll' because it is being used by another process.

{{< figure src="assemblygenerr.png" alt="Error in Il2CppAssemblyGenerator" >}}
This error can appear eg. if the game was started twice. The system is trying to access the same file, but a lock hasn't been released. This can be resolved by deleting the `<game dir>\MelonLoader\Dependencies\Il2CppAssemblyGenerator\Cpp2IL\cpp2il_out` folder and restarting the game. The folder will be recreated automatically.
