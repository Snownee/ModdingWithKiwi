# 可变贴图方块

!!! Note
	仅支持原版的方块模型和 Multipart 模型。

回想一下匠魂中的工具装配台，或是 Blockcraftery。可变贴图方块就是用来实现类似功能的。

首先我们需要一个最基本的模型：

```json
{
    "parent": "block/cube_all",
    "textures": {
        "all": "block/glowstone"
    }
}
```

其中 `all` 指定了所有面的贴图。我们需要在适当时候告诉模型这个 key 是可变贴图的。

## 注册模型

```java
@SubscribeEvent
@OnlyIn(Dist.CLIENT)
public void onModelBake(ModelBakeEvent event) {
	Block block = MyModule.COOL_BLOCK;
	BlockState inventoryState = block.defaultBlockState();
	TextureModel.register(event, block, inventoryState, "all"); // 最后一个参数为粒子所使用的key
	ModBlockItem.INSTANT_UPDATE_TILES.add(COOL_BLOCK_TILE);
}
```

其中 `inventoryState` 指定了该方块以物品形式显示时的 BlockState。它可以为空，表示不变更。

有时候你需要为方块的物品添加一个独立的模型。Kiwi 完全能应对这一需求：

```java
@SubscribeEvent
@OnlyIn(Dist.CLIENT)
public void onModelBake(ModelBakeEvent event) {
    Block block = MyModule.COOL_BLOCK;
    TextureModel.register(event, block, null, "all");
    TextureModel.registerInventory(event, block, "all");

    ModBlockItem.INSTANT_UPDATE_TILES.add(COOL_BLOCK_TILE);
}
```

## 实现 BlockEntity

一般情况下，该方块应该创建 BlockEntity，且 BlockEntity 继承自 `snownee.kiwi.block.entity.TextureBlockEntity`。

一个最基本的实现：

```java
public class TestBlockEntity extends TextureBlockEntity {
	public TestBlockEntity(BlockPos pos, BlockState state) {
		super(MyModule.COOL_BLOCK_TILE, pos, state, "all"); // all为可变贴图的key，支持多个key
	}

	@Override
	public void load(CompoundTag compound) {
		readPacketData(compound);
		super.load(compound);
	}

	@Override
	public CompoundTag save(CompoundTag compound) {
		writePacketData(compound);
		return super.save(compound);
	}

}
```

!!! Note
	可能你已经注意到了：应用的贴图是根据方块的粒子来决定的。所以此功能无法满足你“做个木头（不是木板）台阶”的需求。（因为粒子贴图不能同时为顶面和侧面）
