# Pyhton Notes

## Q1. What is GIL ?
Ans. In Python, the threads may be running on different processors, but they will only be running one at a time. Threading in python is not actually multithreading, it is coordinated multitasking. Using multiple threads to do above computation would make sense in other languages because we can take advantage of all the CPU cores, but in python it takes longer time. Therefore it’s not recommended to use multithreading for CPU intensive tasks in Python. If not for CPU intensive tasks, Python threads are helpful in dealing with blocking I/O operations including reading & writing files, interacting with networks, communicating with devices like displays etc. These tasks happen when Python make certain types of system calls. Tasks that spend much of their time waiting for external events are generally good candidates for threading.
The Python Global Interpreter Lock or GIL, in simple words, is a mutex (or a lock) that allows only one thread to hold the control of the Python interpreter.

[Video Explaination](https://youtu.be/f9q5m321iEU)
