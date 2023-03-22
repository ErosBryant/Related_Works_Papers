## 1.  `WOKV:A write-optimized key-value store`

把多个SSTable合并成一个SSTable，这个过程叫做Compaction。这个过程会产生大量的写放大，因为要把多个SSTable的数据合并到一个SSTable中，这个过程会产生大量的写操作。为了减少写放大，WOKV提出了Least-Rewrite Compaction Strategy，这个策略会尽量减少写放大。

> R = Sa / Sb
> - sa: upper level sstable size
> - sb: next level sstable size

选择 R 最小的 SSTable 进行合并，这样写放大最小。

![image](https://user-images.githubusercontent.com/86946575/226817141-cc17ac77-3a5f-4571-836a-562b6b34f5b2.png)

<br><br><br>
## 2. `On Log-Structured Merge for Solid-State Drives`

在下一层中与许多块重叠，但通过做更多的工作，我们可以选择与最少块重叠的范围。
我们称此策略为 `ChooseBest`。

- 在所有数据集大小和所有底层大小范围（相对于最大值）中，ChooseBest的成本始终低于Full——对于20MB数据，底层大小约为其最大值的20%，而对于100MB数据，底部大小约为Full。
- 成本通常随着底层规模的增长而线性上升，但ChooseBest的增长率低于Full。

- ChooseBest在Normal中比在Uniform中比Full有更大的优势。正如我们将在第五节中看到的那样，这种优势在工作负载更加倾斜的情况下更加明显。
  
<br><br><br>
## 3. `Reducing Write Amplification of LSM-Tree with Block-Grained Compaction`

在传统的LSM-Tree压实策略中，每个键值对都是独立压实的，这意味着同一数据可能会被多次写入磁盘。这会导致写放大问题，并降低LSM-Tree的性能。为了解决这个问题，作者提出了一种新的压实策略，即块级压实。

块级压实将LSM-Tree分成固定大小的块，并将每个块视为一个整体。当一个块被填满新数据时，它将与相邻的块合并成一个更大的块。这样，每个块的压实只需进行一次，这大大降低了同一数据被多次写入磁盘的次数，从而减少了写放大问题。

![image](https://user-images.githubusercontent.com/86946575/226840790-ec6b2bdb-5a46-4097-9215-f0c0a1561132.png)

Block Compaction过程简单讲解
- Li 层的SSTable需要Compaction到下一层，以其中两个Block为例
- key=22落入下一层block(21,25)，key=38落入block(37,41)，key=51，key=60无对应 下层block。从而可看到 
- Li+1层中block（21，25）,block(37,41)识别为dirty block，其余块为clean block。
- dirty block不动，clean保持，在SSTable末尾添加新的block，包括dirty block合并后的新block和未匹配上的keys组成的新的block。
- 旧的index block失效，新建一个index block。
- 由于BLOCK(51，60）被下一层的block（53，57）打断，51和60必须分拆到两个block中。


block compaction的思路是可以的，以block 为粒度，确实可以减少写。

但是作者的处理，旧的块不删除，导致了比较多的空间浪费。

clean block位置不动是为了保持block cache的有效。同时做到不用写clean block。

最底层如果使用了block compaction的话，dirty block将没人来回收，占用只会越来越满。


block的重复比例没有用实验给出比较没有说服力

<br><br><br>
## 4. `Dostoevsky: Better Space-Time Trade-Offs for LSM-Tree Based Key-Value Stores via Adaptive Removal of Superfluous Merging`

![](https://img.toutiao.io/c/3c80b77e2dffe70bcb9cc1f7bc67a7e3)

||leveled|tiered|
|--|--|--|
|Updatas|O(L * T / B)|O(L / B)|
|Point Lookups| O(L)|O(L * T)|
|Range Lookups|O(L)|O(L * T)|
|SAF| O(1 / T)|O(T)|

### `Lazy Leveling`

首先就是 Lazy Leveling，原理非常简单，也就是混合了 Tiered 以及 Leveled，在最大层使用 Leveled，而其它层使用 Tiered。这样最大层的 runs 就是 1，而其他层的则是 T - 1。另外，因为小的 Level 现在使用的是 Tiered，为了加速点查，Dostoevsky 为不同的 Level 的 Bloom filter 使用了不同的内存。

![](https://img.toutiao.io/c/4883d730aa83d16203fd8bcf9c1d3185)

上图列出了使用 Lazy Leveling 跟其他两种 compaction 方式的对比，可以看到，在不同 case 下面的最坏开销其实还是挺不错的。譬如对于 update 来说，在 Level 1 到 L - 1 层，开销都是O(T)，而 L 层则是O(L)，那么总的开销就是O((T + L) / B)。而对于空间放大来说，因为最大层有最多的 entries，所以整体的开销仍然接近于O(1 / T)。

### `Fluid LSM-Tree`

在 Lazy Leveling 基础上面，Dostoevsky 引入了 Fluid LSM-Tree，其实原理也很简单，相比于 Lazy Leveling 最大层是 Leveled，其它层是 Tiered，Fluid 使用了一个可调解的方式，在最大层使用最多 Z runs，而其它层最多使用 K runs。

可以发现：

- Z = 1， K = 1，就是 Leveled Compaction
- Z = T - 1， K = T - 1，就是 Tiered Compaction
- Z = 1， K = T - 1，就是 Lazy Leveling

