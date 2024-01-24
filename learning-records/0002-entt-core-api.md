# 0002-entt-core-api

## Date
2026-06-27

## Lesson
第2课：EnTT 核心 API

## Key Insights

1. **Registry 是 ECS 世界的「数据库」**：`entt::registry reg` 管理所有实体和组件。在项目中作为 `Game` 类的成员变量。

2. **create / emplace / get 是最核心的三个操作**：
   - `reg.create()` → 创建空实体（得到一个整型 ID）
   - `reg.emplace<T>(e, args...)` → 挂载组件（覆盖已有同类型）
   - `reg.get<T>(e)` → 读取组件引用（没有则未定义行为）

3. **get vs try_get**：
   - `get` 返回引用，用于你确定组件存在时（如在 view 迭代中）
   - `try_get` 返回指针，没有则返回 nullptr，用于不确定时

4. **运行时改身份 = remove + emplace**：幽灵模式切换（ChaseMode ↔ ScatterMode ↔ ScaredMode）就是通过移除旧标签、挂上新标签实现的。

5. **emplace 覆盖语义**：给已有同类型组件的实体 emplace，会静默覆盖旧数据。
