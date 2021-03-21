# Adding private variables

## Adding private variables

Adding private variables to an existing Raft class is sometimes very necessary. There're many ways to add new variables. In this article we will consider the method provided by [Whitebrim](https://www.raftmodding.com/user/Whitebrim). (Discord Whitebrim#4444)

We will store variables in the newly created class and add an extension for original Raft class. You can read how C\# extensions work [here](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).

We need to create new **`ConditionalWeakTable<TKey, TValue>`**. It will store our data classes and link them to original classes. This class is specifically created for our case, if you're interested you can read more about it [here](https://docs.microsoft.com/ru-ru/dotnet/api/system.runtime.compilerservices.conditionalweaktable-2?view=netframework-4.5).

In this example we will add new **`private bool isLocked`** variable to **`SteeringWheel`** class:

**First**, we need to **create data class** to store all data we need to add:

```csharp
[Serializable]
public class SteeringWheelAdditionalData
{
    public bool isLocked;

    public SteeringWheelAdditionalData()
    {
        isLocked = false;
    }
}
```

**You must add `[Serializable]` attribute that our class could be saved.** Also you need to create default constructor with no parameters to initialize class with default values.

**Second**, we need to **create an extension class** to store and handle data:

```csharp
public static class SteeringWheelExtension
{
    private static readonly ConditionalWeakTable<SteeringWheel, SteeringWheelAdditionalData> data = 
        new ConditionalWeakTable<SteeringWheel, SteeringWheelAdditionalData>();

    public static SteeringWheelAdditionalData GetAdditionalData(this SteeringWheel steeringWheel)
    {
        return data.GetOrCreateValue(steeringWheel);
    }

    public static void AddData(this SteeringWheel steeringWheel, SteeringWheelAdditionalData value)
    {
        try
        {
            data.Add(steeringWheel, value);
        }
        catch (Exception) { }
    }
}
```

Then you need to **patch original class**. In our case we need to change displaying text depending on **`IsLocked`** state. We will patch **`OnIsRayed()`** method inside **`SteeringWheel`** class:

```csharp
[HarmonyPatch(typeof(SteeringWheel), "OnIsRayed")]
class SteeringWheelPatchOnIsRayed
{
    private static void Postfix(SteeringWheel __instance, ref DisplayTextManager ___displayText)
    {
        if (!__instance.GetAdditionalData().isLocked)
            ___displayText.ShowText("Press to LOCK rotation", MyInput.Keybinds["RMB"].MainKey, 2, 0, false);
        else
            ___displayText.ShowText("Press to UNLOCK rotation", MyInput.Keybinds["RMB"].MainKey, 2, 0, false);

        if (MyInput.GetButtonDown("RMB"))
            __instance.GetAdditionalData().isLocked = !__instance.GetAdditionalData().isLocked; // Toggle bool
    }
}
```

You can read more about **Harmony patching** [here](https://github.com/pardeike/Harmony/wiki/Patching).

Also you need to **apply patch**:

```csharp
public class BetterSteeringWheel : Mod
{
    private HarmonyInstance harmonyInstance;

    public void Start()
    {
        harmonyInstance = new Harmony("com.whitebrim.bettersteeringwheel"); // It's custom patch name, you need to name your patch differently
        harmonyInstance.PatchAll(Assembly.GetExecutingAssembly());
    }

    public void OnModUnload()
    {
        harmonyInstance.UnpatchAll();
        Destroy(gameObject);
    }
}
```

You can access new data using class instance. **`SteeringWheel.GetAdditionalData()`**.

Example for **`float`** and **`string`**:

```csharp
[Serializable]
public class YourClassAdditionalData
{
    public float floatVar;
    public string stringVar;

    public CustomClassAdditionalData()
    {
        floatVar = 0;
        stringVar = "None";
    }
}

public static class CustomExtension // Nothing changed :)
{
    private static readonly ConditionalWeakTable<YourClass, YourClassAdditionalData> data = 
        new ConditionalWeakTable<YourClass, YourClassAdditionalData>();

    public static YourClassAdditionalData GetAdditionalData(this YourClass yourClass)
    {
        return data.GetOrCreateValue(yourClass);
    }

    public static void AddData(this YourClass yourClass, YourClassAdditionalData value)
    {
        try
        {
            data.Add(yourClass, value);
        }
        catch (Exception) { }
    }
}
```

## Saving private variables

To save private variables you need to patch **`RDG_%RaftClassName%`** class and method that is invoked inside switch in **`SaveAndLoad`** class in **`RestoreRGDGame(RGD_Game game)`** method.

In this example we will use **`SteeringWheel`** class and save data from **`Adding private variables`** section.

**First**, we have to add new link **`RGD Class -> our additional data class`** \(ConditionalWeakTable\):

```csharp
public static class SteeringWheelExtension
{
    public static ConditionalWeakTable<RGD_SteeringWheel, SteeringWheelAdditionalData> RGD_data = 
            new ConditionalWeakTable<RGD_SteeringWheel, SteeringWheelAdditionalData>();

    public static void AddData(this RGD_SteeringWheel RGD_SteeringWheel, SteeringWheelAdditionalData value)
    {
        try
        {
            RGD_data.Add(RGD_SteeringWheel, value);
        }
        catch (Exception) { }
    }
}
```

**Second**, we need to patch **`RGD_SteeringWheel`** class. There're two constructors \(one for saving, another for loading\) and **`GetObjectData`** method \(to control flow of Serialization\).

```csharp
class RGD_SteeringWheelPatch
{
    [HarmonyPatch(typeof(RGD_SteeringWheel), MethodType.Constructor, new Type[]{ typeof(RGDType), typeof(SteeringWheel) })]
    class RGD_SteeringWheelConstructor1 // Constructor for saving
    {
        private static void Prefix(RGD_SteeringWheel __instance, ref SteeringWheel steeringWheel)
        {
            __instance.AddData(steeringWheel.GetAdditionalData());
        }
    }

    [HarmonyPatch(typeof(RGD_SteeringWheel), MethodType.Constructor, new Type[] { typeof(SerializationInfo), typeof(StreamingContext) })]
    class RGD_SteeringWheelConstructor2 // Constructor for loading
    {
        private static void Prefix(RGD_SteeringWheel __instance, ref SerializationInfo info)
        {
            try
            {
                __instance.AddData((SteeringWheelAdditionalData)info.GetValue("IsLocked", typeof(SteeringWheelAdditionalData)));
            }
            catch (Exception) { }
        }
    }

    [HarmonyPatch(typeof(RGD_SteeringWheel), "GetObjectData")]
    class GetObjectData // Interfering with the serialization flow and adding our custom data to the save
    {
        private static void Postfix(RGD_SteeringWheel __instance, ref SerializationInfo info)
        {
            SteeringWheelAdditionalData value;
            if (SteeringWheelExtension.RGD_data.TryGetValue(__instance, out value))
                info.AddValue("IsLocked", value);
        }
    }
}
```

**Lastly**, we need to find method that loads saved block data and patch it. This method is invoked inside switch in **`SaveAndLoad`** class in **`RestoreRGDGame(RGD_Game game)`** method:

```csharp
switch (rgd.type)
    {
    case RGDType.Block:
    {
      // Important code
      break;
    }
    case RGDType.Block_Door:
    {
      // Important code
      break;
    }
    // etc...
```

Our _case_:

```csharp
case RGDType.Block_SteeringWheel:
    {
      RGD_SteeringWheel rgd_SteeringWheel = rgd as RGD_SteeringWheel;
      Block block19 = this.RestoreBlock(network_Player.BlockCreator, rgd_SteeringWheel);
      if (block19 != null)
      {
          SteeringWheel componentInChildren3 = block19.GetComponentInChildren<SteeringWheel>();
          if (componentInChildren3 != null)
          {
              componentInChildren3.RestoreWheel(rgd_SteeringWheel); // <- This line
          }
      }
      break;
    }
```

In our case method is called **`RestoreWheel(RGD_SteeringWheel rgdWheel)`**. This method contains instructions on how to deserialize RGD class to retrieve saved data. We will add our custom instructions:

```csharp
[HarmonyPatch(typeof(SteeringWheel), "RestoreWheel")]
class SteeringWheelRestoreWheelPatch
{
    private static void Prefix(SteeringWheel __instance, RGD_SteeringWheel rgdWheel)
    {
        SteeringWheelAdditionalData value;
        if (SteeringWheelExtension.RGD_data.TryGetValue(rgdWheel, out value))
            __instance.AddData(value);
    }
}
```

**Congratulations**, our custom data for Steering Wheel is saving and loading successfully.

Full code you can find downloading [Better Steering Wheel](https://www.raftmodding.com/mods/better-steering-wheel) mod.
