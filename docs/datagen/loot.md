# 战利品表

## KiwiLootTableProvider

```java
@Override
public void gatherData(GatherDataEvent event) {
	DataGenerator generator = event.getGenerator();
	if (event.includeServer()) {
		KiwiLootTableProvider provider = new KiwiLootTableProvider(generator);
		provider.add(CoreBlockLoot::new, LootContextParamSets.BLOCK);
		provider.add(...);
		generator.addProvider(provider);
	}
}
```

## KiwiBlockLoot

```java
public class CoreBlockLoot extends KiwiBlockLoot {

	public CoreBlockLoot() {
		// 指定从该模块中获取所有方块
		super(Util.RL("fruittrees:core"));
	}

	@Override
	protected void _addTables() {
		// 根据方块类设置战利品表（无继承关系）
		handle(DoorBlock.class, $ -> createDoorTable($));
		handle(SlabBlock.class, $ -> createSlabItemTable($));
		handle(FlowerPotBlock.class, $ -> createPotFlowerItemTable(((FlowerPotBlock) $).getContent()));
		// 设置未被匹配情况下的战利品表
		handleDefault($ -> createSingleItemTable($));
	}
}
```

!!! Warning
	由于访问性问题，不要将 `$ -> createSingleItemTable($)` 替换为 `BlockLoot::createSingleItemTable`。其他方法同理。