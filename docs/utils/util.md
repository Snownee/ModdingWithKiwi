# 杂项

## Util.RL

创建新的 ResourceLocation。失败返回 null，而不是抛出异常。

```java
Util.RL("foo"); // minecraft:foo
Util.RL("foo", "my_mod"); // my_mod:foo
Util.RL("minecraft:foo", "my_mod"); // minecraft:foo
```

## Util.trimRL

清除 ResourceLocation 默认的命名空间。

```java
Util.trimRL("minecraft:foo"); // foo
Util.trimRL("my_mod:foo"); // my_mod:foo
Util.trimRL("my_mod:foo", "my_mod"); // foo
// 也支持 ResourceLocation 参数
```

## Util.getRecipeManager

无需提供参数获取 RecipeManager。

## Util.getRecipes

获得某一类别的所有配方。原版中对应方法为私有，故设置此方法方便访问。

## MathUtil.fibonacciSphere

获得一个球面上均匀排列的点。

## MathUtil.RGBtoHSV

提供 RGB 到 HSV 的转换。注：HSV 到 RGB 的转换在 Mth 中已有实现。
