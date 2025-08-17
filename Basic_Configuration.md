# Basic Configuration

## 📋 Configuration File Overview

The leaderboard system uses a single JSON configuration file located at:
```
$profile:\FoxBox_mods\FoxBoxLeaderboard\FoxBoxLeaderboardConfig.json
```

## 🏗️ Basic Structure

### Minimal Configuration
```json
{
    "Leaderboards": [
        {
            "id": "zombieKills",
            "label": "Zombie Kills",
            "entityScores": [
                {"entityType": "Zmb", "scoreValue": 1}
            ],
            "rewardThresholds": [10, 25, 50],
            "deathPenalty": 0.5,
            "resetKillsOnDeath": 1,
            "resetRewardsOnDeath": 0,
            "rewards": []
        }
    ],
    "debugMode": 0
}
```

## ⚙️ Configuration Parameters

### Leaderboard Properties

#### **Required Fields**

| Field | Type | Description | Example |
|-------|------|-------------|---------|
| `id` | String | Unique identifier | `"zombieKills"` |
| `label` | String | Display name | `"Zombie Kills"` |
| `entityScores` | Array | What to track and point values | `[...]` |
| `rewardThresholds` | Array | Kill counts for rewards | `[10, 25, 50]` |
| `deathPenalty` | Number | Score lost on death (0.0-1.0) | `0.5` |
| `resetKillsOnDeath` | Number | Reset kills on death (0/1) | `1` |
| `resetRewardsOnDeath` | Number | Reset rewards on death (0/1) | `0` |

#### **Optional Fields**

| Field | Type | Description | Default |
|-------|------|-------------|---------|
| `rewards` | Array | Reward configurations | `[]` |

### Entity Scoring Configuration

#### **Standard Entity Score Object**
```json
{
    "entityType": "Zmb",     // Entity identifier
    "scoreValue": 1          // Points awarded per kill
}
```

#### **Enhanced Entity Score Object (with Vehicle Kills)**
```json
{
    "entityType": "Animal_UrsusArctos",
    "scoreValue": 5,              // Points for weapon kills
    "enableVehicleKills": true,   // Allow vehicle kills for this entity
    "vehicleKillMultiplier": 2.0  // Multiplier for vehicle kills (5 × 2.0 = 10 points)
}
```

#### **Entity Type Examples**
```json
"entityScores": [
    {"entityType": "Zmb", "scoreValue": 1},                    // All zombies
    {"entityType": "ZmbM_SoldierNormal", "scoreValue": 3},     // Specific zombie type
    {"entityType": "Animal_", "scoreValue": 2},                // All animals
    {
        "entityType": "Animal_UrsusArctos", 
        "scoreValue": 5,
        "enableVehicleKills": true,
        "vehicleKillMultiplier": 2.0
    },                                                          // Bears with vehicle kill bonus
    {"entityType": "FoxLB_PVP_PlayerKill", "scoreValue": 10}   // Player kills
]
```

#### **Vehicle Kill Properties**

| Field | Type | Description | Default |
|-------|------|-------------|---------|
| `enableVehicleKills` | Boolean | Allow vehicle kills for this entity | `false` |
| `vehicleKillMultiplier` | Number | Score multiplier for vehicle kills | `1.0` |

**Score Calculation**:
- **Weapon Kill Score** = `scoreValue`
- **Vehicle Kill Score** = `scoreValue × vehicleKillMultiplier` (if enabled)

**Vehicle Kill Examples**:
```json
// Large animals - higher reward for vehicle kills
{
    "entityType": "Animal_UrsusArctos",
    "scoreValue": 5,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 2.0  // Vehicle: 10 points, Weapon: 5 points
}

// Small animals - vehicle kills disabled
{
    "entityType": "Animal_GallusGallusDomesticus", 
    "scoreValue": 1,
    "enableVehicleKills": false   // Only weapon kills count
}

// PVP - lower reward for vehicle kills
{
    "entityType": "FoxLB_PVP_PlayerKill",
    "scoreValue": 10,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 0.5  // Vehicle: 5 points, Weapon: 10 points
}
```

#### **Entity Matching Rules**
- **Partial matching**: `"Zmb"` matches `"ZmbM_SoldierNormal"`
- **First match wins**: Order matters in the array
- **Case sensitive**: Must match exact capitalization
- **Special types**: Use constants like `"FoxLB_PVP_PlayerKill"`
- **Vehicle kill detection**: Automatically detects vehicle vs weapon kills
- **Backward compatibility**: Missing vehicle properties default to safe values

#### **Kill Attribution System**
The mod features **enhanced kill tracking** that handles DayZ's complex event timing:
- **Robust event handling**: Fixes timing issues between EEHitBy and EEKilled events
- **Posthumous kill detection**: Handles one-shot kills where events fire out of order
- **Vehicle kill detection**: Analyzes damage source chain for vehicle involvement
- **Double-counting prevention**: Boolean flags prevent duplicate kill records
- **Attack timeout system**: 60-second attribution window for bleeding deaths

### Death Penalties

#### **Death Penalty Value (0.0 - 1.0)**
- `0.0` = No penalty (keep 100% of score)
- `0.25` = Lose 25% of score (keep 75%)
- `0.5` = Lose 50% of score (keep 50%)
- `1.0` = Lose all score (keep 0%)

#### **Reset Options (0 or 1)**
- `resetKillsOnDeath: 1` = Kill count resets to 0 on death
- `resetKillsOnDeath: 0` = Kill count preserved through death
- `resetRewardsOnDeath: 1` = Unclaimed rewards reset on death
- `resetRewardsOnDeath: 0` = Unclaimed rewards preserved

#### **Exploit Prevention**
The system automatically prevents reward farming:
```json
// If this configuration is detected:
{
    "resetRewardsOnDeath": 1,
    "resetKillsOnDeath": 0
}
// System automatically forces: resetKillsOnDeath = 1
```

### Reward Configuration

#### **Reward Threshold Object**
```json
{
    "threshold": 10,    // Kill count required
    "items": [          // Items to award
        {
            "className": "BandageDressing",
            "quantity": 2,
            "stackSize": -1
        }
    ]
}
```

#### **Reward Item Object**
```json
{
    "className": "BandageDressing",  // DayZ item class name
    "quantity": 2,                   // How many to give
    "stackSize": -1                  // Stack size (-1 = default)
}
```

## 🎯 Common Configuration Patterns

### PVE Focused (Zombies & Animals)
```json
{
    "id": "pveKills",
    "label": "PVE Master",
    "entityScores": [
        {"entityType": "Zmb", "scoreValue": 1},
        {"entityType": "ZmbM_Soldier", "scoreValue": 3},
        {
            "entityType": "Animal_UrsusArctos",
            "scoreValue": 8,
            "enableVehicleKills": true,
            "vehicleKillMultiplier": 1.5
        }
    ],
    "rewardThresholds": [25, 50, 100],
    "deathPenalty": 0.4,
    "resetKillsOnDeath": 0,
    "resetRewardsOnDeath": 0
}
```

### PVP Focused (Player Kills with Vehicle Detection)
```json
{
    "id": "pvpKills", 
    "label": "PVP Warrior",
    "entityScores": [
        {
            "entityType": "FoxLB_PVP_PlayerKill", 
            "scoreValue": 15,
            "enableVehicleKills": true,
            "vehicleKillMultiplier": 0.6
        }
    ],
    "rewardThresholds": [3, 5, 10],
    "deathPenalty": 0.25,
    "resetKillsOnDeath": 1,
    "resetRewardsOnDeath": 1
}
```

### Animal Hunter (Vehicle Kill Variety)
```json
{
    "id": "animalHunter",
    "label": "Animal Hunter",
    "entityScores": [
        {
            "entityType": "Animal_UrsusArctos",
            "scoreValue": 5,
            "enableVehicleKills": true,
            "vehicleKillMultiplier": 2.0
        },
        {
            "entityType": "Animal_BosTaurus",
            "scoreValue": 3,
            "enableVehicleKills": true, 
            "vehicleKillMultiplier": 1.5
        },
        {
            "entityType": "Animal_GallusGallusDomesticus",
            "scoreValue": 1,
            "enableVehicleKills": false
        }
    ],
    "rewardThresholds": [20, 50, 100],
    "deathPenalty": 0.1,
    "resetKillsOnDeath": 0,
    "resetRewardsOnDeath": 0
}
```

## 🔢 Value Guidelines

### Score Values
- **Regular zombies**: 1-2 points
- **Military zombies**: 3-6 points
- **Special zombies**: 8-15 points
- **Small animals**: 1-3 points
- **Large animals**: 8-25 points
- **Player kills**: 10-50 points

### Death Penalties
- **Casual servers**: 0.1-0.3 (10-30% loss)
- **Balanced servers**: 0.3-0.5 (30-50% loss)
- **Hardcore servers**: 0.5-0.8 (50-80% loss)

### Reward Thresholds
- **Frequent rewards**: Every 5-10 kills
- **Balanced rewards**: Every 15-25 kills
- **Rare rewards**: Every 50+ kills

## 🛠️ Configuration Tips

### Testing Your Config
1. **Start simple**: Begin with basic zombie tracking
2. **Enable debug**: Use `"debugMode": 1` during setup
3. **Test incrementally**: Add complexity gradually
4. **Validate JSON**: Use online JSON validators

### Entity Type Discovery
To find entity class names:
1. **Enable debug mode**
2. **Kill various entities**
3. **Check server logs** for entity type names
4. **Use those exact names** in configuration

### Balancing Guidelines
1. **Score ratios**: Harder kills should be worth 2-5x more
2. **Threshold spacing**: Increase gaps at higher levels
3. **Death penalties**: Match your server's difficulty level
4. **Reward value**: Valuable but not game-breaking

## 🔍 Validation

### JSON Syntax Check
Common syntax errors:
```json
{
    "rewardThresholds": [10, 25, 50],  // ✓ Comma after array
    "deathPenalty": 0.5,               // ✓ Decimal number
    "resetKillsOnDeath": 1             // ✓ No comma on last item
}
```

### Required Field Check
Every leaderboard must have:
- ✅ Unique `id`
- ✅ Display `label`
- ✅ At least one `entityScore`
- ✅ At least one `rewardThreshold`
- ✅ Valid `deathPenalty` (0.0-1.0)
- ✅ Boolean values as numbers (0/1)

### Logic Validation
- ✅ `rewardThresholds` in ascending order
- ✅ `deathPenalty` between 0.0 and 1.0
- ✅ No duplicate leaderboard `id` values
- ✅ All reward item `className` values exist in DayZ

## 📄 Global Settings

### Debug Mode
```json
{
    "debugMode": 1  // Enable (1) or disable (0) debug logging
}
```

**Debug mode effects**:
- Detailed kill detection logging
- Entity matching process details
- Score calculation information
- Reward system debugging
- Performance impact (disable for production)

---

**Next Steps**:
- See Configuration Examples documentation for complete working setups
- Review Entity Scoring System documentation for advanced entity configuration
- Check Reward System Setup documentation for detailed reward configuration
