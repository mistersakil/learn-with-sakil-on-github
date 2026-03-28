# Introduction of Operating System

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
