# 模块系统

## 介绍

Kiwi 以模块为单位加载功能和维护生命周期。一个最基本的模块会是这样的：

```java
import snownee.kiwi.AbstractModule;
import snownee.kiwi.KiwiModule;

@KiwiModule
public class MyModule extends AbstractModule
{
}
```

你可以通过复写方法来订阅生命周期的事件：

```java
import net.minecraftforge.api.distmarker.Dist;
import net.minecraftforge.api.distmarker.OnlyIn;
import net.minecraftforge.fml.event.lifecycle.FMLClientSetupEvent;
import net.minecraftforge.fml.event.lifecycle.FMLCommonSetupEvent;
import net.minecraftforge.fml.event.server.FMLServerStartingEvent;
import snownee.kiwi.AbstractModule;
import snownee.kiwi.KiwiModule;

@KiwiModule
public class MyModule extends AbstractModule
{
    @Override
    protected void preInit() {}

    @Override
    protected void init(FMLCommonSetupEvent event) {}

    @Override
    @OnlyIn(Dist.CLIENT)
    protected void clientInit(FMLClientSetupEvent event) {}

    @Override
    protected void serverInit(FMLServerStartingEvent event) {}

    @Override
    protected void postInit() {}
}
```

## `@KiwiModule`

所有模块都应该添加这一注解，用于自动加载。

一个模组可以拥有多个模块，不同的模块通过 name 区分。name 默认同 modid。

```java
@KiwiModule(name = "test")
```

某些情况下你可能需要手动指定该模块所属的模组：

```java
@KiwiModule(modid = "another_mod", name = "test")
```

你可以令模块仅当前置模组安装时才加载：

```java
@KiwiModule(dependencies = "crafttweaker;jei")
```

你甚至可以声明该模块的前置模块：

```java
@KiwiModule(dependencies = "@my_mod:dab")
// 等效但是更短：
@KiwiModule(dependencies = "@dab")
```

## `@KiwiModule.Optional`

此注解可令该模块通过配置文件禁用。配置文件名为 kiwi.toml ，它的结构大概长这样：

```text
[modules]
	[modules.my_mod]
		my_mod = true
		test = false
```

添加了该注解的模块默认启用。但你可以这样将其设为默认禁用：

```java
@KiwiModule.Optional(disabledByDefault = true)
```

## `@KiwiModule.Subscriber`

将该模块类作为 Event Handler，仅在模块被加载时订阅事件。

```java
@KiwiModule
@KiwiModule.Subscriber(side = Dist.CLIENT)
public class MyModule extends AbstractModule
{
    @SubscribeEvent
    @OnlyIn(Dist.CLIENT)
    public void onModelBake(ModelBakeEvent event)
    {
        // do sth
        // 注意：非静态方法！
    }
}
```

### 指定事件总线（默认 `MinecraftForge.EVENT_BUS`）

```java
@KiwiModule.Subscriber(Bus.MOD)
```

### 指定物理端（默认客户端和服务端）

```java
@KiwiModule.Subscriber(side = Dist.CLIENT)
```

## `@KiwiModule.Group`

设置该模块下的物品默认的 ItemGroup。将在下一节详细讲解。

## 实例注入

这种方式可以令 Kiwi 在启用此模块时直接将实例注入到该字段中：

```java
@KiwiModule
public class MyModule extends AbstractModule
{
    public static MyModule INSTANCE;
}
```

## 自定义加载条件

有时，仅仅通过配置文件或依赖来判断模块是否需要加载仍满足不了我们的需求。比如我们希望模块仅在 Mixin 生效时加载。这时候我们需要用到 `@KiwiModule.LoadingCondition`，将静态方法放在任意位置即可。

```java
@LoadingCondition("dab")
public static boolean shouldLoadDab(LoadingContext ctx) {
    return Hook.mixin;
}
```
