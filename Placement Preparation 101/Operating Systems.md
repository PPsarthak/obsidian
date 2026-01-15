#placement-preparation #os 
**What is an OS?**
An operating system is an interface between the user and underlying hardware.
It acts as intermediate between them to trigger and control various actions
- E.g., To print a PDF file, the OS instructs the printer hardware to print the PDF
It is also responsible for memory allocation, process management, file management,  etc.

*What is a System call?*
A basic request made by the user to the OS/kernel
It allows a user to request a service from a kernel that it does not have access to (in user mode)
E.g., Double clicking a file to open it, `Ctrl+P` to print a file, etc.
> System calls are also a way to switch from user mode to kernel mode

**Pre-emptive and Non-pre-emptive**
Pre-emptive is the ability of OS to interrupt an ongoing process in favor of a higher priority process
Vice-versa is non-pre-emptive
	*Context Switching*: when pre-emption occurs and the CPU starts executing a higher priority process, it is c/a as Context Switching

##### What is a Kernel?
Kernel is the heart of OS
It performs the following functions:
-  Process management
-  Resource management - CPU, RAM, Disk, I/O
-  Storage management (Secondary memory)
-  Memory management (Primary memory)
-  Security

**User Mode and Kernel Mode**
User mode and kernel mode are two distinct operating modes in a computer system that define the level of privilege a process has when accessing system resources. These modes are part of the protection mechanism implemented by the operating system to ensure security and stability.

In user mode, processes run with limited privileges and cannot directly access hardware or critical system resources. Applications like web browsers and text editors operate in this mode. 

In kernel mode, the operating system's kernel runs with full privileges and has unrestricted access to all hardware and system resources. It handles critical tasks like process management, memory allocation, and I/O operations. 

**Types of kernel**:
-  Monolithic kernel:
	-  All functions like - process management, resource management, file management is performed by it are in the kernel
	-  Hence the OS always runs in kernel mode
	-  E.g., Linux
-  Micro kernel:
	-  Only memory management and process management is in the kernel
	-  Rest of the functions operate in user mode
	-  E.g., MINIX
-  Hybrid kernel:
	-  Few more functionality other memory and process management are in kernel mode while remaining are in user mode
	-  E.g., Windows

#### Types of OS
1. Batch OS
	-  Multiple jobs are grouped together into "batches" based on some similarity
	-  system then executes each batch 
	-  lacks interactivity with user
2. Multi-programmed OS - *Non-pre-emptive*
	-  multiple tasks are loaded in RAM and added into a queue (c/a as "<span style="color:rgb(102, 207, 255)">Ready queue</span>") 
	-  each task is sent to CPU one-by-one from the queue
	-  when the current process requires I/O, the CPU does not wait for the I/O to complete. Instead, it brings in the next process from the ready queue and starts executing it.
	-  once the I/O operation for the original process is complete, the process is moved back to the ready queue for resumption.
3. Time sharing OS / Multi-tasking OS - *Pre-emptive*
	-  allocates a fixed time for each task; if the task does not gets finished in that time, it added back in the ready queue
	-  gives the user the illusion of simultaneous execution
4. Multi-processing OS:
	-  Multiple processors - i.e., CPU > 1

---
#### Types of System calls
![[Types of System Calls in OS.png|750]]

> File Mode - chmod command
> ![[File Mode in OS.png|350]]

### Process
A process is a program in execution
<span style="color:rgb(102, 207, 255)">Process Counter</span> keeps the track of address of the next instruction to be executed
<span style="color:rgb(102, 207, 255)">Program Control Block</span> holds the meta data about the process, it's state, it's priority, resources, etc.

![[Program in OS.png]]
#### Process State Model
**2 State Process Model**
![[2 State Process Model.png|550]]

**5 State Process Model**
![[5 State Process Model.png|550]]

**7 State Process Model**
![[7 State Process Model.png|550]]

#### Types of Schedulers
##### Long-term scheduler
- brings max no of process from secondary memory to primary memory
- LTS operates on a longer time frame (does not make frequent decisions) and increases degree of multi-programming

##### Mid-term scheduler
- swaps processes from primary memory to secondary memory and back to primary memory as well
- MTS operates on medium-time frame and is responsible for context switching

##### Short-term scheduler
- brings max no of process from ready state to running state
- STS operates on shorter time frame and it's goal is to minimize process waiting time and maximize CPU utilization and time

### Process Scheduling

##### Important Terminologies

**Arrival Time (A)** : time (stamp) when the process arrived
**Burst Time (B)** : time to execute (duration)
**Completion Time (C)** : time (stamp) when the process got completed

**Turn Around Time** : *(C-A)* time difference of when the process got completed and when it arrive 
**Waiting Time** : *(C-A-B)* time for which the process had to be in waiting (not in execution)
**Response Time** : *Time (stamp) when CPU first picked it for execution - A* : in case of a non-pre-emptive CPU, Waiting Time = Response Time

#### First Come First Serve
The job that comes first (earliest arrival time) is picked when there are multiple jobs in the ready queue
If there is a single one, then that one is picked
#### Shortest Job First
The job having minimum burst time is picked when there are multiple jobs in the ready queue
If there is a single one, that one is picked
#### Shortest Remaining Job First
Same as SJF, but difference is -
	In SJF, the CPU completes the current job and then decides which job to pick by looking at their burst time
	Here, as soon as a new job enters the ready queue, the CPU pauses current job executes, compares the current job's burst time (that is left) against the new job's burst time and then chooses
#### Round Robin
Each job has a fixed time to execute and complete itself
If the burst time > fixed time, then the remaining part of the job is rescheduled
#### Priority based
Each job has a priority associated with it.
If a higher priority job comes in ready queue, the current job in execution is paused and the higher priority job is brought in execution


### Threads
> <span style="color:rgb(102, 207, 255)">Threads are simply lightweight unit of a process</span> 

Each process has a thread, if the OS allows multi-threading, more threads can be created in the process
##### Process vs Threads

| Process                                 | Threads                                            | (My Note)                             |
| --------------------------------------- | -------------------------------------------------- | ------------------------------------- |
| systems calls are involved              | systems calls are not involved                     | I can't think of a reason for this ?? |
| different - Data, Code, Stack, Register | same - Data, Code, <br>different - Stack, Register |                                       |
| each process is independent             | threads can be dependent                           |                                       |
| context-switching is slower             | context-switching is faster                        |                                       |
##### User level threads  vs Kernel level threads

| User level thread                                   | Kernel level thread                       |
| --------------------------------------------------- | ----------------------------------------- |
| creation and management is done by libraries        | creation and management is done by kernel |
| typically faster*                                   |                                           |
| context-switching is faster*                        |                                           |
| easy to synchronize*                                |                                           |
| more scalable*                                      |                                           |
| if 1 thread gets blocked, entire process is blocked |                                           |
\* - reason for all these points is that thread is managed by library and not the kernel
