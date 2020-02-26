# 前言

## 欢迎

欢迎使用 Kiwi！

Kiwi 是一个集合了许多实用功能的前置库，它能够协助你方便快捷地开发 Forge 模组。

本指南面向 Minecraft 1.14+（Kiwi 2.0+），不适用于旧版本。使用前请确定你已经对 Java 和 Forge 有了足够的了解。

[使用 Kiwi 开发的模组](https://www.curseforge.com/minecraft/mc-mods/kiwi/relations/dependents?filter-related-dependents=3)
你可以参考以上模组来更好地理解 Kiwi 的使用方法。让我们来看看它们都用到了哪些特性：

 - [Snow! Real Magic!](https://www.curseforge.com/minecraft/mc-mods/snow-real-magic)：替换原版注册项、可变材质方块
 - [Fruit Trees](https://www.curseforge.com/minecraft/mc-mods/fruit-trees)：模块加载条件、添加新的配方类型

## 准备工作

在[这里](https://www.curseforge.com/minecraft/mc-mods/kiwi/files)下载未混淆（deobf）版本的 Kiwi。打开你项目中的 `build.gradle` ，在 `dependencies` 中添加一行：

```groovy
compile files("Kiwi-${minecraft_version}-${kiwi_version}-deobf.jar")
```

将 `${minecraft_version}` 和 `${kiwi_version}` 替换为下载到的版本后，此时你的 `build.gradle` 的一部分可能是这样的：

```groovy
dependencies {
    minecraft "net.minecraftforge:forge:1.15.2-31.1.0"
    compile files("Kiwi-1.15.2-2.7.0-deobf.jar")
}
```

重新部署你的开发环境。这时若运行项目时 Kiwi 被加载，即说明准备工作已经完成。

记得在 mods.toml 中将 Kiwi 声明为前置模组：

```toml
[[dependencies.my_mod]]
    modId="kiwi"
    mandatory=true
    versionRange="[2.7, 2.11)"
    ordering="BEFORE"
    side="BOTH"
```
