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
    {"entityType": "PVP_PLAYER_KILL", "scoreValue": 10}        // Player kills: 10 points
]
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
