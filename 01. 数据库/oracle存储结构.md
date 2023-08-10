# oracle存储结构
数据块（Block）
 
数据块Block是Oracle存储数据信息的最小单位。注意，这里说的是Oracle环境下的最小单位。Oracle也就是通过数据块来屏蔽不同操作系统存储结构的差异。无论是Windows环境，还是Unix/Linux环境，他们的操作系统存储结构和方式、甚至字符排列的方式都是不同的。Oracle利用数据块将这些差异加以屏蔽，全部数据操作采用对Oracle块的操作，相当于是一个层次的抽象。
 
Oracle所有对数据的操作和空间分配，实际上都是针对数据块Block的操作。我们从数据表中搜索出一行，实际中Oracle就会从内存缓冲区（或者硬盘）中读取到该行所在的数据块，再返回这数据块上的指定数据行。Oracle无论是在缓冲区，还是在硬盘，进行数据操作的最小单位也就是数据块。
 
数据块是有大小的，在一个数据库建立的时候，通过参数进行设置。注意，在Oracle数据库参数中，只有数据块大小的参数是建库之后不能进行修改的。数据块的大小，在一个数据库中可以支持多个，但是一般没有太大的意义，会给管理和调试带来一定的负担。
 
数据块的大小是通过kb字节个数来指定的，默认为8KB。相关参数为db_block_size，下面是查看block大小的语句。
 
SQL> show parameter db_block_size;
 
NAME                                TYPE       VALUE
------------------------------------ ----------- ------------------------------
db_block_size                       integer    8192 //1024×8
 
设置数据块的大小是依据不同类型的系统的。如果数据块设置比较大，那么一次读取的数据行较多，相应对SGA内存消耗比较大，特定查询引发的换入换出可能较多。如果设置的过小，频繁的IO逻辑物理读也会引起性能问题。
 
与数据块有关系的另一个参数就是db_file_multiblock_read_count，表示一次从物理存储中读取的数据块数量。对一些数据挖掘系统，可以考虑调节此参数略大一些。
 
接下来，我们看比block更高的一个单位，区extent。
 
区extent
 
区extent是比数据块大一级的存储结构，表示的是一连串连续的数据块集合。我们知道，物理存储通常是随机的读写过程。即使在同一个文件里，我们也不能保证相同的一个信息是存储在绝对连续的物理存储空间的。Oracle数据存储同样如此。
 
在进行存储数据信息的时候，Oracle将分配数据块进行存储，但是不能保证所有分配的数据块都是连续的结构。所以，出现分区extent的概念，表示一系列连续的数据块集合。
 
视图dba_extents（或者all_extents、user_extents）是我们研究分区结构和存储构成的重要手段。
 
SQL> desc dba_extents;
Name           Type        Nullable Default Comments                                                 
--------------- ------------ -------- ------- ---------------------------------------------------------
OWNER          VARCHAR2(30) Y        Owner of the segment associated with the extent          
SEGMENT_NAME   VARCHAR2(81) Y        Name of the segment associated with the extent           
PARTITION_NAME VARCHAR2(30) Y        Partition/Subpartition Name, if any, of the segment      
SEGMENT_TYPE   VARCHAR2(18) Y        Type of the segment                                      
TABLESPACE_NAME VARCHAR2(30) Y        Name of the tablespace containing the extent             
EXTENT_ID      NUMBER      Y        Extent number in the segment                             
FILE_ID        NUMBER      Y        Name of the file containing the extent                   
BLOCK_ID       NUMBER      Y        Starting block number of the extent                      
BYTES          NUMBER      Y        Size of the extent in bytes                              
BLOCKS         NUMBER      Y        Size of the extent in ORACLE blocks                      
RELATIVE_FNO   NUMBER      Y        Relative number of the file containing the segment header
 
从视图中，我们可以清晰看出分区的几个特点。
 
首先分区是带有段特定性的。数据段segment是分区的上层组织单位，一个数据库对象对应一个segement，数据库对象是归属在不同的schema（owner）上的。所以，通过不同的数据段名称、不同的owner，乃至不同的tablespace表空间信息，就可以定位到数据区extent的信息描述。
 
另一部分信息是关于该区extent的分配信息，如所在文件编号，起始数据块block编号和数据块数量等内容。
 
 
数据段segment
 
数据段是与数据库对象相对应，一般一个数据库对象对应一个数据段。多个extent是对应一个数据段，每个数据段实际上就是数据库一个对象的代表。从dba_segments视图中，可以比较清楚看清数据段的结构。
SQL> desc dba_segments;
Name           Type        Nullable Default Comments                                                                                                                              
--------------- ------------ -------- -------
OWNER          VARCHAR2(30) Y               Username of the segment owner                                                                                                         
SEGMENT_NAME   VARCHAR2(81) Y               Name, if any, of the segment                                                                                                          
PARTITION_NAME VARCHAR2(30) Y        Partition/Subpartition Name, if any, of the segment                                                                                   
SEGMENT_TYPE   VARCHAR2(18) Y    Type of segment: "TABLE", "CLUSTER", "INDEX", "ROLLBACK",
"DEFERRED ROLLBACK", "TEMPORARY","SPACE HEADER", "TYPE2 UNDO" or "CACHE"
TABLESPACE_NAME VARCHAR2(30) Y               Name of the tablespace containing the segment                                                                                         
HEADER_FILE    NUMBER      Y               ID of the file containing the segment header                                                                                          
HEADER_BLOCK   NUMBER      Y               ID of the block containing the segment header                                                                                         
BYTES          NUMBER      Y               Size, in bytes, of the segment                                                                                                        
BLOCKS         NUMBER      Y               Size, in Oracle blocks, of the segment                                                                                                
EXTENTS        NUMBER      Y               Number of extents allocated to the segment                                                                                            
INITIAL_EXTENT NUMBER      Y        Size, in bytes, of the initial extent of the segment                                                                                  
NEXT_EXTENT    NUMBER      Y  Size, in bytes, of the next extent to be allocated to the segment                                                                     
MIN_EXTENTS    NUMBER      Y               Minimum number of extents allowed in the segment                                                                                      
MAX_EXTENTS    NUMBER      Y               Maximum number of extents allowed in the segment                                                                                      
PCT_INCREASE   NUMBER      Y  Percent by which to increase the size of the next extent to be allocated                                                              
FREELISTS      NUMBER      Y  Number of process freelists allocated in this segment                                                                                 
FREELIST_GROUPS NUMBER      Y  Number of freelist groups allocated in this segment                                                                                   
RELATIVE_FNO   NUMBER      Y     Relative number of the file containing the segment header                                                                             
BUFFER_POOL    VARCHAR2(7) Y     The default buffer pool to be used for segments blocks           
 
从segment_type列的comment信息中，可以看出数据段的类型是多样的。任何种类的数据库对象，本质上都是一种数据段。数据表、索引、回滚、聚集这些都是数据段的一种表现形式。同时，数据段是在数据对象创建的时候就已经创建出来，随着对象体积的增大，而不断分配多个extents进行管理。
 
另一部分信息可以从dba_segments中读出的，就是该数据对象分配的空间大小和数据块、分区个数。使用这个视图，可以方便的获取到指定schema的所有对象大小。
 
SQL> select owner,sum(bytes)/1024/1024 as vol, sum(blocks) as totalblocks,sum(extents) as totalextents from dba_segments group by owner having wner='SYS';
 
OWNER            VOL TOTALBLOCKS TOTALEXTENTS
------------------------------ ---------- ----------- ------------
SYS                585.5      74944        3248
 
上面查询，说明SYS的schema，所占用空间585.5MB，包括74944个数据块和3248个分区。
 
一个对象创建出来之后，在segment层次上是分配一个分区extent和八个数据块block。
 
有一个问题需要注意，通常我们的数据段是与数据对象相关。一个数据对象对应一个segment。但是，分区表的时候，一个分区要对应一个segment对象。还有就是，segment对象是可以指定存储在那个表空间里，实现存储划分的基础也就在于此。不同类型的segment划分建立在不同的表空间里，才有可能存放在不同的文件中，最后分布在不同的物理存储。
 
分区实际上就是存在分开存储的可能。一般一个对象是不会跨物理存储进行存放的，分区表是对应的多个segment。所以，分区表分开存储空间是可能的。
 
 
表空间tablespace
 
TableSpace是存储结构中的最高层结构。建立一个表空间的时候，是需要指定存储的文件。一个表空间可以指定多个数据文件，多个文件可以在不同的物理存储上。也就是说，表空间是可以跨物理存储的。但是有一点就是，表空间下一级对象数据段的存储，是不能指定存储在那个文件里的。所以，要想让数据对象访问IO负载均衡，需要指定不同的数据对象在不同的表空间里。这也就是为什么将数据表和索引建立在不同的表空间的原因。
 
表空间通过v$tablespace进行访问。
 
SQL> desc v$tablespace;
Name                       Type        Nullable Default Comments
--------------------------- ------------ -------- ------- --------
TS#                        NUMBER      Y                        
NAME                       VARCHAR2(30) Y                        
INCLUDED_IN_DATABASE_BACKUP VARCHAR2(3) Y                        
BIGFILE                    VARCHAR2(3) Y                        
FLASHBACK_ON               VARCHAR2(3) Y                        
ENCRYPT_IN_BACKUP          VARCHAR2(3) Y                        
 
相对于前面的结构视图，表空间视图的结构要简单的多，只是一些描述信息。其中两个参数需要注意一下。
 
一个是bigfile，是一个标志位，标志表空间是不是所谓的大文件表空间。大文件表空间是在10g中推出的一个新特性，处于性能考虑，可以设置表空间为大文件表空间，存储超过百T的数据，但是要求数据文件只能有一个。另一个是flashback_on，表示表空间的闪回特性是否开启。
 
 
要注意，数据表段区块的概念集合，很容易与schema的结构相混淆。schema是一个组织概念，是来自于经典数据库理论范畴。在oracle中，Schema就是一个组织概念，一个user对应的就是一个schema。schema是逻辑对象的集合组织，同表空间等概念不是一个层面的。
 
在一个schema里，是可以将对象建立在任何数据表空间内的，只有一个默认表空间的概念default tablespace。指定默认表空间是在创建用户的时候指定的。

表空间是数据库的逻辑组成部分
从物理上将：数据库数据存放在数据文件中
从逻辑上将：数据库则是存放在表空间中
表空间由一个或是多个数据文件组成
数据文件和日志文件是数据库中最重要的文件。它们是数据存储的地方。
每个数据库至少有一个与之相关的数据文件，通常情况下不只一个，有很多。
表空间（tablespace）、段（segment）、区（extent）、块（block），这些都是oracle数据库在数据文件中组织数据的基本单元。

块是数据存储的物理单位，也是数据文件中最基础的单位，数据直接存储在块上。
是oracle空间分配的最小单位。oracle中的块大小常见的有三种，2KB、4KB、8KB。
块的大小在数据库创建时就已经固定下来，数据库中每个块的大小都是相同的，
而且所有的块都有相同的格式，由“块头＋表目录＋行目录＋空闲空间＋数据空间”组成。
块头包含着块类型（比如是表块、还是索引块）的信息、磁盘上块的位置等信息。
表目录（table directory），如果有的话，包含着此块中存储各行的表的信息（如果一个块中存有多个表中的数据）。
行目录（row directory）包含着数据行的描述信息，它是一个指针数组，指示了每一行在数据块中的物理位置。
块头、表目录、行目录统称为块开销（block overhead），是oracle原来统计、管理块本身的。
剩下的两部分很简单，已经存有数据的就是数据空间，暂时没存的就是空闲空间。

区又叫盘区，是数据文件中一个连续的分配空间，它比块要大，由块组成。有些对象分配空间时可能至少需要两个盘区，比如回滚段，
而这两个盘区不一定要求相连。区的大小从一个块到2GB不等

段是oracle数据库中的分配单位，对象如表、索引等都是以段为单位进行分配。当创建一个表时将创建一个表段，创建一个索引时就创建一个索引段。
每一个消耗存储空间的对象最终被存储到一个单一的段中。有回滚段、临时段、聚簇段、索引段等。

表空间是一个逻辑容器，它和数据文件关联起来，一个表空间至少有一个数据文件与之关联。一个表空间可以有多个段，一个段只能属于一个表空间。

方案（schema）又叫模式，是比表空间小一级的逻辑概念，它也是一个逻辑容器。多个用户可能共用一个表空间，
那如何区分开每一个用户？那么在表空间中对每个用户都有一个对应的方案，用于保存单个用户的信息。

oracle中存储的层次结构总结如下：
一、数据库由一个或多个表空间组成
二、表空间由一个或多个数据文件组成，一个表空间包含多个段
三、段由一个或多个区组成
四、区是数据文件中一个连续的分配空间，由一个或多个块组成
五、块是数据库中最小、最基本的单位，是数据库使用的最小的I/O单元
六、每个用户都有一个对应的方案
SGA：System Global Area是Oracle Instance的基本组成部分，在实例启动时分配;系统全局域SGA主要由三部分构成：共享池、数据缓冲区、日志缓冲区。
PGA：Process Global Area是为每个连接到Oracle database的用户进程保留的内存。是每个连接私有的。并不属于数据库实例