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
- <b>Process File System</b> : Creates a file system view of processes and other aspects of the kernal and each process is presented as a directory. Aspects of the process are presented as files within the filesystem
- <b>Device File System</b> : Presents all the kernals's devices as files so that one does not need to create and remove them from the /dev folder in the physical file system
- <b>FUSE</b> : Alows one to make user level file systems. VFS layer sends a request to FUSE which sends to a user level process that interprets the responce.

##Client Server Networking
- <b>Baseband</b> : One node transmits at a time, however that node gets full bandwith of the netword at that time
- <b>Broadband</b> : Has its bandwidth split over a frequency or multiple channels. Cable is an example of this. However cable internet uses a form of baseband where there is an up stream channel and downstream channel, aming it baseband within broadband

###Baseband Sharing
- <b>Circuit Switching</b> : The network is split into short time slots in which each node can use the network, termed TDM or time division multiplexing.
- This provides guarenteed bandwidth and letency however does not use the network efficently
- Public telephone network was an example of this
- <b>Packet Switching</b> : Uses varaible length time slots, Data is split in variable sized packets. Cannot guarentee constant bandwidth or latency, ethernet is an example of this.

###OSI Layers
- <b>Data Link Layer</b> : Transfers data within a physical local area network.
- <b>Network Layer</b> : Manages routing of packets / data
- <b>Transport Layer </b> : Manager communication of data from one application to another
- <b>Presentation Layer</b> : Manages the representation of data and handles any conversion of data types

###IP Networking
- <b>Internet Protocol</b>
