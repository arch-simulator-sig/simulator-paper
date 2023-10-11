# Computer Architecture Performance Evaluation Methods 读书笔记



## ch1 Introduction

evaluatione的作用：引导开发过程朝正确的方向进行，是关键！

科学研究 vs 系统研究 ：本质上都是作出假设，设计实验验证假设，根据实验运行结果选择继续该方案还是切换方案。其中系统研究的设计实验部分，选用的是针对不同模块的baseline和workload；

区别在于：系统研究需要基于经验，而不是随机抽样。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927122928021.png" alt="image-20230927122928021" style="zoom: 50%;" />

但是性能评估的细节很多，如何选择具有代表性的workload和抽象的建模层级会影响最终的性能



## ch2 Performance Metrics

### 单线程

单线程benchmark的公式如下：T总时间，N(有用的指令数，排除错误分支预测路径的指令)   CPI(cycle per instruction)   f(frequency)

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927124331371.png" alt="image-20230927124331371" style="zoom:50%;" />

Justin Rattner报告中提及，1990-2000年间，x86 处理器性能在 10 年时间内提高了 75 倍以上）：13×来自频率的提升，6×来自微架构的增强，而频率的50×提升又来自技术（13×）和==微架构==（4×）的改进 :)

ipc vs cpi :  假设时钟频率相同，可以对比ipc；但是cpi可以计算一些额外的工作如：分支预测错误、cache miss等未命中事件的cycle数

### 多线程

ipc并不是多线程workload 准确可靠的性能指标，并且会导致系统向不正确的方向发展。因为微小的时序变化会导致不同的执行路径和线程interleavings (交错) ，即并行线程进入关键执行部分的顺序因具体运行而异，因此ipc相差很大。举例，线程获取自旋锁之前会自增，有很多指令，但是这些指令并没有实际作用，这些属于上文提到的无用指令数。还有处理tlb未命中以及空闲循环指令等都是。

Alameldeen and Wood 发现ipc 与 multi-threaded program performance并不是正相关，并且处理器核心越多，相关性越低，因为会有更多进程浪费在自旋上。在产业界体现在web服务器、数据库服务器以及邮件服务器等商业负载，这些都需要花费大量时间在操作系统上。

Wenisch et al. 提出了==u-ipc==，仅考虑用户模式指令，他们发现u-ipc与数据库、web服务器等密切相关。但是u-ipc无法捕获系统级代码的性能， 该部分在商业的workload中占据相当的比例。

Emer and Clark 解决了上面的问题，在==统计指令数据时排除vms null进程==，即单独剥离开自旋锁循环指令。

### 多程序

多线程和多核处理器不仅执行多个线程的workload，还同时执行多个独立的程序。例如，SMT处理器可以在单核处理器上执行多个作业，多核处理器也可以让每个作业在单独的核心运行，或者每个核的硬件线程执行多个作业。

多程序workload性能分析的根本问题是独立的共同执行程序会互相影响性能，性能interaction取决于共享资源的量，多核处理器通常共享最后一级cache、片上互联网络核内存的片外带宽。Chandra et al. 的实验表明，共同执行两个独立程序时，由于==共享资源==，单个程序的性能受到高达65%的影响。Tuck and Tullsen 在intel p4（双硬件线程SMT）上测试，部分程序的性能为独立运行的71%，另一部分是98%。

Eyerman and Eeckhout采用自顶向上方法提出新的性能指标，即==STP(系统吞吐量) 和 平均标准化周转时间 (ANTT)==，这两个指标分别来自用户和系统。对于用户：作业周转时间 （完成-提交），对于系统：整个系统的吞吐量（每个时间单位完成的作业数量），这两个指标没有直接相关性，可能优化一个会对另一个产生副作用，例如短作业优先会优化吞吐量，但是影响长作业周转时间。

下面开始分析系统吞吐量

首先定义程序的==标准化进程==： TiSP是单程序模式执行时间， TiMP是多程序模式执行时间；NPi < 1，该值代表了多程序执行过程中程序的进度，举例NPi=0.7，代表在10ms的多程序执行时间片内进行了7ms的单程序进度。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927152146504.png" alt="image-20230927152146504" style="zoom:50%;" />

STP系统吞吐量定义为NPi之和，即STP是所有作业的累积进度，越高越好。例如，两个程序运行在多线程或多核处理器上，一个标准化为0.75，另一个0.5，那么STP=1.25。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927153623690.png" alt="image-20230927153623690" style="zoom:50%;" />

STP通常>1，如果小于1，那么分时独占更好。对于某些程序组合，严重的资源共享会导致STP<1，如内存密集型workload会从最后一级的共享缓存中替换彼此的工作集，导致未命中大幅增加。

接着定义==平均标准化周转时间==：NTT是NP的倒数，ANTT即取平均，ANTT越低越好。上面的例子中，NP1=0.75, NP2=0.5, NTT1=1.33，NTT2=2，ANTT = 1.67。该值衡量了用户所能感知到的多程序执行相对于但程序执行期间的slowdown。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927162738987.png" alt="image-20230927162738987" style="zoom:50%;" />

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927162757099.png" alt="image-20230927162757099" style="zoom:50%;" />

下面是architecture community中常用的指标： IPC吞吐量，weighted speedup，harmonic mean。

为了和上面的STP和ANTT对比，需要进行转换，基于CPI:

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927163452870.png" alt="image-20230927163452870" style="zoom:50%;" />

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927163507703.png" alt="image-20230927163507703" style="zoom:50%;" />

==ipc_throughout==被评价为naive，在性能方面没有意义。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927163519478.png" alt="image-20230927163519478" style="zoom:50%;" />

==weighted speedup==指标由 Snavely and Tullsen提出，评估作业在多线程处理器上共同执行的效果。思路是如果一个作业调度在一定时间间隔内比另一个作业调度能够执行更多指令，说明更加symbiotic(共生性)，可以衡量作业组合中每个作业的贡献。实际上该公式等价于STP:)

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927164114358.png" alt="image-20230927164114358" style="zoom:50%;" />

==Harmonic mean== （调和平均）指标由Luo et al. 提出，如果一个或多个程序的ipc speedup低，那么会得到比算术平均值更低的值，该指标体现了公平性，但是并不反应系统相关的含义？ 实际上是ANTT的倒数:)

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927165005676.png" alt="image-20230927165005676" style="zoom:50%;" />

==STP + ANTT== : 二者应该互补使用，才能提供完整的视角，这是 平均作业周转时间和系统吞吐量的tradeoff。



### average performance之争

上文讲的都是单个workload的性能指标，但是对这些指标获取平均值的更多，关于平均数，有 算数平均数，调和平均数 ，几何平均数，下面对比。

1.   数学观点中的调和平均数和算数平均数

​	太长不看只说结论，如果提前知道哪些应用程序将在目标系统上运行，以及运行所占的比例，那么可以使用加权的 harmonic mean和 加权的算数

2.   几何平均

     太长不看只说结论，义眼顶真，is of questionable value.

结论：调和平均值（和算术平均值）可以为给定的一组基准提供有意义的摘要，但是对于完整的workload是有问题的。几何平均值提出了对于完整的workload未经证实的假设。



## ch3 Workload Design

选择不具有代表性的workload会导致错误的设计，Maynard et al.将商业应用程序（包括数据库服务器）的行为与计算机体系结构中广泛使用的 SPEC CPU 基准进行比较。他们发现商业工作负载通常表现出更复杂的分支行为、更大的代码和数据占用空间以及更多的操作系统和 I/O 活动。特别是，与商业工作负载相比，SPEC CPU 基准测试的指令缓存占用空间较小；此外，不适合片上高速缓存的内存访问模式通常是规则的或跨步的。因此，SPEC CPU 基准测试非常适合管道研究，但==在内存性能研究中应谨慎使用==。仅根据 SPEC CPU 基准来指导处理器设计决策可能会导致商业工作负载的性能不佳。

### 从workload space 到 representative workload

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230927215350749.png" alt="image-20230927215350749" style="zoom:50%;" />

理想情况下，我们希望有一组较小的workload能够代表更加广泛的workload space，嵌入式系统可以，但是通用计算机系统则依赖经验，但是这部分的workload数量相当多，因此需要减少。

==为什么给出一个良好的workload比较困难？==

1.   workload space可能涉及多个领域，每个领域有大量的程序，因此针对领域提出了不同的workload suits 例如spec cpu, mediabench, bioperf。
2.   workload space本身就在不断变化，需要使用现有的架构来评估未来的系统workload，还需要建立在旧的编译技术之上。Yi et al. 使用spec cpu95基准优化处理器，使用spec cpu2000评估性能和能效，发现前者优化得到的性能比后者差20%，因为sepc cpu2000比 spec cpu95更加需要内存操作。因此架构师应该尽可能的预测未来的workload。
3.   选择将哪些benchmark放到 benchmark suits是一个主观行为，例如 SPEC公司负责制定spec cpu2000 benchmark，制定时参考了代码的可移植性，性能特征和供应商的兴趣，很难说客观:)



如何减少workload的工作量？

想要模拟所有类型的benchmark本身就不实际，因此需要减少benchmark数量。Citron对benchmark进行了调查，发现主要是基于编程语言，可移植性和simulation infrastructure ，从benchmark suit中选择有限数量的benchmark比较常见，但是也会产生具有误导性的性能数据。

如何减少benchmark的数量，留下具有代表性且不冗余的benchmark？ （不同的benchmark会表现出相似的行为）

1.   PCA主成分分析 描述benchmark直接的相似性
2.   采用Plackett and Burman的实验设计

==PCA设计==

pca可以降低数据集的维度，做法是将workload space看成是具有p个维度的空间，维度即程序特征，包括ILP、inst mix、branch predictability，code footprint,memory working set size, and memory access patterns。维度太高很难直观的理解workload space， 并且不同的特征还有相关性。

pca通过p维的workload space转换为q维度的空间实现，转换后的维度中，彼此远离的benchmark表现出不同的行为，彼此接近的benchmark展示出相似的行为。可以和聚类结合，PCA可以减少benchmark数量并且留下具有代表性的workload。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20230928165020660.png" alt="image-20230928165020660" style="zoom:50%;" />

下面讲一下设计的细节，第一步是描述workload中不同的benchmark的行为特征，有如下方法：

1.   描述workload中不同benchmark的行为特征，使用硬件性能监视器，测量的内容包括cache miss、bpu miss、ipc等内容。但是可能存在误导，即不同的workload测试表现出相似的行为，下图中SPEC CPU2000的 gzip和 BioPerf的fasta 在 cpi以及cache miss惊人的相似，但是workset的大小和内存的访问模式却相差很大，二者的大小相差一个数量级。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231009185252821.png" alt="image-20231009185252821" style="zoom:50%;" />

2.   Hardware performance monitor data across multiple machines. 这是对上面的改进，不止在一台机器上收集硬件性能监控数据，而是在许多机器上都收集，从而产生更全面的workload特征。Phansalkar et al. 设计了一个实验，在五台不同的机器上使用四种不同的 ISA 和编译器（IBM Power、Sun UltraSPARC、Itanium 和 x86）来表征 SPEC CPU2006 基准测试套件。使用多机特征作为pca workload的输入，用于研究SPEC CPU2006测试之间的多样性和相似性。SPEC使用这种方法开发了CPU2006。

3.   Detailed simulation. 使用周期精确的模拟器收集程序特征，但是很费时间，比native执行慢5个数量级，但是能够在硬件还未可以使用之前就获取相应的特征。

4.   Microarchitecture-independent workload characterization. 收集独立于特定微体系结构的程序特征，但是这种特征只能通过软件（Pin\Atom)进行测量，也比较耗时，单相比周期精确模拟器还是快点。下图是与微架构无关的特征：

     <img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231009195744727.png" alt="image-20231009195744727" style="zoom: 67%;" />

第二步是PCA主成分分析

将许多可能相关的程序特征转化为较少数量的不相关主成分，保留的主成分数量远小于原始数据集中的维度数量。（其他太多了略

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231009200511186.png" alt="image-20231009200511186" style="zoom:50%;" />

PCA的优势：

1.   workload analysis. 能够可视化workload空间，下图中不同的颜色代表不同的benchmark， 第一主成分pc1量化了benchmark的控制流行为，即具有较少的分支和较低的icache miss rate，ijpeg最高， pc2中最高的是go和compress，其余的类推。

     从图中也能够得到一些结论，一些benchmark与workload中其他的benchmark都不同，ijpeg go compress过于孤立，图2的TPC-D受输入影响较大，而ijpeg几乎不受输入的影响。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231009201133987.png" alt="image-20231009201133987" style="zoom:50%;" />

2.   workload reduction. pca之后聚类分析，可以将benchmark分组为有限的集群，集群的中心可以作为代表性的benchmark。下图中SPEC CPU2006与完整的整数和浮点数测试套件相比，误差只有3.8%和7%，但是工作量大大降低。

![image-20231009202411849](./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231009202411849.png)

### PLACKETT AND BURMAN BASED WORKLOAD DESIGN

Yi et al.设计了一种新方法simulation-friendly，和PCA方法效率差不多，计算效率不如PCA, 基于PCA的程序特征比运行该方法所需的详细周期精确模拟的方法更有效。（这里略

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231010193732175.png" alt="image-20231010193732175" style="zoom:67%;" />

### LIMITATIONS AND DISCUSSION

以上两种方法的有局限性，减少的workload可能无法捕获更广泛的应用程序集的所有行为，即覆盖率不足。举例，value prediction是预测和推测指令结果的微架构技术，架构师想要评估性能，假设减少的workload是根据一组程序特征选择的，并且这组特征不捕获对应的局部价值和可预测性，那么实际的预测中就丢失了这两个概念。因此需要经常检查减少的workload的代表性工作量。



## ch4 Analytical Performance Modeling

分析性能建模方法相比仿真不太准确，但是更快，可以快速探索大型项目的设计空间，在设计早期提供指南，后期再通过仿真进行细粒度探索。即分析建模粗粒度，仿真细粒度。

### 经验建模与机制(mechanistic)建模

1.   机制建模基于第一原理构建性能模型，需要对系统底层机制的理解，自下而上构建性能模型，可以看成白盒建模。

2.   经验建模使用黑盒方法构建性能模型，通过统计或者机器学习方法从训练数据中学习性能建模。经验建模更容易推断出性能模型，但是可以观察到的内容比机制建模要少。

3.   机制-经验混合建模，称为灰盒建模，从经验建模方法学习如何简化性能模型的构建，从机制建模方法学习更好的洞察力。从通用的性能公式开始（改公式来自于对底层系统的理解），公式包含很多未知数，通过拟合来推断未知参数。
4.   总结：这两种建模没有详细的区分，都互相包含一部分。



### 经验建模

1.   线性回归，回归系数代表各自输入变量对整体的影响，线性回归模型会将性能和微架构参数(发射宽度，缓存大小等）联系起来，寻找最佳拟合位置。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231010201710301.png" alt="image-20231010201710301" style="zoom:67%;" />

上图中的公式假设xi和xj各自独立没有干扰，但实际的体系结构互相之间有影响，例如更优的内存层次对应更低的cache miss。下图的公式考虑了xi xj的交互，同样可以拓展到更高阶的xi xj xk 。。。

![image-20231010202820266](./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231010202820266.png)

使用回归建模是为了了解微架构参数之间的相互作用关系，Joseph et al. 提出该方法，参数可以选用管道深度、处理器宽度、rob大小、cache大小等，并且可以选定交互数量。使用最小二乘法寻找最佳拟合模型，最终结果是每个回归系数的估计值，系数的大小和符号代表各个参数对微架构整体性能的影响。

构建回归模型需要处理一系列问题。如何选择微架构输入参数，如果选择太多会影响模型构建时间，太少会影响准确率。参数的范围也难以取舍，通常会设置的很大。Joseph et al.将6个微架构参数及其相互作用作为输入，将ipc作为指标，回归系数可正可负。下图中增加rob size之后回归系数减小，因为变为负数了，但IPC会随着rob size增加

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231010210507227.png" alt="image-20231010210507227" style="zoom:67%;" />

2.   非线性回归

线性假设的限制性太大，还有平方根、对数、幂等函数，但是这些非线性变化如果用于整个输入变量的范围，那么可能会一个区域的好拟合影响到另一个区域的拟合。

Lee and Brooks又提出了分段多项式，每个Si(x)都是一个多项式，高阶多项式的拟合更好，Lee and Brooks基于分段高阶多项式构建了多处理器性能模型。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231011225059552.png" alt="image-20231011225059552" style="zoom: 67%;" />

3.   神经网络

神经网络可以自动从一组输入中预测目标（性能，功率。。）神经网络看作是广义非线性模型，相比与Lee and Brooks的工作，神经网络更自动化，而分段函数则有更好的统计理解。下图是全连接网络，使用sigmoid激活函数。训练神经网络可以视作搜索问题，寻找最佳weight使得预测值和测量值的误差最低，通过反向传播训练。

<img src="./../image/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes/image-20231011225849490.png" alt="image-20231011225849490" style="zoom:67%;" />

4.   经验建模的限制

     需要大量的模拟数据



### 机制建模
