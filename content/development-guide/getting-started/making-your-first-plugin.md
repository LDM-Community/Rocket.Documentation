# Making your first plugin

In this guide we will cover:

1. Setting up development environment.
2. Creating a plugin project.
3. Writing a basic plugin.

## Installing the IDE for coding

### Visual Studio Code
You can install [install Visual Studio Code](https://code.visualstudio.com/) with the [Omnisharp extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) for developing plugins. Visual Studio Code is much lightweighter and faster then a full Visual Studio installation. It is optimal for small-mid size projects.

### Visual Studio
If you want a full IDE experience, download and install [Visual Studio Community Edition](https://visualstudio.microsoft.com/vs/community/). When the installer starts, select "Visual Studio 2019 Community Edition" (or newer). After that select the .NET Desktop Development option and press install/modify.

![Selecting .NET desktop development option](https://docs.microsoft.com/en-us/visualstudio/install/media/vs2017-modify-workloads.png?view=vs-2017g)

## Making a plugin from scratch
First create new "Class Library (.NET Framework)" in Visual Studio. Remember to select Framework to `.NET Framework 4.6.1`

<img src="https://i.imgur.com/6xpxrLt.png" style="max-width: 80%;" />

Now You have to add RocketMod and Unturned dependencies to your project. You can download latest Rocket.Unturned binaries [here](https://github.com/RocketMod/Rocket.Unturned/releases) and latest Unturned binaries `Assembly-CSharp.dll` and `Assembly-CSharp-firstpass.dll` you should find them in `Unturned/Unturned_Data/Managed` directory of your Unturned files. You'll also need `UnityEngine.dll` and `UnityEngine.CoreModule.dll` which are both located in the same directory as Unturned binaries.  

You should create and save all these libraries in your projects directory. Create a folder called `libraries` or `lib` for them.

Now in Visual Studio's solution explorer right click on **References** and press **Add Reference**. Then browse for RocketMod and Unturned libraries that you saved in your projects directory, select and add them.

After that rename the pre-existing Class1.cs file to ExamplePlugin.cs from the solution explorer. On the top of the file add `using Rocket.Core.Plugins`.

We're now gonna start by making ExamplePlugin inherit from `RocketPlugin`, so your `ExamplePlugin.cs` should look like this now.

```csharp
using Rocket.Core.Plugins;

namespace ExamplePlugin
{
    public class ExamplePlugin : RocketPlugin
    {

    }
}
```

Now for the beginning we're gonna send a basic logger message when the plugin loads and unloads. For that you have to override `void Load()` and `void Unload()` methods like below and add your message.

!!! note
    I've added 2 more usings to the top of `ExamplePlugins.cs` (`System` and `Rocket.Core.Logging`)  

```csharp
using System;
using Rocket.Core.Logging;
using Rocket.Core.Plugins;

namespace ExamplePlugin
{
    public class ExamplePlugin : RocketPlugin
    {
        protected override void Load()
        {
            Logger.Log($"{Name} {Assembly.GetName().Version} has been loaded!", ConsoleColor.Yellow);
        }

        protected override void Unload()
        {
            Logger.Log($"{Name} has been unloaded!", ConsoleColor.Yellow);
        }
    }
}
```

!!! note
    `Assembly.GetName().Version` returns your Assembly Version. You can change it in `AssemblyInfo.cs`. You can find under `Properties` in Visual Studio's solution explorer.
