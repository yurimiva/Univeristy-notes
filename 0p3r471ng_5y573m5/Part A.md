Idk what's with professors wanting us to regurgitate what they said during the lectures. Anyway, let's get to the first part.

# Architecture of operating systems

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

Typically, processes rely on a single control flow to carry out all tasks of an application; however, there are situations where you want to execute similar tasks concurrently, or where you want to continue receiving requests even while others are being served.

Traditionally, the system call **fork** is used to create new processes, while **IPC**(**Inter-Process Communication**) techniques are used to share data: this results in a huge waste of memory and CPU time because the fork makes a copy of the parent's address space, even though all child processes created will need to execute the same code and access the same shared data. The solution is to introduce **threads**. A thread is the basic unit of CPU utilization.

It can be seen as a **control flow within a process**, and there can be many of them for each process. It possesses an **ID**, its own **registers**, a **program counter**, a **stack**, and **if necessary private data**, and shares the address space and resources with the other threads of the process.

The use of threads brings several benefits:
1. **Increased availability** → it is possible to handle requests while other threads are busy, making the flow more fluid;
2. **Resource sharing** → all threads share code, data, and resources among themselves, allowing them to access them easily and even communicate (although synchronization is necessary to prevent data inconsistencies);
3. **Efficiency** → creating a new thread is faster and less memory-intensive than creating a new process, as there's no need to duplicate the entire address space;
4. **Exploitation of multiprocessor architectures** → a kernel that supports threads can schedule multiple threads of the same process on different processors simultaneously, improving performance. Thread support can be provided in user space (**user-level threads**) or in the kernel (**kernel-level threads**).

Thanks to a **thread library**, the programmer is provided with functions for creating and managing threads. For user-level threads, all the code and data structures of the library are located in user space and calls to its functions are simple procedure calls. For kernel-level threads, on the other hand, code and data are located in the OS memory, and calls translate into system calls.

There are various threading models:
1. **Multi-threading models** → user-level threads of an application can be mapped onto kernel threads in different ways;
   
2. **Many-to-1 model** → all user-level threads are mapped to a single kernel-level thread; so the user-level library is responsible for simulating a multi-threaded environment by scheduling the user threads on the kernel thread.
   While this model is simple to implement, it has two main drawbacks:
	- **Limited scalability:** Since there's only one kernel thread, it cannot take advantage of multiprocessor architectures;
	- **Reduced concurrency:** If a user thread performs a blocking operation, the entire process will halt since we have a single kernel thread for the process.

3. **One-to-one model** → this is the most commonly used model, mapping each user-level thread to a separate kernel-level thread. It allows for maximum utilization of multiprocessor systems and provides high concurrency (if one thread blocks, only that thread is blocked). However, the overhead of creating a kernel thread for each user thread can impact system performance, especially with a large number of threads;
   
4. **Many-to-many model** → n user-level threads are mapped onto m <= n kernel-level threads. This allows for both the exploitation of multiprocessor architectures and high concurrency (if a kernel thread blocks, it's sufficient to schedule another one for execution), and it allows developers to use the number of user threads they desire. The user-level library will handle scheduling them onto the available kernel threads.
   A variant is the **two-level model**, which allows mapping one user thread to a dedicated kernel thread. This is a useful mode for managing threads that need to perform critical tasks or coordinate the activities of other threads.

**Thread cooperation** can be organized according to various models:
1. **Symmetric threads** all threads can perform the same set of tasks. When a request arrives, any available thread can handle it;
   
2. **Hierarchical threads** → a coordinator thread receives requests and gives orders to multiple worker threads that execute them;
   
3. **Pipeline threads** → each thread is specialized in performing a small operation quickly, then passing the partial result to the next thread, until the complete result is obtained. This allows for high throughput.

There are various **thread management issues**:
1. **Fork and exec within a thread** → in a traditional process the fork system call creates a copy of the process; but in a multi-threaded application it can behave in 2 ways: **it can duplicate all threads of the calling process**, or **only the calling thread**. If there is a choice, it is made based on the **subsequent instructions**: if there is an exec immediately after, the new process will be overwritten so it will be enough to duplicate only the calling thread; otherwise, it may be better to duplicate all of them;
   
2. **Cancellation of a thread** → cancelling a thread means terminating it before it has completed its execution. It can happen in 2 different ways:
	- **Asynchronous cancellation** → the thread is terminated immediately, but it might leave resources in an inconsistent state;
	- **Deferred cancellation** → a variable is set in the target thread, which the thread must check periodically at safe points (cancellation points) where there is no risk of leaving resources in an inconsistent state. However, this allows a thread to ignore the request;

3. **Signal and message handling** → when a signal arrives at a multi-threaded process, a problem arises: to which thread should it be delivered? You can:
	- **Send it to the thread to which it applies**;
	- **Send it to all threads of the target process**;
	- **Send it to the subset of threads that do not block that signal**;
	- **Designate a specific thread to receive and handle all signals**.
	The solution to adopt depends on the application. Typically, the OS delivers the signal to the first thread that does not block it, so that it is handled only once.

For model thread systems like **many-to-one**, **many-to-many** and **two-level** we need to introduce an intermediate data structure between user-level threads and kernel threads: **lightweight process**(**LWP**) which separates the two of them while storing management data.
From the kernel's perspective, an LWP is seen as a **process** and, as such, is scheduled, with the particularity that all LWP belonging to a process share the same address space. From the user-level library's perspective, an LWP appears as a **virtual processor** on which to schedule the execution of user threads based on some algorithm. Additionally, the library can vary the number of kernel-level threads (and therefore LWPs) to always have a certain number of threads running.

----
# Scheduling

In multitasking systems, **scheduling** is responsible for **managing the rotation of processes on the processor according to specific policies**. The CPU scheduler orders the list of ready processes, putting at the top the one that the dispatcher will send for execution. It is important that it is efficient and really fast, as it is called very frequently and one wants to minimize the management overhead. There are 2 other levels of scheduling (**long-term** and **medium-term**), which are not discussed.

Scheduler activation can occur in two primary ways:
- **Without preemption** → the CPU is assigned to a process until it explicitly releases it or waits for an event or peripheral; then control will return to the scheduler. In this case, activation is synchronous with the process' computational evolution;
  
- **With preemption** → the system can force the release of the CPU assigned to a process. If this happens, activation is asynchronous with the process' computation, and if it shares resources with others, it must use synchronization techniques to avoid inconsistencies.

The selection of a scheduling algorithm depends on the criteria you want to optimize. These criteria include:
- **Processor utilization** → how much of the processor's time is used;
- **Throughput** → the number of processes completed per unit of time;
- **Turnaround time** → the time from when a process enters the system to when it finishes execution, including time spent in various queues;
- **Waiting time** → the time a process spends in the ready queue;
- **Response time** → the time from when a request is made to when the first result is presented.

The goal is to maximize the first two and minimize the last three. Additionally, it is desirable to minimize the variance to make the system more predictable, and to evaluate the performance of a scheduling algorithm in a specific application context, various methods can be used:
1. **Analytical Evaluation** → one type of analytical evaluation is **deterministic modeling**, which mathematically defines an algorithm's performance based on a predetermined workload. It's a simple, fast, and precise method, but the results can't be generalized, and in reality, the workload isn't constant;
   
2. **Statistical Evaluation** → statistical evaluation provides results with an **associated degree of uncertainty**. One example is a network model based on queues: it uses distributions of CPU and I/O bursts and views the computer as a network of services, each with an associated queue. It then studies the parameters of these queues. This method is fairly fast but requires simplifications that may deviate too much from reality. 
   Another type of statistical evaluation is **simulation**: a software model of the system, as complete as possible, created and used to test the algorithm's behavior. The input data for the simulation are randomly generated CPU and I/O bursts or a trace of a real system. They are quite accurate but expensive to implement;
   
3. **Implementation:** For greater accuracy, you can proceed with implementation in the real system, using the system's monitoring tools to evaluate the algorithm's efficiency. It's the most accurate method but requires time to implement and cooperation from users. Additionally, to achieve the best performance at various times, it may be necessary to implement multiple scheduling algorithms, and you must decide whether to analyze individual cases or study average values.

We have various **scheduling policies**:
- **FCFS (First-Come, First-Served)** → manages the ready queue using a **FIFO policy**; processes obtain the CPU in the order they enter the queue. It is non-preemptive. This policy is easy to implement and very fast to execute, however, the average waiting time depends heavily on the arrival order of processes (**convoy effect**);
  
- **Priority** → allows association of a **priority index** to each process, defined internally (based on measurable parameters) or externally (by the user or system administrator), and then the queue is ordered based on these indices. 
  It can be implemented with or without preemption: in the first case, when a process becomes ready it requests scheduling, and if it has higher priority than the one currently running, it will replace the one already running; in the second case, scheduling is performed when the CPU is released. 
  A problem with these policies is the possibility of **starvation**, which is the indefinite blocking of low-priority processes because there are always higher-priority processes obtaining the CPU. This can be solved by introducing **aging**, which increasing the priority of a process as it waits. In this way, even low-priority processes will eventually be executed, after which the priority will return to its initial value.
  **SJF (Shortest Job First)** is a special case where the priority is the duration of a process, the lower the higher is the priority. It minimizes the average waiting time, but it is necessary to estimate the burst duration based on the past history, and the estimate may not be accurate;
  
- **Round Robin** → this is the typical policy for **time-sharing systems** and allows the CPU time to be distributed evenly among $n$ processes so that each one gets $1/n$ of it. It is **preemptive**. 
  It is implemented by setting a timer to generate an **interrupt** after the timer reaches the end each time the CPU is assigned to a process, and if the process does not release it by the end of the time, it is preempted, returns to the queue, and the scheduler chooses a new process to execute. 
  If the rotation is fast enough, it creates the **illusion of parallel execution**; and the **execution speed** of the processes depends on the **number of processes** in the queue, while the **turnaround time** depends on the timer. The problem is deciding on its **duration**: a longer timer reduces the overhead of management but also the illusion of parallelism, while a shorter timer increases the illusion of parallelism but also the overhead of management. Ideally, you want about 80% of the CPU bursts to be exhausted within a single timer;

- **Multilevel Queue** → if it is possible to group processes into **homogeneous types**, the ready queue can be split into multiple queues, one for each type, so that the most suitable scheduling algorithm can be chosen for each type of process. The queues are then scheduled with a dedicated algorithm. 
  Processes are permanently assigned to a queue, however, their "behavior" can vary over time, and therefore the most suitable queue as well.
  The **feedback variant** enables **dynamic migration of processes** between queues, employing policies for promotion, demotion, and allocation. This ensures that each process is consistently assigned to the queue that optimally aligns with its current workload characteristics. While offering the highest efficiency, it also presents the greatest complexity.

----

# Synchronization between processes

When multiple processes access the same physical or logical resource at the same time, we will have **concurrency**.
If the outcome of operations depends on the order in which they are executed, we have a **race condition**, which can cause inconsistency in the resource; so a portion of code that causes race conditions if executed concurrently is called a **critical section**.
This must be avoided with **synchronization mechanisms** and **policies**, and a solution to the critical section problem must satisfy 3 conditions:
- **Mutual exclusion** → only one process at a time can execute its critical section;
- **Progress** → competition to enter the critical section must occur among processes that are not in their critical section, and competition must not last indefinitely;
- **Bounded waiting** → a process must not be forced to wait indefinitely to enter its critical section.

Let's now look at some solutions:

- **Turn variables** → this is an **instruction-level approach**: we have a turn variable, which is a **variable shared among processes that establishes which one can use a resource**. To explain this in practice we will use an example for 2 processes/threads: 2 flags are used, initialized to false, which represent when a process is or wants to enter its critical section; and a turn variable, initialized to one of the 2 processes.
  When a process wants to enter its critical section, it will do this:
	  `turn variable initialized the other process ← true`;
	  `flag variable of this process ← true`;
  It will wait if and only if both of this are:
	   `turn variable initialized the other process ← true`;
	   `flag variable of the other process ← true`;

  Thanks to the **double condition**, if both attempt to enter concurrently, only one of the 2 will succeed, depending on the final value assumed by the turn variable, which decides who will enter first. In this way, the 3 conditions are met and the critical section problem is solved. This method scales poorly as the number of processes increases: it is necessary to modify the code or write code that adapts.

- **Lock variables:** This is another instruction-level approach where the resource itself has a variable that indicates whether it is available (0) or in use (1), basically the **lock variable**. This method is **independent of the number of processes**. 
  When a process wants to use the resource, it must obtain a lock in this way: 
	1. Disable interrupts;
	2. Read the lock variable;
	3. If it is 0, set it to 1, re-enable interrupts and use the resource; otherwise enable interrupts and wait. 
  Disabling/enabling interrupts makes the sequence **atomic**, avoiding a race condition if multiple processes try to obtain the lock simultaneously. Many processors provide hardware support, the **TEST-AND-SET instruction**, which performs exactly this sequence of instructions in a single hardware instruction, which by definition is indivisible. Upon release, the lock is reset to 0.

- **Binary and General Semaphores** → a semaphore is a **data structure managed by the OS** that, in its binary form, can hold two values: 1 (resource available) or 0 (resource occupied).
  The system provides two operations for using a semaphore:
	- **Acquire**→ used to request a resource by checking the semaphore's value: if it's 0, the process is blocked until the resource becomes available; otherwise, the value is set to 0, and the process can use the resource.
	- **Release** → used to release a resource. The semaphore's value is incremented to 1, indicating that the resource is now available.
  This mechanism is similar to lock variables but is managed by the operating system.

  General semaphores operate similarly, allowing for multiple instances of a resource. They can hold any $\geq 0$ integer value, initially set to the number of available instances. The operations are:
	- **Acquire** → checks the semaphore's value. If it's 0, the process is blocked. Otherwise, the value is decremented by 1, and the process proceeds;
	- **Release** → the semaphore's value is incremented.

  A potential issue arises with the **busy-waiting implementation** where a process continuously checks the semaphore's value until it becomes available, and ends up wasting CPU cycles during long waits.
  To address this we will create an associated queue to the semaphore: the acquire operation will insert the processes in the queue while the release operation can then remove a process from the queue (if one exists). This minimizes busy waiting, although synchronization is still required for accessing the queue.
  Despite these improvements, semaphores can lead to **deadlocks**, where processes are indefinitely blocked waiting for resources held by other processes. Moreover, the correct usage of semaphores relies heavily on the programmer, who can introduce errors leading to violations of mutual exclusion or starvation conditions.

- **Monitors** → are an abstract data type that, in addition to data, possess **methods** that are executed with **mutual exclusion**, managed by the **compiler** of the programming language, which will use the correct system calls. Therefore, the programmer is no longer responsible for their correct usage, but a programmer can create a monitor suited to their needs by defining variables of type **condition**, on which a process Q can wait for a condition to occur with the wait operation.
  Subsequently, a process P can execute the signal operation on a condition, which will awaken a process Q from the queue. At this point, P can wait until Q leaves the monitor or waits for a condition (signal and wait), or vice versa (signal and continue), or it can leave the monitor immediately.

-----

# Inter-process communication

Processes that share information and perform concurrent computations toward a common goal are called **cooperating processes**. The OS supports this through **IPC** (Inter-Process Communication) and **synchronization mechanisms**. Communication involves a **sender** (P), a **receiver** (Q), and a **channel**.

Every **IPC** method has its own unique characteristics. The choice of which method to use in an application depends on several factors:

- **Amount of information to be transmitted**;
- **Execution speed**;
- **Scalability** → whether the chosen method allows a variable number of processes to communicate or not;
- **Ease of use** in applications;
- **Homogeneity of communications** → it is preferable to avoid using multiple IPC methods in the same application as to avoid common errors;
- **Integration** into the programming language, guaranteeing portability;
- **Reliability**;
- **Security**;
- **Protection**;

There are two main types of IPC:
- **Direct communication** → communicating processes know each other's IDs, and requires both processes to be active for communication to occur;
- **Indirect communication** → processes only know where to retrieve/deposit information.

Now let's see methods for **direct communication**:

1. **Shared memory** → a region of memory can be shared among communicating processes, and there are two ways to achieve it:
	- The OS **copies** **the shared memory area between communicating processes**. In this way, processes can communicate while remaining physically separated. However, this method involves a waste of time and memory due to the copying process;
	- The OS **maps** **a portion of the logical address space of the processes onto the same physical memory area**, which is therefore physically shared. This technique is much faster than the previous one and does not waste memory. Processes remain logically separated and protected by the OS.
   Although it is a simple and fast system, it requires the programmer to use synchronization mechanisms because processes can access the same data concurrently, potentially leading to conflicts.

2. **Sharing messages** → information is exchanged between processes through **messages** managed by the OS. These messages can have fixed or variable lengths and contain the **information to be sent**, the **sender and recipient** **IDs**, and any necessary management information. 
   Messages are stored in **buffers** provided by the OS for each pair of communicating processes or for general use by all the processes in communication. The OS manages these buffers, not the programmer, and there is no shared memory involved.
   The quantity of buffers can be:
	   - **Unlimited** → P can theoretically deposit an infinite number of messages. Q can receive them at any time. This is known as **asynchronous communication**;
	   - **Limited** → P can deposit messages until there are free buffers, then it blocks until Q frees some. This is called **buffered communication**;
	   - **None** → P cannot deposit any messages and remains blocked until Q is ready to receive. This is called **synchronous communication**.
   
   The functions provided by the OS are:
	- **Send** → sends a message to the specified process, depositing it in a free buffer. If there are no free buffers, P is blocked until it can send messages again;
	- **Receive** → receives a message from a specified process (**symmetric case**) or from any process that has sent a message to Q (**asymmetric case**). If there are no messages to receive, Q is blocked until there are.
   There are also conditional versions of these functions that return an error code instead of blocking the process.
   The queue of waiting messages for a process can be scheduled.

Now the methods for **indirect communication**:

1. **Mailbox** → a data structure provided by the OS where processes can send and receive messages, and is identified by a **unique ID**. The capacity of a mailbox can be **unlimited**, **limited** or **nonexistent**. 
   Messages have a fixed or variable size and contain the sender's ID, the destination mailbox ID, the information to be sent, and any necessary management information.
   The OS provides the following functions:
	- **Create** → creates a new mailbox;
	- **Send and conditional send** → deposits a message into the specified mailbox;
	- **Receive and conditional receive** → receives a message from the specified mailbox;
	- **Delete** → deletes a mailbox.   
   The mailbox is located in the memory of the OS and it manages access to it, ensuring **synchronization**. Typically, it is owned by the OS and can be used by all processes. However, it is possible to assign a mailbox to a specific process, which can then decide its permissions; and when a process terminates, the mailbox can be de-allocated with it or it can return to the ownership of the OS. Messages in the mailbox can be scheduled.

   Mailboxes are particularly useful for:
	- **Communication many to 1** → a service process that receives and executes the clients' requests;
	- **Communication 1 to many** → a client process sends requests to any available service process. The first one to receive a request executes it;
	- **Many to many communication** → multiple client processes send requests to any available service process. The first one to receive a request executes it. However, each communication still involves only 2 processes: P and Q.

2. **Communication via files and pipes** → **files** are useful for exchanging **large amounts of data**: messages are deposited and retrieved from a file on mass storage, which is relatively slow and accesses are synchronized by the **file system**. The order of messages depends on the writing process. 
   A **pipe** can be seen as a **file stored in the OS's memory**. The functions used to work with them are the same as those used for files, and synchronization occurs in the same way, but the order of messages will be **FIFO**. 
   In both files and pipes, messages can be fixed or variable length and contain the sender's ID, the information to be transmitted, and any management information.

3. **Communication via sockets** → sockets are used for **network communications**, but can also be used for IPC, even between **processes on separate machines**: one or more senders connect to the receiver by specifying the address and port on which it is listening. 
   Once connected, communication will take place: a socket provides a bidirectional channel, with the ends on the two communicating machines. Messages are of fixed or variable size and only contain the information to be exchanged. They are in FIFO order.

-------
# Deadlock

Deadlock (or stalemate) is a condition where each process in a group is blocked waiting for an event that can only be generated by another process in the group, typically the release of physical or logical resources.
For a deadlock to occur, the following four conditions must hold simultaneously:
- **Mutual exclusion** → at least one resource is used in a mutually exclusive manner by the processes. (Mutual exclusion means that only one process can use a resource at a time);
- **Hold and wait** → a process holding some resources is waiting to acquire additional resources held by other processes;
- **No preemption** → a resource cannot be forcibly removed from a process holding it;
- **Circular wait** → a set of waiting processes exists such that each process is waiting for a resource held by the next process in the set.

Let's now see how to address this problem:

- **Ignoring deadlock** → in most systems, deadlock is a rare condition, so time and resources can be saved by ignoring it and resetting the system if it occurs.

- **Preventing deadlock** → this involves invalidating one of the four conditions that cause deadlock by imposing constraints on requests. However, this can lead to under-utilization of resources and is inconvenient for programmers. Here is how we can invalidate one of them:
    1. **Mutual exclusion** → this condition can be invalidated for resources that are inherently shareable, like read-only files. In general, however, this is not possible because not all resources are shareable;
       
    2. **Hold and wait** → this condition can be invalidated by requiring processes to request all resources at the beginning, or by requiring a process to release all its resources before making a request and then request them all in a block. In both cases, this leads to under-utilization of resources and potential starvation; moreover, in the second case, programmers are required to release resources in a consistent state since they may be assigned to other processes;
       
    3. **No preemption** → this condition can be invalidated in two ways: when a process is put on hold for a request, all its resources are released prematurely, and thus it waits for the request plus the resources it already had; alternatively, only the resources of waiting processes requested by a running process can be released prematurely. The consistency problem described earlier remains, however, it is the OS that releases the resources, and not the programmer. This method works well with resources whose state can be saved and restored;
       
    4. **Circular wait** → this condition can be invalidated by imposing an ordering on resources, by defining a function that assigns an integer to each resource, and imposing this protocol: when a process requests resources of order $i$, it can only obtain them if they are available and it does not already possess resources of order $\ge i$. If it does, it must release them and request them in the correct order (with all the related problems). In this way, circular wait cannot occur, but the function must be defined based on the usual order of resource usage, and it may not be easy.

- **Avoiding deadlock** → deadlock avoidance methods do not restrict how requests are made but ask the process for additional information about resource usage and use it to verify if the resulting state from a request is safe. The problem is that this information may not be known in advance.
  A state is said to be **safe** for $n$ processes if there exists at least one safe sequence of $n$ processes for which the requests of each $P_i$ in the sequence can be satisfied with the currently available resources plus those held by processes $P_j$, with $j<i$.
  If such a sequence does not exist, the state is said to be unsafe, and while a safe state guarantees the absence of deadlock; an unsafe state does not necessarily mean deadlock.
  
  In the case of **single instances of resources**, a **variant of the resource allocation graph** can be used. This is an **oriented graph** that has **processes and resources** as **nodes**, and has **assignment arcs** (from a resource to a process) and **request arcs** (from a process to a resource); while the variant introduces **reservation arcs** (from a process to a resource) instead of request arcs.
  At the beginning, a process must say which resources it will need. This creates a '**reservation**' for those resources. When the process asks for a resource and it's available, we check if giving the resource through an assignment arc to the process would create a **circular waiting** situation. If it does, it means the system might get stuck, so the process has to wait.
  
  In case of **multiple instances**, the more complex **banker's algorithm** can be used.
  It uses several data structures to maintain information about processes ($n$ = number of processes, $m$ = number of resources):
	 **Available** → an array of m elements containing the number of available instances for each resource;
	- **Allocation** → an $n \times m$ matrix containing the allocations for each process;
	- **Max** → an $n \times m$ matrix containing the maximum resources that each process can use;
	- **Need** → an $n \times m$ matrix containing the maximum resources that each process can still request.
  When a process makes a request, it is checked if it is **valid** (<= need of that process) and if it is **satisfiable** (<= available). Then, the data structures are modified to simulate the assignment of the resource, and the **safe state checking algorithm** is executed.
  It will look for all processes whose needs can be satisfied with the current availability plus the resources assigned to the previously analyzed processes, which will be released when finished. 
  If some processes remain undiscovered, the state is unsafe, the old values of the data structures are restored, and the process must wait. Otherwise, the process obtains the requested resources.

- **Deadlock detection and resolution** → approach that allows for increased resource utilization while letting deadlocks occur and use algorithms to detect and resolve them. 
  In case of **single instances** of resources, the **wait-for graph** can be used: a variant of the allocation graph with the resource nodes collapsed to show the waits between processes. If there are cycles in the graph, those processes are involved in a deadlock. 
  In case of **multiple instances**, a similar algorithm to the verification algorithm of the banker's algorithm can be used. It uses the **available** and **alloccation** **data structures**, plus the **request matrix** $n \times m$ which contains the requests of each process. 
  All processes whose requests can be satisfied with the current availability plus the resources held by the previously analyzed processes - assuming they terminate and release them, if not, a deadlock will be detected - are searched. 
  If some processes remain undiscovered, they are involved in a deadlock. To resolve it, all involved processes can be terminated, but the results of long computations may be lost, or they can be terminated one by one based on some criteria until the deadlock is resolved, as to avoid always terminating the same ones causing starvation. The detection algorithm can be executed every $n$ seconds, or when CPU usage falls below a certain threshold (if there is a deadlock, more and more processes will be involved so it will continue to decrease).