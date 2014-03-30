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

##Page Based Virtual Memory
