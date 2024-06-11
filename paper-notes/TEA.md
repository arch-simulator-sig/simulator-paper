# TEA: Time-Proportional Event Analysis

## Intruction

如果没有先进的性能分析工具，如 Intel VTune 和 AMD μProf，性能调优几乎是不可能实现的。不可能在于两个点：

1. 如何指导应用程序执行哪些指令花费的时间最多，即哪些指令是性能关键指令
2. 为什么指令是性能关键性的， 具体到哪些微架构事件导致指令成为performance-critical指令

如何解决：

1. 使用 performance profilier来解决，指令在性能profile的重要性与指令对整体执行事件的相对贡献成正比，时间比例通过分析指令提交时对性能的影响来实现
2. profilers无法解释为什么在性能关键的指令上花费更多时间？因为没有在微架构中分解指令执行的时间贡献

当前的性能分析方法分为两大类：

指令驱动：AMD IBS  / Arm SPE / Ibm RIS等，在流水线fetch或dispatch阶段标记指令，记录标记的指令遇到的性能事件。但是存在问题，在fetch dispatch阶段标记指令会使得配置文件偏向于在这两个阶段花费大量时间的指令，而不是在提交阶段的指令，导致缺乏时间的比例性。

事件驱动：topdown等，依赖于对性能事件（cache miss, bpu error）的计数，再将事件计数归因到具体的指令，但是存在延迟隐藏效应（refer ch5)， 性能计数与事件对性能的影响不一定完全相关

提出解决方案：

问题1 2 都可以通过创建time-proportional Per-Instruction Cycle Stacks（PICS）来解决，PICS将架构执行的每条指令所花费的时间分解为程序执行过程中遇到的性能事件

PICS的特征：

1. cycle stack的高度与静态指令对整体执行时间的影响成正比 ， 解决了question1
2. cycle stack中每个组成部分的大小与该性能时间对整体性能的影响成正比

注：TIP论文，解决了question1, 但是无法创建pics，因为pics需要将每条静态指令的性能影响分解为该指令在动态执行期间所遇到的事件

建立PICS的挑战：

当前从处理器会建立相当多的性能事件，intel的alder laker PMU可以列出297个不同的性能事件，构建pics需要跟踪所有in-flight的指令，因此，限制跟踪的事件数量是控制开销的关键。

如果解决控制开销的问题？

根据第一性原理，回归本质：将重心放在提交阶段，commit stage有3种非计算的状态：

1. commit stall, 指令在完全执行之前就已经到达rob的头部 ？ 
2. 由于前端stall导致耗尽
3. 由于错误的分支预测导致flush

根据回归本质的分析，提出了TEA : time proportional event analysis 

TEA原理：

添加硬件支持跟踪每条指令在执行过程中遇到的性能事件。具体实现为，为每条动态执行的指令分配一个性能特征向量psv，每个事件占据 1 bit，程序执行期间，TEA使用cycle counter以4kHz频率采样，收集psv数据，PMU根据TIP（注：TIP为该团队上一个工作）的工作描述的事件比例归因策略，检索架构在采样时暴露延迟的指令的指针和对应的psv。
采样就绪后，PMU中断内核，中断处理器程序读取指令指针和PSV，并存储到memory buffer。PSV再进行后续的处理，汇总捕获的信息，为每个指令创建pics。

TEA评测：

1. 开销极低，误差极低
2. 分析spec cpu2017的lbm和nab，lbm的性能问题在于non-hidden load指令（?），增加软件预取解决了问题，性能提高1.28x；nab的性能关键是浮点平方根指令，因为之前的指令flush pipeline，导致指令发出太晚，无法因此执行的延迟，使用-finite-math和-fast-math解决，放宽了ieee 754合法性。

### Intruction 总结

1. pics 解决了上文提到的question1 2
2. TEA可以跟踪in-flight 指令的关键性能信息，获取创建pics所需的信息
3. 在boom上实现了tea，开销低，误差低，（对比商业工具ibs spe ris)
4. 使用tea分析spec cpu2017, 提速



