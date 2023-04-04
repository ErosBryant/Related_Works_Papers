## Redesigning LSMs for Nonvolatile Memory with NoveLSM

### `Abstract`
NoveLSM - 使用NVM的LSM树. 以实现低延迟和高吞吐量

1. a byte-addressable skip list
2. direct mutability of persistent state
3. opportunistic read parallelism

### `Introduction`

- 例如 falsh memory 和 hdd，NVM 表现出以下在当前 LSM 设计中未利用的属性：

(1) 随机访问持久存储可以提供高性能；

(2) 就地更新成本低；

(3) 低延迟和高带宽的结合带来了提高应用级并行度的新机会

- `重新设计LSM树，以利用NVM的这些特性`
  
重新设计的 LSM 为数以千计的应用程序提供了向后兼容性。

考虑到比 DRAM 写入延迟高 5 到 10 倍的情况，即使对于 NVM，保持批量、顺序写入的优势也很重要

- `设计目标`
  
首先，NoveLSM 引入了一个持久的基于 NVM 的内存表，显着降低了困扰标准 LSM 设计的序列化和反序列化成本。

其次，NoveLSM 使持久性 NVM 内存表可变，从而允许直接更新； 这显着减少了由于压缩导致的应用程序停顿。

此外，对 NVM memtable 的直接更新是就地提交的，避免了记录更新的需要； 因此，故障后的恢复只涉及映射回持久性 NVM 内存表，使其比 LevelDB 快三个数量级。

三、NoveLSM引入乐观并行读，同时访问NVM或SSD中可能存在的多级LSM，从而降低读请求的延迟，提高系统的吞吐量


![image](https://user-images.githubusercontent.com/86946575/229415617-4614fd50-97f6-4cbc-ad68-a2a98d1318b7.png)

### `Background`

> Byte-addressable NVMs

与 SSD 相比，PCM 等 nVM 技术是·字节可寻址·的持久性设备，预计可提供低 100 倍的读写延迟和高达 5 到 10 倍的带宽 [7、12、17、22]。

> nvm emulaton

我们模拟 NVM 类似于之前的研究 [12、17、22、26]，我们的模拟方法使用 2.8 GHz、32 核 Intel Nehalem 平台和 25 MB LLC，双 NUMA 插槽 每个插槽包含 16 GB DDR3 内存和一个 Intel-510 系列 SSD。

我们使用 Linux 4.13 内核运行为持久内存设计的 DAXenabled Ext4 [6] 文件系统。

  我们使用一个 NUMA 插槽作为 NVM 节点，为了模拟比 DRAM 更低的 NVM 带宽，我们对 NUMA 插槽进行热调节 [25]。

为了模拟更高的写入延迟，我们使用 NVM 模拟器 [37] 的修改版本，并通过估计处理器存储缓存未命中的数量来注入延迟 [12, 17,22]。

### `motivation`

NVM은 SSD에 비해 훨씬 낮은 대기 시간과 최대 8배 더 높은 대역폭을 제공할 것으로 예상됩니다. 그러나 현재 LSM 소프트웨어 스택이 NVM의 하드웨어 성능 이점을 완전히 활용할 수 있습니까?

![image](https://user-images.githubusercontent.com/86946575/229420563-80ab1952-fdf0-405e-89c0-6e4c5ed2661c.png)

>  motivation 总结

总而言之，现有的 LSM 存在插入和读取操作的高软件开销，并且无法利用 NVM 的字节寻址能力、低延迟和高存储带宽。

삽입 작업은 주로 압축 및 로그 업데이트 오버헤드가 높고 읽기 작업은 순차 검색 및 역직렬화 오버헤드가 있습니다.

이러한 소프트웨어 오버헤드를 줄이는 것은 NVM의 하드웨어 이점을 최대한 활용하는 데 중요합니다.

### `design`

1. 利用字节寻址能力来降低序列化和反序列化成本
    - 在压缩过程中，DRAM 内存表数据可以直接移动到 NVM 内存表中，无需序列化或反序列化 （没太能理解）

        序列化 : 将数据从内存中的数据结构转换为可以写入磁盘的格式的过程
        反序列化 : 将数据从磁盘中的格式转换为内存中的数据结构的过程

2. 实现持久状态的可变性，并利用NVM的大容量来降低压缩成本。
   - NVM 字节寻址能力提供了直接更新存储上数据的机会，而无需将它们读取到内存缓冲区或批量写入。 

3. 通过就地持久性减少日志记录开销和恢复成本。
    - NoveLSM 通过将 NVM 内存表映射到内存中的内存表来实现就地持久性。

4. 利用 NVM 的低延迟和高带宽来并行化数据读取操作 
    - NoveLSM 通过并行化数据读取操作来利用 NVM 的低延迟和高带宽。 

![image](https://user-images.githubusercontent.com/86946575/229447473-6d633d8e-c7fb-4bf9-a6c5-4e9e0c9c4399.png)
> we implement a Bloom filter for NVM and DRAM memtable

                 写入数据 -> DRAM memtable -> Nvm memtable  -> sstable
                 즉 write stall 없앨수 있다!



### `evaluation`

> 图5的 a 构造

1. NoveLSM+immut-small = 2GB imm memtable
2. NoveLSM+immut-large = 4GB imm memtable
![image](https://user-images.githubusercontent.com/86946575/229453384-54e34e2f-fcc3-4f6a-866f-47b68744e0a1.png)

> 图5的 b 构造

![image](https://user-images.githubusercontent.com/86946575/229458012-e018bf3e-ca07-4594-a21d-f0682fd09590.png)


---
## `总结`

NoveLSM的优化主要有三点：

1. NoveLSM在NVMs上增加了一个组件，当DRAM中的内存组件写满之后 后续的写入会落到在NVMs上的组件，从而减少write-stall。

2. 去掉了WAL，因为NVMs本身提供了持久化能力，不需要WAL来保证数据一致性。

3. 为了降低读延时，NoveLSM提供了不同Level 之间的并发查找能力。


            在LSM Tree中，数据的持久化通常是通过将内存中的数据序列化后写入磁盘来实现的。序列化是将数据从内存中的数据结构转换为可以写入磁盘的字节序列的过程。而反过来，将磁盘上的数据读入内存中的过程，则称为反序列化。

            在LSM Tree中，反序列化的过程是指将磁盘上的数据读入内存，并将其转换为数据结构的过程。在数据查询时，需要先将磁盘上的数据反序列化到内存中，然后再进行查询操作。反序列化是一项重要的操作，因为它直接影响LSM Tree的查询性能。

            在LSM Tree的实现中，通常使用一些技术来加速反序列化的过程，例如预读取、数据压缩、缓存等。这些技术可以减少磁盘访问的次数和数据传输的量，从而提高查询性能。
            
            
现在对于noveLSM的痛点似乎更加清楚了。痛点一，noveLSM在NVM中使用持久化的skiplis结构，skiplist的复杂度是log(n)，随着数据量的增加，插入和查找的开销都会增加；痛点二，noveLSM的skiplist继承自leveldb的skiplist实现，Put/Delete操作都只会插入新的节点而不会删除原有节点（snapshot、lock-free特性），换言之没有垃圾回收机制，而NVM的空间很珍贵需要充分利用；痛点三，当NVM被写满后，整个skiplist会被迁/compaction到下层存储设备，这种大粒度、阻塞式的数据迁移机制不合理，会造成write stall，严重影响系统性能。       
