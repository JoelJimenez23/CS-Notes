A process is the unit of work in a modern computing system

## The process

Status of the current activity of a process is represented by a
* Program Counter value
* Contents of the processor's registers

### Memory Layout
![Image](https://github.com/user-attachments/assets/5621a2c3-e894-42d3-a516-d78eb86b1f96)

* Stack: Temporary data storage
* Heap: dynamically allocated memory
* Text: executable code

Each time a function is called an activation record is pushed onto stack
Activation record contains:
 * function parameters
 * local variables
 * return address

The stack and heap sections grow toward one another, the operating system must ensure they do not overlap one another
### A program by itself is not a process

#### Program (passive entity)
A file containing a list of instructions stored on a disk (executable file)

#### Process (active entity)
With a program counter specifying the next instruction to execute and a set 
of associated resources


A program becomes a process when an executable file is load into memory



Although two processes may be associated with the same program, they
are nevertheless considered two separate execution sequences. 

 Although the text sections are equivalent, the data, heap,
and stack sections vary.


## Process State

The state of a process is defined in part by the current activity of that process. 

### Possible States
* New: The process is being created.
* Running: Instructions are being executed.
* Waiting: The process is waiting for some event to occur (such as an I/O completion or reception of a signal).
* Ready: The process is waiting to be assigned to a processor.
* Terminated: The process has finished execution.

![Image](https://github.com/user-attachments/assets/7738efc1-43f7-40da-9766-daa48fc7567b)

Only one process can be running on any processor core at any instant


## Process Control Block (PCB)
Also called task control block

![Image](https://github.com/user-attachments/assets/371e08ba-8f93-4e87-8bcc-4df3b7c6f85e)

### Not mentioned pieces
#### CPU-scheduling information
This information includes a process priority, pointers to scheduling queues, and any other scheduling parameters.
#### Memory-management information
Includes the value of the base and limit registers and the page tables, or the
segment tables, depending on the memory system used by the operating
system.
#### Accounting information
Includes the amount of CPU and real time used, time limits, account numbers, job or process numbers, and so on.
#### I/O status information
Includes the list of I/O devices allocated to the process, a list of open files, and so on.

## Threads
On systems that support threads, the PCB is expanded to include information for each thread. Other changes throughout the system are also needed to support threads. 

## Process Scheduling
#### Objective
* Have some process running at all times so as to maximize CPU utilization.
* Switch a CPU core among processes so frequently that users can interact with each program while it is running.
* Each CPU core can run a process at a time.
#### Type of process
##### I/O-bound
Spends more of its time doing I/O than it spends doing computations.
##### CPU-bound 
Generates I/O requests infrequently, using more of its time doing computations.

### Scheduling Queues
As processes enter the system, they are put into a ready queue, where they are
ready and waiting to execute on a CPU’s core This queue is generally stored as
a linked list; a ready-queue header contains pointers to the first PCB in the list,
and each PCB includes a pointer field that points to the next PCB in the ready
queue.

The system also includes other queues. When a process is allocated a CPU
core, it executes for a while and eventually terminates, is interrupted, or waits
for the occurrence of a particular event, such as the completion of an I/O
request.The system also includes other queues. When a process is allocated a CPU
core, it executes for a while and eventually terminates, is interrupted, or waits
for the occurrence of a particular event, such as the completion of an I/O
request.

A new process is initially put in the ready queue. It waits there until it is
selected for execution, or dispatched.

![Image](https://github.com/user-attachments/assets/e1e609a1-1b7a-44b2-96f8-b801159785da)

The CPU scheduler executes at least once every 100 milliseconds, although typically much more frequently.

### Swapping
An intermediate form of scheduling.

The key idea is that sometimes it can be advantageous to remove a process from memory (and from active contention for the CPU) and thus reduce the degree of multi-programming. Later, the process can be reintroduced into memory, and its execution can be continued where it left off. This scheme is known as swapping because a process can be “swapped out” from memory to disk,where its current status is saved, and later “swapped in”
from disk back to memory, where its status is restored.

Swapping is typically only necessary when memory has been over committed and must be freed up.
## Context Switch
##### Interrupts
Cause the operating system to change a CPU core from its current task and to run a kernel routine.
Such operations happen frequently on general-purpose systems.
##### Context of a Process
All the information needed to correctly execute a process if it is resumed after being interrupted.

Is represented in the PCB of the process.

It includes:
* The value of the CPU Registers
* The process state
* Memory-management information

##### Modes
* State save
* State restore

##### What happen when it occurs?
The kernel saves the context of the old process in its PCB and loads the saved context of the new process scheduled to run.

##### Switching Speed
Varies from machine to machine
Depends on:
* Memory Speed
* number of registers that must be copied
* existence of special instructions

Typical speed is a several microseconds

##### Hardware and OS considerations
Context-switch times are highly dependent on hardware support.
 
The more complex the operating system, the greater the amount of work that must be done during a context switch.


![Image](https://github.com/user-attachments/assets/a16b4a1f-7d73-4f97-8e0a-e609819718a8)


## Operations on Processes
The processes in most systems can execute concurrently, and they may be created and deleted dynamically.

### Process Creation
Process may create several new processes.

The creation process is called a parent process and the new processes are called of the children of that process.

The process creation forms a tree of processes

Most OS identify process with a unique process identifier  (PID) which is typically an integer number. Can be use as an index.


![Image](https://github.com/user-attachments/assets/9610ac81-ca15-42e8-9c08-d0fa8c5abda7)


##### Some Unix/Linux Commands
List all the processes
* ps -el
Displays a tree of all processes
* pstree

#### Creating a Child Process
When a child process is created , that child will need some resources to accomplish its task.
##### Needed Resources
* CPU time
* memory
* files
* I/O devices
##### Ways to obtain resources
###### Operating System
Resources obtained directly from the operative system.
###### Parent Resources
* Subset of the parent resources
* Parent can share resources among several children
Restricting a child process to a subset of the parent’s resources prevents any process from overloading the system by creating too many child processes.

##### Creating a child process possibility
###### Execution possibilities
* The parent continues to execute concurrently with its children.
* The parent waits until some or all of its children have terminated.
###### Address-space possibilities
* The child process is a duplicate of the parent process (it has the same program and data as the parent).
* The child process has a new program loaded into it.




##### Mechanism
###### Fork call
A new process is created by the fork() system call.
The new process consists of a copy of the address space of the original process.
This mechanism allows an easily parent-child communication.
Both process continue execution at the instruction after the fork.
Return code of fork() for the child is 0 and for the parent is !=0

###### After fork
One of the two process uses exec() syscall to replace the process memory space with a new program.

###### exec()
Loads a binary file into memory (destroying the memory image of the program containing the exec() system call and starts its execution.

###### Running copies of the same program
We can have different process running the same program.
The differences are:
* PID value for the child is zero and Parent's is >0.

###### Child Inheritance
* Privileges and scheduling attributes
* certain resources (ex. open files)

###### Wait()

The parent waits for the child process to complete with the wait() system call. When the child process completes (by either implicitly or explicitly invoking exit()), the parent process resumes from the call to wait(), where it completes using the exit() system call.

###### Child without exec() 
Child has the possibility of not calling a exec() . In this case, both processes are concurrent running the same code instruction but with their own copy of any data.

![Image](https://github.com/user-attachments/assets/2c9f8609-a205-48d0-bb91-9664fac271fc)### Process Termination

###### When it terminates?

When it finishes executing its final statement and asks the operating system to delete it by using the exit() system call.
###### What does it returns?

Status value (typically integer) to its waiting parent via wait().
All the resources of the process are deallocated and reclaimed by the OS.
###### Extra termination circumstances

A parent process can terminate its child by an appropriate system call
A user or a misbehaving application could arbitrary kill another users's processes
###### Parent's child termination

There are a variety of reason when a parent may terminates its child process

* The child has exceeded its usage of some of the resources that it has been allocated. (To determine whether this has occurred, the parent must have a mechanism to inspect the state of its children.)
* The task assigned to the child is no longer required.
* The parent is exiting, and the operating system does not allow a child to continue if its parent terminates.

##### OS termination handling

Some systems doesn't allow the a child to exist if his parent has terminated.
In this systems if a parent is terminated all their children must be also terminated, this phenomenon is called "cascading termination" and is normally initiated by the OS.

A parent process may wait for the termination of a child process by using the wait() system call. 

The wait() system call is passed a parameter that allows the parent to obtain the exit status and the PID  of the terminated child. The parent know which of its children has terminated by the PID.
###### Memory deallocation 

When a process terminates, its resources are deallocated by the operating
system. However, its entry in the process table must remain there until the
parent calls wait(), because the process table contains the process’s exit status.
###### Zombie Process

A process that has terminated, but whose parent has not yet called wait(), is
known as a zombie process.

All processes pass by this state only briefly.
###### Orphan Process

Now consider what would happen if a parent did not invoke wait() and
instead terminated, thereby leaving its child processes as orphans. 

In UNIX system the init/systemd  process serves as the root of the process hierarchy in this system.  In this scenario this process is assigned as the parent of the orphans. Eventually this process will invoke wait(), allowing the exit status of any orphaned process collecting and releasing the orphan's PID and process-table entry.

In modern Linux systems  systemd is not the only process that can manage orphans.
