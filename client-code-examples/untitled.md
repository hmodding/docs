# Reading private variables

Reading private variables is clearly necessary when modding raft. Let's see how easy it is!

To access private variables we need to use **Harmony**. You can visit the Harmony wiki by clicking [here](https://github.com/pardeike/Harmony/wiki).  
_`Harmony is a library for patching, replacing and decorating .NET and Mono methods during runtime.`_  
  
To use harmony you simply need to add the `Harmony` namespace by adding `using Harmony;` at the top of your class.

  
To access a **non-static private variable** you use Traverse.Create\(\) with the object instance and .Field\(\) with the field name.

```csharp
string myvalue = Traverse.Create(ScriptInstance).Field("fieldname").GetValue() as string;

// For example to get the value "stats" of the class Network_Player we can do that :
PlayerStats stats = Traverse.Create(RAPI.getLocalPlayer()).Field("stats").GetValue() as PlayerStats;
```

  
To access a **static private variable** you also use Traverse.Create\(\) but with the class type and .Field\(\) with the field name.

```csharp
string myvalue = Traverse.Create(typeof(classname)).Field("fieldname").GetValue() as string;

// For example to get the value "allAvailableItems" of the class ItemManager we can do that :
List<Item_Base> allAvailableItems = Traverse.Create(typeof(ItemManager)).Field("allAvailableItems").GetValue() as List<Item_Base>;
```



