# 杂项

## Util.RL

创建新的 ResourceLocation。失败返回 null，而不是抛出异常。

```java
Util.RL("foo"); // minecraft:foo
Util.RL("foo", "my_mod"); // my_mod:foo
Util.RL("minecraft:foo", "my_mod"); // minecraft:foo
```

## Util.trilRL

清除 ResourceLocation 默认的命名空间。

```java
Util.trimRL("minecraft:foo"); // foo
Util.trimRL("my_mod:foo"); // my_mod:foo
Util.trimRL("my_mod:foo", "my_mod"); // foo
// 也支持 ResourceLocation 参数
```
