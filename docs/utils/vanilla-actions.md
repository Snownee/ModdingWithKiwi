# 原版操作

`snownee.kiwi.util.VanillaActions`

VanillaActions 提供了以下行为的简化操作：

 - 注册堆肥配方
 - 使村民可以捡起并堆肥某物品
 - 注册斧头剥皮转换配方
 - 注册锄头耕地转换配方
 - 注册铲子平地转换配方
 - 设置方块可燃性
 - 注册村民堆肥交互物品
 - 注册村民可捡起物品

## 注意！

当你在生命周期事件内执行以上绝大多数操作时，需要使用 `enqueueWork()` 避免数据竞争。

```java
@Override
protected void init(InitEvent event) {
  event.enqueueWork(() -> VanillaActions.registerCompostable(0.5F, Items.DIAMOND));
}
```

!!! Note
	Forge 平台下，`enqueueWork()` 中产生的错误会被静默处理。你可以对 `enqueueWork()` 的返回值添加异常处理器。
