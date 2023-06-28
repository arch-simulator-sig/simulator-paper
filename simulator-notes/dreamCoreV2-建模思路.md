# 建模思路

# 总体

分为component和pipeline



## component

### dff

dff_base维护 dff_list ， sync_all 和 reset_all方法，dff核心方法，类似verilog的reg。

cur_value当前值，new_value更新值

```c++
dff(T init_value)
            {
                this->init_value = init_value;
                this->cur_value = init_value;
                this->new_value = init_value;
            }

dff(const dff<T> &other)
            {
                this->cur_value = other.cur_value;
                this->new_value = other.cur_value;
            }

virtual void sync()
            {
                this->cur_value = this->new_value;
                this->changed = false;
            }
//仅在set时changed改变           
```



### fifo

生产者函数 ：连续push多个元素的时候，之后判满判空的生产者版本函数可以立即检测到之前所有的push状态

消费者函数：只有在下个周期以后才能观测到这个状态
