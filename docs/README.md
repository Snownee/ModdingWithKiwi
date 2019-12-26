# 前言

## 目录

* [模块系统](module/README.md)
* [自动注册](registry/README.md)
* 实用类
  * [NBTHelper](utils/nbthelper.md)
  * [杂项](utils/util.md)
* [物品 / 方块拓展](item-block/README.md)
* [TileEntity](tileentity/README.md)
* [网络](network/README.md)
* 配方
  * [条件：模块加载](recipe/module-loaded.md)
  * [配方：消耗容器](recipe/no-containers.md)
  * [动态配方](recipe/dynamic.md)
* [可变材质方块](textureable/README.md)
  * [合成配方](textureable/crafting.md)
* [任务系统](schedule/README.md)
* [内置命令](command/README.md)

## 欢迎

欢迎使用 Kiwi！

Kiwi 是一个集合了许多实用功能的前置库，它能够协助你方便快捷地开发 Forge 模组。

本指南面向 Minecraft 1.14+（Kiwi 2.0+），不适用于旧版本。使用前请确定你已经对 Java 和 Forge 有了足够的了解。

## 准备工作

在[这里](https://www.curseforge.com/minecraft/mc-mods/kiwi/files)下载未混淆（deobf）版本的 Kiwi。打开你项目中的 `build.gradle` ，在 `dependencies` 中添加一行：

```groovy
compile files("Kiwi-${minecraft_version}-${kiwi_version}-deobf.jar")
```

将 `${minecraft_version}` 和 `${kiwi_version}` 替换为下载到的版本后，此时你的 `build.gradle` 的一部分可能是这样的：

```groovy
dependencies {
    minecraft "net.minecraftforge:forge:1.14.4-28.1.59"
	compile files("Kiwi-1.14.4-2.4.2-deobf.jar")
}
```

重新部署你的开发环境。这时若运行项目时 Kiwi 被加载，即说明准备工作已经完成。

记得在 mods.toml 中将 Kiwi 声明为前置模组：

```toml
[[dependencies.my_mod]]
    modId="kiwi"
    mandatory=true
    versionRange="[2.4, 2.7)"
    ordering="BEFORE"
    side="BOTH"
```