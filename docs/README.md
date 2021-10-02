# 前言

## 欢迎

欢迎使用 Kiwi！

Kiwi 是一个集合了许多实用功能的前置库，它能够协助你方便快捷地开发 Forge 模组。

本指南面向 Minecraft 1.17+（Kiwi 4.0+），不适用于旧版本。使用前请确定你已经对 Java 和 Forge 有了一定的了解。

[使用 Kiwi 开发的模组](https://www.curseforge.com/minecraft/mc-mods/kiwi/relations/dependents?filter-related-dependents=3)

你可以参考以上模组来更好地理解 Kiwi 的使用方法。其中用到特殊功能的模组：

 - [Snow! Real Magic!](https://www.curseforge.com/minecraft/mc-mods/snow-real-magic)：替换原版注册项、可变材质方块
 - [Fruit Trees](https://www.curseforge.com/minecraft/mc-mods/fruit-trees)：模块加载条件、添加新的配方类型

## 准备工作

### 加入依赖

这里有两种方式添加依赖文件：通过 Curse Maven 添加或本地添加。

#### Curse Maven（推荐）

具体用法参见 [Curse Maven](https://www.cursemaven.com/)

此时你的 `build.gradle` 的一部分可能是这样的：

```groovy
dependencies {
    minecraft "net.minecraftforge:forge:1.17.1-37.0.73"
	implementation fg.deobf("curse.maven:jade-324717:3468944")
}
```

#### 本地添加

将 Kiwi 放入项目的 libs 目录。编辑 `build.gradle`，在 `repositories` 中添加：

```groovy
flatDir { dir 'libs' }
```

在 `repositories` 中添加：

```groovy
implementation fg.deobf("libs:Kiwi-1.17.1:4.1.0")
```

重新部署你的开发环境。这时若运行项目时 Kiwi 被加载，即说明准备工作已经完成。

### 向模组加载器声明前置

记得在 mods.toml 中将 Kiwi 声明为前置模组：

```toml
[[dependencies.my_mod]]
    modId="kiwi"
    mandatory=true
    versionRange="[4, 5)"
    ordering="BEFORE"
    side="BOTH"
```
