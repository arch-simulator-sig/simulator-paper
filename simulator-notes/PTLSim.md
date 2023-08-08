# PTLsim

## 摘要

cycle accurate full system simulator and virtual machine

支持部分模块的RTL级别模型， cache和设备在真机上直接运行

对标真实的商用X86处理器，支持单线程的userspace 和 full system + SMT + multi-core

和真实的商用处理器误差在5%以内

支持虚拟机在主机的物理cpu和PTLsim的周期精确处理器模型无缝切换？ 模拟器和Xen进行了集成



## 问题

给人一种NEMU 加了微架构的感觉