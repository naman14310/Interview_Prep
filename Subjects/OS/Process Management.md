# Process Management

Note: Kernel is the first process to be created, is the ancestor of all other processes and is at the root of the process tree.

<br>

## @ Process Basics & Scheduling 

### 1. How does a process look like in memory ?
When a program is created then it is just some pieces of Bytes which is stored in Hard Disk as a passive entity. When a program is loaded in memory it becomes an active entity known as **Process**.

![img](https://www.tutorialspoint.com/assets/questions/media/29467/1.jpg)

**Code (or Text)**

This section of memory contains the executable instructions of a program. It is read-only segment.

**Data**

It contains the global and static variables that are initialized by the programmer prior to the execution of a program. This segment is not read-only, as the value of the variables can be changed at the runtime.

**Stack**

It contains temporary data i.e. function parameters, return addresses, and local variables. Stack grows in opposite direction of heap for avoiding overlapping problem. This section is committed to store all the data needed by a function call in a program. A stack pointer register keeps the tracks of the top of the stack i.e., how much of the stack area using by the current process.

**Heap**

To allocate memory for variables whose size cannot be statically determined by the compiler before program execution, requested by the programmer, there is a requirement of dynamic allocation of memory which is done in heap segment. It can be only determined at run-time. It is managed via system calls to malloc, calloc, free, delete etc.

<br>

[Working Of Stack Vs Heap Memory](https://www.youtube.com/watch?v=PdvGEI-P3-M)

<br>


### 2. Process States

![img](https://media.geeksforgeeks.org/wp-content/uploads/20190604122001/states_modified.png)

<br>

### 3. CPU and IO Bound Processes
If the process is intensive in terms of CPU operations then it is called CPU bound process. Similarly, If the process is intensive in terms of I/O operations then it is called IO bound process. For better performance, we should choose proper mix of CPU bound and I/O bound processes.

<br>

### 4. Zombie process (or Defunct process)
A zombie process is a process that has terminated but its PCB still exists because its parent has not yet accepted its return value. Those Processes has completed their execution by exit() system call but still has an entry in Process Table. It is a process in terminated state.

**Maximum number of Zombie process a system can handle?**

It will depend upon system configuration and strength.

<br>

### 5. PCB (Process Control Block)
PCB is used to track the process’s execution status. When process makes a transition from one state to another, OS must update information in the process’s PCB. It contains following info:
1. Process Number
2. Process State
3. Program Counter (stores address of next instruction)
4. Register
5. Memory info
6. Open file lists

<br>

### 6. Process Scheduling Queues
OS maintains all PCBs in Process Scheduling Queues. 
1. Job Queue: keeps track of all newly created process.
2. Ready Queue: Keeps track of all process residing in main memory and are ready and waiting to execute.
3. Device Queue: Every device has its own device queue.

<br>

### 7. Types of schedulers

**Long term** – performance 

Makes a decision about how many processes while reside in the ready state, this **decides the degree of multiprogramming**.

<br>

**Short term** – Context switching time 

Short term scheduler will **decide which process to be executed** next and then it will call dispatcher. A dispatcher is a software that moves process from ready to run and vice versa. In other words, it is context switching.

<br>

**Medium term** – Swapping time 

Medium term scheduler is **used for swapping** i.e. moving the process from main memory to secondary and vice versa.

<br>

### 8. Dispatcher
A dispatcher is the module of the operating system that gives control of the CPU to the process selected by the CPU scheduler. **Dispatch latency** is the time taken to stop a process and start another. Dispatch latency is a pure overhead.

<br>

### 9. What happens during Context Switch ?
During Context Switch, the state of current running process is stored in its PCB. After this, the state of the process to run next is loaded from its own PCB and used to set the PC, registers, etc. 

![img](https://www.tutorialspoint.com/operating_system/images/context_switch.jpg)

Context switches are computationally intensive since register and memory state must be saved and restored. To avoid the amount of context switching time, some hardware systems employ two or more sets of processor registers.

<br>

### 10. Scheduling Chart

![img](https://github.com/naman14310/Interview_Prep/blob/main/Subjects/OS/scheduling%20chart%20os.png)

<br>

### 11. Which is the Best Scheduling Algorithm ?
Every scheduling algorithm has a type of a situation where it is the best choice. Let's look at different such situations:

**Situation 1: incoming processes are short with no specific priority**

In this case, **FCFS** works best when compared to SJF and RR because the processes are short which means that no process will wait for a longer time. When each process is executed one by one, every process will be executed eventually.

<br>

**Situation 2: If processes are a mix of long and short processes and the task will only be completed if all the processes are executed successfully in a given time.**

**Round Robin scheduling** works efficiently here because it does not cause starvation and also gives equal time quantum for each process.

<br>

**Situation 3: The processes are a mix of user based and kernel based processes.**

**Priority based scheduling** works efficiently in this case because generally kernel based processes have higher priority when compared to user based processes.

<br>

### 12. Which scheduling algorithm is used in Linux ?
Linux uses a **Completely Fair Scheduling (CFS) algorithm**, which is an implementation of weighted fair queueing (WFQ). There is a fixed time interval during which each thread in the system must run at least once.

<br>

### 13. Which scheduling algorithm is used in Windows?
Windows NT/XP/Vista uses a **multilevel feedback queue**, a combination of fixed-priority preemptive scheduling, round-robin, and first in, first out algorithms.

<br>

### 14. Which scheduling algorithm is used in Android?
Android operating system uses O(1) scheduling algorithm as it is based on Linux Kernel 2.6.

<br>

### 15. Independent vs Co-operative process
An independent process is not affected by the execution of other processes while a co-operating process can be affected by other executing processes.

<br>

### 16. Inter Process Communication (IPC)
Inter-process communication (IPC) is a mechanism that allows processes to communicate with each other and synchronize their actions. Processes can communicate with each other through:
1. Shared Memory
2. Message passing

![img](https://forns.lmu.build/assets/images/spring-2018/cmsi-387/week-8/ipc-1.png)

**Shared Memory Method**

Processes can use shared memory for extracting information as a record from another process as well as for delivering any specific information to other processes. Eg: Producer-Consumer problem 

<br>

**Message Passing Method**

If two processes p1 and p2 want to communicate with each other, they proceed as follows:
1. Establish a communication link (if a link already exists, no need to establish it again.)
2. Start exchanging messages using basic primitives.

We need at least two primitives: 

– send(message, destination) or send(message) 

– receive(message, host) or receive(message)

[Detailed Explaination](https://www.geeksforgeeks.org/inter-process-communication-ipc/)

<br>



## @ Multhreading Concepts

[Theory](https://www.studytonight.com/operating-system/multithreading)

[Multithreading in C++ : Blog1](https://www.educative.io/blog/modern-multithreading-and-concurrency-in-cpp)

[Multithreading in C++ : Blog2](https://medium.com/swlh/c-multithreading-and-concurrency-introduction-f640ce986fa7)

<br>

### 1. What are Threads?
A thread is a path of execution within a process. A process can contain multiple threads. A thread consists of its own program counter (keeps track of which instruction to execute next), a stack (contains the history of execution) , and a set of registers (hold its current working variables). 

**Why Threads ?**

Threads are a popular way to improve the performance of an application through parallelism. In the implementation of network servers and web servers threads have been successfully used. In a browser, multiple tabs can be different threads.

<br>

### 2. Which segments do Threads share?
**Everything except stack and registers**

A Thread shares same address space, code section, data section and same global variables (heap memory), same files (file discriptors)

```
           Process   Thread

   Stack   private   private
   Heap    private   shared
   Data    private   shared
   Code    private   shared
```

<br>

### 3. Can Multiple threads can run simultaneously ?
No, The CPU switches rapidly back and forth among the threads giving the illusion that the threads are running in parallel.

<br>

### 4. What are advantages of using threads ?
1. Better utilization of resources.
2. Creating and managing threads becomes easier.
3. Context Switching is smooth.
4. Responsiveness
5. Communication between multiple threads is easier.

<br>

### 5. Difference between Process and Thread
1. Processes are typically independent while threads exist as parts of a process.
2. Processes have separate address spaces, whereas threads share their address space
3. Context switching between threads in the same process is typically faster than context switching between processes
4. If a process gets blocked then the remaining processes can continue their execution, while if a user-level thread gets blocked, all of its peer threads also get blocked.

<br>

### 6. User Threads Vs Kernel Threads

**User Threads**
1. These are the threads that application programmers use in their programs.
2. If one user-level thread performs a blocking operation then the entire process will be blocked.
3. Context switch requires no hardware support.


**Kernel Threads**
1. These threads are implemented within OS Kernels.
2. If one kernel thread performs a blocking operation then another thread can continue the execution.
3. Context switch requires hardware support.

<br>

### 7. Concurrency vs Parallelism
In Parallelism we execute different processes exactly at the same time on multiple processors, while Concurrency means we are executing multiple programs on single processors by switching rapidly among them giving an illusion of parallel execution.

![img](https://www.educative.io/cdn-cgi/image/f=auto,fit=contain,w=1800/api/page/6705609052258304/image/download/5161243770880000)

<br>

### 8. Thread Libraries
Thread libraries provide APIs for the creation and management of threads. Thread libraries may be implemented either in user space or in kernel space. Some common libs are:
1. **POSIX or Ptheads** may be provided as either a user or kernel library, as an extension to the POSIX standard.
2. **Win32 threads** are provided as a kernel-level library on Windows systems.

<br>

### 9. Pthreads Vs Threads
If you want to run code on many platforms, go for Posix Threads. They are available almost everywhere and are quite mature. On the other hand if you only use Linux/gcc, thread is perfectly fine, it has a higher abstraction level, a really good interface and plays nicely with other C++11 classes.

<br>

### 10. What are some best examples of multithreaded applications? 
1. **Web Browsers** - A web browser can download any number of files and web pages (multiple tabs) at the same time and still lets you continue browsing.
2. **Web Servers** - A threaded web server handles each request with a new thread. There is a thread pool and every time a new request comes in, it is assigned to a thread from the thread pool.
3. **Text Editors & IDEs** - When you are typing in an editor, spell-checking, formatting of text and saving the text are done concurrently by multiple threads.
4. **VLC Media Player** - When you open the media player, you play a song and at the same time you kept on adding new songs to the list, displaying ads, searching for a new song.

<br>

### 11. Issues with Multithreading

**Thread Cancellation**

Thread cancellation means terminating a thread before it has finished working. There can be two approaches for this, one is **Asynchronous cancellation**, which terminates the target thread immediately. The other is **Deferred cancellation** allows the target thread to periodically check if it should be canceled.

**Signal Handling**

Signals are used in UNIX systems to notify a process that a particular event has occurred. Now in when a Multithreaded process receives a signal, to which thread it must be delivered? 

**fork() System Call**

fork() is a system call executed in the kernel through which a process creates a copy of itself. Now the problem in the Multithreaded process is, if one thread forks, will the entire process be copied or not?

**Security Issues**

Yes, there can be security issues because of the extensive sharing of resources between multiple threads.

<br>

### 12. Effect of Cores on Multithreading
Multicore refers to a computer or processor that has more than one logical CPU core, that can physically execute multiple instructions at the same time. Programs that support multithreading can use more than one core if available.

**Only one thread can run on a core at once. Different threads running is actually just threads jumping onto the CPU and running for short periods of time, then being switched out with other threads which also need to run.**

Note: The CPU cores mean the actual hardware component whereas threads refer to the virtual component which manages the tasks.

<br>

**Can we do multithreading on Single Core Processor ?**

Yes, even if we have only one core, we can still run multiple threads, and our OS will do its best to make sure that all running threads get their fair share of CPU time.

<br>

**Hyperthreading**

Hyper-threading was Intel’s first effort to bring parallel computation to end user’s PCs. A single CPU with hyper-threading appears as two logical CPUs for an operating system. When a "core" has two "threads" (like in many new Intel chips), it means that the core can run two parallel threads, as if there were two cores.

<br>

**Is there a benefit to running a parallelizable process on more threads than cores?**

If your threads don't do I/O, synchronization etc, 1 thread per core will get you the best performance. However that's very rare in real scenario. Adding more threads usually helps, but after some point, they cause performance degradation due to more context switches.

<br>

### 13. Multithreading in C++
Header: `#include <thread>`

Syntax for creating new thread: `thread t(callable_object, arg1, arg2, ..)` 

This creates a new thread of execution associated with t, which calls callable_object(arg1, arg2). The callable object (i.e. a function pointer, a lambda expression, the instance of a class with a function call operator) is immediately invoked by the new thread, with the (optionally) passed arguments. By default they are copied, if you want to pass by reference you have to warp the argument using std::ref(arg).

Note: a thread object is just movable, not copyable.

<br>

**Single Thread Implementation**

```cpp
#include<bits/stdc++.h>
using namespace std;

void print(int n, const string &str)  {  
    cout<<"Printing integer: "<<n<<endl;  
    cout<<"Printing string: " <<str<<endl;  
} 

int main() {
    thread t1(print, 10, "Educative.blog");            // --> will create new thread t
    t1.join();                                         // --> will join thread t to main thread
    return 0;
}
```

Here, join() will pause the main function’s thread until the specified thread, in this case t1, has finished its task. Without join() here, the main thread would finish its task before t1 would complete print, resulting in an error.

<br>

**MultiThreading Implementation**

```cpp
#include<bits/stdc++.h>
using namespace std;

void print(int n, const string &str)  {
    string msg = to_string(n) + " : " + str + "\n";
    cout<<msg;
}
 
int main() {
    vector<string> s = {"Educative.blog", "Educative", "courses", "are great"};
    vector<thread> threads;
 
    for(int i=0; i<s.size(); i++){
        threads.push_back(thread(print, i, s[i]));                // --> will create new threads and append them to thread vector
    }
    
    for(auto &th : threads)                                       // --> will iterate on every thread and join them to main thread
        th.join();
    
    return 0;
}
```

Here we used a for loop to initialize multiple threads, pass them the print function and arguments, which they then complete concurrently. This multithreading option would be faster one using only the main thread as more of the total CPU is being used.

<br>

### 14. Join() and Detach()
If the main thread exits, all the secondary threads still running suddenly terminate, without any possibility of recovery. To prevent this to happen, the parent thread has two options for each child:
1. Blocks and waits for the child termination, by invoking the **join()** method on the child.
2. Explicitly declaring that the child can continue its execution even after the parent’s exit, using the **detach()** method.

<br>

### 15. What is Thread Safe ?
Thread-safe code is code that will work even if many Threads are executing it simultaneously.

<br>

### 16. Why static variables are considered evil ?
1. Since statics live in one space, they are not by defualt thread safe. We need to use synchonisation mechanism to handle them.
2. We can't control statics in terms of creation and destruction.
3. Also they violates the principle of OOPS that data is encapsulated within objects.
4. Statics have a lifetime that matches the entire runtime of the program. This means, even once we are done using the class, the memory from all those static variables cannot be garbage collected.

<br>



## @ Process Synchronisation

**Race condition** is a situation where several processes access and manipulate the same data concurrently and the outcome of the execution depends on the particular order in which the accesses take place. To avoid such situations, it must be ensured that only one process can manipulate the data at a
given time. This can be done by process synchronization.

<br>

### 1. Critical Section
1. Each process has a section of code, called the critical section, in which the process access shared resources, changes common variables and files.
2. The problem is to ensure that when one process is executing in its critical section then no other process can execute its own critical section
3. The critical section is preceded by an *entry section* in which a process seeks permission from other processes. The critical section is followed by an *exit section.*

<br>

### 4. Priority inversion
A low-priority process gets the priority of a high-priority process waiting for it.

<br>

### 2. Condition for Synchronisation mechanisms
A solution to the critical section problem must satisfy the following properties:
1. Mutual exclusion
2. Progress 
3. Bounded waiting
4. Architectural Neutrality

<br>

### 3. Semaphores
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

### 4. Binary Semaphores (Mutexes)
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

### 5. Counting semaphore
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

### 6. Do Semaphore suffers from deadlock
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



### 8. Producer Consumer Problem (Bounded Buffer)
We have a buffer of fixed size. A producer can produce an item and can place in the buffer. A consumer can pick items and can consume them. We need to ensure that when a producer is placing an item in the buffer, then at the same time consumer should not consume any item. In this problem, buffer is the critical section. 

To solve this problem, we need two counting semaphores – Full and Empty. “Full” keeps track of number of items in the buffer at any given time and “Empty” keeps track of number of unoccupied slots. 

```cpp
Initialization of semaphores:

mutex = 1; 
Full = 0;               // --> Initially, all slots are empty. Thus full slots are 0 
Empty = n;              // --> All slots are empty initially 

/* --------------------------------------------------------------------------------------------------*/

Solution for Producer:

do {
    produce_item();

    wait(empty);
    wait(mutex);

    place_in_buffer();

    signal(mutex);
    signal(full);

} while(true)

/* --------------------------------------------------------------------------------------------------*/

Solution for Consumer:

do {

    wait(full);
    wait(mutex);

    remove_item_from_buffer();

    signal(mutex);
    signal(empty);

    consume_item();

} while(true)
```

<br>

### 9. Reader Writer Problem
Consider a situation where we have a file shared between many people. Then,
1. If one of the people tries editing the file, no other person should be reading or writing at the same time.
2. However if some person is reading the file, then others may read it at the same time.

<br>

**Solution when Reader has the Priority over Writer**

Here priority means, no reader should wait if the share is currently opened for reading. Three variables are used: mutex, wrt, readcnt to implement solution.


```cpp
/*
    mutex   : used to ensure mutual exclusion when readcnt is updated i.e. when any reader enters or exit from the critical section.
    wrt     : used to restrict access of writers if atleast 1 reader is present.
    readcnt : tells the number of processes performing read in the critical section, initially 0.
*/

semaphore mutex, wrt;           
int readcnt;  

/* ---------------------------------------------------------------------------------------*/

Writer process:

do {
    wait(wrt);                      // --> writer requests for critical section
    write();
    signal(wrt);                    // --> leaves the critical section

} while(true);

/* ---------------------------------------------------------------------------------------*/

Reader process:

do {
   wait(mutex);         // --> Reader wants to enter the critical section (mutex is used to ensure mutual exclusion on readcnt)
   
   readcnt++;           // --> The number of readers has now increased by 1                   

   /*   
        If there is atleast one reader in the critical section
        we will ensure that no writer can enter..
        thus we give preference to readers here
   */
   
   if (readcnt==1)     
      wait(wrt);                    

   signal(mutex);       // --> other readers can enter while this current reader is inside the critical section (hence release mutex) 


   reading();
   
   
   wait(mutex);         // --> a reader wants to leave, again we will apply acquire lock on mutex before changing readcnt value

   readcnt--;

   /* 
        If no reader is left in the critical section,
        release wrt lock so that writer can perform their actions
   */
    
   if (readcnt == 0) 
       signal(wrt);         

   signal(mutex);       // --> reader leaves and releasing mutex lock

} while(true);

```

<br>

### 10. Dinning Philosopher Problem
The Dining Philosopher Problem states that K philosophers seated around a circular table with one chopstick between each pair of philosophers. A philosopher may eat if he can pick up the two chopsticks adjacent to him. One chopstick may be picked up by any one of its adjacent followers but not both. 

```cpp
Semaphores:

chopsticks[5];      // --> all semaphores initialized to 1

Pi:

do { 
    wait (chopstick[i]);            // --> acquire lock on left chopstick
    wait (chopStick[(i+1)%5]);      // --> acquire lock on right chopstick
    
    eat()
    
    signal (chopstick[i]);          // --> release lock for left
    signal (chopstick[(i+1)%5]);    // --> release lock for right
    
} while (true);
```

Above system may suffer from deadlock if all philosopher pick there left chopstick at same instant. 

**Solution for Deadlock:** We make (n-1) philospher to pick left chopstick first and we will reverse the order for 1 philosopher so he will pick his right chopstick first.
