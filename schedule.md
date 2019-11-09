# 任务系统

{% hint style="warning" %}
该系统仍不完善，请谨慎使用。
{% endhint %}

为了在未来某个时间（甚至是下次游玩时）执行一段代码，我们引入了任务系统。

如果你不需要考虑序列化问题，事情将变得很简单：

```java
Scheduler.add(new SimpleWorldTask(world, Phase.END, tick -> {
    if (tick >= 50) {
        System.out.println("五十已到");
        return true;
    } else {
        return false;
    }
}));
```

Kiwi 内置了两种 Task 以供使用：SimpleWorldTask 和 SimpleGlobalTask。顾名思义。

如果需要保存你的 Task 以便下次载入游戏时继续运行，则需要自行实现 Task 类。

```java
public class MyTask extends SimpleWorldTask {
    public static final ResourceLocation ID = new ResourceLocation("my_mod", "test");

    // 0参构造器为必需
    public MyTask() {}

    public MyTask(World world, TickEvent.Phase phase) {
        super(world, phase, null);
    }

    @Override
    public boolean tick(WorldTicker ticker) {
        if (++tick % 20 == 0) {
            MinecraftServer server = ticker.getWorld().getServer();
            if (server != null) {
                TextComponent text = new StringTextComponent("" + tick / 20);
                server.getPlayerList().sendMessage(text);
            }
        }
        return tick >= 200;
    }
}
```

最后别忘了注册你的 Task：

```java
@Override
protected void serverInit(FMLServerStartingEvent event) {
    Scheduler.register(MyTask.ID, MyTask.class);
}
```

