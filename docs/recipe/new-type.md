# 新的配方类型

Forge 在新版本中添加了配方类型的支持，它允许你直接从数据包中加载各种机器的配方。有了它你就可以抛弃 CraftTweaker 支持，兼顾可定制性与稳定性。

在模块中注册 `IRecipeType`：

```java
@Name("milling")
public static final IRecipeType<MillingRecipe> RECIPE_TYPE = new IRecipeType() {};
```

`IRecipe` 要求带有一个 `IInventory` 的类型参数，一定程度上局限了配方的应用。你可以直接令你的 Context（如TileEntity）实现 Kiwi 中的 `EmptyInventory`。

```java
public class MillingRecipe extends Recipe<MillingContext> {
    public HybridingRecipe(ResourceLocation id) {
        super(id);
    }

    @Override
    public boolean matches(MillingContext ctx, World worldIn) {
        return false;
    }
}
```
