#OS Exam 2 Study Guide

##Memory Management
<b>Single Partition Multiprogramming</b> : Allows only a single program to be loaded in memory at any given time. Shares its memory with the OS and nothing else.

<b>CPU Utilization</b> : Estimated percentage of time that the CPU is not idle.

<b>The Degree of Multiprogramming</b> : The number of processes and candidates for running in memory at one time.

<b>Absolute Code</b> Uses real memory addresses and expects to be loaded into and run from a specific memory location.

<b>Position Independent Code</b> : Uses relative addresses throughout and may be loaded into any mem location.

<b>Relocation Table</b> : <i>Does not rely on the compiler to gen pos independent code but instead leaves mem references
blank and generates this table.</i> Allows references to be filled when the program is loaded.

<b>Logical and Virtual Addressing</b> : Relys on hardware to dynamically transform address references made by the process into actual memory locations.

<b>Memory Management Unit</b> : Performs the translation for log and virtual addressing.

<b>Base Address</b> : An address added to each memory reference made by a process. Based on where in memory the process is loaded.

<b>Limit</b> : MMU checks against this to ensure that the process is access only memory within itself.

<b>Far Pointer</b> : The reference for combined segment value and offset by Intel. This type of system is called <b>Segmentation</b>.

<b>Base Limit Addressing</b> : Approach requires each process to occupy contiguous memory

<b>Multiple Fixed Partitions</b> : OS devides memory into a bunch of partitions ahead of time.

<b>Internal Fragmentation</b> : After the memory is partitioned processes are assigned to each part, the unused space
within each fragment or partition is known as internal fragmentation.

<b>External Fragmentation</b> : When partitions have nothing to run then the partition memory is wasted.

<b>Variable Partitions</b> : OS creates memory for a process on demand using a block of contiguous memory. Holes will form in memory where new processes might not be able to fill the hole.

<b>Memory Compaction</b> : relocate processes in memory, however this is a time consuming approach. If a process grows it will need more memory and the OS will need to find a bigger unallocated block for that process, relocated other processes or save the processes memory into disk and wait for memory to be freed.

###Page Based Virtual Memory
<b>Page-Based</b> Physical memory is divided into equal-sized chunks that are of size pwr of two.

<b>Page Frames</b> Equal sized chunks of physical memory

<i>A Logical memory address is divided into two components , the high order bits identify a <b>Page Number</b> and the low order bits identify a <b>Page Offset</b>

<b>Page Table</b> : for each reference made by the processor the MMU extracts the page number from the logical address using it as the index to the page table. Each entry into the page table known as the Page Table Entry contains a page frame number.

<b>Direct Mapping Paging System</b> Contains a page table per process.

<b>Page Table Based Register</b> : Contains the address of the process's page table. Loaded when a context switch occurs.

<b>Page Residence Bit</b> : Indicates whether that specific page is mapped onto a page frame.

<b>Page Fault</b> : Occurs when a page is not mapped onto a page frame.

<b>Page Fault Handler</b> :  Handles page fault interrupts. Examines the memory address of the instruction that faulted and decides whether the process needs to be terminated because of an invalid memory address or whether the access  was indeed valid but not mapped to a page frame.

<b>Associative Cache AKA Translation Lookaside Buffer </b> : In order to improve performance an MMU uses these to store recently accessed page numbers and their corresponding Page Table Entries.

<b>Address Space Identifier</b> used in the Translation lookaside buffer  to identify the process to whom an entry belongs.

<b>Multilevel Page Table </b> : Used to save memory , where the virtual address is split into three or more parts and the lowest bits still give us the offest with page and page frame. However the highest bits serve as an offset into the top level <b>Index Table</b>. Each entry in the index table contains the base address of a partial page table. Entries in the table are PTEs that include page frame address and other access flags.

<b>Inverted Page Table</b> : Used to deal with a very large address space. Contains a single table with one entry per page frame. To find a virtual address we search the table for the page frame that contains the virtual address.

<b>Advantages of Page Tables</b>
- Does not let the amount of physical memory limit the size of processes
- Allows the process to work like it owns the entire address space of the processor
- Allows memory allocation to be non-contiguous and has no external framentation problems, getting rid of the need for compaction
- Keeps more processes in memory thatn the sum of their memory needs so the we can use as much of the CPU as possible
- Lets us keep just the parts of a process that we're using in memory, rest stored on disk

<b>Disadvantages of Page Tables<b>
- Searching through a page table for a virtual address is not good for memory performance
- Must rely on an associative cache and the use of hashing to perform a lookup

<b>Combined Segmentation and Paging System</b> : Logical address is offset by a segment base register. The result address is a linear virtual address and is found in a 2 level pge table used to generate a physical address

###Demand Paging
Refers to the technique of allocating and loading memory pages on demand, when a process makes a demand for memory

<b>Page Fault</b> : When a process makes a memory reference to a page that is not mapped onto a page frame.

<b>Page File</b> : ?

<b>Page Replacement </b> : The process of finding a page to save out onto disk to create a free page frame. Idealy we would like to reduce the number of times we have to do this because writing to disk is very slow

####Page Replacement Policies
<b>Least Recently Used LRU</b> : Will write pages to disk that are not used often or are the oldest, only problem is that we cannot keep counts on the MMU.

<b>Clock Algorithm</b> : Looks at a page frames as a circular list. When a page needs to be evicted the algo starts at the current position and looks for page frames whose ref bits are 0 (meaning that they have not been referenced for a while).

<b>nth Chance Replacement</b> : Similar to clock except it keeps a counter per page entry. When using a page we check its

<b>Locality</b> : A process tends to reference the same pages over and over before ,moving onto other pages.

<b>Working Set</b> The set of pages referenced over and over. This working set should be in memory for quick access.

<b>Thrashing</b> : Constantly paging to and from the disk, which occurs when the working set is not in memory.

<b>Working Set Model </b> : Approximates the locality chatacteristics of a process to try to identify the processes that are in the working set, in order to put them in memory and avoid thrashing.

<b>Page Fault Frequency</b> : Manages resident sets, if a process does not have its working set in memory it will generate page faults, however if a process never generates page faults that means it has too much memory allocated to it. Therefore a threshhold of page faults needs to be set to determine the amount of memory or page frames that should be allocated to each process.

##Devices
<b>Block Device</b> : provides structured access to the underlying hardware. Block devices can host a file system. Since it is persistent it can cache memory.

<b>Buffer Cache</b> : a pool of kernal memory that is allocated to hold frequently used blocks from block devices. Through the use of a buffer cache we can min the number of I/O requests that actually require a device I/O operation.

<b>Character Device</b> : Provides unstructured access to the underlying hardware such as :
- printers
- scanners
- mice
- video frame buffer
<i>Basically something that needs a driver to run on your computer</i>

<b>Block Device Table</b> : Kernal maintains this to keeps track of block devices

<b>Character Device Table</b> : Kernal maintains this to keep track of character devices

<b>Major Number</b> : an index into either the block or character device table and allows the kernal to fin functions that are associated with that device; these functions are called the <b>Device Driver</b>

<b>Minor Number</b> : Interpreted within the device driver, identifies which specific instance of a device is being accessed. Such as SATA disks will have one driver but multiple disks. Major number will id the driver and the minor number will identify the disk.

###Execution Contexts
Kernal Code runs one of the following contexts
- <b>Interrupt Context</b> : this is invoked by the spontaneous change in the flow of execution when a hardware interrupt takes place anywhere.
- <b>User Context</b> : THe flow of control when a user process invokes a system call. The mode switch to kernal took place but the context is still that of a user process. I/O may be scheduled but kernal has to put process in blocked state and context switch to another process.
- <b>Kernal Context</b> : Kernal has one or more worker threads that it schedules just like any other process. No relation to user threads they have a context and may block

<i>By putting devices in the same hierachical name space as files the OS provides a degree of transparency where apps do not know if they are interacting with a file or with a device, this is seen when a user uses a terminal window where the terminal can take input from the keyboard and from a built in file in the same manner.</i>

###Device Drivers
- Device drivers are modular and can be compiled into the kernal and are either loaded at initialization or dynamically loaced at any time in the future.
- Next the drivers register themselves with with the kernels interrupt handler.
- Then when an interrupt occurs the handler calls the appropriate handling function in the driver.
- To ensure it does not take two long it is split into to processes
<b>Top Half</b> : the interrupt service routine that is registered with the interrupt handler. Does as little as possible grabbing data placing it into a buffer and scheduling bottom half activity.
<b>Bottom Half</b> : Scheduled by the top half and does much of the work. Because it runs in the kernal thread it may block.

###Disk Scheduling
Spinning magnetic disks are still the dominant device for storing large amounts of information.
<b>Disk Scheduling Algorithm</b> : Used to optimize the flow of data between memory and disk.

<b>First Come First Serve FFS</b> Processes requests in the order in which they are generated.

<b>Elevator Algorithm</b> : Moves the head of the disk back and forth, uses the system to keep track of the current head position and direction. Requests are sorted in the order they arrived and the head moves from track 0 to track n.

<b>Look Algorithm</b> : Reverses the disk's head direction if there are no blocks schedulred beyond the current point.

<b>Circular Scan</b> : Schedules disk requests only when the head is moving in one direction to avoid preferential treatment of blocks in the middle.

<b>Circular Look </b> : Is similar to look except schedules requests only in one direction.

<b>Shortest Seek Time First</b> : the queue of current requests is to be sorted by cylinder numbers closest to the current head position and requests made in that order.

##File Systems
<b>Mount</b> : Multiple File systems can combine into one abstract view via this.

<b>Mount Points</b> : In order to mount the file system the OS will place the root at the mount. The OS maintains a list of mount points in order to seamlessly transfer from one to the other.

<b>Virtual File System</b> : To support transparency this layer of abstraction is used. This is also an object oriented approach to looking at a file system.

<b>File System Modules</b> : Interfaces with the VFS , and they implement a specific underlying file system.

####Virtual File System Layers
- System Call Interface : API's used for user programs
- Virtual File System : Manages the namespace keeps track of open files, reference counts, mount points
- File System Module (Drivers) : Understands how the filesystem is implemented on disk, can fetch and store metadata and data for a file.
- Buffer Cache : No understanding of the file system takes read and write requests from blocks or parts of blocks uses frequently.
- Device Drivers : The components that know how to read and write from disk.

####VFS Crucial Components
<b>Inode</b> : Uniquely identifies a file. It also stores key information about a file and a location of its data. 

<b>Dentry</b> aka directory entry, an abstract representation of a directory's contents. Contains the file name the inode so we can access its contents and a pointer to its parent directory.

<b>File</b> : The VFS keeps track of open files and contains interfaces to open close read and write to files as well as a map to lock them . The file object represents an open file and keeps track of the access mode and reference counts.

<b>SuperBlock</b> : Contains interfaces to get information about a file system such as read write delete inodes. and the ability to lock the file system.

###Managing File Systems
The lowest level of dealing with file systems is allocating and managing memory using certain blocks over others. For flash memory there are no improvements by using one block vs the other. However when using disks there are seek times in order to fetch certain memory

<b>Contiguous Allocation</b> : That is if a file's data requires disk block the blocks will be contiguous.

<b>External Fragmentation</b> : Regions of free blocks are wasted as new disk blocks are allocated and deleted

<b>Extents</b> : A contiguous set of blocks, used as a compromise to contiguous allocation. A file will use one or more extents and they are typically refered to as tuples.

<b>Cluster</b> : Logical block used throughout the file system that is the grouping of multiple blocks.This will result in better performance but more internal fragmentation.

<b>Linked Allocation</b> : Uses a few bytes from each block to store the block number of the next block in file.

<b>File Allocation Table</b> : A table of block numbers are created. Here the number of the next block is stored at table[current_block_number]. Contains the file name and the first block number allocated to that block.
- In order for FAT to be efficient the table must be completely stored in memory for fast access.

<b>Indexed Allocation</b> : Alternatively reading in just the blocks used by the file we are interested in. This is not optimal because there are varying sizes of files therefore the reading in will take longer or shorter each time.

<b>Combined Indexing</b> : Uses fixed length structures. The information for the structure is storeed in the inode.

<b>Direct Block Pointer</b> : refer directly to the block

<b>Indirect Block Pointer</b> : If a file needs to use more blocks then it utilizes this to allocate or gain more memory, the inode contains this.

<b>Double Indirect Block</b> : If a file needs even more blocks then it utilizes this where the inode points to a block that contains a list of indirect block pointers.

###File System Implementation Systems

<b>Unix File System</b>
An example of the inode based file system. When laid out in disk format it contains three sections, super blocks inode blocks and data blocks.

- Performance of UFS is very poor due to the following factors
  - Linked list approach to the file system results in much fragmentation over time in other words when a files allocation of memory is not contiguous.
  - Additionally there is a large seek time on the disk due to the fact that the inode is at the beginning and most of the information in the middle of the block

<b>Berley Fast File System</b> : Berkely redesigned the file system to improves performance in certain areas.
- Larger clusters : FFS Chose a larger cluster size, bringing eight fold improvement in contiguous allocation.
- Cylinder Groups : Instead of having one region for inodes and another for data multiple regions were created. The information specified for the most part will be in the same cylinder as the inode.
- Bitmap Allocation : Instead of using a linked list of free blocks a bitmaps is used to keep track of which blocks are in use within each cylinder. Makes it easy to find contiguous or nearby clusters rather than just using any avalible free cluster.
- Prefetch : If two or more sequential blocks are read from a file FFS thinks file access is sequential and will prefetch additional blocks from that file.

####Linux Ext 2
Adaptation of Berkleys FFS, Uses simply cluster allocation to allocate memory. 

<b>Block Group</b> Instead of cylinder groups this uses block groups, since modern disks make it imposible to determine cylinder groups. This is just an abstraction of a long list of blocks. Caches all its memory internally in the buffer cache. FFS instead would flush metadata in order to prevent corruption.

####Linux Ext 3
<b>Consistent<b/> : all free blocks and inode bitmaps are accurate all allocated inodes are referenced by files , and each block is marked as either free or filled.

<b>Inconsistent State</b> : If something spontaneous happens such as a loss of power not all of the blocks will be writen to disk resulting in this state.

<b>Consistent update problem</b> : performing all updates automatically all should be applied to the file system or none should be applied.

<b>Journaling</b> : method of applying this atomicity. All changes are first writen to a log called a journal. When all changes are complete the journal is marked as the transaction end only when the transaction is complete on disk the same sequence is now applied to the file system. When complete the transaction entry is deleted from the log. Should anything happen to the system upon restart all changes currently in the log take place. 
- The downside to this is that journaling does not have good performace
- <b> Full data Journaling</b> : All changes appended to log and then applied to the file system
- <b>Ordered Journaling</b> : Write the data blocks first and the journals the metadata, terminates the process and then applies the metadata from the journal. Always in consistent state but can lead to partially moded data due to an interrupt when writing data before end of transaction occurs.
- <b>Writeback Journaling</b> : Does metadata journaling without writing the data blocks first. It is the fastest form of journaling however can result in corruption of the files. Leads to no more corruption than the typical file sysytem however is faster.


####Linux Ext 4
Improvement to ext 3 and can be run on larger files. The two enhancements are : 
- <i>Extent Based allocation</i> : Instead of listing block numbers in inodes it uses extents. The extents contain the starting cluster number of a group of clusters along with a cluster count. Can allow for more data to be addressed from a list of blocks
- <i>Delayed Allocation of Free Space</i> : Instead of allocating data clusters as the file grows the blocks are kept in teh buffer cache and allocated to physical blocks only when they are ready to be flushed, increasing the likely hood for contiuguous blocks of memory.

###Microsoft NTFS
