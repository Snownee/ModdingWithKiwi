# 标签

## TagsProviderHelper

### getModEntries(String modId, Registry<T> registry)

获取 registry 中所有命名空间为 modId 的游戏对象。

```java
TagsProviderHelper.getModEntries(modId, registry);
```

### addOptional(TagAppender<T> builder, T... objects)

批量添加可选标签。

## KiwiBlockTagsProvider

主要添加了根据方块 Material 设置所需挖掘工具的功能。

```java
@Override
protected void addTags() {
	TagsProviderHelper.getModEntries(modId, registry).forEach(this::processTools);
	...
}
```