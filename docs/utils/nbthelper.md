# NBTHelper

NBTHelper 提供了对 CompoundTag 的快捷操作，使你不必担心操作 NBT 时某个中间节点为空。

## 创建

新建一个实例：

```java
NBTHelper data = NBTHelper.create();
```

基于已有的数据创建：

```java
NBTHelper data = NBTHelper.of(tag);
```

```java
NBTHelper data = NBTHelper.of(itemstack);
```

## 操作

```java
data.setInt("a", 1); // { a: 1 }
data.setString("b.c", "2"); // { a: 1, b: { c: "2" } }
data.remove("a"); // { b: { c: "2" } }

data.get(); // { b: { c: "2" } }

data.getString("a"); // null
data.getString("a", "empty"); // "empty"
```



