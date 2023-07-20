# ASim笔记

特点：模块化，可重用，性能建模

设计团队：VAX and Alpha procesors developing model team

目标：为复杂处理器构建细节的时序模型，高效管理大型软件工程项目，modeling, maintaining, extending, and managing processor and system architecture models



## 基础组件

asimi中，基础的模块通常代表一个物理设计如cache，或者代表硬件算法的行为，例如cache替换算法。

asim模块的接口允许在不同的上下文中重用，并且模块的算法实现部分能够进行更换。

asim的核心是Ports接口，抽象了模块间的通信channel和模块间的时序特性。

asim将指令输入和和性能模型拆开，二者独立，使用下图中的Feeder作为输入，Feeder作为功能建模，而performance model管理时序预测，包括错误的speculation导致的路径问题。最左边的是一组静态的module，通过中间的workbench configuator选中特定模块来运行，右边是performance与feeder的交互，runtime diplay用于按周期显示处理器的活动

<img src="./img/asim%20notes/image-20230720105543202.png" alt="image-20230720105543202" style="zoom: 50%;" />

asim同时支持timing和独立的性能模型。timing model可以预测全部或者部分程序所需时间，独立的性能模型能够单独研究每个模块行为。举例：使用独立模型预测分支预测的准确性，但是为了查看时序行为，需要将独立模型接入完整的处理器时序模型中。

## 模块

asim用c++表征硬件结构、lib库和工具集，通常使用#ifdef条件编译实现性能模型算法的切换，但是会导致接口复杂，不利于代码重用。

### expressing alternate designs

asim中，使用c++ class代表不同的module,如下图branch_predictor_class是通用module，该class从asim_module_class继承。asim_module_class提供了极大的灵活性，例如可以多次实例化一个代表cpu的module来创建多处理器。asim要求module的接口定义优良，从而能够在多个上下文中重用或者使用不用的算法实现，下图的3种分支预测算法就能够实现直接替换。

![image-20230720113405249](./img/asim%20notes/image-20230720113405249.png)

asim提供了ports和method-call两种接口。ports接口中，代表硬件box的module使用ports按时钟周期通信；method-call接口中，只对功能进行通信，通常嵌入在父模块中的子模块，仅在周期内通信并且不使用硬件组件的情况下，用method-call.

### managing design choices

一个完整的处理器有上百个模块，每个模块都有不同的策略，模块间也互有依赖关系，因此需要一个管理框架。asim使用两种机制管理依赖关系：

1. 每个模块都有asim module type, 定义功能但隐藏接口，并且指定了所需的module和type，如下图provide predictor和require algorithm，

   <img src="./img/asim%20notes/image-20230720145633506.png" alt="image-20230720145633506" style="zoom:50%;" />

2. asim使用.awb文件单独维护每个模块的信息，例如gshare_branch_predictor.awb文件会存放gshare.h和gshare..cpp以及相关的模块信息和任何必要的依赖。.awb文件中，module可以定义多个对应的属性，属性指示module与特定的完整设计或者兼容模块相关。

### Ports

wire delay对处理器性能有很大的影响，因此硬件中的delay必须作为硬件最高优先级的抽象。为了使得模块间的接口更加清晰和更好定义，于是有了Ports。Ports的功能如下：

1. module通过ports交换信息，真实处理器并不能在任何位置都能即时访问全局信息，处理器的状态分布在多个module中，这些module需要花费多个cycle通信，通过显式通信来访问其他部件，ports代表通信channel，使得module能够相互通信彼此的状态。

<img src="./img/asim%20notes/image-20230720160603501.png" alt="image-20230720160603501" style="zoom:50%;" />

2. asim使用port模拟模块间的通信delay，asim建立了同步时钟系统，每个模块都有对应的clock function，clock every module per cycle，在模拟时调用clock function。module定义了在一个cycle内需要做的所有Logic行为，例如通过port接受或者发送信息。
3. ports支持latency和bandwidth，只有满足固定latency，接收方才能收到发送方的信息，同时bandwidth规定了最大承载的信息量。
4. 使用ports对硬件结构内的dealy建模，例如上图中dcache到functional unit隐含了一个周期的delay。

传统性能模型使用方法调用和全局变量在硬件结构之间传递信息，这两种方法缺乏校验，会出现意想不到的bug。使用ports使得开发人员必须指定module间的latency，避免了大量的估计误差。例如asim中的分支预测module有两个ports,一个请求预测，一个返回预测，必须满足latency的delay之后才能获得有用的预测信息。

asim通过fifo队列实现ports，每个module声明port的一端。使用utility代码处理输入和输出port，utility代码在初始化时运行，一个模块中的子模块可以连接到其他模块的子模块，并且父模块不知情。

### Feeders

传统的性能模型只针对影响性能的操作进行建模，通过使用trace或者抽象的架构模拟来避免陷入执行细节，feeders支持这些功能。feeders根据不同的需求提供不同的接口。

之后的略。



**可以参考时序模型 module设计 ports设计**

