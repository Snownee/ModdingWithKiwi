# 注解式配置文件

Kiwi 实现了一套类似于 Forge 1.12.2 时代的注解式配置文件系统。

## 定义类

这是示例配置文件类：

```java
import java.util.Arrays;
import java.util.List;

import snownee.kiwi.config.KiwiConfig;
import snownee.kiwi.config.KiwiConfig.*;

@KiwiConfig(type = ConfigType.COMMON)
public class MyConfig {

    public static int intValue = 5;

    @Range(min = 0, max = 114514)
    public static long longValue = 6;

    @Range(min = 0, max = 10.5)
    public static float floatValue = 7.5f;

    @Path("Malay.P")
    @Comment("\\ MalayP /")
    public static String strValue = "MalayP";

    public static boolean booleanValue = true;

    public static List<String> listValue = Arrays.asList("test");

    @WorldRestart
    public static ModConfig.Type enumValue = ModConfig.Type.COMMON;
}
```

这是该类生成的默认配置文件：

```toml
#Range: > -2147483648
intValue = 5
#Range: 0 ~ 114514
longValue = 6
#Range: 0.0 ~ 10.5
floatValue = 7.5
booleanValue = true
listValue = ["test"]
#Allowed Values: COMMON, CLIENT, SERVER
enumValue = "COMMON"

[Malay]
	#\ MalayP /
	P = "MalayP"
```

## 监听选项刷新

你可以在配置类里添加 `onChanged` 静态方法来监听选项刷新。

```java
public static void onChanged(String path) {
	// do sth
}
```

!!! Note
	如果你的模组只使用到了 Kiwi 提供的该系统，你可以将 Kiwi 作为可选依赖。当用户未安装 Kiwi 时，将自动使用默认值。