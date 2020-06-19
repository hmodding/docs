# RConsole

Check if the console is open or closed.

{% tabs %}
{% tab title="Field" %}
```csharp
bool isConsoleOpen;
```
{% endtab %}

{% tab title="Example" %}
```csharp
bool consoleStatus = RConsole.isConsoleOpen;
// The value will be true when the console is open and false when closed.
```
{% endtab %}
{% endtabs %}

  
Get the last command arguments in an array.

{% tabs %}
{% tab title="Field" %}
```csharp
string[] lcargs;
```
{% endtab %}

{% tab title="Example" %}
```csharp
string[] args = RConsole.lcargs.Skip(1).ToArray();
// If value.length == 0 it means that the user didn't specified any argument.

// For example if the user type "urcommand 29" the first array value will be 29.
string firstArgument = args[0];
RConsole.Log("The user has entered the argument : "+firstArgument);
// This will return "The user has entered the argument : 29"
```
{% endtab %}
{% endtabs %}

  
Logging in the console

{% tabs %}
{% tab title="Methods" %}
```csharp
void Log(string log)
```

```csharp
void Log(string log, LogType LogType)
```

```csharp
void LogAssert(string log)
```

```csharp
void LogError(string log)
```

```csharp
void LogException(string log)
```

```csharp
void LogWarning(string log)
```
{% endtab %}

{% tab title="Examples" %}
```csharp
RConsole.Log("my message");
```
{% endtab %}
{% endtabs %}

  
Register a console command.

{% tabs %}
{% tab title="Method" %}
```csharp
void registerCommand(Type modType, string desc, string command, Action action)
```
{% endtab %}

{% tab title="Example" %}
```csharp
RConsole.registerCommand(typeof(ModClass), "Kill yourself.", "kill", kill);

void kill()
{
// put your code here.
}
```
{% endtab %}
{% endtabs %}

