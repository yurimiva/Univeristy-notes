Idk what's with professors wanting us to regurgitate what they said during the lectures. Anyway, let's get to the first part.

## Architecture of operating systems

Since an operating system can get really big and complicated, there will be various implementations with different advantages and purposes:

1. **Monolithic system** → traditionally, an operating system was a **single block** that would perform all system functions from CPU to user interface management. Also, there was no ordering between the system functions: all functions and data structures were accessible from any point in the kernel.
   This can cause major problems: **maintenance and expansion are very difficult**. For example, a bug could cause a crash in the entire system, and finding and fixing it could be difficult because the code is very integrated... Almost like an integrated circuit.
   The main advantage is that the system is **compact**, **fast** and **efficient**, so this is mostly used for simple systems, like embedded systems.

 2. **Hierarchically-structured systems** → are an *evolution* to the monolithic system since they can organize the system into **functional levels**: a function at a certain level can only call functions at a lower level. 
    However, there is no separation between the components of the OS; so distinguishing hierarchical dependencies may not be immediate, while maintenance and expansion still remain difficult but are more manageable than a monolithic system.

3. **Layered system** → is a **first modular approach**, where the system is decomposed into a number of layers (Everything is a layer in life istg). Each layer is a **module**, a self-contained unit of code, that implements a component of the OS while hiding its implementation from others. Adjacent layers communicate through a well-defined interface. 
   This implementation makes easier the development and the debugging of the OS, but the **system's performances are reduced**: a call to a function of a certain level must pass through all the levels above it.

4. **Microkernel** → is an further *evolution*, which structures the system by removing all non-essential functions from the kernel and implementing them as user-level services. The kernel thus created is very small and has only minimal functions for **process management**, **memory**, and **communication**. 
   Its main purpose is to allow communication between client programs and services, as well as between the services themselves so that they can request functions from each other. The communication takes place through messages exchanged by the kernel, so there is a large overload in management, which affects system efficiency. 
   This model allows for **maximum separation between mechanisms and policies** (it is always good to separate "how it is done" from "what is done"), **easy modifiability**, **maximum portability** (it is sufficient to reimplement the few microkernel functions used by the services to make the system work on a different machine) and **reliability**.

5. **Modular stricture** → this approach involves the use of **object-oriented programming** techniques, a programming paradigm that organizes code into objects that have both data and behavior,  and this approach is the most widely used. 
   The kernel possesses only a minimal set of functions, but here it **dynamically links** to the modules that implement the various system functions.
   The implementations of each module are hidden from others but have well-defined interfaces for communication. Communication is direct, thus improving performance compared to the microkernel approach (although it remains limited). The other advantages of this implementation are the same as those of microkernel systems.

6. **Virtual machines (VMs)** → the kernel of a VM system is responsible for **providing an exact copy of the underlying hardware they want to be based on** to each virtual machine, each of which executes an OS (even different ones).
   *Note*: the kernel of a VM is different from the kernel of our physical machine, even tho it helps the implementation of VM's kernel. 
   This way, it is possible to give the illusion to each user of having their own machine: **scheduling** and **virtual memory** are responsible for **dividing CPU time and memory among the VMs**, while **spooling** and the **file system** provide them with **virtual peripherals** and **disks**. A further advantage is the **isolation of each virtual machine** from the others and from the real machine, which makes them suitable for testing changes to an operating system without causing downtime in the system. Unfortunately, this also prevents the direct sharing of data and resources between VMs. 
   However, it is possible to share a **virtual disk** between VMs, or create a **virtual network** between them that allows them to communicate while remaining completely isolated. The main disadvantage of these systems is the **performance degradation** due to the **virtualization layer**, which greatly reduces performance. (Basically is the same problem as the layered system, wwith the layers being the VM's kernel, the system that connects all VMs, and then the physical kernel)

-----
# Threads