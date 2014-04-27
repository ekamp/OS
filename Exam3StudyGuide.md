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
- <b>Internet Protocol</b> : Handles the interconnection of wide and local area networks
- IP is layer 3 and ethernet or physical layer is layer 2
- In order to send a packet out the system must find the corresponding ip address that relates to the machines MAC address
- <b>Address Resolution Protocol</b> : Finds the MAC address for the corresponding IP address via a broadcast on the network
- <b>Transmission Control Protocol</b> : is a connection oriented service that makes sure that packets are transmitted to the source in the order that they were transmitted and resends lost packets. Keeps track of the destination so that it has an illusion of a connected data stream
- <b>User Datagram Protocol</b> : is a connectionless service that drops packets with corrupt data and does not ensure in order service of packets or delivery of packets.
- <b>Port Numbers</b> : Allow the OS to direct the data to the correct endpoint, or more precisly the socket that is associated with the connection stream.
- <b>Protocol Encapsulation</b> : Keeps the layer of the networking stack seperate. TCP or UDP data and packet data are simply treated as an IP packet. The 6-byte IP header identifies the type of protocol to use so it can send it to the correct OSI layer. Likewise a <b>Frame</b> or ehternet packet treats the entire IP packet as data. It has a 2 byte header so that the ethernet driver can send to the correct module

###Sockets
- <b>Sockets</b> : are an interface to the nertwork that is provided to an application via the OS
- The socket allows one application to communicate with another
- <b>Socket System Call</b> : Sockets are assigned an address and port number.
- <b>Bind</b> : Associates a local network transport address with a socket
- <b>Listen</b> : a socket used to listen for connections used for TCP or connection oriented protocols
- <b>Accept</b> : Blocks until a connection is recieved at a listening socket, and at that point assigns a new socket to that connection
- <b>Connect</b> : Client establishes a connection with a server
- <b>Compatable</b> : Afer the connection is established the same read and write commands may be used accross systems
- <b>Close/Shutdown</b> : When the connection is complete and the transfer is done then the close or shutdown system call can be issued to close the connection / socket
- Every socket has its own <b>Socket Structure</b>.
- The structure identifies the set of opperations that be performed on it as well as its data send and recieve queries
- Additionally ids the networking protocol that can be used with it
- <b>Socket Buffer</b> : As soon as a packet is created it is placed here. Instead of moving the data a pointer to the buffer goes throughout the stack. The data is originally copied from the user space to the socket itself and then transmitted. Enough space is allocated for the data and the headers
- <b>Abstract Device Interface</b> : Sits below the networking layers and above the actual device drivers.
    - Contains functions for Initializing devices , sending data , allocating socket buffers for recieving data
- <b>Network device driver</b> : (Ethernet or 802.11) (Interacts with the physical network) Transmitting data out to the network and grabbing data from the hardware
- <b>Linux NAPI</b> : disables network device interrupts when a packet is recieved and reverts to periotic polling. If a poll yeilds no data then the interrupts are reenabled.

###Remote Procedure Calls
- Remote Procedure Calls are a programming language construct (Provided by the compiler) as opposed to an OS construct such as packets
- <b>Stub Functions</b> : creates the illusion of a remote procedure call
- <b>Client Stub</b> : is the same interface as the desired remote procedure
    - Its function is to take the procedures , turn them into a network message, send them to the server , await for a reply, decrypt the message and return to the cleint
- <b>Server Stub</b> : Registers the service , and awaits incoming requests for running the remote procedure.

##Network File Systems
- <b>Download Upload Model</b> : An entire file is downloaded onto the machine if the file is opened. If the file is modified then it is uploaded.
- <b>Remote Access Model</b> : Individual requests are sent to the remote server as remote procedures

##Network File System Examples
###NSF
- Server does not maintain state therefore there is no need for remote open or close procedures.
- This works well is faulty environments , since there is no state to restore if the server crashes
- To improve performance the client requests a large block of data performing a read ahead reading future blocks ahead of time
- NFS is ambiguous because the server and clients do not know what blocks each one of the clients have cahced so far
- File locking was not possible because it is stateless however a seperate file manager was implemented later on
    
###AFS
- Improvement of NFS to support file sharing over a large scale
- Introduced the use of a large cache on the client side to hold remote files for a long time. (Long term caching)
- This supports the file upload and download model
- <b>Whole File Download</b> : The whole file is downloaded on first access
- <b>Session Semantics</b> : The last one to close a modified file wins and the other changes are overwritten
- <b>Callback promise</b> : Server makes this and keeps track of all the clients that have downloaded a file. When a modification is mad it issues a callback where it then goes through the clients that have downloaded the file and invalidates the old version of the file that they have.
- The next time that the client opens the file it will be the new file that was downloaded from the server
- <b>Volumes</b> : Files under AFS are shared under this , directory on a file system that has a unique ID . If the server wants to move a volume it just sends a requenst with the data to the new server and the client is unaware of this change

###SMB
- Server Message Block : Connection oriented stateful file system
- Uses the remote access model

##Protection and Security
- <b>Protection</b> : Provides and enforces controlled access of resources to users and processes
- <b>Security</b> : Set of policies that define authorized access to a system , 
- <b>Principle of Least Privledge</b> : Any entry can only access files that are essential to completing a task
- <b>Privlege Seperation</b> : Segment a process into multiple parts , each granted only privleges needed to operate
- <b>Protection Domain</b> : Every thread operates in this which defines what resource is allowed to access
- <b>Access Matrix</b> : Model the full system-wide set of access rights, rows represent protection domains whereas columns represent objects
- <b>Access Control List</b> : Column of an access matrix , which is a list of permission for a certain domain
- <b>Capability List</b> : Represents a row of an access matrix, an enumeration of all objects in the system and the operations that can be performed on them
- <b>Discretionary Access Control</b> : Allows the thread to access objects it has permissions to and also pass that information to other processes or write it to other objects
- <b>Manditory Access Control</b> : where the <b>Centrally Controlled</b> policy restricts the actions of domains on objects
- <b>Multilevel Secure </b> : Objects are classified in a security hierarchy : unclassified , confidential, secret , and top secret. 
- The overall policy is not read up no write down

##Cryptography
- <b>Restricted Cypher</b> : Workings of the cypher are kept secret. Has many flaws, such as leaking the cypher, and reverse engineering the cypher.
- <b>Symmetric Encryption Algorithm</b> : Uses the same key for encryption and decryption. 
- <b>Public Key Algorithm</b> : One for encryption and one for decryption
- <b>One way function</b> : One that can be computed easily in one direction but immpossible in the other
        - These are useful for digital signatures
        - <b>Hash Function</b> : One way function whose output is a fixed number of bits
        - <b>Collision Resistence</b> : Hard to construct text that hashes to a specific hash, and it is hard to construct a text from a hash

###Secure Communication
- If alice wants to communicate with Bob they need to encode their messages using a key
- However if Alice then wants to communicate with Syd then she needs another key
- All these keys form a key explosion, a system with n users needs n<sup>2</sup> keys
- <b>Diffie-Hellman exponential key exchange </b> Each party generates a private and public key or really numbers
        - Uses one way function where Alice can compute a common key using Bob's public key and her private key

###Session Keys
- Random key used for communication during a single communication session
- This is good because if the key is ever take it will not allow the user to have access for long
- <b>Hybrid Crypto System</b> : uses public key cryptography to send a session key secrurely
        - System generates a random session key encripting it with the users public key
        - The user decripts using they private key and then uses symmetric for communication
        - Higher performance
        - Ease of communication

- <b>Digital Signature</b> : Encrypting a hash of a message with the creators private key

##Authentication
- Three factors of authentication
        - <b>Something you have</b> : Card key / ID card
        - <b>Something you know</b> : Password
        - <b>Something you are</b> : biometric scanner

###Password Authentication Protocol
- Password is associated with each user and is stored within a file
- If the password matches with the user then you are authorized
- One problem with this is if someone gets a hold of the password file then they have all the passwords
- Instead should implement a hash where compare the hash of the password to the hash of the user
- <b>Dictionary Attacks</b> Hash tables are vulnerable to this because one can just create their own hash of values and hit the hash table with them. From the result they can guess the hash function
- <b>Salt</b> : extra data that is added to the password prior to hashing it. This makes it much harder to attack the hash because now it hashes to saltpassword as opposed to just the password
- <b>One time passwords</b> : password is only good for one login and then need a new one to access the system

###Challenge handshake authentication
- Generates a challenge or random bits , both the client and the server share a secret key, when the client recieves the challenge it computes a hash using its secret and sends back to the server. If the hash matches the servers hash then they are authenticated

###SecureID
- Two factor authentication that generates one time passwords for users. Relies on a user password and a small computing device called a token. The token generates a new number every 30 seconds. 

###Public Key Authenitcation
        
