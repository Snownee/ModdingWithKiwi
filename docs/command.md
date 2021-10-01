# 内置命令

## 应用调试世界规则

```text
/kiwi debugWorldRules
```

等效于

```text
/gamerule doDaylightCycle false
/gamerule doWeatherCycle false
/gamerule doMobLoot false
/gamerule doMobSpawning false
/difficulty peaceful
/kill @e[type=!minecraft:player]
/time set day
/weather clear
/gamerule doMobLoot true
```
