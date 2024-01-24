# 0003-entt-view

## Date

2026-06-27

## Lesson

第3课：EnTT View — 批量查询实体

## Key Insights

1. **View = 筛选 + 迭代**：`reg.view<A, B, C>()` 返回所有同时拥有组件 A、B、C 的实体集合。这是系统批量操作的基础。

2. **组件越多，筛选越精确**：
   - `view<Position>()` → 所有有位置的
   - `view<Position, Player>()` → 只有玩家
   - `view<Target, ChaseMode, BlinkyChaseTarget>()` → 只有追逐模式的 Blinky

3. **view.get() vs reg.get()**：在 view 循环内必须用 `view.get<T>(e)`（更快），view 循环外访问其他实体用 `reg.get<T>(other)`。

4. **多 View 模式**：碰撞检测等需要比较两类实体时，创建两个独立的 view 分别迭代（如 playerGhostCollide 系统）。

5. **迭代中修改当前实体的组件是安全的**：remove/emplace 当前实体不会有迭代器失效问题。

6. **设计原则**：View 的组件列表直接定义了系统的职责范围。写新系统时，第一步就是确定 view 签名。
