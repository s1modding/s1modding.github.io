---
weight: 22150
title: "Creating Custom Templates"
description: "Build your own reusable MelonLoader mod template for Schedule I"
icon: "construction"
date: "2025-08-08T14:00:00+00:00"
lastmod: "2025-08-08T14:00:00+00:00"
draft: false
toc: true
---

If you plan to create multiple mods or want to share your setup with other developers, creating your own project template can save significant time and ensure consistency across projects.

## Why Create Your Own Template?

Creating a custom template provides several benefits:

- **Instant project setup** - No need to configure references, paths, or build settings for each new mod
- **Consistency** - All your projects follow the same structure and conventions
- **Customization** - Tailor the template to your specific development environment and preferences
- **Multi-backend support** - One template that works with both Mono and IL2CPP

## Prerequisites

Before creating your template, ensure you have:

- A working MelonLoader mod project (either Mono or IL2CPP)
- Visual Studio installed
- Schedule I with MelonLoader properly set up (see [Getting Started](/docs/moddevs/environment_setup/))
- Basic understanding of MSBuild project files (csproj)

## Creating the Template

### 1. Create the Project Structure

Start with a basic C# Class Library project targeting `.NETStandard 2.1`:

```xml
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.1</TargetFramework>
        <ImplicitUsings>enable</ImplicitUsings>
        <RootNamespace>$safeprojectname$</RootNamespace>
        <LangVersion>default</LangVersion>
        <NeutralLanguage>en-US</NeutralLanguage>
        <Configurations>Mono;Il2cpp</Configurations>
    </PropertyGroup>
```

{{< alert context="info" >}}
The `$safeprojectname$` placeholder will be replaced with the actual project name when creating a new project from the template.
{{< /alert >}}

### 2. Define Configuration-Specific Properties

Set up different paths and settings for each backend:

```xml
<!-- Define paths for different configurations -->
<PropertyGroup Condition="'$(Configuration)'=='Mono'">
    <GamePath>Path\to\Schedule I (mono)</GamePath>
    <ManagedPath>$(GamePath)\Schedule I_Data\Managed</ManagedPath>
    <MelonLoaderPath>$(GamePath)\MelonLoader\net35</MelonLoaderPath>
    <ModsPath>$(GamePath)\Mods</ModsPath>
    <DefineConstants>MONO</DefineConstants>
    <AssemblyName>$safeprojectname$_Mono</AssemblyName>
</PropertyGroup>

<PropertyGroup Condition="'$(Configuration)'=='Il2cpp'">
    <GamePath>Path\to\Schedule I (il2cpp)</GamePath>
    <ManagedPath>$(GamePath)\MelonLoader\Il2CppAssemblies</ManagedPath>
    <MelonLoaderPath>$(GamePath)\MelonLoader\net6</MelonLoaderPath>
    <ModsPath>$(GamePath)\Mods</ModsPath>
    <DefineConstants>IL2CPP</DefineConstants>
    <AssemblyName>$safeprojectname$_Il2cpp</AssemblyName>
</PropertyGroup>
```

{{< alert context="warning" >}}
Update the `GamePath` values to match your actual Schedule I installation directories. You may have separate installations for Mono and IL2CPP testing.
{{< /alert >}}

### 3. Add Assembly References

Include all necessary Unity and game assemblies for each configuration:

```xml
<!-- Mono references -->
<ItemGroup Condition="'$(Configuration)'=='Mono'">
    <Reference Include="Assembly-CSharp-publicized">
        <HintPath>$(ManagedPath)\Assembly-CSharp-publicized.dll</HintPath>
    </Reference>
    <Reference Include="UnityEngine">
        <HintPath>$(ManagedPath)\UnityEngine.dll</HintPath>
    </Reference>
    <Reference Include="UnityEngine.CoreModule">
        <HintPath>$(ManagedPath)\UnityEngine.CoreModule.dll</HintPath>
    </Reference>
    <Reference Include="UnityEngine.UI">
        <HintPath>$(ManagedPath)\UnityEngine.UI.dll</HintPath>
    </Reference>
    <!-- Add other Unity assemblies as needed -->
    <Reference Include="MelonLoader">
        <HintPath>$(MelonLoaderPath)\MelonLoader.dll</HintPath>
    </Reference>
    <Reference Include="0Harmony">
        <HintPath>$(MelonLoaderPath)\0Harmony.dll</HintPath>
    </Reference>
</ItemGroup>

<!-- Il2cpp references -->
<ItemGroup Condition="'$(Configuration)'=='Il2cpp'">
    <Reference Include="Assembly-CSharp">
        <HintPath>$(ManagedPath)\Assembly-CSharp.dll</HintPath>
    </Reference>
    <Reference Include="UnityEngine">
        <HintPath>$(ManagedPath)\UnityEngine.dll</HintPath>
    </Reference>
    <Reference Include="UnityEngine.CoreModule">
        <HintPath>$(ManagedPath)\UnityEngine.CoreModule.dll</HintPath>
    </Reference>
    <Reference Include="UnityEngine.UI">
        <HintPath>$(ManagedPath)\UnityEngine.UI.dll</HintPath>
    </Reference>
    <!-- Add other Unity assemblies as needed -->
    <Reference Include="MelonLoader">
        <HintPath>$(MelonLoaderPath)\MelonLoader.dll</HintPath>
    </Reference>
    <Reference Include="0Harmony">
        <HintPath>$(MelonLoaderPath)\0Harmony.dll</HintPath>
    </Reference>
    <Reference Include="Il2CppInterop.Runtime">
        <HintPath>$(MelonLoaderPath)\Il2CppInterop.Runtime.dll</HintPath>
    </Reference>
</ItemGroup>
```

### 4. Add Build Configuration

Enable unsafe blocks and set up post-build actions:

```xml
<PropertyGroup>
    <AllowUnsafeBlocks>True</AllowUnsafeBlocks>
    <EnableDefaultCompileItems>false</EnableDefaultCompileItems>
</PropertyGroup>

<Target Name="PostBuild" AfterTargets="PostBuildEvent">
    <Exec Command="(tasklist | findstr /I &quot;Schedule.*I.exe&quot; &gt;nul &amp;&amp; taskkill /F /IM &quot;Schedule I.exe&quot; &amp;&amp; timeout /t 3 /nobreak &gt;nul) || echo Game not running&#xD;&#xA;for /L %%i in (1,1,5) do (COPY &quot;$(TargetPath)&quot; &quot;$(ModsPath)&quot; &amp;&amp; goto copied || timeout /t 1 /nobreak &gt;nul)&#xD;&#xA;:copied&#xD;&#xA;START &quot;&quot; &quot;$(GamePath)\Schedule I.exe&quot;" />
</Target>
```

This post-build action will:
1. Close Schedule I if it's running
2. Copy the compiled mod to the Mods folder
3. Restart the game for immediate testing

### 5. Create a Basic Mod Class

Include a starter mod class in your template:

```csharp
using MelonLoader;

[assembly: MelonInfo(typeof($safeprojectname$.MainMod), "$safeprojectname$", "1.0.0", "YourName")]
[assembly: MelonGame("TVGS", "Schedule I")]

namespace $safeprojectname$;

public class MainMod : MelonMod
{
    public override void OnInitializeMelon()
    {
        LoggerInstance.Msg("$safeprojectname$ loaded!");
    }

#if MONO
    public override void OnSceneWasLoaded(int buildIndex, string sceneName)
    {
        LoggerInstance.Msg($"Scene loaded: {sceneName} (Build Index: {buildIndex})");
    }
#elif IL2CPP
    public override void OnSceneWasLoaded(int buildIndex, string sceneName)
    {
        LoggerInstance.Msg($"Scene loaded: {sceneName} (Build Index: {buildIndex})");
    }
#endif
}
```

## Exporting the Template

Once your project is working correctly:

1. **Clean your project** - Remove any build artifacts and temporary files
2. **In Visual Studio**, go to **Project** → **Export Template**
3. **Choose "Project Template"**
4. **Fill in the template details:**
   - Name: e.g., "Schedule I MelonLoader Template"
   - Description: Brief description of your template's features
   - Icon: Optional icon for the template
   - Preview Image: Optional preview image
5. **Click "Finish"** - The template will be saved to your Visual Studio templates folder

{{< alert context="success" >}}
Template Location: Your exported template will be saved to:  
`%USERPROFILE%\Documents\Visual Studio 202X\Templates\ProjectTemplates\`
{{< /alert >}}

## Using Your Template

After exporting, your template will be available when creating new projects:

1. **File** → **New** → **Project**
2. **Search for your template name** or browse under "Visual C#"
3. **Enter project name and location**
4. **Click "Create"**

Your new project will have all references, configurations, and build settings automatically configured!

## Sharing Your Template

To share your template with others:

1. **Locate the template file** (`.zip` file in the templates folder)
2. **Share the zip file** with other developers
3. **Installation:** Others can copy the zip file to their Visual Studio templates folder

Alternatively, you can create a Visual Studio extension (.vsix) for easier distribution.

## Using Conditional Compilation

With your template, you can use preprocessor directives to handle backend-specific code:

```csharp
#if MONO
    // Access private members directly when you publicize the assembly-csharp.dll
    var privateField = someObject.privateField;
#elif IL2CPP
    var someField = someObject.someField;
#endif
```

## Tips and Best Practices

- **Test both configurations** - Make sure your template works for both Mono and IL2CPP
- **Include common assemblies** - Add references to Unity assemblies you frequently use
- **Document your template** - Include a README with setup instructions
- **Version your template** - Keep track of template versions as you improve it
- **Consider multiple templates** - Create specialized templates for different types of mods

## Next Steps

Now that you have your template set up, you're ready to start creating mods efficiently! Check out the [Creating Your First Mod](/docs/moddevs/creating_your_first_mod/) guide to learn about the basics of mod development.

If you haven't set up your development environment yet, head back to [Getting Started](/docs/moddevs/environment_setup/) for the complete setup guide.

## Additional Resources

- [Visual Studio Project Templates Documentation](https://docs.microsoft.com/en-us/visualstudio/ide/how-to-create-project-templates)
- [MSBuild Reference](https://docs.microsoft.com/en-us/visualstudio/msbuild/msbuild-reference)
- [MelonLoader Documentation](https://melonwiki.xyz/)
