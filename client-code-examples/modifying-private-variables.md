# Modifying private variables

Modifying private variables is clearly necessary when modding raft. Let's see how easy it is!

To modify private variables we need to use **Harmony**. You can visit the Harmony wiki by clicking [here](https://github.com/pardeike/Harmony/wiki).  
_`Harmony is a library for patching, replacing and decorating .NET and Mono methods during runtime.`_  
  
To use harmony you simply need to add the `HarmonyLib` namespace by adding `using HarmonyLib;` at the top of your class.

  
To modify a **non-static private variable** you use Traverse.Create\(\) with the object instance, .Field\(\) with the field name and .SetValue\(\)

```csharp
Traverse.Create(ScriptInstance).Field("fieldname").SetValue(newvalue);

// For example to set the value "stats" of the class Network_Player we can do that :
Traverse.Create(RAPI.getLocalPlayer()).Field("stats").SetValue(mynewstats);
```

  
To modify a **static private variable** you also use Traverse.Create\(\) but with the class type, .Field\(\) with the field name and also .SetValue\(\);

```csharp
Traverse.Create(typeof(classname)).Field("fieldname").SetValue(newvalue);

// For example to set the value "allAvailableItems" of the class ItemManager we can do that :
Traverse.Create(typeof(ItemManager)).Field("allAvailableItems").SetValue(mynewitemlist);
```



