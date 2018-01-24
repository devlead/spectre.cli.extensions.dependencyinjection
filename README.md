# Spectre.CommandLine.Extensions.DependencyInjection

A type provider for [`Spectre.CommandLine`](https://github.com/spectresystems/spectre.commandline) using the [`Microsoft.Extensions.DependencyInjection`](https://www.nuget.org/packages/Microsoft.Extensions.DependencyInjection/) container.

## Getting started

Once you've installed both `Spectre.CommandLine` and this package, just change the call to `new CommandApp()` in your `Program.cs` to match the below:

```csharp
var services = new ServiceCollection();
// add extra services to the container here
var app = new CommandApp(new DependencyInjectionRegistrar(services));
app.Configure(config =>
{
    // configure your commands as per usual
    // commands are automatically added to the container
}
return app.Run(args);
```

## Using the container

Once you've changed your `Program.cs` as above, you can inject anything you need into your commands:

```csharp
// Program.cs
var services = new ServiceCollection();
services.AddSingleton<ICoolService, MyCoolService>();
var app = new CommandApp(new DependencyInjectionRegistrar(services));
app.Configure(config =>
{
    config.AddCommand<SomeCommand>("command");
    // commands are automatically added to the container
}
```

```csharp
// in SomeCommand.cs
public class SomeCommand : Command<SomeCommand.Settings>
{
    public ProfileCreateCommand(ICoolService service)
    {
        // the `ICoolService` parameter will be automatically injected with an instance of `MyCoolService`
    }

    public override int Execute(SomeCommand.Settings settings, ILookup<string, string> unmapped)
    {
        // command logic here
    }
}
```

> For a more complex example, check out [git-profile-manager](https://github.com/agc93/git-profile-manager).