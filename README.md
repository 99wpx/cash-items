# Cash as Item Integration for QBCore Framework

This guide will help you configure cash to be used as an item in your QBCore-based FiveM server. Follow the steps below to implement this feature.

---

## Step 1: Update `qb-core/config.lua`

Add the following configuration settings to `qb-core/config.lua`:

```lua
QBConfig.UseMoneyAsItem = true
QBConfig.MoneyItem = "cash"
```

---

## Step 2: Modify `qb-core/server/player.lua`

### Add Money Logic
Find the function `self.Functions.AddMoney` and locate the line:

```lua
if not self.Offline then
```

Add the following code above it:

```lua
if QBCore.Config.UseMoneyAsItem then
    if self.Functions.GetItemByName(QBCore.Config.MoneyItem) ~= nil then
        self.Functions.RemoveItem(QBCore.Config.MoneyItem, self.PlayerData.money[moneytype] - amount, nil)
        self.Functions.AddItem(QBCore.Config.MoneyItem, self.PlayerData.money[moneytype], nil)
    end
end
```

### Remove Money Logic
Find the function `self.Functions.RemoveMoney` and locate the line:

```lua
if not self.Offline then
```

Add the following code above it:

```lua
if QBCore.Config.UseMoneyAsItem then
    if self.Functions.GetItemByName(QBCore.Config.MoneyItem) ~= nil then
        self.Functions.RemoveItem(QBCore.Config.MoneyItem, self.PlayerData.money[moneytype] + amount, nil)
        self.Functions.AddItem(QBCore.Config.MoneyItem, self.PlayerData.money[moneytype], nil)
    end
end
```

### Set Money Logic
Find the function `self.Functions.SetMoney` and locate the line:

```lua
if not self.Offline then
```

Add the following code above it:

```lua
if QBCore.Config.UseMoneyAsItem then
    if self.Functions.GetItemByName(QBCore.Config.MoneyItem) ~= nil then
        self.Functions.RemoveItem(QBCore.Config.MoneyItem, self.PlayerData.money[moneytype], nil)
        self.Functions.AddItem(QBCore.Config.MoneyItem, amount, nil)
    end
end
```

---

## Step 3: Update `qb-core/shared/items.lua`

Add the following entry for the cash item in `qb-core/shared/items.lua`:

```lua
['cash'] = {
    ['name'] = 'cash',
    ['label'] = 'Cash',
    ['weight'] = 0,
    ['type'] = 'item',
    ['image'] = 'cash.png',
    ['unique'] = false,
    ['useable'] = false,
    ['shouldClose'] = true,
    ['combinable'] = nil,
    ['description'] = 'Physical cash that can be carried around.'
}
```

Ensure you place the image `cash.png` in your inventory images folder.

---
