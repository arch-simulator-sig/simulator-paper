# 开源模拟器/仿真器列表

要求：理解框架的原理，并给出报告讲解相关实现，录屏，thanks ArchShineZ

| 开源项目                                        | 链接                                                         | 概述                         | 评级    |
| ----------------------------------------------- | ------------------------------------------------------------ | ---------------------------- | ------------- |
| gem5                                            | https://github.co                                 | 乱序核是tick driven, cache是event driven，很多优秀的写法可以借鉴，比如实现了门级的stdcell，实现了动态功耗统计，还有一些抽象功能的写法，都较为优秀。微架构是alpha21264，缺点是写法要基于原有的框架进行实现，但是原有框架兼容性较高，损失了相当的性能，且修改微架构成本过高。使用python作为用户接口调用底层的C++实现，目前可能并不需要 | :star::star::star: |
| raspsim                                         | https://github.com/aengelke/raspsim                          | 32核vcpu，代码量过大，看也能看，但是没文档，优先级低，后边再看 | :star::star: |
| esesc | https://github.com/masc-ucsc/esesc | qemu based，update recently | :star::star: |
| risc-vp | https://github.com/agra-uni-bremen/riscv-vp | tlm2.0 + instruction-based timing model，不考虑乱序、流水线、cache等 | :star: |
| Asim | https://github.com/Asim-Modeling |  | |
| simplescalar                                    | https://github.com/toddmaustin/simplesim-3.0                 | ooo  processor simulator |         |
| esysim | https://github.com/wangeddie67/ESYSim |  | |
| hsaemu                                          | https://github.com/SSLAB-HSA/HSAemu                          | apu system. too old | :star: |
| multi2sim                                       | https://github.com/Multi2Sim/multi2sim                       | cpu and gpu simulator | :star: |
| macsim                                          | https://github.com/gthparch/macsim                           | trace driven or execution-drive cycle level simulator,支持多核、互联网络模型和电源模型 | :star::star: |
| mgsim                                           | https://github.com/svp-dev/mgsim                             |                              |               |
| zsim                                            | https://github.com/s5z/zsim                                  |                              |               |
| sniper                                          | https://github.com/snipersim/snipersim                       |                              |               |
| scarab                                          | https://github.com/hpsresearchgroup/scarab                   |                              |  |
| ramulator                                       | https://github.com/CMU-SAFARI/ramulator                      |                              |               |
| mcpat                                           | https://github.com/HewlettPackard/mcpat                      | https://github.com/H2020-COSSIM/cMcPAT |               |
| dramsys4.0                                      | https://github.com/tukl-msd/DRAMSys                          |                              |               |
| dramsim3                                        | https://github.com/umd-memsys/DRAMsim3                       |                              |               |
| dramsim2                                        | https://github.com/umd-memsys/DRAMSim2                       |                              |               |
| cacti                                           | https://github.com/HewlettPackard/cacti                      |                              |               |
| marss-riscv                                     | https://github.com/bucaps/marss-riscv                        |                              |               |
| map                                             | https://github.com/sparcians/map                             |                              |               |
| champsim                                        | https://github.com/ChampSim/ChampSim                         |                              |               |
| riscv_vhdl                                      | https://github.com/sergeykhbr/riscv_vhdl                     |                              |               |
| dreamCore v1 & v2                               | https://github.com/lizhirui/DreamCoreV2                      | cycle accurate ooo simulator | :star::star::star: |
| openSparc T1 & T2                               | 找我要code，官网失效                                         | 工业界源码                   |               |
| mipt-mips                                       | https://github.com/MIPT-ILab/mipt-mips                       | cycle accurate | :star::star::star: |
| cycle-accurate-SystemC-simulator-over-ramulator | https://github.com/Liu-Cheng/cycle-accurate-SystemC-simulator-over-ramulator |                              | :star::star::star: |
| noculator                                       | https://github.com/CMU-SAFARI/NOCulator                      |                              |               |
| soclib                                          | http://www.soclib.fr/trac/dev                                |                              |               |
| patos-simulator                                 | https://github.com/t-crest/patmos-simulator                  | 多核，基于ramulator进行Mem模拟 |               |
| snipersim | https://github.com/snipersim/snipersim | 可视化相当NB | |
| or1ksim | https://github.com/openrisc/or1ksim |  | |
| parade | https://github.com/cdsc-github/parade-ara-simulator |  | |
| swarmArch | https://github.com/SwarmArch/sim |  | |
| out-of-roder simulator | https://github.com/Jacob-Hoff-man/cs2410 |  | |
| kite simulaotr | https://github.com/yonsei-icsl/Kite | 已经看完，没有太大的参考价值，主要维护了ticks变量用于控制时钟，latency标识exu的执行模块具体的流水级别，但是仅限于ticks++，同时bpu设计缺失，cache write throuth，整体为5级流水架构 |  |
| Wisconsin Wind Tunnel II | https://pages.cs.wisc.edu/~wwt/wwt2/ |  |  |

