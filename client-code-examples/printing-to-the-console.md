# Printing to the console

To print to the console you can use the `RConsole` class or the unity `Debug` class.

```csharp
// using the RConsole class
RConsole.Log("This is a simple log.");
RConsole.LogAssert("This is an assert...");
RConsole.LogError("This is an error...");
RConsole.LogException("This is an exception...");
RConsole.LogWarning("This is a warning...");
RConsole.Log("It also support <color=red>COLORS</color> and <b><i><size=20>RichText</size></i></b>");


// using the unity Debug class.
Debug.Log("Simple log");
Debug.Log(new Vector3(0,0,0));
Debug.Log(myvalue);
```

