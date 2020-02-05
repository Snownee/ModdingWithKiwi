# IRewardProvider

想要给赞助者奖励，我们首先要知道赞助者有哪些，这就是 `IRewardProvider` 的作用。

注册你的 IRewardProvider：

```java
@Override
protected void init(FMLCommonSetupEvent event) {
    registerRewardProvider(new MyRewardProvider());
}
```

Kiwi 内置了一个实现 —— `JsonRewardProvider`。允许你从某个 URL 中以 JSON 格式读取赞助者列表。它的格式就像这样：（其中`*`代表所有等级。注意等级要全部小写）

```json
{
    "*"     : [ "Snownee" ],
    "2020q1": [ "MalayP" ],
    "2020q2": [ "MalayP", "Baka943" ]
}
```

一般来说，你只需要继承并注册这个类就可以了：

```java
public class MyRewardProvider extends JsonRewardProvider {
    public MyRewardProvider() {
        super("Snownee", "https://raw.githubusercontent.com/Snownee/Kiwi/master/contributors.json");
    }
}
```
