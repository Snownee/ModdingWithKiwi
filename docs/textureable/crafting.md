# 合成配方

## 介绍

像匠魂中的工具装配台一样，允许玩家通过合成的方式为可变材质方块指定材质。

举个例子，一个支持以任何完整方块作为材料的楼梯：

```json
{
  "type": "kiwi:texture_block",
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
    "#": [ "top", "bottom", "side" ]
  },
  "result": {
    "item": "my_mod:stairs",
    "count": 4
  }
}
```

其中 `"type": "kiwi:full_block"` 表示匹配所有完整方块。但决定材质的材料并非强制使用这一类型。

## 标记物品

“这的确很棒，可如果我想在物品名中显示这是什么材质的楼梯怎么办？"

你需要添加标记让可变材质方块在合成以及序列化时记录来源的物品：（第16行）

```json
{
  "type": "kiwi:texture_block",
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
    "#": [ "top", "bottom", "side" ]
  },
  "mark": [ "top" ],
  "result": {
    "item": "my_mod:stairs",
    "count": 4
  }
}
```

这样你得到的物品 NBT 中就会记录 `top` key 所使用的物品了。

除此之外，你还需要告诉 TileEntity 这个是作为标记的 key：

```java
@Override
public boolean isMark(String key)
{
    return "top".equals(key);
}
```

!!! Note

  需要得到被标记 key 的物品名？只需使用 `snownee.kiwi.util.Util#getTextureItem`
