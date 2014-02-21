#Exam 1 Study Guide

##Introduction Terms

<b>Batch Systems</b> : Operating Systems that run a program to completion and then load and run the next program

<b>Multiprogramming</b> : Keep several programs in memory at once and switch between them

<b>Time-Sharing</b> : Multiprogramming with preemption where the system may stop one process from running and start another.

<b>Portable Operating System</b> : An operating system that is not tied to a specific hardware

<b>Microkernal OS</b> : an operating system that provides just the bare essential mechanisms that are necessary to
interact with the hardware and manage threads and memory.

<b>Mechanism</b> : Is the presentation of software abstraction defining how to do something. 

<b>Policy</b> : Defines how a mechanism behaves, a good system will keep a policy and mech seperate. Policy defines things such as permissions time limits and goals.

##Operating System Booting
<b>Boot Loader</b> : a small program that is run at boot time that loads the operating system.
<b>Multi-Stage Boot Loader</b> Starts off by loading the simple boot loader, then loads a more complex boot loader which then loads the operating system. 
<b>Chain Loading</b> Is the cascading process by which the multi stage boot loader follows.
- In a classic PC the computer starts running code in the systems BIOS on startup. 
- Then the BIOS performs a power on test identifies some hardware and then loads the first block of the disk
<b>Master Boot Record</b> The first block of the disk loaded by the BIOS
    - Contains a table identifiing disk partitions and code that will load the Volume Boot Record, which is the first disk block of the designated boot partition.
<b>Volume Boot Record</b> : First disk block of the designated disk partition. Contains code to read successive consecutive blocks to load additional blocks that comprise the second stage boot loader. 
<b>Universal Extensible Firmware Interface</b> successor to the BIOS and contains disk blocks. The files that are loaded from the EFI are often bootloaders for individual operating systems.

##Operating Systems: essential concepts
<b>A program that loads and runs other programs, providing programs with a level of abstraction so they dont have to deal with the details of accessing hardware. 
    - Manages access to resources such as the CPU via the scheduler, memory via the memory management unit, persistent files via the file system, communication network via sockets and IP drivers, and devices via device drivers
    
<b>Kernel Mode</b> : The processor and operating system run in this mode. In this mode the processor can execture priveldged instructions that define interrupt vecotrs enable interrupts interact with IO ports set timers and manipulate memory mapings
