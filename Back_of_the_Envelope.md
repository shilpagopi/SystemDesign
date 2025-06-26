# Back_of_the_Envelope

## Back-of-the-envelope Calculation
* Estimate QPS (queries per second),DAU (daily active users) and storage requirements:  \
* Peek QPS = 2 * QPS  \
* 1 Char = 1 byte, 1 Integer = 4 bytes, 1 Float = 4 bytes
* 1 day = 10^5 seconds
* 1 year = 3 x 10^7 seconds
<img width="819" alt="image" src="https://github.com/user-attachments/assets/6dfa9681-c5e5-463d-920a-f1d628531c0e">

## In-Memory (RAM) Capacities

Typical Ranges for a Single Modern Server:
* Entry-Level/General Purpose: **64GB** to 128GB
* Mid-Range/Database/Application Servers: 256GB to 768GB
* High-End/HPC/Large Database Servers: 1TB to 4TB+ (Some specialized servers can go much higher, e.g., for in-memory databases like SAP HANA, reaching tens of TBs).

10,000connections/server

## Disk Storage
* HDD (Hard Disk Drive): Much larger capacities, lower cost per GB. Slower I/O and higher latency than SSDs. Individual Server Disk: **200TB**
  If using servers with, say, 16 x 16TB HDDs (16TB is a common enterprise HDD size), that's 16×16TB=256TB raw capacity per server. With RAID 6 (e.g., 2-disk parity, or roughly 80% usable), that's about 200TB usable per server.
* SDD (solid State Drive) :
  Typical Capacities for a Single Modern Server:
  * Boot Drive/OS/Logs: 500GB to **2TB** (NVMe SSDs are common here for speed).
  * Primary Data Drives (General Purpose): 2TB to 8TB (SATA/SAS SSDs).
  * High-Performance Data Drives (e.g., Databases): 1TB to 15TB+ (NVMe SSDs or high-end enterprise SATA/SAS SSDs are common for transactional workloads).
