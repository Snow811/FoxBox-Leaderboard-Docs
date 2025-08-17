# Server Setup Guide

## 🎯 Overview
Complete step-by-step guide to install and configure the FoxBox Leaderboards with Rewards system on your DayZ server.

## 📋 Prerequisites

### Server Requirements
- **Server file access** (FTP/local access)
- **Server restart capability**
- **Basic JSON editing knowledge**

### Dependencies
- No external dependencies required
- Compatible with most existing mods
- Works with Community Framework (optional)

## 🚀 Installation Steps

### Step 1: Install the Mod
1. **Subscribe to mod** on Steam Workshop (or download from mod source)
2. **Add to server launch parameters**:
   ```
   -mod="@FoxBox_Leaderboards_with_Rewards"
   ```
3. **Restart server** to generate initial configuration files

### Step 2: Initial Configuration
The system will automatically create default configuration on first run:

**Config File Location**: `$profile:\FoxBox_mods\FoxBoxLeaderboard\FoxBoxLeaderboardConfig.json`

**Default Generated Config**:
- ✅ Zombie kills leaderboard
- ✅ PVP kills leaderboard  
- ✅ Basic reward tiers
- ✅ Balanced death penalties

### Step 3: Verify Installation
1. **Check server logs** for successful initialization:
   ```
   [FoxBoxLeaderboard] Leaderboard manager initialized.
   [FoxBoxLeaderboard] Loaded 2 leaderboard definitions.
   ```

2. **Test in-game**:
   - Join server as player
   - Press `K` key (default keybind)
   - Leaderboard UI should appear

## ⚙️ Basic Configuration

### Understanding the Config Structure
```json
{
    "Leaderboards": [
        {
            "id": "zombieKills",
            "label": "Zombie Kills", 
            "entityScores": [...],
            "rewardThresholds": [10, 25, 50],
            "deathPenalty": 0.5,
            "resetKillsOnDeath": 1,
            "resetRewardsOnDeath": 0
        }
    ],
    "debugMode": 0
}
```

### Key Configuration Options

#### **Leaderboard Properties**
- `id`: Unique identifier (used internally)
- `label`: Display name shown to players
- `entityScores`: What entities to track and their point values
- `rewardThresholds`: Kill counts that trigger rewards
- `deathPenalty`: Percentage of score lost on death (0.0-1.0)

#### **Death Behavior**
- `resetKillsOnDeath`: `1` = reset kills on death, `0` = preserve
- `resetRewardsOnDeath`: `1` = reset rewards on death, `0` = preserve

**⚠️ Important**: The system auto-prevents exploits if misconfigured

### Entity Scoring Examples

#### **Zombie Difficulty Tiers**
```json
"entityScores": [
    {"entityType": "Zmb", "scoreValue": 1},                    // All zombies: 1 point
    {"entityType": "ZmbM_SoldierNormal", "scoreValue": 3},     // Military: 3 points  
    {"entityType": "ZmbM_usSoldier_Officer", "scoreValue": 6}  // Officers: 6 points
]
```

#### **PVP Tracking**
```json
"entityScores": [
    {
        "entityType": "FoxLB_PVP_PlayerKill",
        "scoreValue": 10,
        "enableVehicleKills": true,
        "vehicleKillMultiplier": 0.8
    }
]
```
- Player weapon kills: 10 points (full value)
- Player vehicle kills: 8 points (10 × 0.8)

#### **Animal Hunting with Vehicle Options**
```json
"entityScores": [
    {
        "entityType": "Animal_GallusGallusDomesticus",
        "scoreValue": 1,
        "enableVehicleKills": false
    },
    {
        "entityType": "Animal_UrsusArctos", 
        "scoreValue": 15,
        "enableVehicleKills": true,
        "vehicleKillMultiplier": 1.5
    }
]
```
- Chickens: Only weapon kills count (1 point)
- Bears: Weapon kills = 15 points, Vehicle kills = 23 points (bonus)

## 🚗 Vehicle Kill Configuration

### Understanding Vehicle Kills
The system can differentiate between weapon kills and vehicle kills, allowing different scoring for each method.

#### **Vehicle Kill Properties**
```json
{
    "entityType": "Animal_UrsusArctos",
    "scoreValue": 20,
    "enableVehicleKills": true,        // Allow vehicle kills  
    "vehicleKillMultiplier": 1.2       // 20% bonus for vehicle kills
}
```

#### **Vehicle Kill Policies**

**Disable Vehicle Kills** (Traditional Combat Only):
```json
{
    "enableVehicleKills": false
}
```
- Only weapon/melee kills will count
- Vehicle kills are completely ignored
- Promotes traditional combat skills

**Equal Scoring** (Method Doesn't Matter):
```json
{
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 1.0
}
```
- Vehicle and weapon kills worth same points
- Players can choose their preferred method
- Balanced approach

**Reduced Vehicle Scoring** (Weapon Preference):
```json
{
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 0.6
}
```
- Vehicle kills worth 60% of weapon kills
- Encourages skillful weapon combat
- Still allows vehicle tactics

**Bonus Vehicle Scoring** (Creative Kills):
```json
{
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 1.5
}
```
- Vehicle kills worth 50% more than weapon kills
- Rewards creative/tactical gameplay
- Encourages vehicle utilization

### Vehicle Kill Examples by Entity Type

#### **Small Animals** (Usually Disabled):
```json
{
    "entityType": "Animal_GallusGallusDomesticus",
    "scoreValue": 1,
    "enableVehicleKills": false    // Too easy with vehicles
}
```

#### **Large Predators** (Often Bonus):
```json
{
    "entityType": "Animal_UrsusArctos",
    "scoreValue": 20,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 1.3   // Bonus for risky vehicle kill
}
```

#### **PVP Combat** (Usually Reduced):
```json
{
    "entityType": "FoxLB_PVP_PlayerKill",
    "scoreValue": 25,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 0.7   // Promotes skill-based combat  
}
```

#### **Zombie Hordes** (Often Equal or Bonus):
```json
{
    "entityType": "Zmb",
    "scoreValue": 1,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 1.2   // Slight bonus for horde clearing
}
```

## 🎮 Player Experience

### Default Keybind
- **Open/Close Leaderboard**: `K` key
- **Navigate Between Boards**: Arrow buttons in UI
- **Claim Rewards**: Green "Claim Rewards" button

### What Players See
1. **Leaderboard Rankings** (top 15 players)
2. **Personal Statistics** (kills, score, rank)
3. **Reward Progress** (current/next thresholds)
4. **Multiple Leaderboards** (navigate with arrows)

## 🔄 Configuration Updates

### Making Changes
1. **Edit config file** while server is running
2. **Restart server** OR use admin command (if implemented)
3. **Changes apply immediately** on server restart

### Testing Changes
1. **Enable debug mode**: `"debugMode": 1`
2. **Check server logs** for detailed output
3. **Test with multiple players** for full verification

## 📊 Data Management

### Player Data Storage
- **Location**: `$profile:\FoxBox_mods\FoxBoxLeaderboard\Players\`
- **Format**: `FoxBoxLeaderboard_Player_[SteamID].json`
- **Automatic**: Created when players first get kills

### Backup Recommendations
1. **Regular backups** of entire `FoxBoxLeaderboard` folder
2. **Before major updates** or server wipes
3. **Player data persists** across server restarts

## ⚡ Performance Considerations

### Server Impact
- **Minimal CPU usage** (event-driven system)
- **Low memory footprint** (data cached efficiently)
- **Optimized I/O** (batch saves, smart caching)

### Recommended Settings
- **Auto-save interval**: 60 seconds (default)
- **Snapshot updates**: 30 seconds (default)
- **Debug mode**: Disable for production (`"debugMode": 0`)

## 🐛 Common Issues

### Config Not Loading
- **Check file path**: Must be exact location
- **Validate JSON**: Use JSON validator tool
- **Check permissions**: Server must have write access

### Players Not Tracked
- **Verify entity types**: Check spelling in `entityScores`
- **Check kill detection**: Review server logs
- **Steam ID issues**: Ensure players have valid identities

### UI Not Appearing
- **Keybind conflict**: May conflict with other mods using K key
- **Layout missing**: Verify mod installation completed successfully
- **Client-server sync**: Check RPC communication logs

## 🔧 Advanced Setup

### Multiple Server Instances
- Each server needs **separate config files**
- **Player data location** is per-server
- **Cross-server tracking** not supported (by design)

### Integration with Other Mods
- **Compatible** with most kill-tracking mods
- **Event priority** may affect kill attribution
- **Test thoroughly** with your mod stack

## ✅ Quick Checklist

- [ ] Mod installed and subscribed to Steam Workshop
- [ ] Launch parameters updated with mod reference
- [ ] Server restarted successfully
- [ ] Config file generated automatically
- [ ] Leaderboard UI accessible in-game (K key)
- [ ] Kill tracking working (test with zombie kills)
- [ ] Rewards system functional
- [ ] Server logs show no errors

---

**Next Steps**: 
- Review Configuration Examples documentation for advanced setups
- Check Player Guide documentation to understand the player experience
- See Troubleshooting documentation if you encounter issues
