# Configuration Examples

## 🎯 Ready-to-Use Configurations

This guide provides tested, working configurations for common server types.

## 🔥 PVP-Focused Server

Perfect for competitive combat servers with emphasis on player vs player action.

```json
{
    "Leaderboards": [
        {
            "id": "pvpKills",
            "label": "PVP Champions",
            "entityScores": [
                {
                    "entityType": "PVP_PLAYER_KILL",
                    "scoreValue": 15
                }
            ],
            "rewardThresholds": [2, 5, 10, 20, 50],
            "deathPenalty": 0.2,
            "resetKillsOnDeath": 1,
            "resetRewardsOnDeath": 1,
            "rewards": [
                {
                    "threshold": 2,
                    "items": [
                        {"className": "Ammo_556x45", "quantity": 90, "stackSize": 30},
                        {"className": "Morphine", "quantity": 1, "stackSize": -1}
                    ]
                },
                {
                    "threshold": 5,
                    "items": [
                        {"className": "M4_Suppressor", "quantity": 1, "stackSize": -1},
                        {"className": "AmmoBox_556x45_20Rnd", "quantity": 2, "stackSize": -1}
                    ]
                },
                {
                    "threshold": 10,
                    "items": [
                        {"className": "ACOGOptic", "quantity": 1, "stackSize": -1},
                        {"className": "M4_RISHndgrd", "quantity": 1, "stackSize": -1},
                        {"className": "AmmoBox_556x45_20Rnd", "quantity": 4, "stackSize": -1}
                    ]
                }
            ]
        },
        {
            "id": "zombieCleanup",
            "label": "Zombie Cleanup",
            "entityScores": [
                {"entityType": "Zmb", "scoreValue": 1}
            ],
            "rewardThresholds": [50, 100, 200],
            "deathPenalty": 0.3,
            "resetKillsOnDeath": 1,
            "resetRewardsOnDeath": 0,
            "rewards": [
                {
                    "threshold": 50,
                    "items": [
                        {"className": "BandageDressing", "quantity": 5, "stackSize": -1},
                        {"className": "Ammo_9x19", "quantity": 60, "stackSize": 15}
                    ]
                }
            ]
        }
    ],
    "debugMode": 0
}
```

## 🧟 PVE Survival Server

Ideal for PVE-focused servers with emphasis on zombie survival and exploration.

```json
{
    "Leaderboards": [
        {
            "id": "zombieHunter",
            "label": "Zombie Hunter",
            "entityScores": [
                {"entityType": "ZmbF_Citizen", "scoreValue": 1},
                {"entityType": "ZmbM_Citizen", "scoreValue": 1},
                {"entityType": "ZmbF_Clerk", "scoreValue": 1},
                {"entityType": "ZmbM_Clerk", "scoreValue": 1},
                {"entityType": "ZmbM_PatrolNormal", "scoreValue": 3},
                {"entityType": "ZmbM_SoldierNormal", "scoreValue": 5},
                {"entityType": "ZmbM_usSoldier_normal", "scoreValue": 7},
                {"entityType": "ZmbM_usSoldier_Heavy", "scoreValue": 10},
                {"entityType": "ZmbM_usSoldier_Officer", "scoreValue": 15}
            ],
            "rewardThresholds": [25, 50, 100, 250, 500],
            "deathPenalty": 0.4,
            "resetKillsOnDeath": 0,
            "resetRewardsOnDeath": 0,
            "rewards": [
                {
                    "threshold": 25,
                    "items": [
                        {"className": "BandageDressing", "quantity": 3, "stackSize": -1},
                        {"className": "Disinfectant", "quantity": 1, "stackSize": -1}
                    ]
                },
                {
                    "threshold": 50,
                    "items": [
                        {"className": "Morphine", "quantity": 1, "stackSize": -1},
                        {"className": "AmmoBox_9x19_25rnd", "quantity": 1, "stackSize": -1}
                    ]
                },
                {
                    "threshold": 100,
                    "items": [
                        {"className": "M4_Suppressor", "quantity": 1, "stackSize": -1},
                        {"className": "Ammo_556x45", "quantity": 120, "stackSize": 30}
                    ]
                }
            ]
        },
        {
            "id": "wildlifeHunter",
            "label": "Wildlife Hunter",
            "entityScores": [
                {"entityType": "Animal_Gallus", "scoreValue": 1},
                {"entityType": "Animal_Lepus", "scoreValue": 2},
                {"entityType": "Animal_CapraHircus", "scoreValue": 4},
                {"entityType": "Animal_CapreolusCapreolus", "scoreValue": 6},
                {"entityType": "Animal_CervusElaphus", "scoreValue": 10},
                {"entityType": "Animal_SusScrofa", "scoreValue": 12},
                {"entityType": "Animal_CanisLupus", "scoreValue": 20},
                {"entityType": "Animal_UrsusArctos", "scoreValue": 25}
            ],
            "rewardThresholds": [20, 50, 100],
            "deathPenalty": 0.2,
            "resetKillsOnDeath": 0,
            "resetRewardsOnDeath": 0,
            "rewards": [
                {
                    "threshold": 20,
                    "items": [
                        {"className": "HuntingKnife", "quantity": 1, "stackSize": -1},
                        {"className": "Ammo_308Win", "quantity": 40, "stackSize": 20}
                    ]
                }
            ]
        }
    ],
    "debugMode": 0
}
```

## ⚖️ Balanced Mixed Server

Great for servers that want both PVP and PVE elements with balanced progression.

```json
{
    "Leaderboards": [
        {
            "id": "combatMaster",
            "label": "Combat Master",
            "entityScores": [
                {"entityType": "Zmb", "scoreValue": 1},
                {"entityType": "ZmbM_Soldier", "scoreValue": 3},
                {"entityType": "ZmbM_usSoldier", "scoreValue": 5},
                {"entityType": "PVP_PLAYER_KILL", "scoreValue": 20}
            ],
            "rewardThresholds": [15, 35, 75, 150],
            "deathPenalty": 0.3,
            "resetKillsOnDeath": 1,
            "resetRewardsOnDeath": 1,
            "rewards": [
                {
                    "threshold": 15,
                    "items": [
                        {"className": "BandageDressing", "quantity": 2, "stackSize": -1},
                        {"className": "Ammo_9x19", "quantity": 30, "stackSize": 15}
                    ]
                },
                {
                    "threshold": 35,
                    "items": [
                        {"className": "Morphine", "quantity": 1, "stackSize": -1},
                        {"className": "AmmoBox_9x19_25rnd", "quantity": 1, "stackSize": -1}
                    ]
                },
                {
                    "threshold": 75,
                    "items": [
                        {"className": "M4_Suppressor", "quantity": 1, "stackSize": -1},
                        {"className": "Ammo_556x45", "quantity": 60, "stackSize": 30}
                    ]
                }
            ]
        },
        {
            "id": "survivalist",
            "label": "Survivalist",
            "entityScores": [
                {"entityType": "Animal_", "scoreValue": 3},
                {"entityType": "Zmb", "scoreValue": 1}
            ],
            "rewardThresholds": [30, 75, 150],
            "deathPenalty": 0.25,
            "resetKillsOnDeath": 0,
            "resetRewardsOnDeath": 0,
            "rewards": [
                {
                    "threshold": 30,
                    "items": [
                        {"className": "SteakKnife", "quantity": 1, "stackSize": -1},
                        {"className": "Matches", "quantity": 1, "stackSize": -1}
                    ]
                }
            ]
        }
    ],
    "debugMode": 0
}
```

## 🏃 Hardcore Server

For servers with harsh penalties and high-stakes gameplay.

```json
{
    "Leaderboards": [
        {
            "id": "eliteSurvivor",
            "label": "Elite Survivor",
            "entityScores": [
                {"entityType": "Zmb", "scoreValue": 2},
                {"entityType": "ZmbM_Soldier", "scoreValue": 8},
                {"entityType": "ZmbM_usSoldier", "scoreValue": 15},
                {"entityType": "PVP_PLAYER_KILL", "scoreValue": 50},
                {"entityType": "Animal_UrsusArctos", "scoreValue": 30},
                {"entityType": "Animal_CanisLupus", "scoreValue": 25}
            ],
            "rewardThresholds": [50, 150, 500],
            "deathPenalty": 0.75,
            "resetKillsOnDeath": 1,
            "resetRewardsOnDeath": 1,
            "rewards": [
                {
                    "threshold": 50,
                    "items": [
                        {"className": "Morphine", "quantity": 1, "stackSize": -1},
                        {"className": "AmmoBox_9x19_25rnd", "quantity": 1, "stackSize": -1}
                    ]
                },
                {
                    "threshold": 150,
                    "items": [
                        {"className": "M4_Suppressor", "quantity": 1, "stackSize": -1},
                        {"className": "ACOGOptic", "quantity": 1, "stackSize": -1},
                        {"className": "AmmoBox_556x45_20Rnd", "quantity": 2, "stackSize": -1}
                    ]
                }
            ]
        }
    ],
    "debugMode": 0
}
```

## 🎪 Casual/Fun Server

Perfect for relaxed servers with frequent rewards and low penalties.

```json
{
    "Leaderboards": [
        {
            "id": "casualKiller",
            "label": "Casual Killer",
            "entityScores": [
                {"entityType": "Zmb", "scoreValue": 1},
                {"entityType": "Animal_", "scoreValue": 2},
                {"entityType": "PVP_PLAYER_KILL", "scoreValue": 5}
            ],
            "rewardThresholds": [5, 10, 20, 30, 50, 75, 100],
            "deathPenalty": 0.1,
            "resetKillsOnDeath": 0,
            "resetRewardsOnDeath": 0,
            "rewards": [
                {
                    "threshold": 5,
                    "items": [
                        {"className": "BandageDressing", "quantity": 1, "stackSize": -1}
                    ]
                },
                {
                    "threshold": 10,
                    "items": [
                        {"className": "Ammo_9x19", "quantity": 30, "stackSize": 15}
                    ]
                },
                {
                    "threshold": 20,
                    "items": [
                        {"className": "Morphine", "quantity": 1, "stackSize": -1}
                    ]
                }
            ]
        }
    ],
    "debugMode": 0
}
```

## 🔧 Configuration Tips

### Entity Type Matching
- **Partial matching**: `"Zmb"` matches all zombie types
- **Specific types**: `"ZmbM_SoldierNormal"` matches only that type
- **Order matters**: First match wins, so put specific types before general ones

### Reward Item Classes
Always use exact DayZ class names:
- ✅ `"BandageDressing"` 
- ❌ `"Bandage"`
- ✅ `"Ammo_556x45"`
- ❌ `"5.56 Ammo"`

### Death Penalty Guidelines
- **0.1-0.2**: Very casual (lose 10-20%)
- **0.3-0.4**: Balanced (lose 30-40%)
- **0.5-0.6**: Competitive (lose 50-60%)
- **0.7+**: Hardcore (lose 70%+)

### Testing Your Configuration
1. **Start small**: Test with simple configurations first
2. **Enable debug**: `"debugMode": 1` for detailed logs
3. **Check syntax**: Use JSON validator before deployment
4. **Test rewards**: Verify all item class names exist

---

**Next Steps**:
- Copy and modify these examples for your server
- See Basic Configuration documentation for detailed parameter explanations
- Check Troubleshooting documentation if you encounter issues
