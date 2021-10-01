# 网络

编写一个 Packet：

```java
import java.util.function.Supplier;

import net.minecraft.network.FriendlyByteBuf;
import net.minecraftforge.fmllegacy.network.NetworkDirection;
import net.minecraftforge.fmllegacy.network.NetworkEvent.Context;
import snownee.kiwi.network.Packet;

public class MyPacket extends Packet {
	private int number;

	public MyPacket(int number) {
		this.number = number;
	}

	public static class Handler extends PacketHandler<MyPacket> {

		@Override
		public void encode(MyPacket msg, FriendlyByteBuf buffer) {
			buffer.writeVarInt(msg.number);
		}

		@Override
		public MyPacket decode(FriendlyByteBuf buffer) {
			return new MyPacket(buffer.readVarInt());
		}

		@Override
		public void handle(MyPacket message, Supplier<Context> ctx) {
			ctx.get().enqueueWork(() -> {
				System.out.println(message.number);
			});
			ctx.get().setPacketHandled(true);
		}

		@Override
		public NetworkDirection direction() {
			return NetworkDirection.PLAY_TO_CLIENT;
		}

	}
}
```

在 preInit 阶段注册这个 Packet：

```java
NetworkChannel.register(MyPacket.class, new MyPacket.Handler());
```

发送这个 Packet：

```java
// 仅发送给位于主世界的玩家
new MyPacket(42).send(PacketDistributor.DIMENSION.with(() -> DimensionType.OVERWORLD));
```

若你的 Packet 只有一种发送方式，可以尝试复写 send 方法：

```java
new MyPacket(42).send();
```

若你的 Packet 只需从客户端发出，可以直接继承 `snownee.kiwi.network.ClientPacket`。使用上述方法发送你的 Packet。
