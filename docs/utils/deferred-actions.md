# 推迟任务

在 Forge 声明周期中进行注册堆肥配方、斧头剥皮配方等任务时可能出现并发问题，而 DeferredActions 正是解决这一问题的。

```java
DeferredActions.add(() -> doSth());
```

DeferredActions 提供的内置任务有：

 - 注册堆肥配方
 - 使村民可以捡起并堆肥某物品
 - 注册斧头剥皮转换配方
 - 注册锄头耕地转换配方
 - 设置方块可燃性