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

- Fork : Creates a new process that is the copy of the parent and runs the same program however does not share the same memory 
- Execv : Loads a new process from the file system to override the current process. Typically run after fork
- Exit : Tells the OS to exit the current process
- Wait : Allows a parent to wait for and detect the termination of a child process
- Signal : Allows a process to detect signals which include software exceptions user defined signals and death of child signals
- Kill : Sends a signal to a specified process. Can be detected with the "signal" command

##Threads

<b>Thread</b> : Can be thought of as the part of a process that is related to the execution flow


###Thread Advantages
- Creating a threads and switching between them within a process is more efficent than switching between processes
- Makes programming easier since the memory is shared
- Threads on multi processor systems can be scheduled and run at the same time parallel

<b>Multithreaded</b> : A process with more than one thread. However now the PCB now keeps track of the threads for that process. 

<b>Thread Control Block</b> : Thread specific information such as registers, program counters, stack pointers, priority.
(Even though each thread gets its own stack the stacks all reside in memory accessable to all threads)

<b>Kernal Level Threads</b> : Threads implemented, and managed by the OS.

<b>Thread Library</b> : Save and restore registers and manager switching stacks amoung threads. May ask the OS forperiotic software interrupts to enable it to preempt threads. 

<b>User Level Threads</b> : All threads map to a single kernal or single threaded process. <b>Disadvantage</b> with this is that if one user level thread blocks then no other user level threads can run. Many OS's offer non-blocking versions of a block. Another disadvantage is that the user threads cannot take advantage of multiprocessors. <b>Advantage</b> is that user level threads is that they can be more efficent than kernal level threads since they do not have to switch to kernal mode.

<b>Hybrid Threading</b> : Maps N user level threads into M kernal level threads

- When a new thread is created it starts execution at a given function in a program. When it returns from the function the thread is destroyed. A thread can also exist via a system call. 

<b>Join</b> : One thread within a process can wait for another thread to terminate. 

<b>Mutal Exclusion Lock (Mutex)</b> Allows a thread to grab a lock so that all other threads that want to grab the same lock will have to wait. 

##Synchronization
<b>Concurrent</b> When threads or processes exist at the same time.

<b>Asynchronous</b> : if threads need to synchronize with each other occationally

<b>Synchronous</b> : If threads synchronize frequently so that their relative order of execution is guaranteed.

<b>Race Condition</b> : Bug where the outcome of concurrent threads is dependeent on the percise sequence of execution. Happens when two or more threads trys to make changes to the same peice of memory at the same time.

<b>Critical Sections</b> : Regions of a program that try to access shared resources and cause race conditions

<b>Mutual Exclusion</b> : Enforcing that only one thread at a time can execute within a critical section.

<b>DeadLock</b> : Condition where there is a curcular dependency for resources amoung threads and each oneis waiting on the other to release a lock.

<b>Starvation</b> : The scheduler never gives a thread a chance to run. If it never getsa chance to run will never have a chance to release its resources.

<b>Disabling Interrupts</b> : When entering a critical section that other threads will never get a chance to preempt you and run at all. Also requires you to be in kernal mode. 

<b>Test and Set Locks</b> : This can introduce a race condition, a thread can be preempted between testing the value of a lock and setting it. This can allow multiple threads to get the same lock. if(!locked){lock = 1}

<b>Atomic Instructions</b> : perform operations such as test and set as one indivisible operation meaning that another thread cannot preempt the sequence of operations and they all execute sequentially and indivisibly. Some of these instructions include :

- <b>Test and set</b> : Set a memory location to 1 and return the pervious value of the location. If the returned value is 0 you know there was not lock before you exe the function and you grabbed the lock when executing. If the returned value is 1 then it means someone already has control of the lock.
- <b>Compare and Swap</b> Compare the value of the mem location with an old value that is passed in through the instruction. If the two values match then write the new value into that mem location. With this instruction we set a mem location to a specific value provided that someone did not change the contents before we read. If they did then the change we made will not happen.
- <b>Fetch and Increment</b> Increment a memory location and return the previous value of that location. Similar to deli line tickets. You grab a number wait for your turn, when its your turn write and then increment the counter.

<b>Spin Lock</b> : Looping in software to wait until the lock is released. Undesirable because it keeps the threads in a ready to run state even though they have nothing to do but loop and wait for a value to change. 

<b>Priority Inversion</b> : The low priority thread needs to be given a higher priority so it can exit the critical section as quickly as possible and let the higher priority threads get in the crit section.

<b>Semaphores</b> : Counts the number of wakeups that are saved for future use. Each time you call down on a semaphore you decrement its value. If the value is 0 and you call down the thread will sleep. When a thread calls up on a semaphore the os will wake up one of the threads that is sleeping on that semaphore. The most basic use is to initialize the semaphore to 1 and when a thread wants to enter a crit section it calls down and enters the section. When the thread is done with the critical section the OS will set the value to 1 so the next thread can decrement and get access to the cit section. 

<b>Event Counters</b> : works similar to the way you would use a fetch and increment instruction but without the spin lock. Three operations can occur , read the current value of the counter, increment it, or wait for it to be a certain value.

<b>Condition variables</b> Supports two operations , wait until a contion var is signaled and signal a condition var. Signaling a con var will wake up a thread that is waiting on a condition variable

<b>Message Passing</b> : Supports two operations send and recieve. The most common behavior of message passing is where a send operation does not block and a receive operation does block. The first thread that does a receive  will get the message and then enter the citical section. The second thread will block on the recieve.

<b>Rendezous</b> : Message passing system where the send will block until data is recieved via the recieve command. This is efficent when messaging is donw because it does not require the message to be copied to an immediate buffer.

<b>Direct Addressing</b> : Conventional message passing requires that the sender knows the identify of the thread that will be receiving message. 

<b>Indirect Addressing</b> : Alternative form of messaging via mail boxes

<b>Mailboxes</b> : Allow any number of threads to send to an intermediate queue. Any number of threads can read from this queue

##Scheduling
<b>CPU Burst</b> : When a process executes for a while then blocks I/O then executes some more.

<b>Scheduler</b> : responsible for deciding what process should get to run next, and if the current process needs to be switched out for another process. If a scheduler can preempt a process and context switch another process then it is a preemptive scheduler, otherwise it is a cooperative scheduler. 

<b>Time Slice</b> : mac time that a process will be allowed to run before it gets preempted and another process gets a chance to run. Short time slices are good for maxing interactive performance. Long time slices reduce the overhead of context switching.

<b>Dispatcher</b> : the component of the scheduler that is responsible for restarting the chosen process. It does so by restoring the process' context onto the CPU and exectuing a return from interrupt instruction that will cause the process to execute from the location that was saved on the stopped running. 

###First Come first served scheduling
Non-preemptive scheduler that uses simple FIFO queue. New processes are added to this queue. First process is taken out of the queue and run. A disadvantage to this is that it is non-preemptive and processes with long CPU bursts will delay other processes. 

###Round Robin Scheduling
Preemptive version of first come first serve. When a quantum expires for running process is preempted and brought to the rear of the queue. The ready process at the head of the queue is then run. It gives every process in the queue an equal share of the CPU but since it does not schedule highly interactive processes more frequently it is not the best.

###Shortest remaining time first scheuduling
Picks the process with the shortest estimated CPU burst time to run next. The idea is to maximize average response time 
