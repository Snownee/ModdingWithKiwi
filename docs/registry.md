# 自动注册

## 介绍

模块提供对所有 IForgeRegistryEntry 的自动注册。修饰符必须为 `public static final`。

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final Item COOL_ITEM = new ModItem(new Item.Properties().rarity(Rarity.EPIC));

    public static final Block COOL_BLOCK = new ModBlock(Block.Properties.create(Material.WOOD));
}
```

这样你就会发现它们以 `my_mod:cool_item` 和 `my_mod:cool_block` 被注册了。而 COOL\_BLOCK 相应的 `BlockItem` 也会被自动注册。

物品和方块并非必须继承 ModItem 和 ModBlock，但这样做的好处会在之后提到。

你可以这样来简化代码：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final Item COOL_ITEM = new ModItem(itemProp().rarity(Rarity.EPIC));

    public static final Block COOL_BLOCK = new ModBlock(blockProp(Material.WOOD));
}
```

## 字段注解

### `@Skip`

使用 `@Skip` 可以跳过该字段的自动注册。

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    @Skip
    public static final Item COOL_ITEM = new ModItem(itemProp());
}
```

### `@NoItem`

有些方块不需要注册相应的 BlockItem。使用 `@NoItem` 可以禁用该功能。

### `@NoGroup`

此字段可以禁用物品或方块的默认 ItemGroup 设置。

### `@Name`

重新命名此注册项，而不是使用字段名。

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    @NoItem
    public static final Block MY_TORCH = new MyTorchBlock();

    @NoItem
    public static final Block MY_WALL_TORCH = new MyWallTorchBlock();

    @Name("my_torch")
    public static final Item MY_TORCH_ITEM = new WallOrFloorItem(MY_TORCH, MY_WALL_TORCH, itemProp());
}
```

你也可以用此字段替换已有的注册项：

```java
@Name("minecraft:torch")
public static final Item TORCH = ...;
```

## 自定义 BlockItem 属性

自动注册的方块会尝试将前一个 Item.Properties 应用于它的 BlockItem。`@Skip` 可以跳过前一个 Item.Properties。

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final Item.Properties COOL_BLOCK_ITEM_BUILDER = itemProp().rarity(Rarity.RARE);
    public static final Block COOL_BLOCK = new ModBlock(blockProp(Material.WOOD));
}
```

!!! Warning

  该 Item.Properties 被使用后会被设置为 null

## 设置默认 ItemGroup

手动为模块内的每个物品和方块设置 ItemGroup 是一件相当麻烦的事情，为此 Kiwi 提供了这种方式注入默认的 ItemGroup。

```java
@KiwiModule
@KiwiModule.Group
public class MyModule extends AbstractModule
{
    public static final ItemGroup GROUP = new ItemGroup("my_mod.items")
    {
        @Override
        public ItemStack createIcon()
        {
            return new ItemStack(COOL_ITEM);
        }
    };

    public static final Item COOL_ITEM = new ModItem(itemProp());
}
```

如果有 `@KiwiModule.Group` 注解，Kiwi 会将模块中第一个 ItemGroup 字段作为默认，注入到每个无 `@NoGroup` 注解的物品和方块中。

你也可以这样将原版的 ItemGroup 设为默认。可用的值：`building_blocks`, `decorations`, `redstone`, `transportation`, `misc`, `food`, `tools`, `combat`, `brewing`。

```java
@KiwiModule
@KiwiModule.Group("building_blocks")
public class MyModule extends AbstractModule {...}
```

你还可以添加一个 ItemGroup：

```java
KiwiManager.GROUPS.put("my_awesome_item_group", group);
```

```java
@KiwiModule
@KiwiModule.Group("my_awesome_item_group")
public class MyModule extends AbstractModule {...}
```
