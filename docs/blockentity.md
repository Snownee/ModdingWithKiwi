# BlockEntity

## 介绍

Kiwi 对 BlockEntity 进行了简单的网络同步封装。

在基类 BaseBlockEntity 中，数据同步和数据存储的方法被分开：

```java
import net.minecraft.core.BlockPos;
import net.minecraft.nbt.CompoundTag;
import net.minecraft.world.level.block.state.BlockState;
import snownee.kiwi.block.entity.BaseBlockEntity;

public class MyBlockEntity extends BaseBlockEntity {

	public MyBlockEntity(BlockPos pos, BlockState state) {
		super(MyModule.MY_TILE, pos, state);
	}

	@Override
	protected void readPacketData(CompoundTag data) {
	}

	@Override
	protected CompoundTag writePacketData(CompoundTag data) {
		return data;
	}

	@Override
	public void load(CompoundTag data) {
		super.load(data);
	}

	@Override
	public CompoundTag save(CompoundTag data) {
		return super.save(data);
	}
}
```

不过通常你可以直接这么写：

```java
@Override
public void load(CompoundTag data) {
	readPacketData(data);
	super.load(data);
}

@Override
public CompoundTag save(CompoundTag data) {
	writePacketData(data);
	return super.save(data);
}
```

将服务端数据同步至客户端：

```java
this.refresh();
```

## 方块破坏时保留数据

```java hl_lines="5"
public class MyBlockEntity extends BaseBlockEntity {

	public MyBlockEntity(BlockPos pos, BlockState state) {
		super(MyModule.MY_TILE, pos, state);
		persistData = true;
	}

    ...
}
```
