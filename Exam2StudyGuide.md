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
