# 0005-component-design

## Date
2026-06-27

## Lesson
第5课：组件设计

## Key Insights

1. **项目共 12 个组件，分三组**：身份标签（Player/Ghost）、模式标签（ChaseMode/ScatterMode/ScaredMode/EatenMode/EnterHouse/LeaveHouse）、数据组件（Position/DesiredDir/ActualDir/Target/HomePosition/ChaseTarget族/Sprite族）

2. **五种设计模式**：
   - Tag Component：空结构体标记状态，让 view 精确筛选
   - Data Component：纯数据在最外层，无方法、无 private
   - 实体引用：组件字段存 entity ID，使用时解引用取对方数据
   - 门票模式：EnterHouse/LeaveHouse 作为鬼屋进出权限
   - 混合体：ScaredMode 既有标签作用又有计时器数据

3. **独立 struct > enum**：每种幽灵模式用独立 struct，view 在查询阶段就完成筛选，不需要运行时 if/switch。

4. **组件粒度**：每种幽灵有独立的 ChaseTarget 类型——因为每种追逐算法需要的数据不同（Inky 需要额外的 blinky 引用）。

5. **组件矩阵**：Player 挂 6 个组件，每种 Ghost 挂约 9 个组件（含动态增减的模式标签）。
