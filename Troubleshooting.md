# Troubleshooting Guide

## 🔍 Quick Diagnosis

### Check These First
1. **Server logs** - Look for `[FoxBoxLeaderboard]` messages
2. **Config file** - Verify JSON syntax and file location
3. **File permissions** - Ensure server can read/write to profile folder
4. **Mod installation** - Confirm mod is loaded in server parameters

## 🚫 Common Issues

### Leaderboard UI Not Appearing

#### **Keybind Not Working**
**Symptoms**: Pressing K (or configured key) does nothing

**Solutions**:
```
✓ Check if other mods use the K key
✓ Ask server admin if custom keybind was configured
✓ Try testing on a fresh server with just this mod
✓ Restart server after any keybind changes
```

#### **UI Layout Missing**
**Symptoms**: Error in logs about missing layout files

**Solutions**:
```
✓ Verify mod installation completed successfully
✓ Check mod was downloaded properly from Steam Workshop
✓ Ensure all mod files are present and not corrupted
✓ Re-subscribe to mod on Steam Workshop if needed
```

#### **RPC Communication Failed**
**Symptoms**: UI opens but shows no data

**Solutions**:
```
✓ Check server logs for RPC errors
✓ Verify client-server version match
✓ Test with multiple players
✓ Restart both client and server
```

### Kills Not Being Tracked

#### **Entity Types Not Matching**
**Symptoms**: Killing zombies/animals but no progress

**Solutions**:
```JSON
// Check entity type configuration
{
    "entityScores": [
        {"entityType": "Zmb", "scoreValue": 1}  // ✓ Correct - matches all zombies
        {"entityType": "ZombieM_Soldier", "scoreValue": 3}  // ❌ Wrong - doesn't exist
    ]
}
```

**Debug Steps**:
1. **Enable debug mode**: `"debugMode": 1`
2. **Check logs**: Look for kill detection messages
3. **Test specific entities**: Kill different zombie types
4. **Verify entity names**: Use correct DayZ class names

#### **Kill Detection Not Triggering**
**Symptoms**: No kill detection messages in logs

**Solutions**:
```
✓ Contact mod developer for technical support
✓ Report on mod's Steam Workshop page  
✓ Check if server has other mods interfering
✓ Test different kill methods (weapons, melee, etc.)
```

#### **PVP Kills Not Detected**
**Symptoms**: Player kills not registering

**Solutions**:
```JSON
// Verify PVP configuration
{
    "entityType": "PVP_PLAYER_KILL"  // ✓ Must be exact
    "entityType": "PLAYER_KILL"     // ❌ Wrong constant
}
```

**Debug Steps**:
```
1. Check server logs for "PVP Kill detected" messages
2. Test different PVP scenarios (weapons, explosions)
3. Verify both killer and victim have valid identities
4. Check attacker resolution is working
```

### Configuration Issues

#### **JSON Syntax Errors**
**Symptoms**: Config not loading, parsing errors in logs

**Common Mistakes**:
```JSON
{
    "rewardThresholds": [10, 25, 50],  // ✓ Comma after array
    "deathPenalty": 0.5,               // ✓ Comma after value
    "resetKillsOnDeath": 1             // ✓ No comma on last item
    "resetRewardsOnDeath": 1,          // ❌ Extra comma
}
```

**Solutions**:
```
✓ Use JSON validator tool
✓ Check all brackets match: { }, [ ]
✓ Verify comma placement
✓ Ensure quotes around strings: "value"
```

#### **Config File Not Found**
**Symptoms**: "Config file not found" in logs

**Solutions**:
```
✓ Check exact path: $profile:\FoxBox_mods\FoxBoxLeaderboard\FoxBoxLeaderboardConfig.json
✓ Verify folder permissions (server must write/read)
✓ Let system auto-generate default config first
✓ Manual creation must match exact filename
```

#### **Invalid Reward Items**
**Symptoms**: Rewards not spawning, item errors in logs

**Solutions**:
```JSON
// Use exact DayZ class names
{
    "className": "BandageDressing"     // ✓ Correct
    "className": "Bandage"            // ❌ Wrong
    "className": "Ammo_556x45"        // ✓ Correct
    "className": "5.56_Ammo"          // ❌ Wrong
}
```

### Performance Issues

#### **Server Lag/Stuttering**
**Symptoms**: Performance drops, high CPU usage

**Solutions**:
```
✓ Disable debug mode: "debugMode": 0
✓ Increase save intervals (reduce frequency)
✓ Check for excessive logging
✓ Verify no infinite loops in kill detection
```

#### **High Memory Usage**
**Symptoms**: Server memory continuously increasing

**Solutions**:
```
✓ Check for memory leaks in tracker objects
✓ Verify arrays are being cleaned up properly
✓ Monitor player data file sizes
✓ Regular server restarts if persistent
```

## 🔧 Debug Mode Usage

### Enabling Debug Mode
```JSON
{
    "debugMode": 1  // Enable detailed logging
}
```

### What Debug Mode Shows
- Detailed kill detection process
- Entity type matching attempts
- Score calculations
- Reward claim attempts
- RPC communication details
- Data save/load operations

### Reading Debug Output
```
[FoxBoxLeaderboard] MatchesEntityType called for: ZmbM_SoldierNormal on leaderboard: zombieKills
[FoxBoxLeaderboard] Checking if 'ZmbM_SoldierNormal' contains 'Zmb'
[FoxBoxLeaderboard] MATCH FOUND: 'ZmbM_SoldierNormal' contains 'Zmb' (score: 1)
[FoxBoxLeaderboard] *** KILL REGISTERED *** for zombieKills - Kills: 5 -> 6, Score: 10 -> 11 (+1)
```

**Remember**: Disable debug mode for production servers!

## 🚨 Critical Errors

### Player Data Corruption
**Symptoms**: Player data not loading, tracker creation failing

**Solutions**:
```
1. Backup corrupted file
2. Delete corrupted player data file
3. Let system recreate fresh data
4. Check file permissions
```

### Config Loading Failure
**Symptoms**: System falls back to default config repeatedly

**Solutions**:
```
1. Validate JSON syntax thoroughly
2. Check file encoding (should be UTF-8)
3. Verify exact file path and name
4. Check server profile folder permissions
```

### RPC System Failure
**Symptoms**: Client-server communication broken

**Solutions**:
```
1. Restart both client and server
2. Check for mod version mismatches
3. Verify RPC manager initialization
4. Test with minimal mod setup
```

## 📋 Diagnostic Checklist

### Basic System Check
- [ ] Mod loaded in server launch parameters
- [ ] Config file exists and valid JSON
- [ ] Player can open leaderboard UI (K key)
- [ ] Debug mode enabled for testing
- [ ] Server logs show no critical errors

### Kill Tracking Check
- [ ] Kill messages appear in logs
- [ ] Entity types configured correctly
- [ ] Score values applied properly
- [ ] Multiple entity types working
- [ ] PVP detection working (if configured)

### Reward System Check
- [ ] Thresholds reachable and logical
- [ ] Reward items use correct class names
- [ ] Items spawn in player inventory
- [ ] Claim system working properly
- [ ] Death penalties applied correctly

### Performance Check
- [ ] No excessive logging (disable debug)
- [ ] Server performance stable
- [ ] Memory usage reasonable
- [ ] Player data saves properly
- [ ] UI responsive and smooth

## 🛠️ Advanced Debugging

### Server-Side Testing
```
1. Enable debug mode
2. Join server as admin
3. Spawn test entities (zombies, animals)
4. Kill entities and check logs
5. Verify score calculations
6. Test reward claiming
7. Test death penalties
```

### Client-Side Testing
```
1. Test UI responsiveness
2. Check keybind functionality
3. Verify data display accuracy
4. Test navigation between leaderboards
5. Confirm reward claiming works
```

### Multi-Player Testing
```
1. Test with multiple players online
2. Verify individual data tracking
3. Check leaderboard rankings
4. Test PVP kill detection
5. Confirm no data conflicts
```

## 📞 Getting Help

### Information to Gather
When reporting issues, include:

1. **Server logs** (with [FoxBoxLeaderboard] messages)
2. **Configuration file** (sanitized)
3. **Exact steps to reproduce** the issue
4. **Server/client versions** and mod list
5. **Expected vs actual behavior**

### Self-Help Resources
- **Configuration Examples** - Working setups to compare
- **Player Guide** - Understanding expected behavior  
- **API Reference** - Technical implementation details
- **Performance Guide** - Optimization recommendations

---

**Still Having Issues?**
- Double-check all file paths and permissions
- Test with minimal configuration first
- Compare with working examples
- Enable debug mode for detailed investigation
