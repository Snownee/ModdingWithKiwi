# TileEntity

## 介绍

Kiwi 的 TileEntity 基类拥有更方便的数据管理功能。

在基类 BaseTile 中，数据同步和数据存储的方法被分开：

```java
import net.minecraft.nbt.CompoundNBT;
import snownee.kiwi.tile.BaseTile;

public class MyTile extends BaseTile
{

    public MyTile()
    {
        super(MyModule.MY_TILE);
    }

    @Override
    protected void readPacketData(CompoundNBT data)
    {
    }

    @Override
    protected CompoundNBT writePacketData(CompoundNBT data)
    {
        return data;
    }

    @Override
    public void read(CompoundNBT data)
    {
        super.read(data);
    }

    @Override
    public CompoundNBT write(CompoundNBT data)
    {
        return super.write(data);
    }
}
```

不过通常你可以直接这么写：

```java
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
```

将服务端数据同步至客户端：

```java
refresh();
```

## 方块破坏时保留数据

```java
public class MyTile extends BaseTile
{

    public MyTile()
    {
        super(MyModule.MY_TILE);
        persistData = true;
    }

    ...

}
```
