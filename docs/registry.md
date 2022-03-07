# 自动注册

## 介绍

模块提供对所有 IForgeRegistryEntry 的自动注册。修饰符必须含有 `public static`。

```java
@KiwiModule
public class MyModule extends AbstractModule {
	public static final KiwiGO<Item> COOL_ITEM = go(() -> new ModItem(new Item.Properties().rarity(Rarity.EPIC)));

	public static final KiwiGO<ModBlock> COOL_BLOCK = go(() -> new ModBlock(BlockBehaviour.Properties.of(Material.WOOD)));
}
```

这样你就会发现它们以 `my_mod:cool_item` 和 `my_mod:cool_block` 被注册了。而 COOL\_BLOCK 相应的 `BlockItem` 也会被自动注册。

物品和方块并非必须继承 ModItem 和 ModBlock，但这样做可以帮助你实现某些方法。

你可以这样来简化代码：

```java
@KiwiModule
public class MyModule extends AbstractModule {
	public static final KiwiGO<Item> COOL_ITEM = go(() -> new ModItem(itemProp().rarity(Rarity.EPIC)));

	public static final KiwiGO<ModBlock> COOL_BLOCK = go(() -> new ModBlock(blockProp(Material.WOOD)));
}
```

## 字段注解

### `@Skip`

使用 `@Skip` 可以跳过该字段的自动注册。

```java
@KiwiModule
public class MyModule extends AbstractModule {
	@Skip
	public static final KiwiGO<Item> COOL_ITEM = go(() -> new ModItem(itemProp()));
}
```

### `@NoItem`

有些方块不需要注册相应的 BlockItem。使用 `@NoItem` 可以禁用该功能。

### `@NoCategory`

此字段可以禁用物品或方块的默认 CreativeModeTab 设置。

### `@Name`

重新命名此注册项，而不是使用字段名。

```java
@KiwiModule
public class MyModule extends AbstractModule {
	@NoItem
	public static final KiwiGO<Block> MY_TORCH = go(MyTorchBlock::new);

	@NoItem
	public static final KiwiGO<Block> MY_WALL_TORCH = go(MyWallTorchBlock::new);

	@Name("my_torch")
	public static final KiwiGO<Item> MY_TORCH_ITEM = go(() -> new StandingAndWallBlockItem(MY_TORCH, MY_WALL_TORCH, itemProp()));
}
```

你也可以用此注解替换已有注册项：

```java
@Name("minecraft:torch")
public static final KiwiGO<Item> TORCH = ...;
```

## 自定义 BlockItem 属性

自动注册的方块会尝试将前一个 Item.Properties 应用于它的 BlockItem。`@Skip` 可以跳过前一个 Item.Properties。

```java
@KiwiModule
public class MyModule extends AbstractModule {
	public static Item.Properties COOL_BLOCK_ITEM_BUILDER = itemProp().rarity(Rarity.RARE);
	public static final KiwiGO<Block> COOL_BLOCK = go(() -> new ModBlock(blockProp(Material.WOOD)));
}
```

!!! Warning
	该 Item.Properties 被使用后会被设置为 null

## 设置 CreativeModeTab

### 设置默认 CreativeModeTab

手动为模块内的每个物品和方块设置 CreativeModeTab 是一件相当麻烦的事情，为此 Kiwi 提供了这种方式注入默认的 CreativeModeTab。

```java
@KiwiModule
@Category
public class MyModule extends AbstractModule {
	public static final CreativeModeTab TAB = 
			itemCategory("my_mod", "test", () -> new ItemStack(Items.DANDELION), null);

    public static final KiwiGO<Item> COOL_ITEM = go(() -> new ModItem(itemProp()));
}
```

如果有 `@KiwiModule.Category` 注解，Kiwi 会将模块中第一个 CreativeModeTab 字段作为默认，注入到每个无 `@NoCategory` 或 `@Category` 注解的物品和方块中。

你也可以这样将任意 CreativeModeTab 设为默认。只需填入 CreativeModeTab 的 label 名，如原版的：`building_blocks`, `decorations`, `redstone`, `transportation`, `misc`, `food`, `tools`, `combat`, `brewing`。

```java
@KiwiModule
@Category("building_blocks")
public class MyModule extends AbstractModule {...}
```

```java
@KiwiModule
@Category("my_mod.my_awesome_group")
public class MyModule extends AbstractModule {...}
```

### 为单个注册项设置 CreativeModeTab

```java
@Category("redstone")
public static final KiwiGO<Item> MY_ITEM = go(() -> new Item(itemProp()));

@Category("building_blocks")
public static final KiwiGO<Block> MY_BLOCK = go(() -> new Block(blockProp(Blocks.SAND)));
```

## 为方块设置 RenderType

方式一：直接设置单个方块：

```java
// 允许的值：CUTOUT, CUTOUT_MIPPED, TRANSLUCENT
@RenderLayer(Layer.CUTOUT)
public static final KiwiGO<Block> MY_GLASS = go(() -> new GlassBlock(blockProp(Blocks.GLASS)));
```

方式二：为某个方块类及其子类设置 RenderType：

```java
@RenderLayer(Layer.CUTOUT)
public class MyBlock extends GlassBlock {...}
```

## 自定义 BlockItem

使自己的方块实现 IKiwiBlock 接口，可为方块设置 BlockItem 工厂，或设置 ItemStack 敏感的名字。

```java
public interface IKiwiBlock {

	default MutableComponent getName(ItemStack stack) {
		return new TranslatableComponent(stack.getDescriptionId());
	}

	default BlockItem createItem(Item.Properties builder) {
		return new ModBlockItem((Block) this, builder);
	}

}
```