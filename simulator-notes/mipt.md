# MIPT模拟器
## Module 设计
采用树状设计，维护模块层级关系，不用于任务调度/执行。（Branch模块样例中自行添加`void Clock(Cycle cycle);`。因此执行流程需要自行设计。
提供保存模块层级关系功能（基于pt::ptree）。

## Port 设计
1. 创建
    基于Module，通过Module::make_write_port、Moudle::make_read_port创建端口，创建端口时将端口对象记录到Root模块的PortMap中`键值对：name-{writer,readers}//即同名下，一个writer对应多个reader`。
2. 连接与初始化
    在创建完成后，Root节点调用`init_portmap();`，在此阶段会遍历所有port的`init();`接口。
    其中`ReadPort::init();`根据bandwidth和latency参数创建足够大小的`std::queue`，queue中存储数据与写入周期。
    `WritePort::init();`将所有同名的ReadPort保存到Vector中便于使用。
3. 端口写入
    WritePort在写入时检查本周期写入次数是否超过bandwidth，超过会抛出异常。
    遍历所有初始化阶段保存在Vector的ReadPort，将`{数据,周期}`写入到所有ReadPort内部的queue中。写入queue时会丢弃未读取的数据（如果有的话）。
4. 端口读取
    ReadPort在读取前应该调用`bool ReadPort::is_ready();`检查是否有数据可供读取，检查的逻辑是判断队列不为空且队列头部元素恰好应当是本周期读取`cycle == read_cycle`。（DQC注：如果某个周期未读取数据，那么下周期再次读取的为上周期数据，这时直接返回false，会误判为无数据可读，事实上可能这周期依然有新的数据可读。解决思路：保持老数据有效检查条件`cycle >= read_cycle`或者检查到老数据时直接丢弃老数据并重新检查队列是否为空）
5. 总结
    读写均采用记录cycle设计，无需维护指针（gem5采用手动在周期结束时更新读写指针）。
    端口连接通过PortMap实现，将同名端口自动连接，代码信息熵高。
    写端口直接通过接口传递数据，无需指针参与（Asim通过共享BufferStorage指针实现数据传递）。


