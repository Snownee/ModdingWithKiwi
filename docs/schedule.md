# 计划任务

为了在未来某个时间（甚至是下次游玩时）执行一段代码，我们引入了计划任务系统。

如果你不需要考虑序列化问题，事情将变得很简单：

```java
Scheduler.add(new SimpleWorldTask(world, Phase.END, tick -> {
    if (tick >= 50) { // 每次执行这个函数时，tick自增
        System.out.println("五十已到");
        return true; // 返回true停止继续tick这个任务
    } else {
        return false;
    }
}));
```

Kiwi 内置了两种 Task 以供使用：SimpleWorldTask 和 SimpleGlobalTask。顾名思义，SimpleWorldTask 会在某个维度卸载后停止 tick。

如果需要保存你的 Task 以便下次载入游戏时继续运行，则需要自行实现 Task 类及其序列化方法。

```java
import net.minecraft.Util;
import net.minecraft.nbt.CompoundTag;
import net.minecraft.network.chat.ChatType;
import net.minecraft.network.chat.TextComponent;
import net.minecraft.resources.ResourceLocation;
import net.minecraft.server.MinecraftServer;
import net.minecraft.world.level.Level;
import net.minecraftforge.event.TickEvent;
import snownee.kiwi.schedule.impl.LevelTicker;
import snownee.kiwi.schedule.impl.SimpleLevelTask;

public class MyTask extends SimpleLevelTask {
	public static final ResourceLocation ID = new ResourceLocation("my_mod", "test");
	private String words;

	public MyTask() {
	}

	public MyTask(Level world, TickEvent.Phase phase, String words) {
		super(world, phase, null);
		this.words = words;
	}

	@Override
	public boolean tick(LevelTicker ticker) {
		if (++tick >= 50) {
			MinecraftServer server = ticker.getLevel().getServer();
			if (server != null) {
				TextComponent text = new TextComponent(words);
				server.getPlayerList().broadcastMessage(text, ChatType.SYSTEM, Util.NIL_UUID);
			}
			return true;
		} else {
			return false;
		}
	}

	@Override
	public void deserializeNBT(CompoundTag data) {
		super.deserializeNBT(data);
		words = data.getString("words");
	}

	@Override
	public CompoundTag serializeNBT() {
		CompoundTag data = super.serializeNBT();
		data.putString("words", words);
		return data;
	}
}
```

最后如果你需要持久化你的 Task，别忘了注册：

```java
@Override
protected void serverInit(ServerInitEvent event) {
	Scheduler.register(MyTask.ID, MyTask.class);
}
```
