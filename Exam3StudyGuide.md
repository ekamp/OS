#Exam 3 Study Guide

##Flash Memory
###NAND Flash Memory
- Block addressable Device
- Data is read or written a page at a time and each page is uniquely addressed
- Very Similar to a magnetic disk drive
- No Disk latency
- However flash memory only has a limited number of erase write cycles
- Because of this we can use <b>wear leveling</b> : which distributes writes accross all pages of the storage.
- <b>Dynmaic Wear Leveling</b> : monitors high and low use areas of memory and at some point swaps them.
- <b>Static Wear Leveling</b> : Unused data is moved from certain blocks to others in order to allow blocks to wear out evenly
- <b>Flash Transition Layer</b> : Layer of softtware between the flash device driver and the block device, that takes care of wear leveling.
- Alternatively wear leveling is taken care of by the NAND flash controller
- The Flash Transition Table maintains a <b>block lookup table</b> that maps logical to physical blocks so the block can be identified and located even after it has moved.

###Log Structured File System
- Used to avoid using the same blocks of memory over and over again
- File entries are written as an entry to a transition log
- One transition may make another obsolete
- When played from start to finish the log allows to construct an up to date file system, the entire system is stored as a sequence of log events
- <b>YAFFS</b> : All data is written as a log as chunks, each chunk is either data or an object header or in other words is metadata or a directory.
- <b>Garbage Collection</b> : Is used to reclaim free blocks
- Free Blocks are also created by moving chunks from one block to anther
- <b>Checkpointing</b> : Process of writing out the filesystem hierachy onto the file system. This will speed up mounts since it can just read the last checkpoint and not have to recreate the entire file system

###Special Devices
- <b>Null Device</b> : Discards any writes and returns EOF on reads
- <b>Zero Device</b> : Always returns bytes containing 0
- <b>Random Device</b> : Returns random numbers
- <b>Loop Device</b> : Device that is given a file name and creates a block device interface to that file. These file systems are formatted on top of block devices

###Special File Systems
- 
