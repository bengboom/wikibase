## File Management
### Objective
系统管理数据的要求
文件有效
保证性能和响应时间
多种不同设备的IO支持
避免数据丢失损坏
提供标准I/O接口
支持多用户操作
### Function
identify, locate file
use **dictionary** to describe location and attributes
describe user access
**Blocking** 组块策略 for access to files
allocate files to free blocks
manage free storage for available blocks
### Elements

![[Screenshot 2023-11-16 at 11.53.58.png]]

整体结构
![[Screenshot 2023-11-21 at 11.03.47.png]]

#### Device Drivers
最底层，与串行设备直接通信，负责启动和完成I/O操作
#### Basic File System
- Physical I/O
- Deals with exchanging blocks of data
- concerning: placement of blocks & buffering blocks in main memory
- 负责记录record与块block的转换
#### Basic I/O supervisor
- 负责I/O init & termination
- 保持控制结构 （FAT32 NTFS等）
- 访问调度 **scheduling access** -> optimize performance
- OS的一部分，分配I/O buffer & secondary memory
#### Logical I/O
为用户和应用提供通用能力，保留文件基本信息
#### Access Method
Pile Sequential Indexed-Seq Indexed Hashed

## File Organization and Access Methods
File Org: refers to
- logical structuring of records
Physical Org: depends on
- blocking strategy
- file allocation strategy
### Criteria
快速、容易更新、存储经济（冗余小）、管理维护容易、可靠
### 种类
#### Pile 堆文件
- data按照到达顺序收集积累
- 目的：积累大量数据并保存
- 没有结构 无结构文件
- 访问：用大量的exhaustive搜索访问记录 非常耗时
- ![[Screenshot 2023-11-21 at 11.32.22.png]]
#### Sequential 顺序文件
- 记录有固定格式，等长，结构相同
- 文件名和长度是其属性
- 一个field是key, uniquely identifies the record
- ![[Screenshot 2023-11-21 at 11.33.27.png]]
- 适合批量存储batch applications
- 唯一一种既适合在tape也可以在disk上的存储方式
- poor performance for querying/updating an individual record 单一文件性能差
- New records are placed in **a log file or transaction file.** 日志或事务处理文件
- Batch update is performed to merge the **log file** with the **master file.**
#### Indexed-Sequential file 索引顺序文件
- 空间换时间 索引 records are organized in sequence based on a key field
- add index -> support random access and overflow file
- index file
	- contains key field and a pointer to main file
	- 索引是减小搜索范围，索引给出更小的搜索范围这样搜索量就会更小
	- lookup capacity to quickly reach **the vicinity** of the desired record
- overflow file (similar to log file)
	- 先放在overflow file 索引文件指向overflow文件
	- 如果新纪录的前序记录在主文件，则由主文件指向overflow file
	- ![[Screenshot 2023-11-21 at 11.55.04.png]]
	- overflow merged to main file during batch update
	- multiple indexed for the same key field can be set up -> increase efficiency
	- 索引的优势
		- • A file contains 1 million records. • On average 500,000 accesses are required to find a record in a sequential file. • If an index contains 1000 entries, it will take on average 500 accesses to find the key, followed by 500 accesses in the main file. Now on average it is 1000 accesses.
		- 若一级索引文件包含10,000条记录，再为之建立二级索引，其中包含100条记录 • 在二级索引文件中平均检索50条记录，可以找到 关键字 • 再在一级索引文件中平均检索50条记录找到指向 主文件的指针 • 最后平均检索50条记录，找到目标记录，共计平均检索150条记录

#### Indexed 索引文件
- 顺序文件和索引顺序文件都只允许**按记录的唯一关键字**检索记录，这不符合某些应用需要按多个字段检索记录的要求 
- 主文件的记录不必按关键字排序
- Uses multiple indexes for different key fields.
- May contain an exhaustive index(完备索引)that contains one entry for every record in the main file. 
- May contain a partial index(部分索引)that contains entries to records where the field of interest exists
- ![[Screenshot 2023-11-22 at 22.15.07.png]]

#### Directed/Hashed 直接/哈希文件
- Directly access a block at a known address.
- Key field required for each record.
- 利用Hash函数，根据记录的关键字计算记录 的存储位置，并按关键字访问记录，提高记录 的访问效率
- 常用于访问速率要求高、一次存取一条记录且记录为定长的文件。如文件目录、价格表、名单等文件记录的存取

## File Directions
- Directory itself is a file owned by the operating system.
- Provides mapping between file names and the files themselves.
Contains information about files. 
- Basic Information
	- File Name, File Type, File Organization 
- Addr. Information 
	- Volume, Starting Addr., Size Used, Size Alloc.
- Access Contr. Information 
	- Owner, Access Info., Permitted Actions 
- Usage information
	- Date Created, ID. of Creator, Date Last Read Access, ID of Last Reader, Date of Last Modified, ID of Last Modifier, Date of Last Backup, Current Usage

操作：Search, Create, Delete, List Dictionary

### Structure
#### Simple Structure
一级目录 应用场景简单
List of entries, one for each file.
Sequential file with the name of the file serving as the key.
Provides **no help in organizing** the files.
Forces user to be careful not to use the same name for two different files.
#### Two-level Scheme for a Dictionary
二级目录
One directory for each user and a master directory.(用户目录和主目录) 
Master directory contains entry for each user.
	Provides address and access control information.
Each user directory is a simple list of files for that user.
Still provides **no help in structuring collections of files.**
#### Hierarchical or tree-structured
树形目录
Master directory with user directories underneath it.（主目录下建立若干用户目录）
Each user directory may have subdirectories and files as entries.（每个用户目录下允许再创建子目录及文件目录）
目前的主流使用的，最方便![[Screenshot 2023-11-23 at 10.29.48.png]]
文件路径太长了效率很低，于是又有pathname长度、目录层数的限制。总有tradeoff。
文件由路径访问pathname, from root/master, down to branches
不同目录下可以有文件名相同的文件
current dictionary: working dictionary
files are referenced relative to current dictionary 相对路径
## File Sharing
### access rights
- 种类
	- none
		- hidden file's existence to user
	- knowledge 探知
		- only know existence and owner
	- execution
		- cannot copy
	- reading
		- read & copy & execution (read for any purpose)
	- appending
		- add data, cannot modify or delete
	- updating
		- can modify and delete
	- changing protection
		- change access rights granted to other users
	- deletion

Owners: Have all rights listed.
用户分组：授权给特定某个用户、用户组、公用public
### management of simultaneous access
User may lock entire file when it is to be updated.
User may lock the individual records during the update.
同步互斥
## Record Blocking
记录是存取文件的逻辑单位，而数据块是I/O的基本单位，记录必须组织成数据块以便于I/O。
大多数系统采用固定长度的数据块，以简化I/O操作、Buffer的分配及辅存中数据块的组织管理。
大数据块适合顺序访问，随机访问效率低。大数据块需要更大的I/O buffer。
- fixed blocking
	- 块内浪费空间![[Screenshot 2023-11-23 at 10.34.56.png]]
	- hardware gap: 利于磁头更好定位扇区
- variable blocking
	- spanned
		- 允许一条记录跨越两个数据块，用指针连接 磁道变化性能慢![[Screenshot 2023-11-23 at 10.35.45.png]]
	- unspanned
		- 变长但不允许跨越，块内存在浪费![[Screenshot 2023-11-23 at 10.36.35.png]]
## Secondary Storage Management
空间分配、空闲空间记载
- Preallocation
	- 创建文件时分配最大可能的空间，但难以估计、估大了浪费空间
- Dynamic allocation
	- allocate space to a file in portions as needed

### Portion size
分区大小：最大 能容纳整个文件，最小 1个磁盘块大小
Tradeoff: 单文件效率 vs 整个系统效率
连续空间提升表现。一对碎片分区让管理更难。
- Having fixed-size portions (for example, blocks) simplifies the reallocation of space.
- Having variable-size or small fixed-size portions minimizes waste of unused storage due to overallocation.
### Methods
#### Contiguous allocation 连续分配
Preallocation strategy using variable-size portions.
Single set of blocks is allocated to a file at the time of creation.
Only a single entry in the file allocation table.(Starting block and length of the file)![[Screenshot 2023-11-23 at 11.31.56.png]]
Allocation strategies
- first fit: Choose the first unused contiguous group of blocks of sufficient size.
- best fit 让内零头更小 Choose the smallest unused group of sufficient size 可能剩下很多单个小扇区未被分配，整体的空间浪费高，需要大量的compaction
- nearest fit: Choose the unused group of sufficient size that is closest to the previous allocation for the file to increase locality.

有外零头，需要compaction algorithm -> free up space
![[Screenshot 2023-11-23 at 11.34.15.png]]
#### Chained allocation 链接分配
无外零头，适合序列文件，没有对局部性原理的适应性。
Each block contains a pointer to the next block in the chain.
![[Screenshot 2023-11-23 at 11.34.42.png]]

访问数量多。一些系统周期性的consolidate整合文件，让链表在物理空间上也变成连续的
只是为了让存储空间有效，可靠性不好，中间断了后面的地址就找不到了。
#### Indexed allocation 索引分配
File allocation table contains a separate one-level index for each file.(一级索引)
文件分配表记录索引节点，索引节点下一层记录文件具体位置。
Fixed size blocks: ![[Screenshot 2023-11-23 at 11.35.50.png]]
**variable length portions** 这样可以减少(连续的几个块只记录一次 降低了记录量 也利用了局部性原理)   **此办法得到了广泛应用**  无外零头，可能需要file consolidation，支持序列或直接访问文件 **Most popular form**
![[Screenshot 2023-11-23 at 11.40.14.png]]

如果还要继续改进：如Linux中的**多级文件索引**：![[Screenshot 2023-11-23 at 11.48.14.png]]
(回顾：[[Part 3 Memory Management]] 多级页表)

拓展：软连接硬链接，软连接可以用于版本管理 VersionControl，例如Python2/3
### Free Space Management

#### Bit table 位表
位表，又称为位示图，其中的每一位对应一个磁盘块。位的值为0或1，分别表 示磁盘块空闲，或磁盘块已分配。一个表的0/1。
查找快，适用各种文件分配方法，空间小可装入内存。
#### Chained free portion 空闲分区链
每个空闲分区包含一个指向下一个分区的指针，并记载分区大小
无额外空间开销
#### Indexing 索引
空闲分区也视为文件，建立索引。