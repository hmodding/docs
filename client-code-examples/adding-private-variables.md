# Adding private variables

## Adding private variables

Adding private variables to an existing Raft class is sometimes very necessary. There're many ways to add new variables. In this article we will consider the method provided by [Whitebrim](https://www.raftmodding.com/user/Whitebrim).

We will store variables in the newly created class and add an extension for original Raft class. You can read how C\# extensions work [here](https://docs.microsoft.com/dotnet/csharp/programming-guide/classes-and-structs/extension-methods).

If your class has only one instance you can add **static private variable**. If not, you need to add `Dictionary<class, variable>` that will contain variables for each class instance.

In this example we will add new `private bool isLocked` variable to `SteeringWheel` class:

First, we need to **create an extension class** to store and handle variables:

```csharp
public static class SteeringWheelExtension
{
    private static Dictionary<SteeringWheel, bool> isLocked = new Dictionary<SteeringWheel, bool>();

    public static bool IsLocked(this SteeringWheel steeringWheel)
    {
        if (!isLocked.ContainsKey(steeringWheel))
        {
            isLocked.Add(steeringWheel, false); // False is default value for isLocked bool in our case
        }
        return isLocked[steeringWheel];
    }

    public static void Lock(this SteeringWheel steeringWheel, bool value)
    {
        if (!isLocked.ContainsKey(steeringWheel))
        {
            isLocked.Add(steeringWheel, value);
            return;
        }
        isLocked[steeringWheel] = value;
    }
}
```

Then you need to **patch original class**. In our case we need to change displaying text depending on `IsLocked` state. We will patch `OnIsRayed()` methon inside `SteeringWheel` class:

```csharp
[HarmonyPatch(typeof(SteeringWheel), "OnIsRayed")]
class SteeringWheelPatchOnIsRayed
{
    private static void Postfix(SteeringWheel __instance, ref DisplayTextManager ___displayText)
    {
        if (!__instance.IsLocked())
            ___displayText.ShowText("Press to LOCK rotation", MyInput.Keybinds["RMB"].MainKey, 2, 0, false);
        else
            ___displayText.ShowText("Press to UNLOCK rotation", MyInput.Keybinds["RMB"].MainKey, 2, 0, false);

        if (MyInput.GetButtonDown("RMB"))
            __instance.Lock(!__instance.IsLocked()); // Toggle bool
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
        harmonyInstance = HarmonyInstance.Create("com.whitebrim.bettersteeringwheel"); // It's custom patch name, you need to name your patch differently
        harmonyInstance.PatchAll(Assembly.GetExecutingAssembly());
    }

    public void OnModUnload()
    {
        harmonyInstance.UnpatchAll();
        Destroy(gameObject);
    }
}
```

You can access new variable using class instance. `SteeringWheel.IsLocked()`, `SteeringWheel.Lock(bool newValue)`.

Example for **`float`** and **`string`**:

```csharp
public static class CustomExtension
{
    private static Dictionary<YourClass, float> floatVarName = new Dictionary<YourClass, float>();
    private static Dictionary<YourClass, string> stringVarName = new Dictionary<YourClass, string>();

    public static float GetFloatVarName(this YourClass instance)
    {
        if (!floatVarName.ContainsKey(instance))
        {
            floatVarName.Add(instance, 0); // 0 is default value for floatVarName in our case
        }
        return floatVarName[instance];
    }

    public static void SetFloatVarName(this YourClass instance, float value)
    {
        if (!floatVarName.ContainsKey(instance))
        {
            floatVarName.Add(instance, value);
            return;
        }
        floatVarName[instance] = value;
    }

    public static string GetStringVarName(this YourClass instance)
    {
        if (!stringVarName.ContainsKey(instance))
        {
            stringVarName.Add(instance, string.Empty); // string.Empty is default value for stringVarName in our case
        }
        return stringVarName[instance];
    }

    public static void SetStringVarName(this YourClass instance, string value)
    {
        if (!stringVarName.ContainsKey(instance))
        {
            stringVarName.Add(instance, value);
            return;
        }
        stringVarName[instance] = value;
    }
}
```

## Saving private variables

To save private variables you need to patch `RDG_%RaftClassName%` class and method that is invoked inside switch in `SaveAndLoad` class in `RestoreRGDGame(RGD_Game game)` method.

In this example we will use `SteeringWheel` class and save `IsLocked` variable \(from `Adding private variables` section\).

**First**, we have to add new link `RGD Class -> our variable` \(Dictionary\):

```csharp
public static class SteeringWheelExtension
{
    public static Dictionary<RGD_SteeringWheel, bool> RGDisLocked = new Dictionary<RGD_SteeringWheel, bool>();

    public static void AddLockRGD(this RGD_SteeringWheel RGD_SteeringWheel, SteeringWheel steeringWheel) // For saving
    {
        RGD_SteeringWheel.AddLockRGD(steeringWheel.IsLocked());
    }

    public static void AddLockRGD(this RGD_SteeringWheel RGD_SteeringWheel, bool value) // For loading
    {
        RGDisLocked.Add(RGD_SteeringWheel, value);
    }
}
```

**Second**, we need to patch `RGD_SteeringWheel` class. There're two constructors \(one for saving, another for loading\) and `GetObjectData` method \(to control flow of Serialization\).

```csharp
class RGD_SteeringWheelPatch
{
    [HarmonyPatch(typeof(RGD_SteeringWheel), MethodType.Constructor, new Type[]{ typeof(RGDType), typeof(SteeringWheel) })]
    class RGD_SteeringWheelConstructor1 // Constructor for saving
    {
        private static void Prefix(RGD_SteeringWheel __instance, ref SteeringWheel steeringWheel)
        {
            __instance.AddLockRGD(steeringWheel); // Linking this RGD object to variable value
        }
    }

    [HarmonyPatch(typeof(RGD_SteeringWheel), MethodType.Constructor, new Type[] { typeof(SerializationInfo), typeof(StreamingContext) })]
    class RGD_SteeringWheelConstructor2 // Constructor for loading
    {
        private static void Prefix(RGD_SteeringWheel __instance, ref SerializationInfo info)
        {
            foreach (SerializationEntry serializationEntry in info)
            {
                string name = serializationEntry.Name;
                if (name == "IsLocked") // If you'll open this save without mod installed - it'll not crash your game
                    __instance.AddLockRGD((bool)serializationEntry.Value); // Linking this RGD object to loaded value
            }
        }
    }

    [HarmonyPatch(typeof(RGD_SteeringWheel), "GetObjectData")]
    class GetObjectData // Interfering with the serialization flow and adding our custom data to the save
    {
        private static void Postfix(RGD_SteeringWheel __instance, ref SerializationInfo info)
        {
            bool value;
            if (SteeringWheelExtension.RGDisLocked.TryGetValue(__instance, out value))
                info.AddValue("IsLocked", value); // Getting variable value via link, using this RGD object as a key
        }
    }
}
```

**Lastly**, we need to find method that loads saved block data and patch it. This method is invoked inside switch in `SaveAndLoad` class in `RestoreRGDGame(RGD_Game game)` method:

```csharp
switch (rgd.type)
    {
    case RGDType.Block:
    {
      //Some code
      break;
    }
    case RGDType.Block_Door:
    {
      //Some code
      break;
    }
    //etc...
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

In our case method is called `RestoreWheel(RGD_SteeringWheel rgdWheel)`. This method contains instructions on how to deserialize RGD class to retrieve saved data. We will add our custom instructions:

```csharp
[HarmonyPatch(typeof(SteeringWheel), "RestoreWheel")]
class SteeringWheelRestoreWheelPatch
{
    private static void Prefix(SteeringWheel __instance, RGD_SteeringWheel rgdWheel)
    {
        bool value;
        if (SteeringWheelExtension.RGDisLocked.TryGetValue(rgdWheel, out value)) // Using RGD class as a key to get isLocked value, check if save had data about isLocked state
            __instance.Lock(value);
    }
}
```

**Congratulations**, our custom data for Steering Wheel is saving and loading successfully.

