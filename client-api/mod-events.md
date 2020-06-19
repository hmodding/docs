---
description: >-
  Mod Events makes your life easier! it allows you to run code when something
  specific happens in the game!
---

# Mod Events

_**How to use them?**  
Its simple! In your mod class simply override them like shown below._

```csharp
public override void WorldEvent_WorldLoaded()
{
    Debug.Log("The world is loaded!");
}
```

**Mod Events List**

| Method | Description |
| :--- | :--- |
| **`void WorldEvent_WorldLoaded()`** | Called when the world is loaded. |
| **`void WorldEvent_WorldSaved()`** | Called when the world is saved. |
| **`void LocalPlayerEvent_Hurt(float damage, Vector3 hitPoint, Vector3 hitNormal, EntityType damageInflictorEntityType)`** | Called when the local player gets hurt. |
| **`void LocalPlayerEvent_Death(Vector3 deathPosition)`** | Called when the local player die. |
| **`void LocalPlayerEvent_Respawn()`** | Called when the local player respawn. |
| **`void LocalPlayerEvent_ItemCrafted(Item_Base item)`** | Called when the local player craft an item. |
| **`void LocalPlayerEvent_PickupItem(PickupItem item)`** | Called when the local player pickup an item. |
| **`void LocalPlayerEvent_DropItem(ItemInstance item, Vector3 position, Vector3 direction, bool parentedToRaft)`** | Called when the local player drop an item. |

