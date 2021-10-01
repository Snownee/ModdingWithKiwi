# 新的配方类型

你可以创建属于自己的配方类型，它允许你直接从数据包中加载各种机器的配方。有了它你就可以抛弃 CraftTweaker 支持，兼顾可定制性与稳定性。

在模块中注册 RecipeType：

```java
@Name("milling")
public static RecipeType<MillingRecipe> RECIPE_TYPE = new RecipeType<>() {};
```

Recipe 要求带有一个 Container 的类型参数，一定程度上局限了配方的应用。你可以直接令你的合成上下文（如 BlockEntity）实现 Kiwi 中的 `snownee.kiwi.crafting.Simple`。

```java
public class MillingRecipe extends Simple<MillingContext> {
    public HybridingRecipe(ResourceLocation id) {
        super(id);
    }

    @Override
    public boolean matches(MillingContext ctx, Level level) {
        return false;
    }
}
```
