# Execute code on world loading

If you want to execute code only once a player joined a world use this method.  
  
Sometimes you want to execute code only once the player joined a world, maybe because it depends on gameobjects which are only then initialized, or maybe it simply doesn't need to be executed sooner. This is the way to go about.

```csharp
public override void WorldEvent_WorldLoaded()
{
    Debug.Log("The world is loaded!");
}
```

