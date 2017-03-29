---
layout: post
title:  "Shhh, keep your ASP.NET Core Secrets to Yourself"
date:   2017-03-28 22:00:00 -0500
categories: ASP.NET Core
comments: true
---

Azure provides wonderful PaaS options for developers, especially those using .NET. In many cases, services like Azure Service Bus, require connection strings and secured tokens used to authenticate our applications. If you want to make your repository publicly accessible, it is not a good idea to include these connection strings and other sensitive bits of information as part of the source code.

## Microsoft to the Rescue

Thankfully, with the newcoming of ASP.NET Core, there's a project called **[Secret Manager](https://github.com/aspnet/DotNetTools)** that lets you manage these secrets via command line so you don't have to include them in the source code. Then you can load these secrets into your ASP.NET Core app using a NuGet package.

### 1. Install the Secret Manager CLI

Right click on the project that you want to use these secrets against. Then edit the `.csproj` of the ASP.NET Core app and add the following item group:

``` xml
<ItemGroup>
    <DotNetCliToolReference Include="Microsoft.Extensions.SecretManager.Tools" Version="1.0.0" />
</ItemGroup>
```

Also generate your own Guid and add it as follows:

``` xml
<PropertyGroup>
    <UserSecretsId>{Your Guid}</UserSecretsId>
</PropertyGroup>
```

Then using the command line, navigate to ASP.NET Core project directory and use the .net CLI:

```
    C:\Shhhh>dotnet restore
    C:\Shhhh>dotnet user-secrets --help
    User Secrets Manager 1.0.0-rtm-10308

    Usage: dotnet user-secrets [options] [command]

    Options:
    -?|-h|--help                        Show help information
    --version                           Show version information
    -v|--verbose                        Show verbose output
    -p|--project <PROJECT>              Path to project, default is current directory
    -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'
    --id                                The user secret id to use.

    Commands:
    clear   Deletes all the application secrets
    list    Lists all the application secrets
    remove  Removes the specified user secret
    set     Sets the user secret to the specified value

    Use "dotnet user-secrets [command] --help" for more information about a command.
```

### 2. Create your secrets

Start storing your secrets! Like the `--help` command indicated, you can use the `set` command to start storing away.

```
    C:\Shhhh>dotnet user-secrets set AppConnectionString SomeAmazingConnectionStringToADatabaseOnTheCloudThatYouDontWantOthersToKnowAbout
```

You can very how it was stored by running the `list` command.

```
    C:\Repos\DeleteMe\DeleteMe>dotnet user-secrets list 
    AppConnectionString = SomeAmazingConnectionStringToADatabaseOnTheCloudThatYouDontWantOthersToKnowAbout
```

### 3. Consume your secrets 

Install the `Microsoft.Extensions.Configuration.UserSecrets` NuGet package.

Then in the `Startup.cs` file, add the following to the configuration builder:

``` csharp
    builder.AddUserSecrets<Startup>();
```

Lastly, you can use the `IConfigurationRoot` instance to consume your secrets when setting up the dependency injection or the middleware. 

For example:

``` csharp
 var myConnectionString = Configuration["AppConnectionString"]
```

## Caveats

1. **Secrets are not encrypted**. What this tooling buys you is the ability to manage configuration settings that you don't want to have in your source code repository.

2. When working with others, you're going to have to share these secrets with them. At this point you might consider the Azure Key Vault or using a tool like LastPass.
 