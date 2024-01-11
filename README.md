# simulator-paper

整理体系结构模拟器相关的paper，开源代码

| 开源项目                                        | 链接                                                         | 概述                                                         | 评级                           |
| ----------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------ |
| calipers                                        | https://github.com/microsoft/calipers                        | 图依赖的性能建模，同时还需要参考白晨的macro paper，比较重要！ | :star::star::star::star::star: |
| sim-godson                                      | https://github.com/dzwduan/sim-godson                        | 执行驱动，cache部分精确建模，代码风格极好，几乎没有发现bug   | :star::star::star::star::star: |
| macsim                                          | https://github.com/gthparch/macsim                           | trace driven or execution-drive cycle level simulator,支持多核、互联网络模型和电源模型 | :star::star::star::star:       |
| akita                                           | https://github.com/sarchlab/akita                            | https://space.bilibili.com/91181150      有课程              | :star::star::star::star::star: |
| esesc                                           | https://github.com/masc-ucsc/esesc                           | qemu based，update recently, multi-core perf model           | :star::star::star::star::star: |
| marss-riscv                                     | https://github.com/bucaps/marss-riscv                        | 全系统，cycle accurate, 支持ooo, 支持dramsim3和ramulator     | :star::star::star::star:       |
| ml-rsim                                         | https://github.com/dzwduan/ml-rsim                           | cache bus 建模可以参考                                       | :star::star::star::star:       |
| mipt-mips                                       | https://github.com/MIPT-ILab/mipt-mips                       | cycle accurate, fork asim 能run                              | :star::star::star::star:       |
| SimEng                                          | https://github.com/UoB-HPC/SimEng                            | 比较简洁，有文档，最近有更新                                 | :star::star::star::star:       |
| onikiri2                                        | https://github.com/onikiri/onikiri2                          | cycle accurate                                               | :star::star::star::star:       |
| map                                             | https://github.com/sparcians/map                             | 20年老法师框架，应有尽有，维护也很积极，可以与systemc gem5等联动 | :star::star::star::star:       |
| raspsim                                         | https://github.com/aengelke/raspsim                          | fork from PTLsim，但是可以借鉴一部分超标量的设计写法         | :star::star::star::star:       |
| scarab                                          | https://github.com/hpsresearchgroup/scarab                   | 挑不出毛病，除了no smp                                       | :star::star::star::star:       |
| champsim                                        | https://github.com/ChampSim/ChampSim                         | for education，简简单单入个门                                | :star::star::star::star:       |
| dreamCore v1 & v2                               | https://github.com/lizhirui/DreamCoreV2                      | cycle accurate ooo simulator                                 | :star::star::star::star:       |
| Asim                                            | https://github.com/Asim-Modeling                             | 30年老法师框架，藏东西了,和论文不符合，降级                  | :star::star::star:             |
| simflex                                         | https://parsa.epfl.ch/simflex/                               | smarts                                                       |                                |
| riscv-ovpsim                                    | https://github.com/riscv-ovpsim/imperas-riscv-tests          | 印度理工和sifive建模用，riscv主流工具，只开源了bin           | :star::star::star:             |
| gem5                                            | https://github.com/gem5/gem5                                 | 乱序核是tick driven, cache是event driven，很多优秀的写法可以借鉴，比如实现了门级的stdcell，实现了动态功耗统计，还有一些抽象功能的写法，都较为优秀。微架构是alpha21264，缺点是写法要基于原有的框架进行实现，但是原有框架兼容性较高，损失了相当的性能，且修改微架构成本过高。使用python作为用户接口调用底层的C++实现，目前可能并不需要 | :star::star::star:             |
| cycle-accurate-SystemC-simulator-over-ramulator | https://github.com/Liu-Cheng/cycle-accurate-SystemC-simulator-over-ramulator |                                                              | :star::star::star:             |
| fifo_ca_model                                   | https://github.com/liang-aquarius/ca_model                   | 每个周期对所有module run and update，类似SCore的思路，有dump wave的功能，实现是每个clk进行连线，大量的赋值语句，会很慢，跟Score一个样，说明不能瞎琢磨 | :star::star:                   |
| risc-vp                                         | https://github.com/agra-uni-bremen/riscv-vp                  | tlm2.0 + instruction-based timing model，不考虑乱序、流水线、cache等 | :star:                         |
| hsaemu                                          | https://github.com/SSLAB-HSA/HSAemu                          | apu system. too old，异构计算，zhi'chi                       | :star:                         |
| multi2sim                                       | https://github.com/Multi2Sim/multi2sim                       | cpu and gpu simulator                                        | :star:                         |
| vcml                                            | https://github.com/machineware-gmbh/vcml                     | tlm组件库                                                    |                                |
| sinuca                                          | https://github.com/mazalves/sinuca                           |                                                              |                                |
| simplescalar                                    | https://github.com/toddmaustin/simplesim-3.0                 | ooo  processor simulator                                     |                                |
| esysim                                          | https://github.com/wangeddie67/ESYSim                        | 仿真模型                                                     |                                |
| mgsim                                           | https://github.com/svp-dev/mgsim                             | for teaching                                                 |                                |
| zsim                                            | https://github.com/s5z/zsim                                  | 数亿条指令/秒，not ca，cache建模很厉害                       |                                |
| sniper                                          | https://github.com/snipersim/snipersim                       | 很强，但是代码比较难读                                       |                                |
| ramulator                                       | https://github.com/CMU-SAFARI/ramulator                      | 高精度dram simulator，二代好像有问题                         |                                |
| mcpat                                           | https://github.com/HewlettPackard/mcpat                      | https://github.com/H2020-COSSIM/cMcPAT                       |                                |
| dramsys4.0                                      | https://github.com/tukl-msd/DRAMSys                          | Cycle-accurate **DDR3/4**， 支持gem5, 基于systemc TLM2       |                                |
| cacti                                           | https://github.com/HewlettPackard/cacti                      | 对ppa建模很有用                                              |                                |
| riscv_vhdl                                      | https://github.com/sergeykhbr/riscv_vhdl                     |                                                              |                                |
| openSparc T1 & T2                               | 找我要code，官网失效                                         | 工业界源码,not cycle accurate 暂时搁置                       | :star::star::star:             |
| noculator                                       | https://github.com/CMU-SAFARI/NOCulator                      |                                                              |                                |
| soclib                                          | http://www.soclib.fr/trac/dev                                |                                                              |                                |
| patos-simulator                                 | https://github.com/t-crest/patmos-simulator                  | 多核，基于ramulator进行Mem模拟                               |                                |
| snipersim                                       | https://github.com/snipersim/snipersim                       | 区间模型，数学模型，比较难上手                               |                                |
| or1ksim                                         | https://github.com/openrisc/or1ksim                          | function model                                               |                                |
| parade                                          | https://github.com/cdsc-github/parade-ara-simulator          | gem5套壳ca                                                   |                                |
| swarmArch                                       | https://github.com/SwarmArch/sim                             | not ca，比较新，MIT，更注重分析应用程序                      |                                |
| out-of-roder simulator                          | https://github.com/Jacob-Hoff-man/cs2410                     |                                                              |                                |
| kite simulaotr                                  | https://github.com/yonsei-icsl/Kite                          | 已经看完，没有太大的参考价值，主要维护了ticks变量用于控制时钟，latency标识exu的执行模块具体的流水级别，但是仅限于ticks++，同时bpu设计缺失，cache write throuth，整体为5级流水架构 |                                |
| Wisconsin Wind Tunnel II                        | https://pages.cs.wisc.edu/~wwt/wwt2/                         |                                                              |                                |



[龙芯cpu发展史](龙芯cpu发展史.md)

[龙芯3号发展史](龙芯3号发展史.md)

[arch相关工具集合](https://pages.cs.wisc.edu/~arch/www/tools.html)
