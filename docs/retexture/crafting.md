# 合成配方

像匠魂中的工具装配台一样，允许玩家通过合成的方式为可变材质方块指定材质。

举个例子，一个支持以任何完整方块作为材料的楼梯：

```json
{
  "type": "kiwi:retexture",
  "pattern": [
    "#  ",
    "## ",
    "###"
  ],
  "key": {
    "#": {
      "type": "kiwi:full_block"
    }
  },
  "texture": {
    "#": [ "all" ]
  },
  "result": {
    "item": "my_mod:stairs",
    "count": 4
  }
}
```

其中 `"type": "kiwi:full_block"` 表示匹配所有完整方块。但决定材质的材料并非强制使用这一类型。
