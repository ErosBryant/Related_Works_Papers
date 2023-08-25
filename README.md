##  `Related-Works`
> 整理有关Lsm-Tree的论文和研究方向

[Log_Structured_File_System](Log-Structured_File_System/Log-Sturctured_File_System.pdf)
   - [organize - Y.J ZHU](Log-Structured_File_System/Log-Sturctured_File_System_zhengli.pdf)

Collect some books and papers about distributed sysntem


## `Scalability`

[1. KVell: the Design and Implementation of a Fast Persistent Key-Value Store](https://github.com/ErosBryant/LSMT-DB-Related-Works/blob/b8a32ea6f6dfdb08501574dc5687832dc77acef3/Scalability/sosp19-kvell.pdf)
   - SOSP 2019

[2. WB-Tree:Design of a Write-Optimized Data Store](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/bde2f7c93b93bbc998171c8203d533af1e7b1f19/Scalability/WB-Tree.pdf)
   - VLDB 2019



[3. LUDA: Boost LSM Key Value Store Compactions with GPUs](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/cbc7a0725342181503304e253020a8047733c8ba/Scalability/LUDA.pdf)   
   - VLDB 2020
    - GPU

[4. p2KVS: a Portable 2-Dimensional Parallelizing Framework to Improve Scalability of Key-value Stores](https://github.com/ErosBryant/LSMT-DB-Related-Works/blob/9451c7d79208ee71dfad5afd96d1183bab4d7f78/Scalability/EUROSYS2022(p2KVS).pdf)
   -  EuroSys 22



## `NVM`
[1. Redesigning LSMs for Nonvolatile Memory with NoveLSM](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/20e8061994431c366565e514be976d69ef00cfbf/NVM/Redesigning%20LSMs%20for%20Nonvolatile%20Memory.pdf)
   
   - FAST 2018

[2. An efficient memory-mapped key-value store for flash storage](NVMe/SoCC18_efficient_kv_store.pdf)
   
   - ACM 2018


[3. MatrixKV: Reducing Write Stalls and Write Amplification in LSM-tree Based KV Stores with Matrix Container in NVM](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/162e83a566a00e61fb0e5562fa811064474608c9/NVMe/MatrixKV.pdf)
   - USENIX  ATC 2020
     - matrix container
     - column compaction


[4. ChameleonDB: a Key-value Store for Optane Persistent Memory](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/de2f745862325c98bf4853843be883c7a0b333b4/NVMe/ChameleonDB.pdf)
   - EuroSys 2021

[5. Blizzard: Adding True Persistence to Main Memory Data Structures ](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/1706a7a35cadfc20cfde6bda0fa489f253cb3c63/NVMe/Blizzard.pdf)
   - arXiV 2023


[6. Prism: Optimizing Key-Value Store for Modern Heterogeneous Storage Devices]( https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/de2f745862325c98bf4853843be883c7a0b333b4/NVMe/prism-song-asplos23.pdf)
   - ASPLOS 2023

[7. Revisiting Log-Structured Merging for KV Stores in Hybrid ](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/b1934aa80e981c7543f73524660284ec911f2b9a/NVM/Revisiting%20Log-Structured%20Merging%20for%20KV%20Stores%20in%20Hybrid.pdf)
   - ASPLOS 2023

[8. Revisiting Secondary Indexing in LSM-based Storage Systems with Persistent Memory](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/adf739024f901b1458d427c6b0b0bd7dee349653/NVMe/Revisiting%20Secondary%20Indexing%20in%20LSM-based%20Storage%20Systems%20with%20Persistent%20Memory.pdf)
   - USENIX ATC 23



## `Compaction`
> [整理一下Compaction论文的重点](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/7ab0b657365953fb5a8cd688128a2598b537a45b/Compaction/README.md)

[1. On Log-Structured Merge for Solid-State Drives](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/e6c49af37553cafa934d9ffc886802f71b93b19e/Compaction/On_Log-Structured_Merge_for_Solid-State_Drives.pdf)
   - ICDE 2017

[2. WOKV_A_write-optimized_key-value_store](https://github.com/ErosBryant/LSMT-DB-Related-Works/blob/358db4ffa3bf7ae29548a0f46438b2cec53acf9a/Compaction/WOKV_A_write-optimized_key-value_store.pdf)
   - ICCCBDA 2018
     
[3. Dostoevsky: Better Space-Time Trade-Offs for LSM-Tree Based Key-Value Stores via Adaptive Removal of Superfluous Merging](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/f6b16554ecd80f81dea7100ee422bf8e094ab472/Compaction/dostoevskykv.pdf)
   - Proceedings of the 2018 International Conference on Management of Dat
     
[4. Reducing Write Amplification of LSM-Tree with Block-Grained Compaction](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/f6b16554ecd80f81dea7100ee422bf8e094ab472/Compaction/Reducing%20Write%20Amplification%20of%20LSM-Tree%20with%20Block-Grained%20Compaction.pdf)
   - ICDE 2022


## `Hot/Cold`
[1.TRIAD: Creating Synergies Between Memory,Disk and Log in Log Structured Key-Value Stores](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/abb3b05e20f93b9a93555447e6f9b1c28ea017ca/HotClod/TRIAD.pdf)
- [presentation](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/2f69b66f562bd7d3438221c4d9b64906394efe07/HotClod/LSM-trie_%20An%20LSM-tree-based%20Ultra-Large%20Key-Value%20Store%20for%20Small%20Data%20%E8%AF%B4%E6%98%8E.pdf)
- USENIX ATC 17

## `K/V size`
[1. LSM-trie: An LSM-tree-based Ultra-Large Key-Value 
Store for Small Data ](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/82f5ae6a6065035f13fb038d4c910efd270cfd69/Small%20kv%20size/Lsm-trie.pdf)
- USENIX ATC 15

## `write stall`
[1.SILK: Preventing Latency Spikes in Log-Structured Merge Key-Value Stores](https://github.com/ErosBryant/LSM-T_DB_Related_Works/blob/956e6e4a891380cf0d2bdaf81ad68d6c18f07a32/write%20stall/SILK.pdf)
- USENIX ATC 19


## `AI`
[]