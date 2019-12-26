# 条件：模块加载

令一个配方仅在指定模块被加载时生效。

```json
{
  "conditions": [
    {
      "type": "kiwi:is_loaded",
      "module": "my_mod:test"
    }
  ]
}
```
