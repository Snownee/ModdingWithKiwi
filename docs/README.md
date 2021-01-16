# 前言

## 欢迎

欢迎使用 Kiwi！

Kiwi 是一个集合了许多实用功能的前置库，它能够协助你方便快捷地开发 Forge 模组。

本指南面向 Minecraft 1.15+（Kiwi 2.0+），不适用于旧版本。使用前请确定你已经对 Java 和 Forge 有了足够的了解。

[使用 Kiwi 开发的模组](https://www.curseforge.com/minecraft/mc-mods/kiwi/relations/dependents?filter-related-dependents=3)
你可以参考以上模组来更好地理解 Kiwi 的使用方法。让我们来看看它们都用到了哪些特性：

 - [Snow! Real Magic!](https://www.curseforge.com/minecraft/mc-mods/snow-real-magic)：替换原版注册项、可变材质方块
 - [Fruit Trees](https://www.curseforge.com/minecraft/mc-mods/fruit-trees)：模块加载条件、添加新的配方类型

## 准备工作

### 加入依赖

这里有两种方式添加依赖文件：通过 Curse Maven 添加或本地添加。推荐使用 Curse Maven 添加。

#### Curse Maven

具体用法参见 https://www.cursemaven.com/

此时你的 `build.gradle` 的一部分可能是这样的：

```groovy
dependencies {
    minecraft "net.minecraftforge:forge:1.15.2-31.1.0"
    compile fg.deobf("curse.maven:kiwi-303657:3103508")
}
```

### 本地添加

在[这里](https://www.curseforge.com/minecraft/mc-mods/kiwi/files)下载未混淆（deobf）版本的 Kiwi。打开你项目中的 `build.gradle` ，在 `dependencies` 中添加一行：

```groovy
compile files("Kiwi-${minecraft_version}-${kiwi_version}-deobf.jar")
```

将 `${minecraft_version}` 和 `${kiwi_version}` 替换为下载到的版本后，此时你的 `build.gradle` 的一部分可能是这样的：

```groovy
dependencies {
    minecraft "net.minecraftforge:forge:1.15.2-31.1.0"
    compile files("Kiwi-1.15.2-2.8.5-deobf.jar")
}
```

重新部署你的开发环境。这时若运行项目时 Kiwi 被加载，即说明准备工作已经完成。

**注意！由于本地添加不支持反混淆，你所使用的 MCP 映射版本必须与 Kiwi 使用的一致，否则会出现 NoSuchMethodException 等问题。**

### 向模组加载器声明前置

记得在 mods.toml 中将 Kiwi 声明为前置模组：

```toml
[[dependencies.my_mod]]
    modId="kiwi"
    mandatory=true
    versionRange="[2.8, 2.12)"
    ordering="BEFORE"
    side="BOTH"
```
