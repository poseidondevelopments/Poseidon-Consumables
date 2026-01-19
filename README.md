# Poseidon-Consumables

A complete consumables system for FiveM/QBCore with export support, progress circles, screen effects, and comprehensive effect management.

---

## 🚀 Quick Start

1. **Install Dependencies**
   - `ox_lib` (required)
   - `qb-core` (required)

2. **Install Resource**
   ```bash
   # Place in your resources folder
   # Rename folder to: poseidon_consumables
   ```

3. **Add to server.cfg**
   ```
   ensure ox_lib
   ensure poseidon_consumables
   ```

4. **Add Item to Inventory**
   ```lua
   ['your_item'] = {
       label = 'Your Item',
       weight = 100,
       stack = true,
       close = true,
       description = 'Item description',
       client = {
           export = 'poseidon_consumables.useConsumable',
       }
   }
   ```

5. **Add Consumable Config**
   ```lua
   -- In config.lua
   Config.Consumables = {
       ['your_item'] = {
           category = 'joint',
           duration = 4500,
           anim = 'smoke',
           label = "Using Item",
           -- Add effects below
       }
   }
   ```

---

## 📋 Features

- **Export-based** - Simple inventory integration
- **Progress Circles** - ox_lib visual feedback
- **Screen Effects** - Customizable visual effects
- **Multiple Effects** - Armor, speed, stamina, health regen
- **Animations** - Smoke, eat, drink with props
- **Cooldowns** - Prevents spam
- **Auto-cleanup** - Memory leak prevention
- **Dynamic Registration** - Add items at runtime

---

## ⚙️ Configuration

### Consumable Structure

```lua
Config.Consumables = {
    ['item_name'] = {
        -- REQUIRED
        category = 'joint',        -- joint | edible | drink | baked
        duration = 4500,           -- Effect duration (milliseconds)
        anim = 'smoke',            -- smoke | eat | drink
        label = "Using Item",      -- Progress circle label
        
        -- OPTIONAL EFFECTS
        armor = {add = 15, cap = 100},      -- Armor boost
        speedMultiplier = 1.15,            -- Speed multiplier
        staminaBoost = true,                -- Infinite stamina
        healthRegen = true,                 -- Health regeneration
        screenEffect = "DrugsMichaelAliensFight",  -- Screen effect
        
        -- OPTIONAL
        cooldown = 60              -- Cooldown in seconds
    }
}
```

### Available Options

**Categories:**
- `'joint'` - 6 second animation
- `'edible'` - 3 second animation
- `'drink'` - 3 second animation
- `'baked'` - 3 second animation

**Screen Effects:**
- `"DrugsMichaelAliensFight"`
- `"DrugsTrevorClownsFight"`
- `"DrugsDrivingIn"`
- `"DrugsDrivingOut"`
- `"MenuMGSelectionTint"`

**Animations:**
- `'smoke'` - Smoking with cigarette prop
- `'eat'` - Eating animation
- `'drink'` - Drinking with bottle prop

---

## 📖 Usage

### Basic Setup

**1. Add to config.lua:**
```lua
Config.Consumables = {
    ['your_item'] = {
        category = 'your_item',
        duration = 4500,
        armor = {add = 15, cap = 100},
        speedMultiplier = 1.15,
        screenEffect = "DrugsMichaelAliensFight",
        anim = 'smoke',
        label = "Smoking Joint",
        cooldown = 60
    }
}
```

**2. Add to items.lua:**
```lua
['your_item'] = {
    label = 'your_item',
    weight = 5,
    stack = true,
    close = true,
    description = 'A sweet joint',
    client = {
        export = 'poseidon_consumables.useConsumable',
    }
}
```

### How It Works

1. Player uses item → Progress circle appears
2. Animation plays (3s for most, 6s for joints)
3. Progress completes → All effects activate
4. Effects run for `duration` from config
5. Effects auto-expire after duration
6. Cooldown prevents immediate reuse

---

## 🔧 Exports

### Client Exports

```lua
-- Use a consumable
exports['poseidon_consumables']:useConsumable('item_name')

-- Register new consumable dynamically
exports['poseidon_consumables']:RegisterConsumable('item_name', {
    category = 'joint',
    duration = 4500,
    -- ... config
})

-- Check if item is consumable
local isConsumable = exports['poseidon_consumables']:IsConsumable('item_name')

-- Get consumable data
local data = exports['poseidon_consumables']:GetConsumableData('item_name')
```

### Server Exports

```lua
-- Register consumable from server
exports['poseidon_consumables']:RegisterConsumable('item_name', consumableData)
```

---

## 💡 Effects Explained

### Armor Boost
```lua
armor = {add = 15, cap = 100}  -- Adds 15 armor, max 100
```

### Speed Multiplier
```lua
speedMultiplier = 1.15  -- 15% faster movement
```

### Stamina Boost
```lua
staminaBoost = true  -- Infinite stamina while active
```

### Health Regeneration
```lua
healthRegen = true  -- Regenerates 1 HP per second
```

### Screen Effect
```lua
screenEffect = "DrugsMichaelAliensFight"  -- Visual post-processing
```

---

## 📦 Available Screen Effects

Use these effect names in your `screenEffect` config:

- `DrugsMichaelAliensFight`
- `DrugsTrevorClownsFight`
- `DrugsDrivingIn`
- `DrugsDrivingOut`
- `MenuMGSelectionTint`

---

## 🎯 Examples

### Joint Example
```lua
-- config.lua
['your_item'] = {
    category = 'joint',
    duration = 4500,
    armor = {add = 15, cap = 100},
    speedMultiplier = 1.15,
    screenEffect = "DrugsMichaelAliensFight",
    anim = 'smoke',
    label = "Smoking Joint",
    cooldown = 60
}

-- items.lua
['your_item'] = {
    label = 'your_item',
    weight = 5,
    stack = true,
    close = true,
    description = 'A sweet joint',
    client = {
        export = 'poseidon_consumables.useConsumable',
    }
}
```

### Edible Example
```lua
-- config.lua
['your_item'] = {
    category = 'edible',
    duration = 7500,
    armor = {add = 40, cap = 100},
    speedMultiplier = 1.1,
    screenEffect = "DrugsMichaelAliensFight",
    anim = 'eat',
    label = "Eating Gummies",
    cooldown = 120
}

-- items.lua
['your_item'] = {
    label = 'your_item',
    weight = 10,
    stack = true,
    close = true,
    description = 'Delicious gummies',
    client = {
        export = 'poseidon_consumables.useConsumable',
    }
}
```

---

## ⚠️ Important Notes

- **Resource Name**: Export prefix matches folder name (`poseidon_consumables`)
- **Animation Duration**: Joints = 6s, Others = 3s
- **Effect Duration**: Uses `duration` from config (starts after animation)
- **Cooldowns**: Automatically cleaned up every minute
- **Memory**: Full cleanup on resource stop/disconnect

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Screen effect not showing | Check effect name spelling, verify it's in ScreenEffects mapping |
| Animation not playing | Verify animation type (`smoke`, `eat`, `drink`) and dictionary exists |
| Progress circle missing | Ensure `ox_lib` is installed and started before this resource |
| Effects not applying | Verify item name matches exactly in config and items.lua |
| Export not working | Check resource folder name matches export prefix |

---

## 📝 Events

### Client Event
```lua
TriggerEvent('consumables:useItem', 'item_name')
```

### Server Event
```lua
TriggerServerEvent('consumables:server:useItem', 'item_name')
```

---

## 🔒 Memory Management

The system includes automatic cleanup:
- ✅ Threads tracked and terminated
- ✅ Animation dictionaries unloaded
- ✅ Prop models released
- ✅ Screen effects stopped
- ✅ Caches cleared
- ✅ Cooldowns cleaned

---

## 📞 Support

Check console for errors, verify dependencies are installed, and ensure resource folder name matches export calls.

---

**Developed by Poseidon Developments**
