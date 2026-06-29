# 0004-game-loop

## Date
2026-06-27

## Lesson
第4课：游戏主循环

## Key Insights

1. **两层分离**：Application 管 SDL 平台，Game 管 ECS 逻辑。Game 不依赖 SDL，换渲染库只需改 Application。

2. **逻辑帧 ≠ 渲染帧**：逻辑每 8 帧跑一次（2.5 Hz），渲染每帧都画（20 FPS）。frame % tileSize（0~7）是插值参数，实现像素级平滑过渡。

3. **系统调用顺序至关重要**：每一步读前一步的输出。顺序错 = 逻辑滞后或错误。原作者的注释强调：不要用抽象隐藏系统顺序。

4. **正确的数据流**：输入 → wallCollide → movement → eatDots → setTarget → pursueTarget → collideDetection。这是一个单向流水线。

5. **ECS 内外状态划分**：附着在单个实体上的状态放进组件（位置、方向、模式）；全局/共享状态留在 Game 类（迷宫、总豆子数、游戏状态、随机数生成器）。
