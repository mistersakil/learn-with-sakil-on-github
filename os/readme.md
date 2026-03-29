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

## Computer architecture and history

- Mechanical computer

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
