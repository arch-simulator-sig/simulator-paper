# Rocket-Core

## Rocket-Core架构设计

rocket-core : rv64gc + mmu + bpu + float  + unblocked dcache + boot linux

<img src="./img/boom/image-20230718195300783.png" alt="image-20230718195300783" style="zoom: 50%;" />

rocket-core pipeline

<img src="./img/boom/image-20230718195747728.png" alt="image-20230718195747728" style="zoom: 33%;" />

rocket-core icache

<img src="./img/boom/image-20230718195911394.png" alt="image-20230718195911394" style="zoom: 33%;" />

rocket-core dcache

<img src="./img/boom/image-20230718195946046.png" alt="image-20230718195946046" style="zoom: 33%;" />



### Rocket Core Fetch

Frontend用于取指和预译码，

![img](./img/boom/v2-84295f421135df55c8d844653ee3fb90_r.jpg)















https://www.zhihu.com/column/c_1425551698199130112

[Rocket Core: Return Address Stack](https://www.jianshu.com/p/27f38bae827d)

[Rocket Core: 译码逻辑](https://www.jianshu.com/p/ec38c4ce56ce)

