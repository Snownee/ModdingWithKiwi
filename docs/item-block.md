# 物品 / 方块拓展

Kiwi 对 Item / Block 类进行了拓展，推荐直接继承这些类编写自己的物品 / 方块。

## 气泡提示

你可以非常方便地为物品 / 方块添加气泡提示信息。

```text
{
  "item.my_mod.cool_item": "Cool Item",
  "item.my_mod.cool_item.tip": "你正在看着一个很酷的物品！",
  "item.my_mod.cool_item.tip.shift": "这条提示仅会在按住 Shift 时显示！",
  "item.my_mod.cool_item.tip.ctrl": "这条提示仅会在按住 Ctrl 时显示！"
}
```

玩家可以在配置文件中调整提示信息的换行宽度以及显示方式。

当高级气泡提示（`F3` + `H`）开启并按下 `Shift` 时，会在气泡中打印该物品的 NBT。

## 属性推断

试想你一般是如何注册一个方块的：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final Block COOL_BLOCK = new Block(blockProp(Material.WOOD));
}
```

结束了吗？并没有。你的方块不会燃烧，发出石头的声音，也没有硬度，这意味着它会被瞬间破坏。而 Kiwi 会帮你根据方块的 Material 自动设置这一切。你只需要：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final Block COOL_BLOCK = new ModBlock(blockProp(Material.WOOD));
}
```

当然，总有些时候你的方块无法继承 ModBlock，这时你可以这样做：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final SlabBlock COOL_SLAB = init(new SlabBlock(blockProp(Material.WOOD)));
}
```

## 注册标签

你可以轻易在模块中引入标签：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final Tag<Block> THONK = blockTag("my_mod", "thonk");
    public static final Tag<EntityType<?>> BAT = entityTag("my_mod", "bat");
}
```

## 在客户端写入 TileEntity 数据

原版中，BlockItem 通过 NBT 中 `BlockEntityTag` 标签写入 TileEntity 数据只在服务端发生。有些时候你希望数据瞬间被更新，这时你只需要：

```java
ModBlockItem.INSTANT_UPDATE_TILES.add(TILE_ENTITY_TYPE);
```
