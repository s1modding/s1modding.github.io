---
weight: 22100
title: "Getting Started"
description: "Setting up your modding environment for Schedule I"
icon: "rocket_launch"
date: "2025-06-26T16:51:26+02:00"
lastmod: "2025-06-26T16:51:26+02:00"
draft: false
toc: true
---

To start off, you need to set up your modding environment. This guide will help you install the necessary tools and get started with modding Schedule I.

## Prerequisites

{{< alert context="info" >}}
Before starting, ensure you have a copy of Schedule I installed via Steam and basic familiarity with programming concepts and C#.
{{< /alert >}}

- A copy of Schedule I installed via Steam.
- Familiarity with basic programming concepts and C#.
- A code editor or IDE (e.g., Visual Studio, Visual Studio Code, Rider).
- Basic understanding of Unity and MelonLoader.

{{< alert context="warning" >}}
Install MelonLoader and be sure to read through <a href="/docs/modusers/">Mod Users</a> section to familiarize yourself with the modding ecosystem.
{{< /alert >}}

Install .NET SDK 6.0 or later. You can use newer versions, like [10.0](https://dotnet.microsoft.com/en-us/download/dotnet/10.0), allowing you to use the latest C# features.

## Setting up your development environment

**Install a Code Editor or IDE**
- Download and install [Rider](https://www.jetbrains.com/rider/) or [Visual Studio](https://visualstudio.microsoft.com/).
- If you prefer a lightweight option, you can use [Visual Studio Code](https://code.visualstudio.com/) with extensions.

### Setting up your project

**Choose your setup approach:**

📦 **[Templates](#templates)** - Quick start with existing templates (recommended for beginners)  
🛠️ **[Creating Custom Templates](/docs/moddevs/creating_templates/)** - Build a reusable template for future projects  
⚙️ **[Manual Setup](#manual-setup)** - Complete control over project configuration  

#### Templates

{{< alert context="success" >}}
There's an assortment of templates available for quickly starting your modding project. Using a template is the recommended approach for beginners.
{{< /alert >}}

- [MelonLoader Mod Template](https://github.com/TrevTV/MelonLoader.VSWizard/releases) - Official MelonLoader template for Visual Studio.
- [Mono Mod Template for MelonLoader and BepInEx](https://github.com/MaxtorCoder/ScheduleOnePluginTemplate) - Customizable template for creating mods with Mono and MelonLoader or BepInEx. Includes optional support for Unity scripts projects to create asset bundles. By [MaxtorCoder](https://github.com/MaxtorCoder).
- [Basic modding template for Mono](https://github.com/XOWithSauce/schedule-mono-example) - A simple template for creating mods with Mono. By [XOWithSauce](https://github.com/XOWithSauce).
- [Basic modding template for IL2CPP](https://github.com/XOWithSauce/schedule-il2cpp-example) - A simple template for creating mods with IL2CPP. By [XOWithSauce](https://github.com/XOWithSauce).
- [S1 Mono+IL2CPP C# Project Template](https://github.com/weedeej/S1MONO_IL2CPP_Template) - Clean template with conditional compilation for Mono and IL2CPP. Includes AssetBundleUtils for loading asset bundles. By [weedeej](https://github.com/weedeej).
- [Yet Another MelonMod Template with batteries included](https://github.com/k073l/S1MelonModTemplate) - Template featuring cross-backend (IL2CPP and Mono) compatibility, easy build and test process, automatic loading of testing mods, and more. By [k073l](https://github.com/k073l).

**Want to create your own template?** If you plan to create multiple mods or need a custom setup, check out our [Creating Custom Templates](/docs/moddevs/creating_templates/) guide. You'll learn how to build a reusable template with zero setup time for future projects.

#### Manual Setup

{{< alert context="warning" >}}
Manual setup is more complex and error-prone. Consider using a template first if you're new to modding.
{{< /alert >}}

If you prefer to set up your project manually, follow these steps:

1. Create a new C# Class Library project in your IDE. Be sure to target:
    - For Mono: .NETStandard 2.1
    - For IL2CPP: .NET 6.0

2. Include references to files you'll need:
    - `MelonLoader.dll` (from `MelonLoader/net35` or `MelonLoader/net6` folder in your game directory, depending on your target backend)
    - `UnityEngine.dll` (from `<game dir>/Schedule I_Data/Managed` or `MelonLoader\Il2CppAssemblies\` for IL2CPP)
    - `Assembly-CSharp.dll` (from `<game dir>/Schedule I_Data/Managed` or `MelonLoader\Il2CppAssemblies\` for IL2CPP)
    - other references as needed

3. If you are using Mono, you can also add a NuGet package - [BepInEx Publicizer](https://www.nuget.org/packages/BepInEx.AssemblyPublicizer.MSBuild). Then, when you include `Assembly-CSharp.dll` in your project, add this reference to publicize the types in that assembly:

```xml
<Reference Include="Assembly-CSharp" Publicize="true">
    <Private>false</Private>
    <HintPath><game dir>\Schedule I_Data\Managed\Assembly-CSharp.dll</HintPath>
</Reference>
```

{{< alert context="info" >}}
The BepInEx Publicizer allows you to access private and protected members without using reflection, making development easier.
{{< /alert >}}

{{< alert context="default" >}}
It's recommended to use Git for version control, regardless of whether you use a template or set up your project manually. This will help you manage your mod's development and allows you to share your code with others on platforms like <a href="https://github.com">GitHub</a>.
{{< /alert >}}

{{< alert context="success" >}}
You're now ready to start coding your mod by creating a class that inherits from <code>MelonMod</code>.
{{< /alert >}}

Further information can be found in [Creating Your First Mod](/docs/moddevs/creating_your_first_mod/) guide.

## Additional Resources
- [MelonLoader Documentation](https://melonwiki.xyz/)
- [Unity Documentation](https://docs.unity3d.com/2022.3/Documentation/Manual/index.html)
- [C# Programming Guide](https://docs.microsoft.com/en-us/dotnet/csharp/)
- [Schedule I Modding Discord](https://discord.gg/9Z5RKEYSzq)