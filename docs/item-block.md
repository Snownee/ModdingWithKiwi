# 物品 / 方块拓展

Kiwi 对 Item / Block 类进行了拓展，推荐直接继承这些类编写自己的物品 / 方块。

## 气泡提示

你可以非常方便地为物品 / 方块添加气泡提示信息。

```json
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

注册方块时，你可以使用辅助方法 `blockProp` 快速创建 AbstractBlock.Properties：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final Block COOL_BLOCK = new Block(blockProp(Material.WOOD));
}
```

在辅助方法 `blockProp` 中，Kiwi 会根据你选定的 Material 设置一个默认的硬度和声音类型。

从3.5版本开始，Kiwi 会自动为所有方块设置可燃性。

## 注册标签

你可以轻易在模块中引入标签：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static final INamedTag<Block> THONK = blockTag("my_mod", "thonk");
    public static final INamedTag<EntityType<?>> BAT = entityTag("my_mod", "bat");
}
```

## 在客户端写入 TileEntity 数据

原版中，BlockItem 通过 NBT 中 `BlockEntityTag` 标签写入 TileEntity 数据只在服务端发生。有些时候你希望数据瞬间被更新，这时你只需要：

```java
ModBlockItem.INSTANT_UPDATE_TILES.add(TILE_ENTITY_TYPE);
```
