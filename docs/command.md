# 内置命令

## 批量生成方块战利品表

```text
/kiwi dumpLoots
```

按以下格式为每个方块生成最基础的战利品表。导出位置为 dumps 目录。

```json
{
  "type": "minecraft:block",
  "pools": [
    {
      "rolls": 1,
      "entries": [
        {
          "type": "minecraft:item",
          "name": "%s"
        }
      ],
      "conditions": [
        {
          "condition": "minecraft:survives_explosion"
        }
      ]
    }
  ]
}
```

你也可以用正则表达式指定需要导出的方块：

```text
/kiwi dumpLoots my_mod:.+
```

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
