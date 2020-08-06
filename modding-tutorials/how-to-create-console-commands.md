---
description: >-
  This tutorial aims to show you how to create custom console commands using the
  new console attributes.
---

# How to create console commands

Console commands are method attributes, that means that you just have to add `[ConsoleCommand(name: "...", docs:"...")]` or `[ConsoleCommand("...","...")]` above your method.  
The **name** argument is the command name itself.  
The **docs** argument is the command description.

{% hint style="info" %}
**Your method does not need to be public but it needs to be static!**
{% endhint %}

If you want to get the arguments of your command when its ran you can simply add a string\[\] parameter named `args` as shown below.

```csharp
[ConsoleCommand(name: "useful", docs: "this command is useful.")]
public static void MyCommand(string[] args)
{
    Debug.Log("You entered " + args.Length + " arguments!");
}
```

The method can also have a string return type and will display the returned value as shown below.

```csharp
[ConsoleCommand(name: "useful", docs: "this command is useful.")]
public static string MyCommand(string[] args)
{
    return "You entered " + args.Length + " arguments!";
}
```

As said above, commands needs to be static, so in order to access your non static variables you can use the mod instance as shown below.

```csharp
public string myNonStaticVariable = "some stuff";
public MyModType instance;

void Start(){
    instance = this;
}

[ConsoleCommand(name: "useful", docs: "this command is useful.")]
public static string MyCommand(string[] args)
{
    return "My Variable : " + instance.myNonStaticVariable;
}
```

{% hint style="info" %}
The commands are automatically registered and automatically unregistered when the mod is unloaded.
{% endhint %}

