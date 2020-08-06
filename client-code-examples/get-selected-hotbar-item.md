# Get selected hotbar item

To get the current item that the player has selected we use the `GetSelectedHotbarItem` method from the `PlayerInventory` class.

```csharp
ItemInstance currentItem = RAPI.GetLocalPlayer().Inventory.GetSelectedHotbarItem();
RConsole.Log("Current Item : " + currentItem.settings_Inventory.DisplayName);
```

