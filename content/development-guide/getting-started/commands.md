# Commands

Commands are an interface for users and other plugins to trigger specific actions of your plugins. They are usually executed via chat, console, web interface or rcon.

Example command: `/kick PlayerA "Some kick Reason"`.

## Implementing Commands
There are two ways to implement commands in RocketMod 4. The first one is by implementing the IRocketCommand interface, the second one is by using the `[RocketCommand]` attribute.

### 1. Registering Commands with IRocketCommand
Implement the `IRocketCommand` interface like this:
```csharp
using Rocket.API;
using Rocket.Unturned.Chat;
using System.Collections.Generic;

namespace ExamplePlugin.Commands
{
    public class SampleCommand : IRocketCommand
    {
        public AllowedCaller AllowedCaller => AllowedCaller.Both;

        public string Name => "sample";

        public string Help => "Sample command that writes back Hello World! to you";

        public string Syntax => string.Empty;

        public List<string> Aliases => new List<string>();

        public List<string> Permissions => new List<string>();

        public void Execute(IRocketPlayer caller, string[] command)
        {
            UnturnedChat.Say(caller, "Hello World!", UnityEngine.Color.cyan);
        }
    }
}
```

* **AllowedCaller:** Allow command user (Player, Console or Both)
* **Name:**	The commands name (e.g. `rocket`, `buy`, `kick`, etc).
* **Help:**	A short summary of the commands function.
* **Syntax:** A syntax string for the command `[]` = *optional* and `<>` = *required* (e.g. [steamId] <itemId>).
* **Aliases:** A list of alternative for the name, not required.
* **Permissions:** A list of alternative for the permissions, not required.
* **Execute:**   The method that gets invoked when someone executes your command.


`IRocketCommand`s are automatically registered when your plugin loads.

### 2. Registering Commands with RocketCommand attribute
The exactly same doing `sample` command you can also make using RocketCommand attribute like this:

```csharp
[RocketCommand("sample", "Sample command that writes back Hello World! to you")]
public void SampleCommand(IRocketPlayer caller, string[] command)
{
    UnturnedChat.Say(caller, "Hello World!", UnityEngine.Color.cyan);
}
```

!!! note
    If you want to initialize commands using `RocketCommand` attribute, you must do it in the instance class of a plugin, otherwise commands will not load!