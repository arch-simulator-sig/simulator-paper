<div align="center">

# Simulator Paper

**体系结构模拟器的论文、开源代码与阅读笔记合集**

_A curated collection of computer architecture simulators, papers and reading notes._

[![Stars](https://img.shields.io/github/stars/arch-simulator-sig/simulator-paper?style=flat-square&logo=github)](https://github.com/arch-simulator-sig/simulator-paper/stargazers)
[![Forks](https://img.shields.io/github/forks/arch-simulator-sig/simulator-paper?style=flat-square&logo=github)](https://github.com/arch-simulator-sig/simulator-paper/network/members)
[![Last Commit](https://img.shields.io/github/last-commit/arch-simulator-sig/simulator-paper?style=flat-square)](https://github.com/arch-simulator-sig/simulator-paper/commits/main)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](#-贡献)

[开源模拟器](#-开源模拟器) •
[论文与资料](#-论文与资料) •
[阅读笔记](#-阅读笔记) •
[目录结构](#-目录结构) •
[相关资源](#-相关资源) •
[贡献](#-贡献)

</div>

---

## 🧭 开源模拟器

按类别整理，类别内按评级排序。

| 标记 | 含义 |
| :---: | --- |
| ⭐⭐⭐⭐⭐ | 无脑学 |
| ⭐⭐⭐⭐ | 有亮点 |
| ⭐⭐⭐ | 有瑕疵 |
| ⭐⭐ / ⭐ | 参考价值有限 |
| — | 尚未评级 |
| 📝 / 📄 | 本仓库中有阅读笔记 / 论文 |

**分类导航：**
[模拟框架](#模拟框架) ·
[CPU 微架构模拟器](#cpu-微架构模拟器) ·
[全系统与虚拟平台](#全系统与虚拟平台) ·
[快速、采样与解析建模](#快速采样与解析建模) ·
[异构与加速器](#异构与加速器) ·
[存储与互连](#存储与互连) ·
[功耗与面积](#功耗与面积) ·
[其他参考](#其他参考)

### 模拟框架

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [akita](https://github.com/sarchlab/akita) | 有[文档](https://akitasim.dev/docs/akita/)、有[课程](https://space.bilibili.com/91181150)、能在 GitHub Discussions 提问 | ⭐⭐⭐⭐⭐ |
| [MAP](https://github.com/sparcians/map) | 20 年老法师框架，应有尽有，维护积极，可与 SystemC、gem5 等联动 | ⭐⭐⭐⭐ |
| [Asim](https://github.com/Asim-Modeling) [📝](simulator-notes/asim-notes.md) [📄](paper/Asim%20-%20A%20Performance%20Model%20Framework%20%282002.02.computer.asim%29.pdf) | 30 年老法师框架，藏东西了，和论文不符，降级 | ⭐⭐⭐ |
| [HAsim](https://github.com/HAsim-Modeling) [📝](simulator-notes/HAsim.md) [📄](paper/asim/HAsim%20Cycle-Accurate%20Multicore%20Performance.pdf) | Hardware Asim，基于 FPGA 的多核 cycle-accurate 模拟器 | — |

### CPU 微架构模拟器

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [sim-godson](https://github.com/dzwduan/sim-godson) | 执行驱动，cache 部分精确建模，代码风格极好，几乎没有发现 bug | ⭐⭐⭐⭐⭐ |
| [ESESC](https://github.com/masc-ucsc/esesc) | 采样模式下 40 MIPS；可配置的内存层次，支持 PPA；发表于 HPCA 2013。仓库已归档，后继为 [desesc](https://github.com/masc-ucsc/desesc) | ⭐⭐⭐⭐⭐ |
| [MacSim](https://github.com/gthparch/macsim) | trace-driven / execution-driven 的 cycle-level 模拟器，支持多核、NoC、PPA | ⭐⭐⭐⭐ |
| [MIPT-MIPS](https://github.com/MIPT-ILab/mipt-mips) [📝](simulator-notes/mipt.md) | cycle-accurate 的 MIPS / RISC-V 模拟器，能跑通；2022 年起已归档 | ⭐⭐⭐⭐ |
| [SimEng](https://github.com/UoB-HPC/SimEng) | 比较简洁，有文档，近期仍在更新 | ⭐⭐⭐⭐ |
| [DreamCore V1 & V2](https://github.com/lizhirui/DreamCoreV2) [📝](simulator-notes/dreamCoreV2-建模思路.md) | 将全局 DFF 放到一个列表里统一更新，速度不是很快，更注重微架构的实现；正在尝试复刻一个 demo 🏗️ | ⭐⭐⭐⭐ |
| [raspsim](https://github.com/aengelke/raspsim) [📝](simulator-notes/raspsim.md) | fork 自 PTLsim，可以借鉴一部分超标量的设计写法 | ⭐⭐⭐⭐ |
| [Scarab](https://github.com/hpsresearchgroup/scarab) | x86 ISA；trace-driven 600 KIPS、exec-driven 100 KIPS；支持 wrong-path 模拟、DVFS；但不支持 32 位程序，不支持多线程、操作系统、mesh 等。原仓库已不活跃，现由 UCSC 在 [litz-lab/scarab](https://github.com/litz-lab/scarab) 继续维护 | ⭐⭐⭐⭐ |
| [ChampSim](https://github.com/ChampSim/ChampSim) | 面向教学，简单入门，可在其基础上做分支预测器、预取器设计 | ⭐⭐⭐⭐ |
| [onikiri2](https://github.com/onikiri/onikiri2) | 能跑起来，但 XML 结果不知如何查看，lib 仍是 Win32 库，可视化也打不开；流水线基于队列建模 | ⭐⭐⭐ |
| [gem5](https://github.com/gem5/gem5) [📝](simulator-notes/Gem5.md) | 乱序核为 tick-driven，cache 为 event-driven。很多写法值得借鉴：附带的 DSENT 含 stdcell 级功耗 / 面积模型（面向 NoC）、动态功耗统计、各类抽象功能的写法。O3 微架构大致基于 Alpha 21264。缺点：需基于原有框架开发，框架兼容性高但损失了相当的性能，修改微架构成本过高；用 Python 作为用户接口调用底层 C++，目前可能并不需要 | ⭐⭐⭐ |
| [SiNUCA](https://github.com/mazalves/sinuca) [📝](simulator-notes/SiNUCA.md) [📄](paper/Alves%20等%20-%202015%20-%20SiNUCA%20A%20Validated%20Micro-Architecture%20Simulator.pdf) | 经过验证的微架构模拟器（2015） | — |
| [SimpleScalar](https://github.com/toddmaustin/simplesim-3.0) [📝](simulator-notes/SimpleScalar.md) [📄](paper/hack_guide.pdf) | 乱序处理器模拟器 | — |
| [MGSim](https://github.com/svp-dev/mgsim) | 面向教学 | — |
| [Patmos Simulator](https://github.com/t-crest/patmos-simulator) | 多核，内存模拟可选基于 Ramulator | — |
| [Kite](https://github.com/yonseicasl/Kite) [📝](simulator-notes/Kite.md) | 已看完，参考价值不大：主要维护 ticks 变量控制时钟，latency 标识执行单元的具体流水级（仅限于 ticks++）；分支预测器 / BTB 只有桩实现（恒 not-taken），cache 为 write-back，整体为 5 级流水 | — |
| [cs2410 OoO Simulator](https://github.com/Jacob-Hoff-man/cs2410) | 乱序处理器模拟器（课程项目） | — |

### 全系统与虚拟平台

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [MARSS-RISCV](https://github.com/bucaps/marss-riscv) | 全系统，cycle-accurate，支持乱序，支持 DRAMSim3 和 Ramulator。实测需要在它的 VM 里跑 CoreMark，且连 CoreMark 都跑不动；但驱动很齐全，值得学习。2022 年已停止更新 | ⭐⭐⭐ |
| [ML-RSIM](https://github.com/dzwduan/ml-rsim) [📄](paper/mlrsim_utility.pdf) | cache / bus / DRAM 建模可以参考，考虑了 IO 与系统交互的影响，搭建了 IO 和操作系统的基础设施；但代码过于古老，需要重新移植才能在 Linux 上运行 | ⭐⭐⭐ |
| [riscv-ovpsim](https://github.com/riscv-ovpsim/imperas-riscv-tests) | Imperas 出品的 RISC-V 主流工具，SiFive、IIT Madras（SHAKTI）等在用；模拟器只发布二进制，处理器模型源码以 Apache 2.0 开源 | ⭐⭐⭐ |
| [riscv-vp](https://github.com/agra-uni-bremen/riscv-vp) | TLM 2.0 + 基于指令的时序模型，不考虑乱序、流水线、cache 等 | ⭐ |
| [QFlex](https://github.com/parsa-epfl/qflex) | QEMU + cycle-accurate | — |
| [P2IM](https://github.com/RiS3-Lab/p2im) [📝](paper-notes/P2IM.md) | 外设建模，基于 QEMU（USENIX Security 2020）；另见[组会纪要](https://whutslab.github.io/2023/04/26/%E7%BB%84%E4%BC%9A%E7%BA%AA%E8%A6%81/) | — |
| [VCML](https://github.com/machineware-gmbh/vcml) | TLM 组件库 | — |
| [SoCLib](http://www.soclib.fr/trac/dev) | 基于 SystemC 的 SoC 虚拟原型平台 | — |
| [ESYSim](https://github.com/wangeddie67/ESYSim) | 片上网络（NoC）仿真与可视化工具集，支持路由、虚通道、容错与功耗分析 | — |
| [or1ksim](https://github.com/openrisc/or1ksim) | 功能模型 | — |
| [riscv_vhdl](https://github.com/sergeykhbr/riscv_vhdl) | RISC-V SoC 的 VHDL 实现与配套模拟器 | — |

### 快速、采样与解析建模

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [Calipers](https://github.com/microsoft/calipers) [📄](paper/2201.05884.pdf) | 基于图依赖的性能建模，同时需要参考白晨的 [ArchExplorer](https://github.com/baichen318/arch-explorer)（MICRO 2023，[论文](paper/C180-MICRO2023-ArchExplorer%281%29.pdf)），比较重要！ | ⭐⭐⭐⭐⭐ |
| [ZSim](https://github.com/s5z/zsim) [📝](simulator-notes/zsim.md) | 每秒数亿条指令，非 cycle-accurate，cache 建模很厉害 | — |
| [Sniper](https://github.com/snipersim/snipersim) [📝](simulator-notes/sniper.md) | 以区间模型（数学分析模型）著称，9.0 起默认改用 ROB 模型（`-c interval` 仍可用）；很强，但代码比较难读、难上手 | — |
| SimFlex [📄](paper/smarts2003isca.pdf) | SMARTS 采样；原主页已失效 | — |
| [SwarmArch](https://github.com/SwarmArch/sim) | cycle-accurate，MIT 出品（MICRO 2015 起），更注重分析应用程序 | — |
| [Wisconsin Wind Tunnel II](https://pages.cs.wisc.edu/~wwt/wwt2/) | 并行多处理器模拟器 | — |

### 异构与加速器

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [Multi2Sim](https://github.com/Multi2Sim/multi2sim) | CPU + GPU 模拟器 | ⭐ |
| [HSAemu](https://github.com/SSLAB-HSA/HSAemu) | APU 系统，异构计算；过于古老 | ⭐ |
| [PARADE](https://github.com/cdsc-github/parade-ara-simulator) | 基于 gem5 套壳的 cycle-accurate 模拟器 | — |

### 存储与互连

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [cycle-accurate-SystemC-simulator-over-ramulator](https://github.com/Liu-Cheng/cycle-accurate-SystemC-simulator-over-ramulator) | 基于 Ramulator 的 SystemC 周期精确模拟器 | ⭐⭐⭐ |
| [Ramulator](https://github.com/CMU-SAFARI/ramulator) | 高精度 DRAM 模拟器，[二代](https://github.com/CMU-SAFARI/ramulator2)好像有问题 | — |
| [DRAMSys](https://github.com/tukl-msd/DRAMSys) | cycle-accurate，支持 DDR3/4、LPDDR4、GDDR5/6、HBM 等，支持 gem5，基于 SystemC TLM2 | — |
| [NOCulator](https://github.com/CMU-SAFARI/NOCulator) | 片上网络（NoC）模拟器 | — |

### 功耗与面积

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [McPAT](https://github.com/HewlettPackard/mcpat) | 另见 [cMcPAT](https://github.com/H2020-COSSIM/cMcPAT) | — |
| [CACTI](https://github.com/HewlettPackard/cacti) | 对 PPA 建模很有用 | — |

### 其他参考

| 项目 | 简介 | 评级 |
| --- | --- | :---: |
| [OpenSPARC T1 & T2](https://www.oracle.com/servers/technologies/opensparc-overview.html) | 工业界源码，非 cycle-accurate，暂时搁置。官网已迁至 Oracle，源码仍可下载 | ⭐⭐⭐ |
| [fifo_ca_model](https://github.com/liang-aquarius/ca_model) | 每个周期对所有 module 执行 update 和 run，类似 SCore 的思路，支持 dump 波形；每个 clk 进行连线，大量赋值语句，速度很慢，跟 SCore 一样——说明不能瞎琢磨 | ⭐⭐ |

---

## 📚 论文与资料

所有 PDF 位于 [`paper/`](paper) 目录。

**综述与方法论**

- [A Survey of Computer Architecture Simulation Techniques and Tools](paper/A_Survey_of_Computer_Architecture_Simulation_Techn.pdf)
- [Computer Architecture Performance Evaluation Methods](paper/computer%20architecture%20performance%20evaluation%20methods.pdf) · [读书笔记](simulator-notes/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes.md)
- [SMARTS: Accelerating Microarchitecture Simulation via Rigorous Statistical Sampling](paper/smarts2003isca.pdf)（ISCA 2003）
- [计算机体系结构软件模拟技术研究](paper/计算机体系结构软件模拟技术研究.pdf)
- [体系结构模拟器在处理器设计过程中的作用](paper/体系结构模拟器在处理器设计过程中的作用.pdf)

**模拟器与性能建模**

- Asim：[A Performance Model Framework](paper/Asim%20-%20A%20Performance%20Model%20Framework%20%282002.02.computer.asim%29.pdf)（IEEE Computer 2002）
- PTLsim：[论文](paper/PTLSim.pdf) · [手册](paper/PTLsimManual.pdf) · [笔记](simulator-notes/PTLSim.md)
- SimpleScalar：[A User's and Hacker's Guide](paper/hack_guide.pdf) · [SimpleScalar 4.0](paper/simpleScalar4.0.pdf)
- RSIM / ML-RSIM：[RSIM 手册](paper/rsim_manual.pdf) · [The Design and Utility of the ML-RSIM System Simulator](paper/mlrsim_utility.pdf)
- [SiNUCA: A Validated Micro-Architecture Simulator](paper/Alves%20等%20-%202015%20-%20SiNUCA%20A%20Validated%20Micro-Architecture%20Simulator.pdf)（2015）
- [Calipers: A Criticality-aware Framework for Modeling Processor Performance](paper/2201.05884.pdf)
- [ArchExplorer](paper/C180-MICRO2023-ArchExplorer%281%29.pdf)（MICRO 2023）
- [A Framework for End-to-end Simulation of High-performance Computing Systems](paper/A_Framework_for_End_to_End_Simulation_of.pdf)
- CVA6：[Performance Modeling of CVA6](paper/Performance%20Modeling%20of%20CVA6.pdf) · [CVA6's Data Cache: Structure and Behavior](paper/cva6-data-cache.pdf)
- [Arm Performance Models Library Release Note](paper/arm_performance_models_library_r1p2_00rel0_release_note_107623_05.pdf)

**工业界经验**

- [Performance Modeling and Analysis at AMD](paper/Performance%20Modeling%20and%20Analysis%20at%20AMD.pdf)
- [Experiences in Simulation at IBM](paper/Experiences%20in%20Simulation%20at%20IBM.pdf)

**处理器设计与规范**

- [超标量处理器设计](paper/书签版超标量处理器设计_姚永斌.pdf)（姚永斌）
- [微处理器性能分析与优化](paper/微处理器性能分析与优化_张福新.pdf)（张福新） · [读书笔记](simulator-notes/微处理器性能分析与优化.md)
- [开源处理器的架构迭代加速方法与指令调度架构研究](paper/开源处理器的架构迭代加速方法与指令调度架构研究.pdf)
- [顺序双发设计参考](paper/顺序双发设计参考.pdf)
- [Cache Memory](paper/CacheMemory.pdf)（王齐 等）
- [海思 Verilog 代码规范](paper/海思verilog代码规范.pdf)
- RISC-V 规范：[非特权级](paper/riscv-spec.pdf) · [特权级](paper/riscv-privileged.pdf)

**专题目录**

- [`paper/loongson/`](paper/loongson) — 龙芯 1 / 2 号处理器设计与性能优化、TLB 优化、Sim-Godson、SimOS-Goodson、SimpleScalar 分析等
- [`paper/asim/`](paper/asim) — Asim / AWB / HAsim、Bluespec、FPGA 加速模拟相关论文与报告（RAMP Gold、HySIM 等）

---

## 📝 阅读笔记

<table>
<tr>
<th>模拟器（<a href="simulator-notes">simulator-notes/</a>）</th>
<th>处理器微架构（<a href="cpus">cpus/</a>）</th>
<th>论文与专题</th>
</tr>
<tr valign="top">
<td>

**源码分析**

- [Asim](simulator-notes/asim-notes.md)
- [HAsim](simulator-notes/HAsim.md)
- [gem5](simulator-notes/Gem5.md)
- [SimpleScalar](simulator-notes/SimpleScalar.md)
- [PTLsim](simulator-notes/PTLSim.md)
- [raspsim](simulator-notes/raspsim.md)
- [MIPT-MIPS](simulator-notes/mipt.md)
- [SiNUCA](simulator-notes/SiNUCA.md)
- [Kite](simulator-notes/Kite.md)
- [ZSim](simulator-notes/zsim.md)
- [Sniper](simulator-notes/sniper.md)
- DreamCore V2：[建模思路](simulator-notes/dreamCoreV2-建模思路.md) · [行为级描述](simulator-notes/dreamCoreV2-行为级描述.md)

**方法与书籍**

- [Computer Architecture Performance Evaluation Methods](simulator-notes/Computer%20Architecture%20Performance%20Evaluation%20Methods%20notes.md)
- [微处理器性能分析与优化](simulator-notes/微处理器性能分析与优化.md)
- [Verilog 仿真模型](simulator-notes/Verilog仿真模型.md)

</td>
<td>

**经典处理器**

- [Alpha 21264](cpus/alpha-21264.md)
- [MIPS R10K](cpus/MIPS-R10K.md)
- [Intel P6](cpus/Intel-P6.md)
- [PowerPC 620](cpus/PowerPC-620.md)

**开源处理器**

- [BOOM](cpus/boom.md)
- [Rocket](cpus/rocket-core.md)
- [香山 XiangShan](cpus/XiangShan.md)
- [玄铁 C910](cpus/c910.md)
- [CVA6](cpus/cva6.md)
- [E203](cpus/E203.md)
- [riscv-mini](cpus/riscv-mini.md)

**书籍**

- [超标量处理器设计](cpus/超标量处理器设计.md)
- [现代处理器设计](cpus/现代处理器设计.md)
- [处理器微架构实现](cpus/处理器微架构实现.md)
- [高性能超标量 CPU](cpus/高性能超标量CPU.md)

</td>
<td>

**论文笔记**

- [TEA: Time-Proportional Event Analysis](paper-notes/TEA.md)（ISCA 2023，[PDF](paper-notes/isca-2023-TEA.pdf)）
- [P2IM 组会纪要](paper-notes/P2IM.md)

**龙芯**

- [龙芯 2 号发展史](paper/loongson/龙芯2号发展史.md)
- [龙芯 3 号发展史](paper/loongson/龙芯3号发展史.md)
- [模拟器项目推进](paper/loongson/项目推进.md)

</td>
</tr>
</table>

---

## 📁 目录结构

```text
simulator-paper
├── paper/              # 论文、手册与书籍 PDF
│   ├── asim/           # Asim / HAsim / FPGA 加速模拟相关资料
│   └── loongson/       # 龙芯处理器与模拟器相关资料
├── paper-notes/        # 论文阅读笔记
├── simulator-notes/    # 开源模拟器源码分析笔记
├── cpus/               # 处理器微架构笔记
└── image/              # 笔记配图
```

---

## 🔗 相关资源

- [WWW Computer Architecture Page — Tools](https://pages.cs.wisc.edu/~arch/www/tools.html)：威斯康星大学整理的体系结构工具集合

---

## 🤝 贡献

欢迎通过 Issue 或 Pull Request 补充新的模拟器、论文、笔记，或修正评级与描述。

新增模拟器时，请放入对应分类的表格，并保持类别内按评级排序：

```markdown
| [项目名](https://github.com/owner/repo) | 一句话简介：建模方式、精度、速度、优缺点 | ⭐⭐⭐ |
```

---

<div align="center">

[![Star History Chart](https://api.star-history.com/svg?repos=arch-simulator-sig/simulator-paper&type=Date)](https://star-history.com/#arch-simulator-sig/simulator-paper&Date)

</div>
