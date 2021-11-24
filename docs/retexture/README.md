# 可变贴图方块

回想一下匠魂中可用任意金属块制作的工具装配台，或是 Blockcraftery。可变贴图方块就是用来实现类似功能的。

首先这是我们需要支持的模型：

my_block.json:

```json
{
    "parent": "block/cube_all",
    "textures": {
        "all": "block/glowstone"
    }
}
```

其中 `all` 指定了所有面的贴图。我们需要在适当时候告诉模型这个 key 是可变贴图的。

对模型文件设置 Forge 模型管线的模型加载器，一般为 `minecraft:`

my_block.json:

```json hl_lines="2"
{
	"loader": "elements",
	"parent": "block/cube_all",
	"textures": {
		"all": "block/glowstone"
	}
}
```

包装一层模型，通知管线其贴图可被修改。

retex_my_block.json:

```json
{
	"loader": "kiwi:retexture",
	"base": "mymod:block/my_block"
}
```

## 实现 BlockEntity

一般情况下，该方块应该创建 BlockEntity，且 BlockEntity 继承自 `snownee.kiwi.block.entity.RetextureBlockEntity`。

一个最基本的实现：

```java
public class TestBlockEntity extends RetextureBlockEntity {
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
	当你像给一些原版方块（主要是台阶）添加 BlockEntity 时，注意方块是否复写了 onRemove 方法，避免出现 BlockEntity 滞留问题。

## 注册 BlockEntity

```java
public static TestBlock MY_BLOCK = new TestBlock(blockProp(Material.WOOD));
public static BlockEntityType<?> MY_TILE = BlockEntityType.Builder.of(TestBlockEntity::new, MY_BLOCK).build(null);

@Override
@OnlyIn(Dist.CLIENT)
public void clientInit(ClientInitEvent event) {
	// 使用户在放下方块时贴图能立即更新
	ModBlockItem.INSTANT_UPDATE_TILES.add(MY_TILE);
}
```

!!! Note
	需要得到被标记 key 的物品名？只需使用 `Util.getBlockDefName(stack, key)`

## RenderType

如果你需要让方块支持透明或半透明方块，则在 `clientInit` 中执行如下代码：

```java
ItemBlockRenderTypes.setRenderLayer(MY_BLOCK, EnumUtil.BLOCK_RENDER_TYPES::contains);
```

!!! Note
	如果想要正常面剔除仍需要编写额外的 mixin