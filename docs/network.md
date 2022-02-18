# 网络

## 编写 IPacketHandler

IPacketHandler 的主要功能是处理包接收方的行为。

一般情况下可以使用 IPacketHandler 的默认实现 PacketHandler。

```java
// 设置包的类型名及发送方向。并自动注册
@KiwiPacket(value = "uniqueId", dir = Direction.PLAY_TO_CLIENT)
public class MyPacket extends PacketHandler {
	// 将自动注册的实例注入到 I 中
	public static MyPacket I;

	@Override
	public CompletableFuture<FriendlyByteBuf> receive(Function<Runnable, CompletableFuture<FriendlyByteBuf>> executor, FriendlyByteBuf buf, @Nullable ServerPlayer sender) {
		int number = buf.readVarInt();
		// 在主线程上执行游戏相关行为
		return executor.apply(() -> System.out.println(number));
		// return CompletableFuture.completedFuture(null);
	}

	// 助手方法
	public static void send(ServerPlayer player, int n) {
		I.send(player, $ -> $.writeVarInt(n));
	}
}
```

## 发送包

```java
MyPacket.send(player, 42);
```
