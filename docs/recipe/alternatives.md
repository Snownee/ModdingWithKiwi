# 原料：替代方案

依次尝试解析 `list` 中的每个元素作为原料（Ingredient）。若解析成功则返回该原料，否则尝试解析下个元素。若所有元素均解析失败则会抛出异常。

```json
{
  "type": "kiwi:alternatives",
  "list": [
    {
      "tag": "forge:grain/rice"
    },
    {
      "tag": "forge:seeds/rice"
    }
  ]
}
```

若你不希望在解析完全失败后抛出异常，可以在 `list` 末尾添加一个空数组，意为空的原料。
