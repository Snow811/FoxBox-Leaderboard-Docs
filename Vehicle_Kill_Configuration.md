# Vehicle Kill Configuration

The FoxBox Leaderboards mod now supports configurable vehicle kill tracking with separate scoring and enable/disable options per entity type.

## Overview

The vehicle kill system allows server administrators to:
- Enable or disable vehicle kills for specific entity types
- Set different score multipliers for vehicle vs weapon kills
- Track and log both kill methods separately
- Maintain backward compatibility with existing configurations

## Configuration

### Basic EntityTypeScore Structure

```json
{
    "entityType": "Animal_UrsusArctos",
    "scoreValue": 5,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 2.0
}
```

### Configuration Properties

- **entityType**: The class name of the entity (e.g., "Animal_UrsusArctos", "ZmbM_SoldierNormal")
- **scoreValue**: Base score points awarded for killing this entity (weapon kills)
- **enableVehicleKills**: Boolean - whether vehicle kills count for this entity type
- **vehicleKillMultiplier**: Multiplier applied to scoreValue for vehicle kills (default: 1.0)

### Score Calculation

- **Weapon Kill Score** = `scoreValue`
- **Vehicle Kill Score** = `scoreValue × vehicleKillMultiplier` (only if enableVehicleKills = true)

## Examples

### Large Animals (Higher reward for vehicle kills)
```json
{
    "entityType": "Animal_UrsusArctos",
    "scoreValue": 5,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 2.0
}
```
- Weapon kill: 5 points
- Vehicle kill: 10 points (5 × 2.0)

### Small Animals (Vehicle kills disabled)
```json
{
    "entityType": "Animal_GallusGallusDomesticus",
    "scoreValue": 1,
    "enableVehicleKills": false,
    "vehicleKillMultiplier": 1.0
}
```
- Weapon kill: 1 point
- Vehicle kill: 0 points (disabled)

### Zombies (Equal scoring)
```json
{
    "entityType": "Zmb",
    "scoreValue": 1,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 1.0
}
```
- Weapon kill: 1 point
- Vehicle kill: 1 point (1 × 1.0)

### PVP (Lower reward for vehicle kills)
```json
{
    "entityType": "FoxLB_PVP_PlayerKill",
    "scoreValue": 10,
    "enableVehicleKills": true,
    "vehicleKillMultiplier": 0.5
}
```
- Weapon kill: 10 points
- Vehicle kill: 5 points (10 × 0.5)

## Default Configuration

The default configuration includes an "Animal Hunter" leaderboard demonstrating the vehicle kill system:

- **Bears**: 5 base points, vehicle kills give 10 points (2x multiplier)
- **Cows**: 3 base points, vehicle kills give 4.5 points (1.5x multiplier)
- **Chickens**: 1 base point, vehicle kills disabled

## Backward Compatibility

Existing configurations will continue to work:
- Missing `enableVehicleKills` defaults to `false`
- Missing `vehicleKillMultiplier` defaults to `1.0`
- Old `EntityTypeScore` entries automatically get default values

## Vehicle Detection

The system detects vehicle kills by analyzing the damage source chain:
- Direct vehicle collision damage
- Vehicle-mounted weapon damage
- Vehicle explosion damage

Supported vehicle types include cars, trucks, boats, and helicopters that directly cause damage.

## Debugging

Enable debug mode to see detailed kill tracking logs:
- Kill method detection (weapon vs vehicle)
- Vehicle type identification
- Score calculation details
- Configuration validation

Example debug output:
```
SUCCESS: PlayerName (steamid) killed Animal_UrsusArctos via vehicle (Sedan_02) - Score: 10 points (vehicle multiplier: 2.0)
SUCCESS: PlayerName (steamid) killed Animal_UrsusArctos via weapon - Score: 5 points
```
