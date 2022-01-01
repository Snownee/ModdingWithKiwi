# 贡献者奖励

## 等级

Kiwi 提供了赞助者奖励的支持。你可以分组地给予赞助者们不同饰品作为奖励。

赞助者分为不同等级（tier），不同等级之间没有从属关系。

你可以这样判断玩家是否属于任意或某个等级：

```java
Contributors.isContributor(author, player);
Contributors.isContributor(author, player, tier);
```

## ITierProvider

想要给赞助者奖励，我们首先要知道赞助者有哪些，这就是 ITierProvider 的作用。

注册你的 ITierProvider：

```java
@Override
protected void init(InitEvent event) {
	registerTierProvider(new MyTierProvider());
}
```

Kiwi 内置了一个实现 —— JsonTierProvider。允许你从某个 URL 中以 JSON 格式读取赞助者列表。它的格式就像这样：（其中 `*` 代表这些 ID 拥有所有等级。注意等级要全部小写）

```json
{
  "*"     : [ "Snownee" ],
  "2020q1": [ "MalayP" ],
  "2020q2": [ "MalayP", "Baka943" ]
}
```

一般来说，你只需要继承并注册这个类就可以了：

```java
public class MyTierProvider extends JsonTierProvider {
	public MyTierProvider() {
		super("Snownee", () -> Collections.singletonList("https://cdn.jsdelivr.net/gh/Snownee/Kiwi@master/contributors.json"));
	}
}
```

## 添加饰品

好了，现在我已经知道赞助者有哪些了，那么如何给他们加上炫酷的饰品呢？

首先，我们要在自己的 ITierProvider 中声明某个等级拥有对应的饰品：

```java
@Override
public List<String> getRenderableTiers() {
	return Arrays.asList("2020q3", "2020q4");
}

@Override
@OnlyIn(Dist.CLIENT)
public CosmeticLayer createRenderer(RenderLayerParent<AbstractClientPlayer, PlayerModel<AbstractClientPlayer>> entityRenderer, String tier) {
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
public class MyLayer extends CosmeticLayer {

	public MyLayer(RenderLayerParent<AbstractClientPlayer, PlayerModel<AbstractClientPlayer>> entityRendererIn) {
		super(entityRendererIn);
	}

	@Override
	public void render(...) {
	}

}
```

玩家想要切换不同的特效，需要在游戏内按住 <kbd>K</kbd> 键，打开专门的饰品选择界面。
