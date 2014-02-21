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
<b>Operating System</b> : A program that loads and runs other programs, providing programs with a level of abstraction so they dont have to deal with the details of accessing hardware. 
    - Manages access to resources such as the CPU via the scheduler, memory via the memory management unit, persistent files via the file system, communication network via sockets and IP drivers, and devices via device drivers
    
<b>Kernel Mode</b> : The processor and operating system run in this mode. In this mode the processor can execture priveldged instructions that define interrupt vecotrs enable interrupts interact with IO ports set timers and manipulate memory mapings

<b>User Mode</b> : Other programs run the with processor in this mode. They do not have provledge to execute certain instructions. 

    - A processor running in user mode can switch to kernal mode by executing a trap instruction AKA a software interrupt.
    - Alternatively other OSes offer SYSCALL which is faster since it does not read the branch address from the interrupt vector table stored in mem. It keeps the address in a CPU register.
    - Either of these methods do the following :
        - Switch the process to kernal mode
        - Saves the current program counter on the stack
        - Transfer execution to a perdefined address for that trap
    - To return to user mode a <b>Return from exeption</b> instruction is launched
    
<b>System Calls</b> : Programs interact with the operating system with such calls. System call using a trap to switch control of operating system code from kernal mode. The request is either dealt with immediately or put in a waiting state and dealt with later. If the process is considered to be running for too long the process is stopped and another process will take place. 

<b>Programmable Interval Timer</b> In order to allow the operating system to get control at regular intervals. This generates periotic hardware interrupts. When the interrupt is generated control is transfered to an interrupt handler in the kernal and the process is switched to kernal mode. 

<b>Mode switch</b> The result of an interrupt or trap, where execution is transfered from user mode to kernal mode. 

- If the operating system decides that it is time to replace the currently executing process, it will save the process context and run an existing saved process's context. 

<b>Context</b> Includes the values of a processor's registers counter stack pointer and memory mappings. 

<b>Context Switch</b> Saving the context of one process and restoring that of another.

###OS System devices
<bCharacter Devices</b> Any device whose data that can be thought of as a byte stream. Keyboard input , mouse, camera

<b>Block Devices</b> Any device that has a persistent storage that is randomly addressable and read or witten in fixed size chunks or blocks. These devices include flash and disk memory. Because the data is persistent it can be cached by the OS so that future request for the cached content happens faster. This cache is called a <b>Buffer Cache<b>. Any device that can be used to hold a file system is a block device.

<b>Network Devices</b> Packet based communication networks

<b>Memory Mapped I/O</b> Devices that can be controlled by mapping their control and data registers into memory. 
- The processor can get device status notifications via hardware interrupts or by periodically polling the hardware. 

<b>Programmed I/O</b> Data can be transferred between the device and system via software that wirtes/reads the devices memory.

<b>Direct Memory Access</b> The device may have access to the systems memory bus and use this to transfer data to and from the system without using the processor. 

##Processes
<b>Process</b> A program in execution. It comprises the state of the processors registers and the memory map for the program.

<b>Memory Map</b> Contains serval regions that the OS allocated and initialized when the process was first created.
Such regions are : 

- Text : machine instructions
- Data : initialized static and global data
- BSS Uninitialized static data that was defined in the program
- Heap : dynamically allocated memory
- Stack : the call stack which holds not just return addresses but also local variables temp data and save registers

A Process may be in one of the following states :
- Running : the process is currently executing code on the CPU
- Ready : the process is not currently executing code but is ready to do so if the OS would give it a chance
- Blocked : the process is wating for some event to occur and will not ready  to executre instructions until the event is ready

<b>Process Scheduler</b> Responsible for moving processes between the ready and running states. 

<b>Preemptive Multitasking</b> An OS that can save the context of a running program and restore the context of a ready program to give it a chance to run. 

<b>Non-Preemptive or Cooperative</b> System that allows programs to run until they terminate or blocks I/O.
- An OS keeps track of all prcesses via a list. Each element is a pointer to a data structure containing information about the process. 

<b>Process Control Block</b> Data structure that contains information about one process.

<b>Process ID</b> Each process is uniquely identified with this.

<b>Basic Manipulations of the POSIX system include :

- Fork : 
- Execv :
- Exit :
- Wait :
- Signal :
- Kill :

