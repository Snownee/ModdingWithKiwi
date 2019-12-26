# 配方：消耗容器

令合成完成时不保留容器。（想想蛋糕配方中的牛奶桶）

```text
{
  "type": "kiwi:shaped_no_containers",
  "pattern": [
    "*"
  ],
  "key": {
    "*": {
      "item": "minecraft:lava_bucket"
    }
  },
  "result": {
    "item": "minecraft:bucket"
  }
}
```

这里还有一个无序版本的：

```text
{
  "type": "kiwi:shapeless_no_containers",
  "ingredients": [
    {
      "item": "minecraft:lava_bucket"
    }
  ],
  "result": {
    "item": "minecraft:bucket"
  }
}
```

