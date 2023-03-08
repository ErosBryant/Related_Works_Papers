# KVell: the Design and Implementation of a Fast Persistent Key-Value Store

## `简要`
- hdd --> sata ssd --> nvme ssd --> optane ssd --> optane pmem ，不论是带宽还是延时，都在以量级的方式不断得被优化。
- 使用 LSM-T & B-Tree 。
- Many core 和 Disk 无序 和 No commit log


## `KVell 设计`

![image](https://user-images.githubusercontent.com/86946575/223605142-91959176-f460-433e-be8a-33ace051be3b.png)

1. `Share nothing`
- 这里是说kvell 允许用户指定不同的work thread,每一个work thread 会绑定一个cpu-core 来单独处理一批ke y。线程内部空间独享存储的key相关的数据结构如下：
    1. 轻量级的内存索引。能够支持从内存索引一个key到对应的磁盘位置上。
    2. I/O队列。支持高效的读取请求和写入请求 到磁盘。
    3. free list。管理磁盘块的部分列表，包含用于存储k/v 的空闲位置。
    4. pagecache。自实现了一个类似于操作系统的Pagecache缓存page，加速热点读性能，而不是依赖操作系统本身的pagecache。
- 这个 shared-nothing 的架构是KVell 和其他 k/v 存储主要的一个差异，它独享自己的数据结构，从而避免了多线程之间同步数据结构的开销，尤其是NUMA架构下的跨NUMA的内存访问，对于多线程下的内存操作的性能影响还是很大的。


![image](https://user-images.githubusercontent.com/86946575/223605720-3b0f92d2-24f3-4cdc-9db5-644c49185043.png)
![image](https://user-images.githubusercontent.com/86946575/223605815-63c11dd0-70f6-422b-8afa-8d1548d316b2.png)


2. `Do not sorted on disk, but keep indexes in memory`
- 每一个workthread 维护的磁盘存储数据集是无序的，这样能够完全释放之前的LSM / B-tree 为了维护有序的磁盘数据结构而产生的CPU开销。

3. `Aim for fewer syscalls , not for sequential I/O`
- KVell 的操作都是随机I/O，因为 on -nvme的随机I/O和顺序I/O的性能差不多，这个时候不需要消耗额外的CPU去维护顺序I/O。和 LSM-tree 的k/v存储一样，kvell也提供batch操作，不过kvell的目的是为了减少系统调用的次数，尽可能得减少线程陷入内核的次数。

- 如果想要让磁盘的吞吐维持在他们的峰值qps，那就需要让磁盘支持的队列深度有足够多的可调度的请求，这个时候batch-size的大小设置就需要trade-off，一般是小于磁盘的最大队列深度，否则会导致请求的排队时间过长，增加长尾延时。所以KVell 调度过程的建议是每一个workthread 只操作一个磁盘，因为不同的workthread之间是独立处理请求的，他们之间并不能感知对底层磁盘队列深度的压力。

- 当然，多个workthread 操作同一个磁盘的时候，可能需要一个比较合理的batch-size 来进行压力上限的调整。在热点场景下（频繁调度一个workthread）的时候， 自实现的内部pagecache能够扛住大多数的读请求，从而一定程度得减少了系统调用的I/O操作。

4. No commit log
- KVell 的更新完成是指持久化的磁盘，并更新了内存的索引之后才会返回，这样的话也就不需要WAL的参与了。一旦一个请求已经提交给当前的workthread，那它会在下一个bacth中持久化到磁盘。通过移除了commit log，能够更进一步得由用户请求独占磁盘带宽。

- 而且KVell 在磁盘上的更新是随机写入（update场景直接更新对应的slab-file），并不会有为了解决 append-only产生的多版本问题 而 引入compaction 操作，所以磁盘资源基本都贡献给了上层的用户请求。



![image](https://user-images.githubusercontent.com/86946575/223605634-c8a7d7cf-b7f3-4972-b213-752dee63e653.png)



## `测评`
![image](https://user-images.githubusercontent.com/86946575/223606448-06bce7ac-d333-4008-bc04-ba101a1cb058.png)
![image](https://user-images.githubusercontent.com/86946575/223606659-1405ff0b-398d-413f-8133-1240217dc020.png)
