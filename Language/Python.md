# Pyhton Notes

### Q1. What is GIL ?
**Ans.** The Python Global Interpreter Lock or GIL, in simple words, is a mutex (or a lock) that allows only one thread to hold the control of the Python interpreter. 

In Python, the threads may be running on different processors, but they will only be running one at a time. Threading in python is not actually multithreading, it is coordinated multitasking. Using multiple threads to do above computation would make sense in other languages because we can take advantage of all the CPU cores, but in python it takes longer time. 

Therefore it’s not recommended to use multithreading for CPU intensive tasks in Python. If not for CPU intensive tasks, Python threads are helpful in dealing with blocking I/O operations including reading & writing files, interacting with networks, communicating with devices like displays etc. These tasks happen when Python make certain types of system calls. Tasks that spend much of their time waiting for external events are generally good candidates for threading.

[Video Explaination](https://youtu.be/f9q5m321iEU)

------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Q2. How can we assure synchronisation between threads in python ?
**Ans.** Thread synchronization is defined as a mechanism which ensures that two or more concurrent threads do not simultaneously execute some particular program segment known as critical section.

![img](https://media.geeksforgeeks.org/wp-content/uploads/multithreading-python-1.png)

Concurrent accesses to shared resource can lead to race condition. _A race condition occurs when two or more threads can access shared data and they try to change it at the same time. As a result, the values of variables may be unpredictable and vary depending on the timings of context switches of the processes._

threading module provides a Lock class to deal with the race conditions. Lock is implemented using a Semaphore object provided by the Operating System.

Lock class provides following methods:

1. acquire([blocking]) : To acquire a lock. A lock can be blocking or non-blocking. When invoked with the blocking argument set to True (the default), thread execution is blocked until the lock is unlocked, then lock is set to locked and return True. When invoked with the blocking argument set to False, thread execution is not blocked. If lock is unlocked, then set it to locked and return True else return False immediately.

2. release() : To release a lock. When the lock is locked, reset it to unlocked, and return. If any other threads are blocked waiting for the lock to become unlocked, allow exactly one of them to proceed. If lock is already unlocked, a ThreadError is raised.

```
import threading
lock = threading.Lock()

lock.acquire()
shared_resource()
lock.release()
```



