# 贡献者奖励

Kiwi 提供了赞助者奖励的支持。你可以分组地给予赞助者们不同饰品作为奖励。

赞助者分为不同等级（tier），不同等级之间没有从属关系。

你可以这样判断玩家是否属于任意或某个等级：

```java
Contributors.isContributor(author, player);
Contributors.isContributor(author, player, tier);
```
