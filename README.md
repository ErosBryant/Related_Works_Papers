# [Paper List] AI4DB / ML4DB / LSM-tree


Welcome to PR!

New papers keep coming, remember to **Watch** this repo if you are interested in this topic.

* Log_Structured_File_System
   - [organize (kr.version)- Y.J ZHU](Log-Structured_File_System/Log-Sturctured_File_System_zhengli.pdf)

### `NVM`

* Redesigning LSMs for Nonvolatile Memory with NoveLSM (FAST 18)  
   - [organize (cn.version) - G.X Zhao](NVMe/NoveLSM_review.md)
* An efficient memory-mapped key-value store for flash storage (ACM 18)
* MatrixKV: Reducing Write Stalls and Write Amplification in LSM-tree Based KV Stores with Matrix Container in NVM (ATC 20)
* ChameleonDB: a Key-value Store for Optane Persistent Memory (EuroSys 21)
* Blizzard: Adding True Persistence to Main Memory Data Structures (arXiV 23)
* Prism: Optimizing Key-Value Store for Modern Heterogeneous Storage Devices (ASPLOS 23)
* Revisiting Log-Structured Merging for KV Stores in Hybrid Memory Systems (ASPLOS 23)
* Revisiting Secondary Indexing in LSM-based Storage Systems with Persistent Memory (ATC 23)
* ThanosKV: A Holistic Approach to Utilize NVM for LSM-tree based Key-Value Stores (Bigcomp 24)

---

### `Key-Value separation`

* WiscKey: Separating Keys from Values in SSD-conscious Storage (FAST 16)
* HashKV: Enabling Efficient Updates in KV Storage via Hashing (ATC 18)
* Differentiated Key-Value Storage Management for Balanced I/O Performance (ATC 21)
* Fencekv: Enabling efficient range query for key-value separation (IEEE Transactions on Parallel and Distributed Systems 22)

---

### `Compaction`

* On Log-Structured Merge for Solid-State Drives (ICDE 17)
* WOKV: A Write-Optimized Key-Value Store (ICCCBDA 18)
* Dostoevsky: Better Space-Time Trade-Offs for LSM-Tree Based Key-Value Stores via Adaptive Removal of Superfluous Merging (SIGMOD 18)
* Reducing Write Amplification of LSM-Tree with Block-Grained Compaction (ICDE 22)

---

### `Scalability`

* KVell: the Design and Implementation of a Fast Persistent Key-Value Store (SOSP 19)  
   - [organize (cn.version) - G.X Zhao](Scalability/sosp19-kvell.md)
* WB-Tree: Design of a Write-Optimized Data Store (VLDB 19)
* LUDA: Boost LSM Key Value Store Compactions with GPUs (VLDB 20)
* p2KVS: A Portable 2-Dimensional Parallelizing Framework to Improve Scalability of Key-Value Stores (EuroSys 22)  
   - [organize (cn.version) - G.X Zhao](Scalability/p2KVS_EuroSys'22.md)

---

### `Hot/Cold`

* TRIAD: Creating Synergies Between Memory, Disk and Log in Log Structured Key-Value (ATC 17)

---

### `K/V size`

* LSM-trie: An LSM-tree-based Ultra-Large Key-Value Store for Small Data (ATC 15)

---

### `Write stall`

* SILK: Preventing Latency Spikes in Log-Structured Merge Key-Value Stores (ATC 19)

---

### `Learned index & ML4DB`

* Leaper: A Learned Prefetcher for Cache Invalidation in LSM-tree Based Storage Engines (VLDB 20)  
   - [organize (kr.version) - Y.J ZHU](AI_LSM-T/Leaper.pdf)
* From WiscKey to Bourbon: A Learned Index for Log-Structured Merge Trees (OSDI 20)
* TridentKV: A Read-Optimized LSM-Tree Based KV Store via Adaptive Indexing and Space-Efficient Partitioning (IEEE Transactions on Parallel and Distributed Systems 21)
* TreeLine: An Update-In-Place Key-Value Store for Modern Storage (VLDB 23)
* LearnedKV: Integrating LSM and Learned Index for Superior Performance on SSD (arxiv 24)



