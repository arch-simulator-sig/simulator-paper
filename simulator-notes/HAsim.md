# HAsim

作者目前是NV架构师，2011年发表该博士毕业论文，论文本身是一个集大成者。实现了FPGA加速多核处理器的周期精确性能建模，并且是跨机构和跨公司的项目。

- Joel Emer提供了想法和指导；
- Intel VSSAD小组提供FPGA接口支持，scratchpad memory abstraction，LEAP-CORE虚拟平台基础架构设计
- 支持A-Ports formalism
- 使用Heracles进行对比实验
- bluespec提供编译器支持

## Introduction

处理器的架构设计通常是基于直觉的，通过对架构的参数进行量化研究，反复实验，最终才能够进入RTL步骤。

