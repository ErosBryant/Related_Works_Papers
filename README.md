##  `Related-Works`
> 整理有关Lsm-Tree的论文和研究方向

Log_Structured_File_System
   - [organize (kr.version)- Y.J ZHU](Log-Structured_File_System/Log-Sturctured_File_System_zhengli.pdf)


## `NVM`
1. Redesigning LSMs for Nonvolatile Memory with NoveLSM
   - FAST 18
   - [organize (cn.version)- G.X Zhao](NVMe/NoveLSM_review.md)
  
2. An efficient memory-mapped key-value store for flash storage
   - ACM 18
3. MatrixKV: Reducing Write Stalls and Write Amplification in LSM-tree Based KV Stores with Matrix Container in NVM
   - USENIX  ATC 20
4. ChameleonDB: a Key-value Store for Optane Persistent Memory
   - EuroSys 21
5. Blizzard: Adding True Persistence to Main Memory Data Structures
   - arXiV 23
6. Prism: Optimizing Key-Value Store for Modern Heterogeneous Storage Devices
   - ASPLOS 23
7. Revisiting Log-Structured Merging for KV Stores in Hybrid Memory Systems
   - ASPLOS 23
8. Revisiting Secondary Indexing in LSM-based Storage Systems with Persistent Memory
   - USENIX ATC 23
9. ThanosKV: A Holistic Approach to Utilize NVM for LSM-tree based Key-Value Stores
   - Bigcomp 24  


## `Key-Value separation`
1. WiscKey: Separating Keys from Values in SSD-conscious Storage
   - Fast 16
2. HashKV: Enabling Efficient Updates in KV Storage via Hashing
   - ATC 18
3. Differentiated Key-Value Storage Management for Balanced I/O Performance
   - ATC 21 
4. Fencekv: Enabling efficient range query for key-value separation
   - IEEE Transactions on Parallel and Distributed Systems 22


## `Compaction`
> [organize (cn.version) - GX Zhao :整理Compaction论文的重点](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/7ab0b657365953fb5a8cd688128a2598b537a45b/Compaction/README.md)

1. On Log-Structured Merge for Solid-State Drives
   - ICDE 17
2. WOKV_A_write-optimized_key-value_store
   - ICCCBDA 18

3. Dostoevsky: Better Space-Time Trade-Offs for LSM-Tree Based Key-Value Stores via Adaptive Removal of Superfluous Merging
   - SIGMOD 18
     
4. Reducing Write Amplification of LSM-Tree with Block-Grained Compaction
   - ICDE 22


## `Scalability`

1. KVell: the Design and Implementation of a Fast Persistent Key-Value Store
   - SOSP 19
   - [organize (cn.version)- G.X Zhao](Scalability/sosp19-kvell.md)
2. WB-Tree:Design of a Write-Optimized Data Store
   - VLDB 19
3. LUDA: Boost LSM Key Value Store Compactions with GPUs
   - VLDB 20
4. p2KVS: a Portable 2-Dimensional Parallelizing Framework to Improve Scalability of Key-value Stores
   -  EuroSys 22
   - [organize (cn.version)- G.X Zhao](Scalability/p2KVS_EuroSys'22.md)



## `Hot/Cold`
1.TRIAD: Creating Synergies Between Memory,Disk and Log in Log Structured Key-Value 
   - USENIX ATC 17

## `K/V size`
1. LSM-trie: An LSM-tree-based Ultra-Large Key-Value Store for Small Data 
   - USENIX ATC 15

## `Write stall`
1.SILK: Preventing Latency Spikes in Log-Structured Merge Key-Value Stores
  - USENIX ATC 19


## `AI & Learned index`
1. Leaper: A Learned Prefetcher for Cache Invalidation in LSM-tree based Storage Engines
   - VLDB 20
   - [organize (kr.version) - Y.J ZHU](AI_LSM-T/Leaper.pdf)

2. From WiscKey to Bourbon: A Learned Index for Log-Structured Merge Trees
   - OSDI 20
3. TridentKV: A Read-Optimized LSM-Tree Based KV Store via Adaptive Indexing and Space-Efficient Partitioning
   - IEEE Transactions on Parallel and Distributed Systems, 21 
4. TreeLine: An Update-In-Place Key-Value Store for Modern Storage
   - VLDB 23   
5. LearnedKV: Integrating LSM and Learned Index for Superior Performance on SSD
   -  arxiv 24



