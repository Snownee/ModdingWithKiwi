# 条件：解析原料

尝试解析一个原料（Ingredient），若解析成功则返回 `true`。

当存在有稻米标签的物品时，配方生效：

```json
{
  "conditions": [
    {
      "type": "kiwi:try_parse",
      "value": {
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
    }
  ]
}
```
