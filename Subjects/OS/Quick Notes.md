# OS Quick Notes

[Notes](https://drive.google.com/file/d/1FAxjhyIlsGGouIyCPyR3xqKVgU7mhEmQ/view)

<br>

## @ Basics of OS
An operating system is a piece of software that manages all the resources of a computer system, both hardware and software, and provides an environment in which the user can execute his/her programs in a convenient and efficient manner.

<br>

#### 1. Kernel 
A kernel is that part of the operating system which interacts directly with the hardware and performs the most crucial tasks. 

<br>

#### 2. Shell
A shell, also known as a command interpreter, is that part of the operating system that receives commands from the users and gets them executed.

<br>

#### 3. System Call
A system call is a mechanism using which a user program can request a service from the kernel. User programs typically do not have permission to perform operations like accessing I/O devices and communicating other programs. A user program invokes system calls when it requires such services. System calls provide an interface between a program and the operating system.

Eg: fork, exec, getpid, getppid, wait, exit.

<br>

#### 4. User Mode Vs Kernel Mode
1. In it’s life span a process executes in user mode and kernel mode. The User mode is normal mode where the process has limited access. While the Kernel mode is the privileged mode where the process has unrestricted access to system resources like hardware, memory, etc.

2. Any crash in kernel mode brings down the whole system. But any crash in user mode brings down the faulty process only.

3. The kernel provides System Call Interface (SCI), which are the entry points for kernel. System Calls are the only way through which a process can go into kernel mode from user mode. Kernel space switching is achieved by Software Interrupt, which changes the processor mode and jump the CPU execution into interrupt handler, which executes corresponding System Call routine.

[Explaination](https://www.geeksforgeeks.org/user-mode-and-kernel-mode-switching/)

<br>

#### 5. MicroKernel
1. In microkernel, user services and kernel services are kept in separate address space.
2. If a service crashes, it does not effect on working of microkernel.
3. Smaller in size and slow execution.
4. The advantage to a microkernel is that any failed service can be easily restarted, for instance, there is no kernel halt if the root file system throws an abort.
5. Eg: QNX, Symbian, Mac OS X

<br>

#### 6. Monolithic Kernel
1. In monolithic kernel, both user services and kernel services are kept in the same address space.
2. If a service crashes, the whole system crashes. 
3. Larger then microkernel and fast execution.
4. Linux, Microsoft Windows (95,98), Solaris

[Monolithic Vs Microkernel Explained](https://techdifferences.com/difference-between-microkernel-and-monolithic-kernel.html)

<br>

#### 7. Booting
Booting is the process of starting the computer and loading the kernel. When a computer is turned on, the power-on self-tests (POST) are performed. Then the bootstrap loader, which resides in the ROM, is executed. Then bootstrap loader loads the kernel.

<br>

#### 8. Power on self test (POST) 
It is a set of routines performed by firmware or software immediately after a computer is powered on, to determine if the hardware is working as expected. Roles of POST are:
1. Initialize BIOS.
2. Identify, organize, and select which devices are available for booting.
3. Verify CPU registers.
4. Verify some basic components like DMA, timer, interrupt controller.

<br>

#### 9. Device drivers
The role of drivers is to provide an abstraction of the hardware so applications can use it through the OS API instead of having to know specific details. It also allows for sharing the same piece of hardware among many applications simultaneously.

[Explaination](https://www.geeksforgeeks.org/device-driver-and-its-purpose/)

<br>

#### 10. Virtual Device Driver (VDD)
virtual device drivers manages the virtual devices. They controls/manages the data flow from different application used by different users to the same hardware.

<br>



## @ Process Scheduling and Management

Note: Kernel is the first process to be created, is the ancestor of all other processes and is at the root of the process tree.

<br>

#### 1. Zombie process
A zombie process is a process that has terminated but its PCB still exists because its parent has not yet accepted its return value.

<br>

#### 2. Difference between process and thread
1. processes are typically independent while threads exist as parts of a process
2. processes carry considerably more state information than threads, whereas multiple threads within a process share process state as well as memory and other resources
3. processes have separate address spaces, whereas threads share their address space
4. processes interact only through system-provided inter-process communication mechanisms
5. context switching between threads in the same process is typically faster than context switching between processes

<br>

#### 3. Dispatcher
A dispatcher is the module of the operating system that gives control of the CPU to the process selected by the CPU scheduler. Dispatch latency is the time taken to stop a process and start another. Dispatch latency is a pure overhead.

<br>

#### 4. Priority inversion
A low-priority process gets the priority of a high-priority process waiting for it.

<br>

#### 5. Scheduling Chart

![img](https://github.com/naman14310/Interview_Prep/blob/main/Subjects/OS/scheduling%20chart%20os.png)

<br>

#### 6. Independent vs Co-operative process
An independent process is not affected by the execution of other processes while a co-operating process can be affected by other executing processes.

<br>



## @ Process Synchronisation

**Race condition** is a situation where several processes access and manipulate the same data concurrently and the outcome of the execution depends on the particular order in which the accesses take place. To avoid such situations, it must be ensured that only one process can manipulate the data at a
given time. This can be done by process synchronization.

<br>

#### 1. Critical Section
1. Each process has a section of code, called the critical section, in which the process access shared resources, changes common variables and files.
2. The problem is to ensure that when one process is executing in its critical section then no other process can execute its own critical section
3. The critical section is preceded by an *entry section* in which a process seeks permission from other processes. The critical section is followed by an *exit section.*

<br>

#### 2. Condition for Synchronisation mechanisms
A solution to the critical section problem must satisfy the following properties:
1. Mutual exclusion
2. Progress 
3. Bounded waiting
4. Architectural Neutrality

<br>

#### 3. Semaphores
A semaphore is an integer variable that, apart from initialization, is accessed only through two atomic operations called wait() and signal().

```cpp
wait() is used for acquiring lock

wait(S){
    while(S<=0);    // --> Busy Waiting
    S--;
}


signal() is used for releasing lock

signal(S){
    S++;
}
```

<br>

#### 4. Binary Semaphores (Mutexes)
It is used to implement solution of critical section problem with multiple processes. Initialized to 1.

```cpp
struct semaphore {
    enum value(0, 1);                 // --> Since it is Binary Semaphore, it'll only contains 0 or 1
  
    /* q contains all Process Control Blocks (PCBs) corresponding to processes got blocked while performing wait() operation */
    
    queue<process> q;
}; 


wait(semaphore s){
    if (s.value == 1) {
        s.value = 0;
    }
    else {
        q.push(P);                  // --> add the process to the waiting queue
        sleep();
    }
}


signal(Semaphore s){
    if (s.q is empty()) {
        s.value = 1;
    }
    else {  
        Process p=q.pop();         // --> select a process from waiting queue and give entry to critical section
        wakeup(p);
    }
}
```

<br>

#### 5. Counting semaphore
It is used to control access to a resource that has multiple instances. Initialized to n, where n is the number of resources.

```cpp
struct Semaphore {
    int value;                    // --> Since it is counting semaphore, It will contain any integer val depends on number of resources 
    Queue<process> q;
};


wait(Semaphore s){
    s.value = s.value - 1;
    if (s.value < 0) {
        q.push(p);               // --> add process to queue here p is a process which is currently executing
        block();
    }
    else
        return;
}
  
  
signal(Semaphore s){
    s.value = s.value + 1;
    if (s.value <= 0) {
        Process p=q.pop();      // --> remove process p from queue
        wakeup(p);
    }
    else
        return;
}
```

<br>

#### 6. Do Semaphore suffers from deadlock
Semaphores may lead to indefinite wait (starvation) and deadlocks.

```cpp
Example of two prcoess, where use of semaphores leads to deadlock

   P0                            P1
   
 wait (S);                    wait (Q);
 wait (Q);                    wait (S);
  ...                           ...
signal (S);                  signal (Q);
signal (Q);                  signal (S);
```

<br>

#### 7. Inter Process Communication (IPC)
Inter-process communication (IPC) is a mechanism that allows processes to communicate with each other and synchronize their actions. rocesses can communicate with each other through:
1. Shared Memory
2. Message passing

![img](https://forns.lmu.build/assets/images/spring-2018/cmsi-387/week-8/ipc-1.png)

**Shared Memory Method**

Processes can use shared memory for extracting information as a record from another process as well as for delivering any specific information to other processes. Eg: Producer-Consumer problem 

**Message Passing Method**

If two processes p1 and p2 want to communicate with each other, they proceed as follows:
1. Establish a communication link (if a link already exists, no need to establish it again.)
2. Start exchanging messages using basic primitives.

We need at least two primitives: 

– send(message, destination) or send(message) 

– receive(message, host) or receive(message)

[Detailed Explaination](https://www.geeksforgeeks.org/inter-process-communication-ipc/)

 

