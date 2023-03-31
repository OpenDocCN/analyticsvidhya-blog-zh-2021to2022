# Python 中的多线程和多重处理

> 原文：<https://medium.com/analytics-vidhya/multithreading-and-multiprocessing-in-python-1f773d1d160d?source=collection_archive---------5----------------------->

![](img/1acad9e6edf3143f5b2f34c2ad16103e.png)

# 进程与线程

**进程**是计算机程序(例如 Python 脚本)的执行环境。多个进程可以运行同一个程序，但是它们可以使用不同的数据和计算资源。

一个**线程**是进程中的一个执行单元。线程只能串行执行指令，但是一个进程可以有多个线程同时运行，承担任务的不同部分。

# 全局解释器锁(GIL)

**全局解释器锁(GIL)** 的概念对于理解 Python 中的多线程和多重处理至关重要。GIL 是一个进程锁，可以防止多个线程在一个 Python 进程中同时执行。尽管在一个进程中可以同时运行多个线程，但是在任何给定的时间，只有一个线程可以执行代码，其余的线程必须等待。

# 多线程操作

**多线程**意味着让同一个进程同时运行多个线程，共享同一个 CPU 和内存。然而，由于 Python 中的 GIL，并不是所有的任务都可以通过使用多线程来更快地执行。多个线程不能同时执行代码，但是当一个线程空闲等待时，另一个线程可以开始执行代码。

这就是为什么 Python 中的多线程非常适合受 I/O 限制的任务，这些任务的执行时间主要受等待输入和输出的时间限制。使用多线程可以大大提高速度的任务示例包括从互联网下载数据和将数据写入文件。

在下面的 Python 代码示例中，两个线程都执行 I/O 绑定任务，休眠 1 秒钟。通过使用多线程，第二个任务将开始而不等待第一个任务完成，因此整个过程只需要 1 秒多一点就可以执行，而不是 2 秒。

```
import time
import threadingdef some_task():
    time.sleep(1)
    print("Finished task")if __name__ == "__main__":
    start = time.time() # Create two threads
    t1 = threading.Thread(target=some_task)
    t2 = threading.Thread(target=some_task) # Start running both threads
    t1.start()
    t2.start() # Wait until both threads are complete, and join the process into a single thread
    t1.join()
    t2.join() end = time.time() print(f"Finished process in {end - start} seconds")
```

# 多重处理

**多重处理**是指从主进程派生出多个进程，每个进程都有自己的 CPU 和内存。每个进程也有自己的 GIL，这意味着并发进程可以同时执行代码。

Python 中的多处理非常适合受 CPU 限制的任务，这些任务的执行时间主要受 CPU 速度的限制。CPU 利用率高的任务可以通过使用多处理来加速，因为工作负载分散在多个 CPU 上。

在下面的 Python 代码示例中，两个进程都在执行计算 1+1 一亿次的 CPU 任务。通过使用多重处理，它们将同时执行，并且只需要大约一半的时间就能完成。

```
import time
import multiprocessingdef some_task():
    for _ in range(100_000_000):
        x = 1 + 1
    print("Finished task")if __name__ == "__main__":
    start = time.time() # Create two threads
    p1 = multiprocessing.Process(target=some_task)
    p2 = multiprocessing.Process(target=some_task) # Start running both threads
    p1.start()
    p2.start() # Wait until both threads are complete, and join the process into a single thread
    p1.join()
    p2.join() end = time.time() print(f"Finished process in {end - start} seconds")
```

# 并行未来

Python 3.2 引入了 *concurrent.futures* 模块，该模块提供了一个更简单的接口来将*线程*和*多处理*模块结合在一起。它利用 *ThreadPoolExecutor* 和 *ProcessPoolExecutor* 类来管理线程和进程池，这些线程和进程池共享大部分相同的接口，使多线程和多处理之间的切换更加容易。撇开接口不谈， *concurrent.futures* 模块在概念上与*线程*和*多处理*模块相同。

# 共享内存和竞争条件

进程有一些全局状态，可以在所有线程之间共享，每个线程也可以有自己的本地状态。

由于线程可以共享相同的全局变量，如果多个线程同时访问全局变量，那么使用**锁**(也称为**互斥**)来防止竞争情况是很重要的。*穿线类。锁*是实现锁的一种方式，其中全局变量可以被不同的线程获取和释放。当一个线程获得一个全局变量时，它被锁定，并且不能被另一个线程访问，直到那个线程释放它。

不同的进程不能共享相同的全局变量(如果每个进程试图访问一个全局变量，它实际上会复制一个全局变量)，但是如果进程需要彼此共享数据，它们可以使用**共享内存队列**。*多重处理*模块提供了一个*队列*类，非常类似于 Python 的*队列。Queue* 类，这是一个 FIFO 数据结构。不同的进程可以使用*多重处理来存放和获取数据。使用与单个进程使用*队列放入和获取数据相同的方式对*进行排队。队列*和*多重处理。队列*在共享内存内部使用锁，因此用户在使用*多处理时不必担心竞争情况。队列*。