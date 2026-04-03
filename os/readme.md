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

* Access to the computer hardware.
* Interface between the user and the computer hardware
* Resource management (Aka, Arbitration) (memory, device, file, security, process etc)
* Hides the underlying complexity of the hardware. (Aka, Abstraction)
* Facilitates execution of application programs by providing isolation and protection.

## Types of Operating System

***Goals of OS:***

* Maximum CPU utilization
* Less process starvation
* Higher priority job execution

***OS types:***

* Single process OS [Ex: MS DOS - 1981]

    ```singleProcessOS
    Only 1 process executes at a time from the ready queue.
    ```

* Batch-processing OS [Ex: Atlas - early 1960]

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

* Multiprogramming OS [Ex: Dijkstra - early 1960s]

    ```MultiprogrammingOS
    Multiprogramming increases CPU utilization by keeping multiple jobs (code and data) in the memory so that the CPU always has one to execute in case some job gets busy with I/O.
    - Single CPU
    - Context switching for processes.
    - Switch happens when current process goes to wait state.
    - CPU idle time reduced.
    ```

* Multitasking OS [Ex: MIT - early 1960s]

    ```MultiprogrammingOS
    Multitasking is a logical extension of multiprogramming.
    
    - Single CPU
    - Able to run more than one task simultaneously.
    - Context switching and time sharing used.
    - Increases responsiveness.
    - CPU idle time is further reduced.   
    ```

* Multiprocessing OS [Ex: WINDOWS, MAC]

    ```MultiprocessingOS
    Multi-processing OS, more than 1 CPU in a single computer.
    
    - Increases reliability, 1 CPU fails, other
    can work
    - Better throughput.
    - Lesser process starvation, (if 1 CPU is
    working on some process, other can be executed on other CPU)
    ```

* Distributed OS [Ex: LOCUS]

    ```DistributedOS
    - OS manages many bunches of resources, >=1 CPUs, >=1 memory, >=1 GPUs, etc
    - Loosely connected autonomous,
    interconnected computer nodes.
    - Collection of independent, networked, communicating, and physically separate computational nodes.
    ```

* Real-time OS (RTOS) [Ex: ATCS]

    ```RTOS
    - Real time error free, computations
    within tight-time boundaries.
    - Air Traffic control system, ROBOTS etc.
    ```

## Kernel

```cpu
* User mode
* Kernel mode
```

### What does kernel does?

```kernelDo
* Process management
* Memory management
* Device management
* File system management
* System call
* And more
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
