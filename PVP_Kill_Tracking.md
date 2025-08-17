# PVP Kill Tracking

## 🎯 Overview
Complete guide to setting up and configuring Player vs Player (PVP) kill tracking in the FoxBox Leaderboards system.

## 🔧 Basic PVP Setup

### Special Entity Type
PVP kills use a special entity type identifier:

```json
{
    "id": "pvpKills",
    "label": "Player Kills (PVP)",
    "entityScores": [
        {
            "entityType": "FoxLB_PVP_PlayerKill",
            "scoreValue": 10,
            "enableVehicleKills": true,
            "vehicleKillMultiplier": 1.0
        }
    ]
}
```

**Important**: Use exactly `"FoxLB_PVP_PlayerKill"` - this is the current system constant for player kills.

> **Legacy Support**: The old `"PVP_PLAYER_KILL"` identifier is still supported but deprecated. Use `"FoxLB_PVP_PlayerKill"` for new configurations.

## ⚙️ PVP Configuration Options

### Recommended PVP Settings
```json
{
    "id": "pvpKills",
    "label": "Player Kills (PVP)",
    "entityScores": [
        {
            "entityType": "FoxLB_PVP_PlayerKill",
            "scoreValue": 10,
            "enableVehicleKills": true,
            "vehicleKillMultiplier": 0.8
        }
    ],
    "rewardThresholds": [3, 5, 10, 20],
    "deathPenalty": 0.25,
    "resetKillsOnDeath": 1,
    "resetRewardsOnDeath": 1,
    "rewards": [...]
}
```

### Configuration Explanation

#### **Score Value (10 points)**
- **High value** reflects difficulty of player kills
- **Recommended range**: 5-20 points per kill
- **Balance consideration**: Much higher than zombie kills (1-6 points)

#### **Death Penalty (0.25)**
- **Lower than PVE** penalties (typically 0.5)
- **25% score loss** on death (keeps 75%)
- **Competitive balance**: Not too harsh for PVP environment

#### **Reset Settings**
- **`resetKillsOnDeath: 1`**: Prevents exploit abuse
- **`resetRewardsOnDeath: 1`**: Maintains risk/reward balance
- **System protection**: Auto-prevents misconfigurations

## 🔍 How PVP Detection Works

### Automatic Kill Detection
The system automatically detects PVP kills through:

#### **Direct Weapon Kills**
- Firearms (pistols, rifles, etc.)
- Melee weapons (knives, axes, etc.)
- Thrown weapons (grenades, etc.)

#### **Indirect Kills**
- Vehicle strikes (running over players)
- Explosion kills (landmines, grenades)  
- Environmental kills caused by players

#### **Vehicle Kill Detection**
The system can differentiate between:
- **Weapon kills**: Traditional firearm/melee weapon kills
- **Vehicle kills**: Car/truck impacts, explosions from vehicles
- **Kill method tracking**: Each method can have different score multipliers

#### **Smart Attribution**
- **Killer identification**: Uses `LeaderboardUtils.ResolveAttacker()`
- **Self-kill prevention**: Suicide doesn't count as PVP
- **Weapon tracing**: Properly attributes kills through held weapons

### Kill Detection Examples
```
[FoxBoxLeaderboard] PVP Kill detected - Attacker: PlayerA (SteamID) killed Victim: PlayerB
[FoxBoxLeaderboard] Kill method detected: WEAPON_KILL (multiplier: 1.0)
[FoxBoxLeaderboard] *** KILL REGISTERED *** for pvpKills - Kills: 2 -> 3, Score: 20 -> 30 (+10)
```

**Vehicle Kill Example:**
```
[FoxBoxLeaderboard] PVP Kill detected - Attacker: PlayerA (SteamID) killed Victim: PlayerB  
[FoxBoxLeaderboard] Kill method detected: VEHICLE_KILL (multiplier: 0.8)
[FoxBoxLeaderboard] *** KILL REGISTERED *** for pvpKills - Kills: 2 -> 3, Score: 20 -> 28 (+8)
```

## 🎁 PVP Reward Examples

### Early PVP Rewards (2-3 kills)
```json
{
    "threshold": 3,
    "items": [
        {"className": "Ammo_556x45", "quantity": 60, "stackSize": 30},
        {"className": "Morphine", "quantity": 1, "stackSize": -1},
        {"className": "BandageDressing", "quantity": 2, "stackSize": -1}
    ]
}
```

### Mid-Tier PVP Rewards (5-10 kills)
```json
{
    "threshold": 5,
    "items": [
        {"className": "M4_Suppressor", "quantity": 1, "stackSize": -1},
        {"className": "AmmoBox_9x19_25rnd", "quantity": 2, "stackSize": -1},
        {"className": "Morphine", "quantity": 2, "stackSize": -1}
    ]
}
```

### High-Tier PVP Rewards (10+ kills)
```json
{
    "threshold": 10,
    "items": [
        {"className": "ACOGOptic", "quantity": 1, "stackSize": -1},
        {"className": "M4_RISHndgrd", "quantity": 1, "stackSize": -1},
        {"className": "AmmoBox_556x45_20Rnd", "quantity": 3, "stackSize": -1},
        {"className": "Morphine", "quantity": 3, "stackSize": -1}
    ]
}
```

## 🛡️ Exploit Prevention

### Built-in Protection
The system includes automatic exploit prevention:

```cpp
// Automatic exploit prevention in tracker
if (def.resetRewardsOnDeath && !def.resetKillsOnDeath) {
    Print("[FoxBoxLeaderboard] WARNING: Preventing exploit - forcing kill reset");
    killCounts.Set(def.id, 0);  // Force reset to prevent farming
}
```

### The Exploit (Prevented)
Without protection, players could:
1. Get 5 PVP kills
2. Claim rewards  
3. Die (rewards reset, kills remain)
4. Immediately reclaim same rewards
5. Repeat infinitely

### The Solution
- **Automatic detection** of misconfiguration
- **Force kill reset** when rewards reset
- **Warning logged** for admin awareness
- **Exploit impossible** regardless of config

## 🎮 Server Configuration Styles

### Competitive PVP
```json
{
    "scoreValue": 15,
    "deathPenalty": 0.2,
    "resetKillsOnDeath": 1,
    "resetRewardsOnDeath": 1,
    "rewardThresholds": [2, 5, 10, 25]
}
```
- **High rewards** for skilled play
- **Low penalties** to encourage engagement
- **Frequent thresholds** for progression

### Hardcore PVP
```json
{
    "scoreValue": 25,
    "deathPenalty": 0.1,
    "resetKillsOnDeath": 1,
    "resetRewardsOnDeath": 0,
    "rewardThresholds": [5, 15, 50]
}
```
- **Very high scores** per kill
- **Minimal death penalty** (10%)
- **Keep rewards** through death
- **High thresholds** for exclusive rewards

## 🚗 PVP Vehicle Kill Configuration

### Vehicle Kill Settings
Configure how vehicle kills are handled differently from weapon kills:

```json
{
    "entityType": "FoxLB_PVP_PlayerKill",
    "scoreValue": 20,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 0.6
}
```

### Vehicle Kill Strategies

#### **Balanced Approach (Recommended)**
```json
{
    "vehicleKillMultiplier": 0.7
}
```
- **Weapon kill**: 20 points (full value)
- **Vehicle kill**: 14 points (reduced skill requirement)
- **Promotes weapon skill** while allowing vehicle tactics

#### **Vehicle Discouraged**
```json
{
    "vehicleKillMultiplier": 0.3
}
```  
- **Weapon kill**: 20 points
- **Vehicle kill**: 6 points (heavily reduced)
- **Strongly favors** traditional PVP combat

#### **Vehicle Encouraged**
```json
{
    "vehicleKillMultiplier": 1.5
}
```
- **Weapon kill**: 20 points
- **Vehicle kill**: 30 points (bonus)
- **Rewards creative** tactical gameplay

#### **No Vehicle Kills**
```json
{
    "enableVehicleKills": false
}
```
- **Vehicle kills ignored** completely
- **Only weapon kills** count toward leaderboards
- **Pure combat focus**

### PVP Vehicle Kill Examples

#### **Competitive Server** (Weapon Focus)
```json
{
    "entityType": "FoxLB_PVP_PlayerKill",
    "scoreValue": 25,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 0.4
}
```
- Encourages skilled weapon combat
- Vehicle kills still possible but less rewarding
- Maintains competitive integrity

#### **Tactical Server** (Mixed Combat)
```json
{
    "entityType": "FoxLB_PVP_PlayerKill", 
    "scoreValue": 15,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 1.2
}
```
- Rewards tactical vehicle use
- Slightly favors vehicle creativity
- Balanced risk/reward system

#### **Chaos Server** (Vehicle Mayhem)
```json
{
    "entityType": "FoxLB_PVP_PlayerKill",
    "scoreValue": 10,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 2.0
}
```
- Big bonuses for vehicle kills
- Encourages chaotic vehicle combat
- Fun-focused gameplay style

### Casual PVP
```json
{
    "scoreValue": 8,
    "deathPenalty": 0.4,
    "resetKillsOnDeath": 1,
    "resetRewardsOnDeath": 1,
    "rewardThresholds": [1, 3, 5, 10]
}
```
- **Moderate scores** per kill
- **Higher penalties** to reduce griefing
- **Low thresholds** for quick rewards

## 🔄 Mixed PVP/PVE Setup

### Combined Leaderboard
Track both PVP and PVE kills in one leaderboard:

```json
{
    "id": "combatMaster",
    "label": "Combat Master", 
    "entityScores": [
        {"entityType": "Zmb", "scoreValue": 1},
        {"entityType": "ZmbM_Soldier", "scoreValue": 3},
        {"entityType": "FoxLB_PVP_PlayerKill", "scoreValue": 20},
        {"entityType": "Animal_UrsusArctos", "scoreValue": 15}
    ],
    "rewardThresholds": [25, 60, 150],
    "deathPenalty": 0.3,
    "resetKillsOnDeath": 1,
    "resetRewardsOnDeath": 1
}
```

### Separate Leaderboards
Run dedicated PVP and PVE leaderboards:

```json
{
    "Leaderboards": [
        {
            "id": "pvpWarrior",
            "label": "PVP Warrior",
            "entityScores": [
                {"entityType": "FoxLB_PVP_PlayerKill", "scoreValue": 15}
            ]
        },
        {
            "id": "zombieHunter", 
            "label": "Zombie Hunter",
            "entityScores": [
                {"entityType": "Zmb", "scoreValue": 1}
            ]
        }
    ]
}
```

## 🐛 Troubleshooting PVP

### PVP Kills Not Registering
1. **Check entity type**: Must be exactly `"FoxLB_PVP_PlayerKill"` (or legacy `"PVP_PLAYER_KILL"`)
2. **Verify leaderboard config**: Ensure PVP leaderboard exists
3. **Check server logs**: Look for kill detection messages
4. **Test kill scenarios**: Try different weapons/methods
5. **Vehicle kill settings**: Verify `enableVehicleKills` if testing vehicle kills

### Wrong Kill Attribution
1. **Review attacker resolution**: Check `LeaderboardUtils.ResolveAttackerWithMethod()`
2. **Vehicle kills**: Ensure driver attribution works properly
3. **Explosion kills**: Verify grenade/mine attribution
4. **Weapon kills**: Check held weapon detection
5. **Kill method detection**: Verify weapon vs vehicle kill differentiation

### Reward Issues
1. **Exploit protection**: Check if auto-protection triggered
2. **Threshold values**: Verify thresholds are achievable
3. **Item classes**: Confirm all reward items exist in game
4. **Reset behavior**: Understand death penalty effects

## 📊 Performance Considerations

### Server Impact
- **Minimal overhead**: Only processes actual player deaths
- **Event-driven**: No continuous polling or monitoring
- **Efficient attribution**: Fast attacker resolution
- **Optimized logging**: Detailed but not excessive

### Best Practices
- **Reasonable thresholds**: Don't set too low (spam) or high (unreachable)
- **Balanced rewards**: Valuable but not game-breaking
- **Monitor logs**: Watch for attribution issues
- **Test thoroughly**: Verify all kill methods work

---

**Next Steps**:
- See Configuration Examples documentation for complete PVP setups
- Review Death Penalties documentation for penalty configuration
- Check Troubleshooting documentation for common issues
