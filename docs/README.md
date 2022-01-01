# 前言

## 欢迎

欢迎使用 Kiwi！

Kiwi 是一个集合了许多实用功能的前置库，它能够协助你方便快捷地开发 Forge 模组。

本指南面向 Minecraft 1.18+（Kiwi 5.2+），不适用于旧版本。使用前请确定你已经对 Java 和 Forge 有了一定的了解。

Kiwi 同时支持 Fabric 开发。但本教程还未补充在 Fabric 开发的相关信息，故本指南目前仅适用于 Forge 开发。

[使用 Kiwi 开发的模组](https://www.curseforge.com/minecraft/mc-mods/kiwi/relations/dependents?filter-related-dependents=3)

你可以参考以上模组来更好地理解 Kiwi 的使用方法。

## 准备工作

### 加入依赖

这里有两种方式添加依赖文件：通过 CurseMaven 添加或本地添加。

#### CurseMaven（推荐）

具体用法参见 [CurseMaven](https://www.cursemaven.com/)

此时你的 `build.gradle` 的一部分可能是这样的：

```groovy
dependencies {
	minecraft "net.minecraftforge:forge:1.18.1-39.0.9"
	implementation fg.deobf("curse.maven:kiwi-303657:${kiwi_fileId}")
    annotationProcessor "curse.maven:kiwi-303657:${kiwi_fileId}"
}
```

[**文件ID参考**](https://www.curseforge.com/minecraft/mc-mods/kiwi/files/all)

#### 本地添加

将 Kiwi 放入项目的 libs 目录。编辑 `build.gradle`，在 `repositories` 中添加：

```groovy
flatDir { dir 'libs' }
```

在 `repositories` 中添加：

```groovy
implementation fg.deobf("libs:Kiwi-1.18.1:5.2.3")
annotationProcessor "libs:Kiwi-1.18.1:5.2.3"
```

重新部署你的开发环境。这时若运行项目时 Kiwi 被加载，即说明准备工作已经完成。

### 向模组加载器声明前置

记得在 `mods.toml` 中将 Kiwi 声明为前置模组：

```toml
[[dependencies.my_mod]]
    modId="kiwi"
    mandatory=true
    versionRange="[5, 6)"
    ordering="NONE"
    side="BOTH"
```

## `${mod_id}.kiwi.json` 文件

`${mod_id}.kiwi.json` 文件会在模组构建时自动生成于根目录。该文件列出了模组内几个主要注解的目标类，免去了用户在生产环境每次启动时都需要进行类扫描的麻烦。

!!! Note
	Fabric 平台下，由于限制，开发环境中无法通过正常方式进行类扫描，因此你需要自行将 `${mod_id}.kiwi.json` 文件放入 `resources` 目录。同时记得为模组主类添加 `@Mod` 注解来指定模组的ID。