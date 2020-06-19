# Dropping/Spawning items

To drop/spawn an item we use the `DropItem` method from the `Helper` class.

```csharp
Network_Player player = RAPI.getLocalPlayer();
Item_Base item = ItemManager.GetItemByName("Raw_Potato");
Helper.DropItem(new ItemInstance(item, 1, item.MaxUses), player.transform.position, player.CameraTransform.forward,player.transform.ParentedToRaft());
```

