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
| **`void LocalPlayerEvent_Death(Vector3 deathPosition)`** | Called when the local player dies. |
| **`void LocalPlayerEvent_Respawn()`** | Called when the local player respawns. |
| **`void LocalPlayerEvent_ItemCrafted(Item_Base item)`** | Called when the local player crafts an item. |
| **`void LocalPlayerEvent_PickupItem(PickupItem item)`** | Called when the local player picks up an item. |
| **`void LocalPlayerEvent_DropItem(ItemInstance item, Vector3 position, Vector3 direction, bool parentedToRaft)`** | Called when the local player drops an item. |
| **`void LocalPlayerEvent_OnRespawn()`** | Called when the local player respawn. |
| **`void WorldEvent_OnPlayerConnected(CSteamID steamid, RGD_Settings_Character characterSettings)`** | Called when a player join the world. |
| **`void WorldEvent_OnPlayerDisconnected(CSteamID steamid, DisconnectReason disconnectReason)`** | Called when a player leave the world. |
| **`void WorldEvent_WorldUnloaded()`** | Called when the world is unloaded \(Getting back to main menu\). |

