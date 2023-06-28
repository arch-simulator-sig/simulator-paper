# Kite入门模拟器

可以作为从来没有写过模拟器的同学的入门教程。时序模型设计简单易懂，共1266行

## 代码解析

注意到执行顺序是逆序，画一个流水线模型，发现每个clk从上往下看是逆序的，如果是正序的话，可能会覆盖正确的值。

```c++
        writeback();
        memory();
        execute();
        decode();
        fetch();
```

核心是`proc`，proc互联了各个组件包括memory bpu regfile data_memory data_cache inst_memory等，下面根据实现顺序依次解析。

### ref_file

`load_reg_state需要重写`，好的代码不言自明，不言自证。

检查数据依赖，通过判断rs1/rs2对应的rd寄存器是否ready，否则stall。如果rs1/rs2都未stall，加入当前的dep列表中

flush：dep列表清空。这里需要注意flush和stall的区别，一个传空值，一个保持。

### pipe_reg

只buffer了一个inst

### inst

get_inst_str需要重构，参考nemu

### inst_memory

load_program_code 和 parse_inst_str需要重构，elf_parser。

这里read write没有支持不对齐地址，需要重构。

整体太糙，需要完成重写，支持 8 16 32 64 128 bit等等

### br_predictor

Not implemented yet

### data_cache

该架构设计的比较能看的部分。该部分已经实现了多路组项链的结构，强制地址对齐，地址不对齐的可能都交由软件的异常部分进行处理。

hit则更新最后访问时间，miss记录missed_inst。设计采用的write back + write allocate方法。dirty写回到下级存储器仅仅将计数器++

### data_memory

connect太糙了，run方法主要让cache响应，load_block方法太糙。



## Assignment 1

添加完RV64I所有指令，实现nemu的diff功能，修改上文需要重写的部分。

## Assignment 2

实现竞争的分支预测

## Assignment 3

实现四路组相联cache

## Assignment 4

实现所有的CSR指令和FENCE相关内容

## 拓展

修改时序模型，pipe_reg是不是太糙了？

启RTT

启LINUX

 

