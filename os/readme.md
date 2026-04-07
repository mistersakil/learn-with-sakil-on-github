# Introduction of Operating System

***Application Software:*** performs specific task for the user.

***System Software:*** operates and controls the computer system and provides a platform to run application software.

***Operating System:*** An operating system is a piece of software that manages all the resources of a computer system,both hardware and software, and provides an environment in which the user can execute his/her programs in a convenient and efficient manner by hiding underlying complexity of the hardware and acting as a resource manager.

***Why OS?***

* What if there is no OS?
  * Bulky and complex app.(Hardware interaction code must be in app's code base)
  * Resource exploitation by 1 App.
  * No memory protection.

* What is an OS made up of?
  * Collection of system software.

***Functions of an  OS:***

```FunctionsOfOS
- Access to the computer hardware.
- Interface between the user and the computer hardware
- Resource management (Aka, Arbitration) (memory, device, file, security, process etc)
- Hides the underlying complexity of the hardware. (Aka, Abstraction)
- Facilitates execution of application programs by providing isolation and protection.
```

## Types of Operating System

***Goals of OS:***

* Maximum CPU utilization
* Less process starvation
* Higher priority job execution

***OS types:***

1. Single process OS [Ex: MS DOS - 1981]

    ```singleProcessOS
    Only 1 process executes at a time from the ready queue.
    ```

2. Batch-processing OS [Ex: Atlas - early 1960]

    ```batchProcessingOS
    1. Firstly, user prepares his job using punch cards.
    2. Then, he submits the job to the computer operator.
    3. Operator collects the jobs from different users and sort the jobs into batches with similar needs.
    4. Then, operator submits the batches to the processor one by one.
    5. All the jobs of one batch are executed together.
        - Priorities cannot be set, if a job comes with some higher priority.
        - May lead to starvation. (A batch may take more time to complete)
        - CPU may become idle in case of I/O operations.
    ```

3. Multiprogramming OS [Ex: Dijkstra - early 1960s]

    ```MultiprogrammingOS
    Multiprogramming increases CPU utilization by keeping multiple jobs (code and data) in the memory 
    so that the CPU always has one to execute in case some job gets busy with I/O.

    - Single CPU
    - Context switching for processes.
    - Switch happens when current process goes to wait state.
    - CPU idle time reduced.
    ```

4. Multitasking OS [Ex: MIT - early 1960s]

    ```MultiprogrammingOS
    Multitasking is a logical extension of multiprogramming.
    
    - Single CPU
    - Able to run more than one task simultaneously.
    - Context switching and time sharing used.
    - Increases responsiveness.
    - CPU idle time is further reduced.   
    ```

5. Multiprocessing OS [Ex: WINDOWS, MAC]

    ```MultiprocessingOS
    Multi-processing OS, more than 1 CPU in a single computer.
    
    - Increases reliability, 1 CPU fails, other
    can work
    - Better throughput.
    - Lesser process starvation, (if 1 CPU is
    working on some process, other can be executed on other CPU)
    ```

6. Distributed OS [Ex: LOCUS]

    ```DistributedOS
    - OS manages many bunches of resources, >=1 CPUs, >=1 memory, >=1 GPUs, etc
    - Loosely connected autonomous,
    interconnected computer nodes.
    - Collection of independent, networked, communicating, and physically separate computational nodes.
    ```

7. Real-time OS (RTOS) [Ex: ATCS]

    ```RTOS
    - Real time error free, computations
    within tight-time boundaries.
    - Air Traffic control system, ROBOTS etc.
    ```

## Multi-Tasking vs Multi-Threading

***Program:*** A Program is an executable file which contains a certain set of instructions written to complete the specific job or operation on your computer.

* It's a compiled code. Ready to be executed.
* Stored in Disk

***Process:*** Program under execution. Resides in Computer's primary memory (RAM).

***Thread:***

* Single sequence stream within a process.
* An independent path of execution in a process.
* Light-weight process.
* Used to achieve parallelism by dividing a process's tasks which are independent path of execution.
* EX: Multiple tabs in a browser, text editor (When you are typing in an editor, spell-checking, formatting of text and saving the text are done concurrently by multiple threads.)

### Difference between Multi Tasking and Multi Threading

***Multi Tasking***

```MultiTasking
- The execution of more than one task
simultaneously is called as multitasking.
- Concept of more than 1 processes being
context switched.
- No. of CPU 1.
- Isolation and memory protection exists. OS must allocate separate memory and resources to each program that CPU is executing.

```

***Multi Threading:***

```MultiThreading
- The execution of more than one task
simultaneously is called as multitasking.
- A process is divided into several different
sub-tasks called as threads, which has its
own path of execution. This concept is
called as multithreading.
- Concept of more than 1 thread. Threads are
context switched.
- No. of CPU 1. No. of CPU >= 1. (Better to have more than 1)
- No isolation and memory protection, resources are shared among threads of that process. OS allocates memory to a process, multiple threads of that process share the same memory and resources allocated to the process.
```

***Thread Scheduling:*** Threads are scheduled for execution based on their priority. Even though threads are executing within the runtime, all threads are assigned processor time slices by the operating
system.

### Difference between Thread Context Switching and Process Context Switching

***Thread Context Switching:***

```ThreadContextSwitching
- OS saves current state of thread & switches
to another thread of same process.
- Doesn't includes switching of memory
address space. (But Program counter, registers & stack are
included.)
- Fast switching.
- CPU's cache state is preserved.
```

***Process Context Switching:***

```ProcessContextSwitching
- OS saves current state of process &
switches to another process by restoring its
state.
- Includes switching of memory address space.
- Slow switching.
- CPU's cache state is flushed.
```

## Components of OS

***Kernel:*** A kernel is that part of the operating system which interacts directly with the hardware and performs the most crucial tasks.

* Heart of OS/Core component
* Very first part of OS to load on start-up.

***User space:*** Where application software runs, apps don't have privileged access to the underlying hardware. It interacts with kernel.

* GUI
* CLI

***A shell, also known as a command interpreter, is that part of the operating system that receives commands from the users and gets them executed.***

### Functions of Kernel

1. Process management:
  a. Scheduling processes and threads on the CPUs.
  b. Creating & deleting both user and system process.
  c. Suspending and resuming processes
  d. Providing mechanisms for process synchronization or process
communication.
2. Memory management:
  a. Allocating and deallocating memory space as per need.
  b. Keeping track of which part of memory are currently being used and by
which process.
3. File management:
  a. Creating and deleting files.
  b. Creating and deleting directories to organize files.
  c. Mapping files into secondary storage.
  d. Backup support onto a stable storage media.
4. I/O management: to manage and control I/O operations and I/O devices Buffering (data copy between two devices), caching and spooling.

```IOManagement
Spooling:
    1. Within differing speed two jobs.
    2. Eg. Print spooling and mail spooling.  
Buffering:
    1. Within one job.
    2. Eg. Youtube video buffering  
Caching:
    1. Memory caching, Web caching etc.
```








## SP vs BP

```ram
Ram - Blocks -> Based on byte size
- OS binary code block
- Process - Virtual CPU/Computer
    * Code segment - Function declaration
    * Data segment - Global variables
    * Stack/Function frame 
        - Function parameters
        - Return address
        - Add Address of old BP left to return value
        - Local variables
    * Heap
```

```CPU
CPU
- Processing unit
    * Arithmetic logic unit
    * Control unit
- Registers set (Manages process current states)
    * SP - Stack pointer
    * BP - Base pointer
    * IR - Instruction pointer
    * PC - Program counter
```

## Context switching | Concurrency

```CONTEXT_SWITCHING
- All steps of SP & BP
- Process control block (PCB)
    * Manges process individual states
- Concurrency - Execute multiple process one by one switching context state
```
