
## Overview
A thread is a basic unit of CPU utilization.

Parts:
* Thread ID
* Program Counter
* Register Set
* Stack
Share with other threads from the same Process:
* code section
* data section
* other OS resources (open files, signals, etc).

![Image](https://github.com/user-attachments/assets/74e78418-8ab1-4122-99a9-3430049b5d6a)

### Motivation

Most software applications that run on modern computers and mobile devices are multithreaded.

Most operating system kernels are also typically multithreaded. As an example, during system boot time on Linux systems, several kernel threads are created. Each thread performs a specific task, such as managing devices, memory management, or interrupt handling.

The command ps -ef can be used to display the kernel threads on a running Linux system. Examining the output of this command will show the kernel thread kthreadd (with pid = 2), which serves as the parent of all other kernel threads.

### Benefits

#### Responsiveness

Multithreading an interactive application may allow a program to continue running even if part of it is blocked or is performing a lengthy operation, thereby increasing responsiveness to the user. This quality is especially useful in designing user interfaces.  ( Asynchronous applications )
#### Resource sharing

Threads share the memory and the resources of the process to which they belong by default.

The benefit of sharing code and data is that it allows an application to have several different threads of activity within the same address space.
#### Economy

Allocating memory and resources for process creation is costly. 
Threads share the resources of the process they belong.
Thread creation consumes less time and memory than process creation.
Context switching is typically faster between threads than between processes.

#### Scalability

In multiprocessor systems, threads can run in parallel on different cores, enhancing performance and scalability.

## Multicore Programming

The need for more computing performance, single-CPU systems evolved into multi-CPU systems.

#### Multicore

Trend in system design is to place multiple computing
cores on a single processing chip where each core appears as a separate CPU
to the operating system. 

#### Multithreaded Programming

Multithreaded programming provides a mechanism for more efficient
use of these multiple computing cores and improved concurrency.

#### Concurrency

Concurrent system supports more than one task by allowing all the tasks
to make progress. 

On a system with multiple cores, however, means that some threads can run in parallel, because the system can assign a separate thread to each core.

#### Parallel system

Parallel system can perform more than one task simultaneously.

It is possible to have concurrency without parallelism.

### Programming Challenges

#### Identifying tasks

#### Balance

#### Data splitting

#### Data dependency

#### Testing and debugging

### Amdahl's Law

### Types of Parallelism

#### Data parallelism

#### Task parllelism

## Multithreading Models

User threads:

Kernel Threads:
### Many-to-One Model

![Image](https://github.com/user-attachments/assets/3b0d31ae-1bee-4b9c-98d9-95fc0c83f569)

### One-to-One Model

![Image](https://github.com/user-attachments/assets/10c21650-8911-4b1f-80b8-74aa4bd5a85f)
### Many-to-Many Model

![Image](https://github.com/user-attachments/assets/12db5b40-0fcd-4a32-ae80-d5297e02cd8e)

### Two-level Model

![Image](https://github.com/user-attachments/assets/df4de51a-ec08-42aa-b8dc-571623a44921)

## Thread Libraries
