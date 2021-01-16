# 添加效果

好了，现在我已经知道赞助者有哪些了，那么如何给他们加上炫酷的效果呢？

首先，我们要在自己的 RewardProvider 中声明某个等级拥有对应的特效：

```java
@Override
public List<String> getRenderableTiers() {
    return Arrays.asList("2020q3", "2020q4");
}

@Override
@OnlyIn(Dist.CLIENT)
public RewardLayer createRenderer(IEntityRenderer<AbstractClientPlayerEntity, PlayerModel<AbstractClientPlayerEntity>> entityRenderer, String tier) {
    switch (tier) {
    case "2020q3":
        return new PlanetLayer(entityRenderer);
    case "2020q4":
        return new FoxTailLayer(entityRenderer);
    default:
        return null;
}
```

接下我们只需要编写玩家的 LayerRenderer 就可以了：

```java
@OnlyIn(Dist.CLIENT)
public class MyLayer extends RewardLayer {

    public MyLayer(IEntityRenderer<AbstractClientPlayerEntity, PlayerModel<AbstractClientPlayerEntity>> entityRendererIn) {
        super(entityRendererIn);
    }

    @Override
    public void render(...) {
    }

}
```

玩家想要切换不同的特效，需要在游戏内按住 `K` 键，打开专门的特效选择界面。
