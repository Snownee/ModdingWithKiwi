# 可变材质方块

回想一下匠魂中的工具装配台，或是 Blockcraftery。可变材质方块就是用来实现类似功能的。

首先我们需要一个最基本的模型：

```json
{
    "parent": "block/cube_all",
    "textures": {
        "all": "block/glowstone"
    }
}
```

!!! Note
  仅支持原版的方块模型和 Multipart 模型，不支持 Forge 添加的 ForgeBlockStateV1 等格式。

其中 `all` 指定了所有面的材质。我们需要在适当时候告诉模型这个 key 是可变材质的。

```java
@SubscribeEvent
@OnlyIn(Dist.CLIENT)
public void onModelBake(ModelBakeEvent event)
{
    Block block = MyModule.COOL_BLOCK;
    BlockState inventoryState = block.getDefaultState();
    TextureModel.register(event, block, inventoryState, "all");

    ModBlockItem.INSTANT_UPDATE_TILES.add(COOL_BLOCK_TILE);
}
```

其中 `inventoryState` 决定了该方块以物品形式显示时的 BlockState。它可以为空，表示不变更。

它支持同时变更多个 key：

```java
TextureModel.register(event, block, inventoryState, "top", "side", "bottom");
```

有时候你需要为方块的物品添加一个独立的模型。Kiwi 完全能应对这一需求：

```java
@SubscribeEvent
@OnlyIn(Dist.CLIENT)
public void onModelBake(ModelBakeEvent event)
{
    Block block = MyModule.COOL_BLOCK;
    TextureModel.register(event, block, null, "all");
    TextureModel.registerInventory(event, block, "all");

    ModBlockItem.INSTANT_UPDATE_TILES.add(COOL_BLOCK_TILE);
}
```

一般情况下，该方块应该创建 TileEntity，且 TileEntity 继承自 `snownee.kiwi.tile.TextureTile`。

一个最基本的实现：

```java
import net.minecraft.nbt.CompoundNBT;
import snownee.kiwi.tile.TextureTile;

public class TestTile extends TextureTile
{

    public TestTile()
    {
        super(MyModule.COOL_BLOCK_TILE, "all");
    }

    @Override
    public void read(CompoundNBT compound)
    {
        readPacketData(compound);
        super.read(compound);
    }

    @Override
    public CompoundNBT write(CompoundNBT compound)
    {
        writePacketData(compound);
        return super.write(compound);
    }

}
```

可能你已经注意到了：应用的材质是根据方块的粒子来决定的。所以此功能无法满足你“做个木头台阶”的需求。（因为粒子材质不能同时为顶面和侧面）
