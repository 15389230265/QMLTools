[TOC]



# Thread Support in Qt



Qt以独立于平台的线程类的形式提供线程支持，一种发布事件的线程安全方式，以及跨线程的信号槽连接。这使得开发便携式多线程Qt应用程序和利用多处理器机器变得容易。多线程编程也是一种有用的范例，用于在不冻结应用程序的用户界面的情况下执行耗时的操作。

早期版本的Qt提供了在没有线程支持的情况下构建库的选项。从Qt 4.0开始，线程始终处于启用状态。



## The Threading Classes

These classes are relevant to threaded applications.

|                                                              |                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html) |                                                         |
| [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html) |                                                         |
| [Concurrent Run](https://doc.qt.io/qt-5/qtconcurrentrun.html) |                                                         |
| [QAtomicInteger](https://doc.qt.io/qt-5/qatomicinteger.html) | 对整数进行独立于平台的原子操作                          |
| [QAtomicPointer](https://doc.qt.io/qt-5/qatomicpointer.html) | 模板类，它为指针提供与平台无关的原子操作                |
| [QFuture](https://doc.qt.io/qt-5/qfuture.html)               | 表示异步计算的结果                                      |
| [QFutureSynchronizer](https://doc.qt.io/qt-5/qfuturesynchronizer.html) | 方便类简化了QFuture同步                                 |
| [QFutureWatcher](https://doc.qt.io/qt-5/qfuturewatcher.html) | 允许使用信号和插槽监控QFuture                           |
| [QMutex](https://doc.qt.io/qt-5/qmutex.html)                 | 访问线程之间的序列化                                    |
| [QMutexLocker](https://doc.qt.io/qt-5/qmutexlocker.html)     | 便捷类，简化了锁定和解锁互斥锁                          |
| [QReadLocker](https://doc.qt.io/qt-5/qreadlocker.html)       | 便捷类，简化了锁定和解锁读写访问的读写锁                |
| [QReadWriteLock](https://doc.qt.io/qt-5/qreadwritelock.html) | 读写锁定                                                |
| [QRunnable](https://doc.qt.io/qt-5/qrunnable.html)           | 所有可运行对象的基类                                    |
| [QSemaphore](https://doc.qt.io/qt-5/qsemaphore.html)         | 一般计数信号量                                          |
| [QSemaphoreReleaser](https://doc.qt.io/qt-5/qsemaphorereleaser.html) | QSemaphore :: release()调用的异常安全延迟               |
| [QThread](https://doc.qt.io/qt-5/qthread.html)               | 与平台无关的管理线程的方式                              |
| [QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)       | 管理QThreads的集合                                      |
| [QThreadStorage](https://doc.qt.io/qt-5/qthreadstorage.html) | 每线程数据存储                                          |
| [QWaitCondition](https://doc.qt.io/qt-5/qwaitcondition.html) | 用于同步线程的条件变量                                  |
| [QWriteLocker](https://doc.qt.io/qt-5/qwritelocker.html)     | 便捷类，简化了锁定和解锁写访问的读写锁                  |
| [QtConcurrent](https://doc.qt.io/qt-5/qtconcurrent.html)     | 高级API，可以在不使用低级线程原语的情况下编写多线程程序 |

**注意：** Qt的线程类是使用本机线程API实现的; 例如，Win32和pthreads。因此，它们可以与相同本机API的线程一起使用。



# Multithreading Technologies in Qt



Qt提供了许多用于处理线程的类和函数。以下是Qt程序员可用于实现多线程应用程序的四种不同方法。



## QThread: Low-Level API with Optional Event Loops

[QThread](https://doc.qt.io/qt-5/qthread.html)是Qt中所有线程控制的基础。每个[QThread](https://doc.qt.io/qt-5/qthread.html)实例代表并控制一个线程。

[QThread](https://doc.qt.io/qt-5/qthread.html)既可以直接实例化，也可以子类化。实例化[QThread](https://doc.qt.io/qt-5/qthread.html)提供并行事件循环，允许在辅助线程中调用[QObject](https://doc.qt.io/qt-5/qobject.html)槽。对[QThread](https://doc.qt.io/qt-5/qthread.html)进行子类化允许应用程序在开始其事件循环之前初始化新线程，或者在没有事件循环的情况下运行并行代码。

See the [QThread class reference](https://doc.qt.io/qt-5/qthread.html) and the [threading examples](https://doc.qt.io/qt-5/examples-threadandconcurrent.html) for demonstrations on how to use [QThread](https://doc.qt.io/qt-5/qthread.html).



## QThreadPool and QRunnable: Reusing Threads

经常创建和销毁线程可能很昂贵。为了减少这种开销，可以将现有线程重用于新任务。[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)是可重复使用的QThreads的集合。

要在[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)的一个线程中运行代码，重新实现[QRunnable :: run](https://doc.qt.io/qt-5/qrunnable.html#run)()并实例[化子](https://doc.qt.io/qt-5/qrunnable.html)类化的[QRunnable](https://doc.qt.io/qt-5/qrunnable.html)。使用[QThreadPool :: start](https://doc.qt.io/qt-5/qthreadpool.html#start)()将[QRunnable](https://doc.qt.io/qt-5/qrunnable.html)放入[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)的运行队列中。当一个线程可用时，[QRunnable :: run](https://doc.qt.io/qt-5/qrunnable.html#run)()中的代码将在该线程中执行。

每个Qt应用程序都有一个全局线程池，可以通过[QThreadPool :: globalInstance](https://doc.qt.io/qt-5/qthreadpool.html#globalInstance)()访问。此全局线程池根据CPU中的核心数自动维护最佳线程数。但是，可以显式创建和管理单独的[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)。



## Qt Concurrent: Using a High-level API

在[Qt的并行](https://doc.qt.io/qt-5/qtconcurrent-index.html)模块提供了一些常见的并行计算模式处理的高级别功能： map, filter,  reduce。与使用[QThread](https://doc.qt.io/qt-5/qthread.html)和[QRunnable](https://doc.qt.io/qt-5/qrunnable.html)不同，这些函数从不需要使用[低级线程原语](https://doc.qt.io/qt-5/threads-synchronizing.html#low-level-synchronization-primitives)，如互斥锁或信号量。相反，它们返回一个[QFuture](https://doc.qt.io/qt-5/qfuture.html)对象，可用于在函数准备就绪时检索函数的结果。[QFuture](https://doc.qt.io/qt-5/qfuture.html)还可用于查询计算进度和暂停/恢复/取消计算。为了方便起见，[QFutureWatcher](https://doc.qt.io/qt-5/qfuturewatcher.html)使得能够与交互[QFuture](https://doc.qt.io/qt-5/qfuture.html)经由信号和槽秒。

[Qt Concurrent](https://doc.qt.io/qt-5/qtconcurrent-index.html)的map，filter和reduce算法自动在所有可用处理器内核之间分配计算，因此今天编写的应用程序将在稍后部署在具有更多内核的系统上时继续扩展。

该模块还提供了[QtConcurrent :: run](https://doc.qt.io/qt-5/qtconcurrent.html#run)()函数，该函数可以在另一个线程中运行任何函数。但是，[QtConcurrent :: run](https://doc.qt.io/qt-5/qtconcurrent.html#run)()仅支持map，filter和reduce函数可用的一部分功能。该[QFuture](https://doc.qt.io/qt-5/qfuture.html)可用于获取函数的返回值和检查，如果线程运行。但是，对[QtConcurrent :: run](https://doc.qt.io/qt-5/qtconcurrent.html#run)()的调用仅使用一个线程，无法暂停/恢复/取消，并且无法查询进度。

See the [Qt Concurrent](https://doc.qt.io/qt-5/qtconcurrent-index.html) module documentation for details on the individual functions.



## WorkerScript: Threading in QML

所述[WorkerScript](https://doc.qt.io/qt-5/workerscript-qmlmodule.html) QML类型让与GUI线程并行JavaScript代码运行。

每个[WorkerScript](https://doc.qt.io/qt-5/workerscript-qmlmodule.html)实例都可以`.js`附加一个脚本。当[WorkerScript.sendMessage](https://doc.qt.io/qt-5/qml-workerscript.html#sendMessage-method)()被调用时，该脚本将在一个单独的线程（和单独运行[QML上下文](https://doc.qt.io/qt-5/qqmlcontext.html)）。当脚本完成运行时，它可以将回复发送回GUI线程，该线程将调用[WorkerScript.onMessage](https://doc.qt.io/qt-5/qml-workerscript.html#message-signal)()信号处理程序。

使用[WorkerScript](https://doc.qt.io/qt-5/workerscript-qmlmodule.html)类似于使用已移动到另一个线程的worker [QObject](https://doc.qt.io/qt-5/qobject.html)。数据通过信号在线程之间传输。

有关如何实现脚本的详细信息，以及可在线程之间传递的数据类型列表，请参阅[WorkerScript](https://doc.qt.io/qt-5/workerscript-qmlmodule.html)文档。



## Choosing an Appropriate Approach

如上所述，Qt为开发线程应用程序提供了不同的解决方案。给定应用程序的正确解决方案取决于新线程的用途和线程的生命周期。下面是Qt的线程技术的比较，然后是一些示例用例的推荐解决方案。



### Comparison of Solutions

| Feature                                                      | [QThread](https://doc.qt.io/qt-5/qthread.html)               | [QRunnable](https://doc.qt.io/qt-5/qrunnable.html) and [QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html) | [QtConcurrent::run](https://doc.qt.io/qt-5/qtconcurrent.html#run)() | Qt Concurrent (Map, Filter, Reduce)                          | [WorkerScript](https://doc.qt.io/qt-5/workerscript-qmlmodule.html) |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 语言                                                         | C++                                                          | C++                                                          | C++                                                          | C++                                                          | QML                                                          |
| 可以指定线程优先级                                           | Yes                                                          | Yes                                                          |                                                              |                                                              |                                                              |
| 线程可以运行事件循环                                         | Yes                                                          |                                                              |                                                              |                                                              |                                                              |
| 线程可以通过信号接收数据更新                                 | Yes (received by a worker [QObject](https://doc.qt.io/qt-5/qobject.html)) |                                                              |                                                              |                                                              | Yes (received by [WorkerScript](https://doc.qt.io/qt-5/workerscript-qmlmodule.html)) |
| 可以使用信号控制线程                                         | Yes (received by [QThread](https://doc.qt.io/qt-5/qthread.html)) |                                                              |                                                              | Yes (received by [QFutureWatcher](https://doc.qt.io/qt-5/qfuturewatcher.html)) |                                                              |
| 可以通过[QFuture](https://doc.qt.io/qt-5/qfuture.html)监控线程 |                                                              |                                                              | Partially                                                    | Yes                                                          |                                                              |
| 内置暂停/恢复/取消功能                                       |                                                              |                                                              |                                                              | Yes                                                          |                                                              |



### Example Use Cases

| Lifetime of thread | Operation                                                    | Solution                                                     |
| ------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| One call           | 在另一个线程中运行新的线性函数，可选择在运行期间进行更新。   | Qt提供不同的解决方案：                                                             将函数放在QThread :: run()的重新实现中并启动QThread。发出信号以更新进度。                                              
放置功能的重新实现QRunnable ::运行()，并添加QRunnable到QThreadPool。写入线程安全变量以更新进度。    
使用QtConcurrent :: run()运行该函数。写入线程安全变量以更新进度。 |
| One call           | 在另一个线程中运行现有函数并获取其返回值。                   | 使用[QtConcurrent :: run](https://doc.qt.io/qt-5/qtconcurrent.html#run)()运行该函数。有[QFutureWatcher](https://doc.qt.io/qt-5/qfuturewatcher.html)发射[完成()](https://doc.qt.io/qt-5/qfuturewatcher.html#finished)信号时，该功能已恢复，并调用[QFutureWatcher ::结果](https://doc.qt.io/qt-5/qfuturewatcher.html#result)()来获取函数的返回值。 |
| One call           | 使用所有可用核心对容器的所有项执行操作。例如，从图像列表中生成缩略图。 | 使用Qt Concurrent的[QtConcurrent :: filter](https://doc.qt.io/qt-5/qtconcurrent.html#filter)()函数选择容器元素，使用[QtConcurrent :: map](https://doc.qt.io/qt-5/qtconcurrent.html#map-1)()函数将操作应用于每个元素。要将输出折叠为单个结果，请改用[QtConcurrent :: filteredReduced](https://doc.qt.io/qt-5/qtconcurrent.html#filteredReduced-1)()和[QtConcurrent :: mappedReduced](https://doc.qt.io/qt-5/qtconcurrent.html#mappedReduced-1)()。 |
| One call/Permanent | 在纯QML应用程序中进行长时间计算，并在结果准备就绪时更新GUI。 | 将计算代码放在`.js`脚本中并将其附加到[WorkerScript](https://doc.qt.io/qt-5/workerscript-qmlmodule.html)实例。调用[WorkerScript.sendMessage](https://doc.qt.io/qt-5/qml-workerscript.html#sendMessage-method)()以在新线程中启动计算。让脚本调用sendMessage()，将结果传递回GUI线程。处理结果`onMessage`并在那里更新GUI。 |
| Permanent          | 让对象生活在另一个线程中，该线程可以根据请求执行不同的任务和/或可以接收要处理的新数据。 | 子类化[QObject](https://doc.qt.io/qt-5/qobject.html)以创建工作者。实例化此worker对象和[QThread](https://doc.qt.io/qt-5/qthread.html)。将worker移动到新线程。通过排队的信号槽连接将命令或数据发送到工作者对象。 |
| Permanent          | 在另一个线程中重复执行昂贵的操作，其中线程不需要接收任何信号或事件。 | 在[QThread :: run](https://doc.qt.io/qt-5/qthread.html#run)()的重新实现中直接编写无限循环。在没有事件循环的情况下启动线程。让线程发出信号以将数据发送回GUI线程。 |

  

# Synchronizing Threads



虽然线程的目的是允许代码并行运行，但有时线程必须停止并等待其他线程。例如，如果两个线程同时尝试写入同一个变量，则结果是未定义的。强制线程彼此等待的原则称为*互斥*。这是保护数据等共享资源的常用技术。

Qt提供了低级原语以及用于同步线程的高级机制。



## Low-Level Synchronization Primitives

[QMutex](https://doc.qt.io/qt-5/qmutex.html)是实施互斥的基本类。线程锁定互斥锁以获得对共享资源的访问权。如果第二个线程在锁定时尝试锁定互斥锁，则第二个线程将进入休眠状态，直到第一个线程完成其任务并解锁互斥锁。

[QReadWriteLock](https://doc.qt.io/qt-5/qreadwritelock.html)类似于[QMutex](https://doc.qt.io/qt-5/qmutex.html)，除了它区分“读取”和“写入”访问。当没有写入一条数据时，多个线程同时从中读取数据是安全的。甲[QMutex](https://doc.qt.io/qt-5/qmutex.html)力多个读者轮流读共享数据，但[QReadWriteLock](https://doc.qt.io/qt-5/qreadwritelock.html)允许同时读取，从而提高了并行性。

[QSemaphore](https://doc.qt.io/qt-5/qsemaphore.html)是的一般化[QMutex](https://doc.qt.io/qt-5/qmutex.html)保护一定数目的相同的资源。相比之下，[QMutex只](https://doc.qt.io/qt-5/qmutex.html)保护一种资源。的[信号量实施例](https://doc.qt.io/qt-5/qtcore-threads-semaphores-example.html)示出了信号量的一个典型应用：同步访问一个生产者和消费者之间的环形缓冲器。

[QWaitCondition](https://doc.qt.io/qt-5/qwaitcondition.html)不是通过强制互斥而是通过提供*条件变量来*同步线程。虽然其他原语使线程等待资源解锁，但[QWaitCondition](https://doc.qt.io/qt-5/qwaitcondition.html)使线程等待直到满足特定条件。要允许等待线程继续，请调用[wakeOne()](https://doc.qt.io/qt-5/qwaitcondition.html#wakeOne)以唤醒一个随机选择的线程或[wakeAll()](https://doc.qt.io/qt-5/qwaitcondition.html#wakeAll)以同时唤醒它们。在[等待条件范例](https://doc.qt.io/qt-5/qtcore-threads-waitconditions-example.html)展示了如何使用解决生产者-消费者问题[QWaitCondition](https://doc.qt.io/qt-5/qwaitcondition.html)代替[QSemaphore](https://doc.qt.io/qt-5/qsemaphore.html)。

**注意：** Qt的同步类依赖于使用正确对齐的指针。例如，您不能将压缩类与MSVC一起使用。

这些同步类可用于使方法线程安全。但是，这样做会导致性能下降，这就是为什么大多数Qt方法都不是线程安全的原因。



### Risks

如果线程锁定资源但未解锁，则应用程序可能会冻结，因为该资源将永久不可用于其他线程。例如，如果抛出异常并强制当前函数返回而不释放其锁定，则会发生这种情况。

另一个类似的情况是*僵局*。例如，假设线程A正在等待线程B解锁资源。如果线程B也在等待线程A解锁不同的资源，那么两个线程将永远等待，因此应用程序将冻结。



### Convenience classes

[QMutexLocker](https://doc.qt.io/qt-5/qmutexlocker.html)，[QReadLocker](https://doc.qt.io/qt-5/qreadlocker.html)和[QWriteLocker](https://doc.qt.io/qt-5/qwritelocker.html)是便捷类，可以更轻松地使用[QMutex](https://doc.qt.io/qt-5/qmutex.html)和[QReadWriteLock](https://doc.qt.io/qt-5/qreadwritelock.html)。它们在构造时锁定资源，并在销毁时自动解锁。它们旨在简化使用[QMutex](https://doc.qt.io/qt-5/qmutex.html)和[QReadWriteLock的](https://doc.qt.io/qt-5/qreadwritelock.html)代码，从而减少资源被意外永久锁定的可能性。



## High-Level Event Queues

Qt的[事件系统](https://doc.qt.io/qt-5/eventsandfilters.html)对于线程间通信非常有用。每个线程都可以有自己的事件循环。要在另一个线程中调用一个槽（或任何[可调用](https://doc.qt.io/qt-5/qobject.html#Q_INVOKABLE)方法），请将该调用放在目标线程的事件循环中。这使得目标线程在插槽开始运行之前完成其当前任务，而原始线程继续并行运行。

要在事件循环中进行调用，请进行排队的[信号槽](https://doc.qt.io/qt-5/signalsandslots.html)连接。无论何时发出信号，其参数都将由事件系统记录。信号接收器[所在](https://doc.qt.io/qt-5/qobject.html#thread-affinity)的线程将运行插槽。或者，调用[QMetaObject :: invokeMethod](https://doc.qt.io/qt-5/qmetaobject.html#invokeMethod-5)()以在没有信号的情况下实现相同的效果。在这两种情况下，必须使用[排队连接，](https://doc.qt.io/qt-5/qt.html#ConnectionType-enum)因为[直接连接会](https://doc.qt.io/qt-5/qt.html#ConnectionType-enum)绕过事件系统并立即在当前线程中运行该方法。

与使用低级基元不同，使用事件系统进行线程同步时不存在死锁风险。但是，事件系统不会强制执行互斥。如果可调用方法访问共享数据，则它们仍必须使用低级基元保护。

话虽如此，Qt的事件系统以及[隐式共享](https://doc.qt.io/qt-5/implicit-sharing.html)数据结构提供了传统线程锁定的替代方案。如果信号和插槽是专门使用的，并且线程之间没有共享变量，则多线程程序可以完全没有低级基元。

**See also** [QThread::exec](https://doc.qt.io/qt-5/qthread.html#exec)() and [Threads and QObjects](https://doc.qt.io/qt-5/threads-qobject.html).





# Reentrancy and Thread-Safety



在整个文档中，术语*reentrant*和*thread-safe*用于标记类和函数，以指示它们如何在多线程应用程序中使用：

- 一个*线程安全的*功能，可以同时从多个线程调用，即使调用使用共享数据，因为对共享数据的所有引用序列化。
- 一个*可重入*函数也可以从多个线程同时调用，但只有当每个调用使用自己的数据。

因此，*线程安全*函数始终是*可重入的*，但是*可重入*函数并不总是*线程安全的*。

通过扩展，如果一个类可以从多个线程安全地调用其成员函数，则称该类是可*重入的*，只要每个线程使用该类的*不同*实例即可。如果可以从多个线程安全地调用其成员函数，则该类是*线程安全的*，即使所有线程都使用该类的*相同*实例。

**注意：如果** Qt类打算由多个线程使用，则它们仅记录为*线程安全的*。如果函数未标记为线程安全或可重入，则不应在不同的线程中使用它。如果某个类未标记为线程安全或可重入，则不应从不同的线程访问该类的特定实例。



## Reentrancy

C ++类通常是可重入的，仅仅因为它们只访问自己的成员数据。任何线程都可以在可重入类的实例上调用成员函数，只要没有其他线程可以*同时*在该类的*同一*实例上调用成员函数。例如，`Counter`下面的类是可重入的：

```c++
class Counter
{
public:
    Counter() { n = 0; }

    void increment() { ++n; }
    void decrement() { --n; }
    int value() const { return n; }

private:
    int n;
};
```

该类不是线程安全的，因为如果多个线程尝试修改数据成员`n`，则结果是未定义的。这是因为`++`和`--`运算符并不总是原子的。实际上，它们通常扩展到三个机器指令：

1. 将变量的值加载到寄存器中。
2. 递增或递减寄存器的值。
3. 将寄存器的值存回主存储器。

如果线程A和线程B同时加载变量的旧值，增加它们的寄存器并将其存储回来，它们最终会相互覆盖，并且变量只增加一次！



## Thread-Safety

显然，必须序列化访问：在线程B执行相同的步骤之前，线程A必须执行步骤1,2,3而不中断（原子地）; 或相反亦然。使类线程安全的简单方法是使用[QMutex](https://doc.qt.io/qt-5/qmutex.html)保护对数据成员的所有访问：

```
class Counter
{
public:
    Counter() { n = 0; }

    void increment() { QMutexLocker locker(&mutex); ++n; }
    void decrement() { QMutexLocker locker(&mutex); --n; }
    int value() const { QMutexLocker locker(&mutex); return n; }

private:
    mutable QMutex mutex;
    int n;
};
```

该[QMutexLocker](https://doc.qt.io/qt-5/qmutexlocker.html)类自动锁定在其构造互斥体和解锁它时，析构函数被调用时，在函数的末尾。锁定互斥锁可确保序列化来自不同线程的访问。该`mutex`数据成员与申报`mutable`资格，因为我们需要锁定和解锁的互斥体`value()`，这是一个const函数。



## Notes on Qt Classes

许多Qt类是*可重入的*，但它们不是*线程安全的*，因为使它们成为线程安全会导致重复锁定和解锁[QMutex](https://doc.qt.io/qt-5/qmutex.html)的额外开销。例如，[QString](https://doc.qt.io/qt-5/qstring.html)是可重入的，但不是线程安全的。您可以安全地同时从多个线程访问[QString的](https://doc.qt.io/qt-5/qstring.html)*不同*实例，但是您无法同时从多个线程安全地访问*同*一个[QString](https://doc.qt.io/qt-5/qstring.html)实例（除非您使用[QMutex自行](https://doc.qt.io/qt-5/qmutex.html)保护访问）。

一些Qt类和函数是线程安全的。这些主要是与线程相关的类（例如[QMutex](https://doc.qt.io/qt-5/qmutex.html)）和基本函数（例如[QCoreApplication :: postEvent](https://doc.qt.io/qt-5/qcoreapplication.html#postEvent)()）。

**注意：**多线程域中的术语不是完全标准化的。POSIX使用可重入和线程安全的定义，这些定义与其C API略有不同。在Qt中使用其他面向对象的C ++类库时，请确保理解定义。





# Threads and QObjects



[QThread](https://doc.qt.io/qt-5/qthread.html)继承[QObject](https://doc.qt.io/qt-5/qobject.html)。它发出信号以指示线程已启动或已完成执行，并提供一些插槽。

更有趣的是，[QObject](https://doc.qt.io/qt-5/qobject.html)可以在多个线程中使用，发出调用其他线程中的槽的信号，并将事件发布到在其他线程中“活动”的对象。这是可能的，因为允许每个线程拥有自己的事件循环。



## QObject Reentrancy

[QObject](https://doc.qt.io/qt-5/qobject.html)是可重入的。它的大多数非GUI子类（如[QTimer](https://doc.qt.io/qt-5/qtimer.html)，[QTcpSocket](https://doc.qt.io/qt-5/qtcpsocket.html)，[QUdpSocket](https://doc.qt.io/qt-5/qudpsocket.html)和[QProcess](https://doc.qt.io/qt-5/qprocess.html)）也是可重入的，因此可以同时使用来自多个线程的这些类。请注意，这些类旨在从单个线程中创建和使用; 在一个线程中创建一个对象并从另一个线程调用它的函数不能保证工作。有三个限制需要注意：

- *必须始终在创建父项的线程中创建QObject的子项。*除其他外，这意味着你永远不应该将[QThread](https://doc.qt.io/qt-5/qthread.html)对象（`this`）作为在线程中创建的对象的父对象传递（因为[QThread](https://doc.qt.io/qt-5/qthread.html)对象本身是在另一个线程中创建的）。
- *事件驱动的对象只能在单个线程中使用。*具体而言，这适用于[计时器机制](https://doc.qt.io/qt-5/timers.html)和[网络模块](https://doc.qt.io/qt-5/qtnetwork-module.html)。例如，您无法启动计时器或在不是[对象线程的线程中](https://doc.qt.io/qt-5/qobject.html#thread)连接套接字。
- *在删除QThread之前，必须确保删除线程中创建的所有对象。*这可以通过在[run()](https://doc.qt.io/qt-5/qthread.html#run)实现中在堆栈上创建对象来轻松完成。

尽管[QObject](https://doc.qt.io/qt-5/qobject.html)是可重入的，但GUI类（尤其是[QWidget](https://doc.qt.io/qt-5/qwidget.html)及其所有子类）不可重入。它们只能在主线程中使用。如前所述，还必须从该线程调用[QCoreApplication :: exec](https://doc.qt.io/qt-5/qcoreapplication.html#exec)()。

实际上，通过将耗时的操作放在单独的工作线程中并在工作线程完成时在主线程中的屏幕上显示结果，可以很容易地解决在主线程之外的其他线程中使用GUI类的不可能性。这是用于实现[Mandelbrot示例](https://doc.qt.io/qt-5/qtcore-threads-mandelbrot-example.html)和[阻止Fortune客户端示例的方法](https://doc.qt.io/qt-5/qtnetwork-blockingfortuneclient-example.html)。

通常，不支持在[QApplication](https://doc.qt.io/qt-5/qapplication.html)之前创建QObject，并且可能会在退出时导致奇怪的崩溃，具体取决于平台。这意味着也不支持[QObject的](https://doc.qt.io/qt-5/qobject.html)静态实例。正确结构化的单线程或多线程应用程序应该使[QApplication](https://doc.qt.io/qt-5/qapplication.html)成为第一个创建的，并最后销毁[QObject](https://doc.qt.io/qt-5/qobject.html)。



## Per-Thread Event Loop

每个线程都可以拥有自己的事件循环。初始线程使用[QCoreApplication :: exec](https://doc.qt.io/qt-5/qcoreapplication.html#exec)()或单对话框GUI应用程序（有时是[QDialog :: exec](https://doc.qt.io/qt-5/qdialog.html#exec)()）启动其事件循环。其他线程可以使用[QThread :: exec](https://doc.qt.io/qt-5/qthread.html#exec)()启动事件循环。与[QCoreApplication](https://doc.qt.io/qt-5/qcoreapplication.html)一样，[QThread](https://doc.qt.io/qt-5/qthread.html)提供了一个[exit](https://doc.qt.io/qt-5/qthread.html#exit)（int）函数和一个[quit()](https://doc.qt.io/qt-5/qthread.html#quit)槽。

线程中的事件循环使线程可以使用某些需要存在事件循环的非GUI Qt类（例如[QTimer](https://doc.qt.io/qt-5/qtimer.html)，[QTcpSocket](https://doc.qt.io/qt-5/qtcpsocket.html)和[QProcess](https://doc.qt.io/qt-5/qprocess.html)）。它还可以将来自任何线程的信号连接到特定线程的插槽。这将在下面的 [Signals and Slots Across Threads](https://doc.qt.io/qt-5/threads-qobject.html#signals-and-slots-across-threads) 部分中详细说明。

![Threads, objects, and event loops](qt多线程和线程池使用.assets/threadsandobjects.png)

一个[QObject的](https://doc.qt.io/qt-5/qobject.html)实例据说*住*在其创建的线程。该对象的事件由该线程的事件循环调度。其中螺纹[QObject的](https://doc.qt.io/qt-5/qobject.html)生活可以使用[的QObject ::线程](https://doc.qt.io/qt-5/qobject.html#thread)()。

该[QObject的:: moveToThread](https://doc.qt.io/qt-5/qobject.html#moveToThread)()函数更改为对象及其子线程亲和力（该对象可以，如果它有一个父无法移动）。

调用`delete`一个[QObject的](https://doc.qt.io/qt-5/qobject.html)距离比另一个线程*拥有*的对象（或访问其他方式的对象）是不安全的，除非你能保证该对象不处理在那一刻的事件。使用[QObject :: deleteLater](https://doc.qt.io/qt-5/qobject.html#deleteLater)()代替，并发布一个[DeferredDelete](https://doc.qt.io/qt-5/qevent.html#Type-enum)事件，该事件的最终将获取对象线程的事件循环。默认情况下，该线程*拥有*一个[QObject的](https://doc.qt.io/qt-5/qobject.html)是，线程*创建*的[QObject的](https://doc.qt.io/qt-5/qobject.html)，而不是之后[的QObject :: moveToThread](https://doc.qt.io/qt-5/qobject.html#moveToThread)()被调用。

如果没有运行事件循环，则不会将事件传递给对象。例如，如果在线程中创建[QTimer](https://doc.qt.io/qt-5/qtimer.html)对象但从不调用[exec()](https://doc.qt.io/qt-5/qthread.html#exec)，则[QTimer](https://doc.qt.io/qt-5/qtimer.html)将永远不会发出其[timeout()](https://doc.qt.io/qt-5/qtimer.html#timeout)信号。调用[deleteLater()](https://doc.qt.io/qt-5/qobject.html#deleteLater)也不起作用。（这些限制也适用于主线程。）

您可以使用线程安全函数[QCoreApplication :: postEvent](https://doc.qt.io/qt-5/qcoreapplication.html#postEvent)()随时手动将事件发布到任何线程中的任何对象。事件将由创建对象的线程的事件循环自动分派。

所有线程都支持事件过滤器，但监视对象必须与受监视对象位于同一线程中。类似地，[QCoreApplication :: sendEvent](https://doc.qt.io/qt-5/qcoreapplication.html#sendEvent)()（与[postEvent()](https://doc.qt.io/qt-5/qcoreapplication.html#postEvent)不同）只能用于将事件分派给生成在调用函数的线程中的对象。



## Accessing QObject Subclasses from Other Threads

[QObject](https://doc.qt.io/qt-5/qobject.html)及其所有子类都不是线程安全的。这包括整个事件传递系统。重要的是要记住，当您从另一个线程访问对象时，事件循环可能正在向您的[QObject](https://doc.qt.io/qt-5/qobject.html)子类传递事件。

如果要在[QObject](https://doc.qt.io/qt-5/qobject.html)子类上调用一个不在当前线程中并且该对象可能接收事件的函数，则必须使用互斥锁保护对[QObject](https://doc.qt.io/qt-5/qobject.html)子类内部数据的所有访问; 否则，您可能会遇到崩溃或其他不良行为。

与其他对象一样，[QThread](https://doc.qt.io/qt-5/qthread.html)对象存在于创建对象的线程中 - *而不是*在[调用QThread :: run](https://doc.qt.io/qt-5/qthread.html#run)()时创建的线程中。除非使用互斥锁保护成员变量，否则在[QThread](https://doc.qt.io/qt-5/qthread.html)子类中提供插槽通常是不安全的。

另一方面，您可以安全地从[QThread :: run](https://doc.qt.io/qt-5/qthread.html#run)()实现中发出信号，因为信号发射是线程安全的。



## Signals and Slots Across Threads

Qt支持以下信号槽连接类型：

- [Auto Connection](https://doc.qt.io/qt-5/qt.html#ConnectionType-enum) (default) 如果在接收对象具有亲和力的线程中发出信号，则行为与直接连接相同。否则，行为与排队连接相同。

- [Direct Connection](https://doc.qt.io/qt-5/qt.html#ConnectionType-enum) 发出信号时立即调用插槽。插槽在发射器的线程中执行，该线程不一定是接收器的线程。

- [Queued Connection](https://doc.qt.io/qt-5/qt.html#ConnectionType-enum) 当控制返回到接收者线程的事件循环时，将调用插槽。插槽在接收器的线程中执行。

- Blocking Queued Connection 对于排队连接，调用插槽，但当前线程阻塞，直到插槽返回。

  **注意：**使用此类型连接同一线程中的对象将导致死锁。

- [Unique Connection](https://doc.qt.io/qt-5/qt.html#ConnectionType-enum) 行为与自动连接相同，但仅当连接不与现有连接重复时才建立连接。即，如果相同的信号已经连接到同一对对象的相同插槽，则不进行连接并返回connect()`false`。

可以通过向[connect()](https://doc.qt.io/qt-5/qobject.html#connect-4)传递一个附加参数来指定连接类型。请注意，如果事件循环在接收者的线程中运行，则当发送方和接收方位于不同的线程中时使用直接连接是不安全的，原因与在另一个线程中的对象上调用任何函数是不安全的相同。

[QObject :: connect](https://doc.qt.io/qt-5/qobject.html#connect-4)()本身是线程安全的。

所述[曼德尔布罗实施例](https://doc.qt.io/qt-5/qtcore-threads-mandelbrot-example.html)使用一个排队连接的工作线程和主线程之间进行通信。为了避免冻结主线程的事件循环（以及因此，应用程序的用户界面），所有Mandelbrot分形计算都在一个单独的工作线程中完成。线程在渲染分形时发出信号。

同样，[Blocking Fortune客户端示例](https://doc.qt.io/qt-5/qtnetwork-blockingfortuneclient-example.html)使用单独的线程异步与TCP服务器通信。





# Thread-Support in Qt Modules



## Threads and the SQL Module

只能在创建连接的线程中使用连接。不支持在线程之间移动连接或从其他线程创建查询。

此外，QSqlDrivers使用的第三方库可能会对在多线程程序中使用SQL模块施加进一步的限制。有关更多信息，请参阅数据库客户端的手册



## Painting in Threads

[QPainter](https://doc.qt.io/qt-5/qpainter.html)可以在一个线程中用于绘制到[QImage](https://doc.qt.io/qt-5/qimage.html)，[QPrinter](https://doc.qt.io/qt-5/qprinter.html)和[QPicture](https://doc.qt.io/qt-5/qpicture.html)绘图设备上。*不*支持在QPixmaps和QWidgets上绘画。在macOS上，如果从GUI线程外部打印，则不会显示自动进度对话框。

任何数量的线程都可以在任何给定时间绘制，但是一次只有一个线程可以在给定的绘制设备上绘制。换句话说，如果每个绘制到单独的QImages上，两个线程可以同时绘制，但是这两个线程不能同时绘制到同一个[QImage](https://doc.qt.io/qt-5/qimage.html)上。



## Threads and Rich Text Processing

The [QTextDocument](https://doc.qt.io/qt-5/qtextdocument.html), [QTextCursor](https://doc.qt.io/qt-5/qtextcursor.html), and [all related classes](https://doc.qt.io/qt-5/richtext.html)是可重入。

请注意，在GUI线程中创建的[QTextDocument](https://doc.qt.io/qt-5/qtextdocument.html)实例可能包含[QPixmap](https://doc.qt.io/qt-5/qpixmap.html)图像资源。使用[QTextDocument :: clone](https://doc.qt.io/qt-5/qtextdocument.html#clone)()创建文档的副本，并将副本传递给另一个线程以进行进一步处理（例如打印）。



## Threads and the SVG Module

[QtSvg](https://doc.qt.io/qt-5/qtsvg-module.html)模块中的[QSvgGenerator](https://doc.qt.io/qt-5/qsvggenerator.html)和[QSvgRenderer](https://doc.qt.io/qt-5/qsvgrenderer.html)类是可重入的。



## Threads and Implicitly Shared Classes

Qt 对其许多值类使用称为[隐式共享](https://doc.qt.io/qt-5/implicit-sharing.html)的优化，尤其是[QImage](https://doc.qt.io/qt-5/qimage.html)和[QString](https://doc.qt.io/qt-5/qstring.html)。从Qt 4开始，可以安全地跨线程复制隐式共享类，就像任何其他值类一样。它们是完全[可重入的](https://doc.qt.io/qt-5/threads-reentrancy.html)。隐式共享实际上是*隐含的*。

在许多人看来，隐式共享和多线程是不兼容的概念，因为引用计数通常是这样做的。但是，Qt使用原子引用计数来确保共享数据的完整性，从而避免引用计数器的潜在损坏。

请注意，原子引用计数不保证[线程安全](https://doc.qt.io/qt-5/threads-reentrancy.html)。在线程之间共享隐式共享类的实例时，应使用正确的锁定。这是对共享或不共享的所有[重入](https://doc.qt.io/qt-5/threads-reentrancy.html)类的相同要求。但是，原子引用计数确保一个线程工作在它自己的隐式共享类的本地实例上是安全的。我们建议使用[信号和插槽](https://doc.qt.io/qt-5/threads-qobject.html#signals-and-slots-across-threads)在线程之间传递数据，因为这可以在不需要任何显式锁定的情况下完成。

总而言之，Qt 4中的隐式共享类实际上是*隐式*共享的。即使在多线程应用程序中，您也可以安全地使用它们，就像它们是普通的，非共享的，可重入的基于值的类一样。





# QThread Class

该[QThread的](https://doc.qt.io/qt-5/qthread.html)类提供了一个独立于平台的方式来管理线程。 [More...](https://doc.qt.io/qt-5/qthread.html#details)

|           |                                                |
| --------- | ---------------------------------------------- |
| Header:   | #include <QThread>                             |
| qmake:    | QT += core                                     |
| Inherits: | [QObject](https://doc.qt.io/qt-5/qobject.html) |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qthread-members.html)
- [Obsolete members](https://doc.qt.io/qt-5/qthread-obsolete.html)



## Public Types

|      |                                                              |
| ---- | ------------------------------------------------------------ |
| enum | **Priority** { IdlePriority, LowestPriority, LowPriority, NormalPriority, ..., InheritPriority } |



## Public Functions

|                            | **QThread**(QObject **parent* = nullptr)                     |
| -------------------------- | ------------------------------------------------------------ |
| virtual                    | **~QThread**()                                               |
| QAbstractEventDispatcher * | **eventDispatcher**() const                                  |
| void                       | **exit**(int *returnCode* = 0)                               |
| bool                       | **isFinished**() const                                       |
| bool                       | **isInterruptionRequested**() const                          |
| bool                       | **isRunning**() const                                        |
| int                        | **loopLevel**() const                                        |
| QThread::Priority          | **priority**() const                                         |
| void                       | **requestInterruption**()                                    |
| void                       | **setEventDispatcher**(QAbstractEventDispatcher **eventDispatcher*) |
| void                       | **setPriority**(QThread::Priority *priority*)                |
| void                       | **setStackSize**(uint *stackSize*)                           |
| uint                       | **stackSize**() const                                        |
| bool                       | **wait**(unsigned long *time* = ULONG_MAX)                   |





## Reimplemented Public Functions

|              |                                     |
| ------------ | ----------------------------------- |
| virtual bool | **event**(QEvent **event*) override |

- 32 public functions inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#public-functions)





## Public Slots

| void | **quit**()                                                |
| ---- | --------------------------------------------------------- |
| void | **start**(QThread::Priority *priority* = InheritPriority) |
| void | **terminate**()                                           |

- 1 public slot inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#public-slots)





## Signals

|      |                |
| ---- | -------------- |
| void | **finished**() |
| void | **started**()  |

- 2 signals inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#signals)





## Static Public Members

|                   |                                               |
| ----------------- | --------------------------------------------- |
| QThread *         | **create**(Function &&*f*, Args &&... *args*) |
| QThread *         | **create**(Function &&*f*)                    |
| QThread *         | **currentThread**()                           |
| Qt::HANDLE        | **currentThreadId**()                         |
| int               | **idealThreadCount**()                        |
| void              | **msleep**(unsigned long *msecs*)             |
| void              | **sleep**(unsigned long *secs*)               |
| const QMetaObject | **staticMetaObject**                          |
| void              | **usleep**(unsigned long *usecs*)             |
| void              | **yieldCurrentThread**()                      |

- 10 static public members inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#static-public-members)





## Protected Functions

|              |            |
| ------------ | ---------- |
| int          | **exec**() |
| virtual void | **run**()  |

- 9 protected functions inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#protected-functions)





## Static Protected Members

|      |                                                  |
| ---- | ------------------------------------------------ |
| void | **setTerminationEnabled**(bool *enabled* = true) |

### Additional Inherited Members

- 1 property inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#properties)





## Detailed Description

该[QThread的](https://doc.qt.io/qt-5/qthread.html)类提供了一个独立于平台的方式来管理线程。

甲[的QThread](https://doc.qt.io/qt-5/qthread.html)对象管理程序内的控制的一个线程。QThreads开始在[run](https://doc.qt.io/qt-5/qthread.html#run)()中执行。默认情况下，[run](https://doc.qt.io/qt-5/qthread.html#run)()通过调用[exec](https://doc.qt.io/qt-5/qthread.html#exec)()启动事件循环并在线程内运行Qt事件循环。

您可以使用[QObject :: moveToThread](https://doc.qt.io/qt-5/qobject.html#moveToThread)()将工作对象移动到线程来使用它们。

```c++
class Worker : public QObject
{
    Q_OBJECT

public slots:
    void doWork(const QString &parameter) {
        QString result;
        /* ... here is the expensive or blocking operation ... */
        emit resultReady(result);
    }

signals:
    void resultReady(const QString &result);
};

class Controller : public QObject
{
    Q_OBJECT
    QThread workerThread;
public:
    Controller() {
        Worker *worker = new Worker;
        worker->moveToThread(&workerThread);
        connect(&workerThread, &QThread::finished, worker, &QObject::deleteLater);
        connect(this, &Controller::operate, worker, &Worker::doWork);
        connect(worker, &Worker::resultReady, this, &Controller::handleResults);
        workerThread.start();
    }
    ~Controller() {
        workerThread.quit();
        workerThread.wait();
    }
public slots:
    void handleResults(const QString &);
signals:
    void operate(const QString &);
};
```

然后，Worker的插槽中的代码将在一个单独的线程中执行。但是，您可以自由地将Worker的插槽连接到任何线程中任何对象的任何信号。由于称为[排队连接](https://doc.qt.io/qt-5/qt.html#ConnectionType-enum)的机制，可以安全地跨不同线程连接信号和插槽。

使代码在单独的线程中运行的另一种方法是[继承QThread](https://doc.qt.io/qt-5/qthread.html)并重新实现[run](https://doc.qt.io/qt-5/qthread.html#run)()。例如：

```c++
class WorkerThread : public QThread
{
    Q_OBJECT
    void run() override {
        QString result;
        /* ... here is the expensive or blocking operation ... */
        emit resultReady(result);
    }
signals:
    void resultReady(const QString &s);
};

void MyObject::startWorkInAThread()
{
    WorkerThread *workerThread = new WorkerThread(this);
    connect(workerThread, &WorkerThread::resultReady, this, &MyObject::handleResults);
    connect(workerThread, &WorkerThread::finished, workerThread, &QObject::deleteLater);
    workerThread->start();
}
```

在该示例中，线程将在运行函数返回后退出。除非你调用[exec](https://doc.qt.io/qt-5/qthread.html#exec)()，否则线程中不会运行任何事件循环。

重要的是要记住，[QThread](https://doc.qt.io/qt-5/qthread.html)实例[存在于](https://doc.qt.io/qt-5/qobject.html#thread-affinity)实例化它的旧线程中，而不是在调用[run](https://doc.qt.io/qt-5/qthread.html#run)()的新线程中。这意味着所有[QThread](https://doc.qt.io/qt-5/qthread.html)的排队槽和[调用的方法](https://doc.qt.io/qt-5/qmetaobject.html#invokeMethod)都将在旧线程中执行。因此，希望在新线程中调用插槽的开发人员必须使用工作者 - 对象方法; 不应将新插槽直接实现到子类[QThread中](https://doc.qt.io/qt-5/qthread.html)。

与排队的槽或调用的方法不同，直接在[QThread](https://doc.qt.io/qt-5/qthread.html)对象上调用的方法将在调用该方法的线程中执行。在[继承QThread时](https://doc.qt.io/qt-5/qthread.html)，请记住构造函数在旧线程中执行，而[run](https://doc.qt.io/qt-5/qthread.html#run)()在新线程中执行。如果从两个函数访问成员变量，则从两个不同的线程访问该变量。检查这样做是否安全。

**注意：**在跨不同线程的对象交互时必须小心。有关详细信息，请参阅[同步线程](https://doc.qt.io/qt-5/threads-synchronizing.html)。



### Managing Threads

当线程[启动](https://doc.qt.io/qt-5/qthread.html#started)()和[完成](https://doc.qt.io/qt-5/qthread.html#finished)()时，[QThread](https://doc.qt.io/qt-5/qthread.html)将通过信号通知您，或者您可以使用[isFinished](https://doc.qt.io/qt-5/qthread.html#isFinished)()和[isRunning](https://doc.qt.io/qt-5/qthread.html#isRunning)()来查询线程的状态。

您可以通过调用[exit](https://doc.qt.io/qt-5/qthread.html#exit)()或[quit](https://doc.qt.io/qt-5/qthread.html#quit)()来停止该线程。在极端情况下，您可能希望强制[终止](https://doc.qt.io/qt-5/qthread.html#terminate)()正在执行的线程。但是，这样做是危险和沮丧的。有关详细信息，请阅读[terminate](https://doc.qt.io/qt-5/qthread.html#terminate)()和[setTerminationEnabled](https://doc.qt.io/qt-5/qthread.html#setTerminationEnabled)()的文档。

从Qt 4.8开始，通过将[finished](https://doc.qt.io/qt-5/qthread.html#finished)()信号连接到[QObject :: deleteLater](https://doc.qt.io/qt-5/qobject.html#deleteLater)()，可以释放生活在刚刚结束的线程中的对象。

使用[wait](https://doc.qt.io/qt-5/qthread.html#wait)()来阻塞调用线程，直到另一个线程完成执行（或直到指定的时间已经过去）。

[QThread](https://doc.qt.io/qt-5/qthread.html)还提供静态的，独立于平台的睡眠功能：[sleep](https://doc.qt.io/qt-5/qthread.html#sleep)()，[msleep](https://doc.qt.io/qt-5/qthread.html#msleep)()和[usleep](https://doc.qt.io/qt-5/qthread.html#usleep)()分别允许完整的秒，毫秒和微秒分辨率。这些功能在Qt 5.0中公开。

**注意：**一般来说，[wait](https://doc.qt.io/qt-5/qthread.html#wait)()和[sleep](https://doc.qt.io/qt-5/qthread.html#sleep)()函数是不必要的，因为Qt是一个事件驱动的框架。而不是[wait](https://doc.qt.io/qt-5/qthread.html#wait)()，考虑监听[finished](https://doc.qt.io/qt-5/qthread.html#finished)()信号。而不是[sleep](https://doc.qt.io/qt-5/qthread.html#sleep)()函数，请考虑使用[QTimer](https://doc.qt.io/qt-5/qtimer.html)。

静态函数[currentThreadId](https://doc.qt.io/qt-5/qthread.html#currentThreadId)()和[currentThread](https://doc.qt.io/qt-5/qthread.html#currentThread)()返回当前正在执行的线程的标识符。前者返回线程的特定于平台的ID; 后者返回一个[QThread](https://doc.qt.io/qt-5/qthread.html)指针。

要选择给出线程的名称（例如，由`ps -L`Linux上的命令标识），可以在启动线程之前调用[setObjectName()](https://doc.qt.io/qt-5/qobject.html#objectName-prop)。如果不调用[setObjectName()](https://doc.qt.io/qt-5/qobject.html#objectName-prop)，则为线程指定的名称将是线程对象的运行时类型的类名（例如，`"RenderThread"`在[Mandelbrot示例](https://doc.qt.io/qt-5/qtcore-threads-mandelbrot-example.html)的情况下，因为它是[QThread](https://doc.qt.io/qt-5/qthread.html)子类的名称））。请注意，目前在Windows上的发布版本中不可用。

**See also** [Thread Support in Qt](https://doc.qt.io/qt-5/threads.html), [QThreadStorage](https://doc.qt.io/qt-5/qthreadstorage.html), [Synchronizing Threads](https://doc.qt.io/qt-5/threads-synchronizing.html), [Mandelbrot Example](https://doc.qt.io/qt-5/qtcore-threads-mandelbrot-example.html), [Semaphores Example](https://doc.qt.io/qt-5/qtcore-threads-semaphores-example.html), and [Wait Conditions Example](https://doc.qt.io/qt-5/qtcore-threads-waitconditions-example.html).



## Member Type Documentation

### enum QThread::Priority

此枚举类型指示操作系统应如何计划新创建的线程。

| Constant                        | Value | Description                              |
| ------------------------------- | :---: | ---------------------------------------- |
| `QThread::IdlePriority`         |  `0`  | 仅在没有其他线程正在运行时调度。         |
| `QThread::LowestPriority`       |  `1`  | 预定的次数少于LowPriority。              |
| `QThread::LowPriority`          |  `2`  | 调度次数少于NormalPriority。             |
| `QThread::NormalPriority`       |  `3`  | 操作系统的默认优先级。                   |
| `QThread::HighPriority`         |  `4`  | 调度比NormalPriority更频繁。             |
| `QThread::HighestPriority`      |  `5`  | 比HighPriority更经常安排。               |
| `QThread::TimeCriticalPriority` |  `6`  | 尽可能经常安排。                         |
| `QThread::InheritPriority`      |  `7`  | 使用与创建线程相同的优先级。这是默认值。 |



## Member Function Documentation

### QThread::QThread([QObject](https://doc.qt.io/qt-5/qobject.html#QObject) **parent* = nullptr)

构造一个新的[QThread](https://doc.qt.io/qt-5/qthread.html)来管理一个新线程。该*家长*采取的所有权[的QThread](https://doc.qt.io/qt-5/qthread.html)。在调用[start](https://doc.qt.io/qt-5/qthread.html#start)()之前，线程不会开始执行。

**See also** [start](https://doc.qt.io/qt-5/qthread.html#start)().

### `[virtual]`QThread::~QThread()

Destroys the [QThread](https://doc.qt.io/qt-5/qthread.html).

请注意，删除[QThread](https://doc.qt.io/qt-5/qthread.html)对象不会停止执行它管理的线程。删除正在运行的[QThread](https://doc.qt.io/qt-5/qthread.html)（即[isFinished](https://doc.qt.io/qt-5/qthread.html#isFinished)()返回`false`）将导致程序崩溃。在删除[QThread](https://doc.qt.io/qt-5/qthread.html)之前等待[finished](https://doc.qt.io/qt-5/qthread.html#finished)()信号。

### `[static]`[QThread](https://doc.qt.io/qt-5/qthread.html#QThread) *QThread::create(Function &&*f*, Args &&... *args*)

创建一个新的[QThread](https://doc.qt.io/qt-5/qthread.html)对象，该对象将使用参数*args*执行函数*f*。

新线程未启动 - 必须通过显式调用[start](https://doc.qt.io/qt-5/qthread.html#start)()来[启动](https://doc.qt.io/qt-5/qthread.html#start)。这允许您连接到它的信号，将QObjects移动到线程，选择新线程的优先级等等。函数*f*将在新线程中调用。

返回新创建的[QThread](https://doc.qt.io/qt-5/qthread.html)实例。

**注意：**调用者获取返回的[QThread](https://doc.qt.io/qt-5/qthread.html)实例的所有权。

**注意：**此功能仅在使用C ++ 17时可用。

**警告：**不要多次调用返回的[QThread](https://doc.qt.io/qt-5/qthread.html)实例上的[start](https://doc.qt.io/qt-5/qthread.html#start)(); 这样做会导致未定义的行为。

该功能在Qt 5.10中引入。

**另请参见**[start](https://doc.qt.io/qt-5/qthread.html#start)()。

### `[static]`[QThread](https://doc.qt.io/qt-5/qthread.html#QThread) *QThread::create(Function &&*f*)

创建一个将执行函数*f*的新[QThread](https://doc.qt.io/qt-5/qthread.html)对象。

新线程未启动 - 必须通过显式调用[start](https://doc.qt.io/qt-5/qthread.html#start)()来[启动](https://doc.qt.io/qt-5/qthread.html#start)。这允许您连接到它的信号，将QObjects移动到线程，选择新线程的优先级等等。函数*f*将在新线程中调用。

返回新创建的[QThread](https://doc.qt.io/qt-5/qthread.html)实例。

**注意：**调用者获取返回的[QThread](https://doc.qt.io/qt-5/qthread.html)实例的所有权。

**警告：**不要多次调用返回的[QThread](https://doc.qt.io/qt-5/qthread.html)实例上的[start](https://doc.qt.io/qt-5/qthread.html#start)(); 这样做会导致未定义的行为。

该功能在Qt 5.10中引入。

**另请参见**[start](https://doc.qt.io/qt-5/qthread.html#start)()。

### `[static]`[QThread](https://doc.qt.io/qt-5/qthread.html#QThread) *QThread::currentThread()

返回指向[QThread](https://doc.qt.io/qt-5/qthread.html)的指针，该[QThread](https://doc.qt.io/qt-5/qthread.html)管理当前正在执行的线程。

### `[static]`[Qt::HANDLE](https://doc.qt.io/qt-5/qt.html#HANDLE-typedef) QThread::currentThreadId()

返回当前正在执行的线程的线程句柄。

**警告：**此函数返回的句柄用于内部目的，不应在任何应用程序代码中使用。

**注意：**在Windows上，此函数返回Win32函数GetCurrentThreadId()返回的DWORD（Windows-Thread ID），而不是Win32函数GetCurrentThread()返回的伪HANDLE（Windows-Thread HANDLE）。

### `[override virtual]`bool QThread::event([QEvent](https://doc.qt.io/qt-5/qevent.html) **event*)

Reimplemented from [QObject::event](https://doc.qt.io/qt-5/qobject.html#event)().

### [QAbstractEventDispatcher](https://doc.qt.io/qt-5/qabstracteventdispatcher.html) *QThread::eventDispatcher() const

返回指向线程的事件调度程序对象的指针。如果该线程不存在事件调度程序，则此函数返回0。

该功能在Qt 5.0中引入。

**See also** [setEventDispatcher](https://doc.qt.io/qt-5/qthread.html#setEventDispatcher)().

### `[protected]`int QThread::exec()

进入事件循环并等待直到调用[exit](https://doc.qt.io/qt-5/qthread.html#exit)()，返回传递给[exit](https://doc.qt.io/qt-5/qthread.html#exit)()的值。如果通过[quit](https://doc.qt.io/qt-5/qthread.html#quit)()调用[exit](https://doc.qt.io/qt-5/qthread.html#exit)()，则返回的值为0 。

此函数旨在从[run](https://doc.qt.io/qt-5/qthread.html#run)()内部调用。必须调用此函数来启动事件处理。

**See also** [quit](https://doc.qt.io/qt-5/qthread.html#quit)() and [exit](https://doc.qt.io/qt-5/qthread.html#exit)().

### void QThread::exit(int *returnCode* = 0)

告诉线程的事件循环以退出代码退出。

调用此函数后，线程将离开事件循环并从调用返回到[QEventLoop :: exec](https://doc.qt.io/qt-5/qeventloop.html#exec)()。该[QEventLoop :: EXEC](https://doc.qt.io/qt-5/qeventloop.html#exec)()函数返回*返回码*。

按照惯例，*returnCode*为0表示成功，任何非零值表示错误。

请注意，与同名的C库函数不同，此函数*确实*返回到调用者 - 它是停止的事件处理。

在此线程中不再启动QEventLoops，直到再次调用[QThread :: exec](https://doc.qt.io/qt-5/qthread.html#exec)()。如果[QThread :: exec](https://doc.qt.io/qt-5/qthread.html#exec)()中的eventloop 没有运行，那么下一次调用[QThread :: exec](https://doc.qt.io/qt-5/qthread.html#exec)()也会立即返回。

**See also** [quit](https://doc.qt.io/qt-5/qthread.html#quit)() and [QEventLoop](https://doc.qt.io/qt-5/qeventloop.html).

### `[signal]`void QThread::finished()

该信号在完成执行之前从关联线程发出。

发出此信号时，事件循环已停止运行。除延迟删除事件外，线程中不再处理任何事件。此信号可以连接到[QObject :: deleteLater](https://doc.qt.io/qt-5/qobject.html#deleteLater)()，以释放该线程中的对象。

**注意：**如果使用[terminate](https://doc.qt.io/qt-5/qthread.html#terminate)()终止关联的线程，则不确定从哪个线程发出此信号。

**注意：**这是一个私人信号。它可以用于信号连接，但不能由用户发出。

**See also** [started](https://doc.qt.io/qt-5/qthread.html#started)().

### `[static]`int QThread::idealThreadCount()

返回可在系统上运行的理想线程数。这样就可以查询系统中的实际和逻辑处理器内核的数量。如果无法检测到处理器核心数，则此函数返回1。

### bool QThread::isFinished() const

`true`线程完成后返回; 否则返回`false`。

**See also** [isRunning](https://doc.qt.io/qt-5/qthread.html#isRunning)().

### bool QThread::isInterruptionRequested() const

如果应该停止在此线程上运行的任务，则返回true。[requestInterruption](https://doc.qt.io/qt-5/qthread.html#requestInterruption)()可以请求中断。

此功能可用于使长时间运行的任务干净可中断。从不检查或操作此功能返回的值是安全的，但建议在长时间运行的功能中定期执行此操作。注意不要经常调用它，以保持低开销。

```
void long_task() {
     forever {
        if ( QThread::currentThread()->isInterruptionRequested() ) {
            return;
        }
    }
}
```

This function was introduced in Qt 5.2.

**See also** [currentThread](https://doc.qt.io/qt-5/qthread.html#currentThread)() and [requestInterruption](https://doc.qt.io/qt-5/qthread.html#requestInterruption)().

### bool QThread::isRunning() const

`true`如果线程正在运行则返回; 否则返回`false`。

**See also** [isFinished](https://doc.qt.io/qt-5/qthread.html#isFinished)().

### int QThread::loopLevel() const

返回线程的当前事件循环级别。

**注意：**这只能在线程本身内调用，即当它是当前线程时。

This function was introduced in Qt 5.5.

### `[static]`void QThread::msleep(unsigned long *msecs*)

强制当前线程休眠*msecs*毫秒。

如果您需要等待给定条件进行更改，请避免使用此功能。相反，将一个槽连接到指示更改的信号或使用事件处理程序（请参阅[QObject :: event](https://doc.qt.io/qt-5/qobject.html#event)()）。

**注意：**此功能不保证准确性。在重负载条件下，应用程序可能比*msecs*睡眠时间更长。某些操作系统可能会将*msecs*舍入到10 ms或15 ms。

**See also** [sleep](https://doc.qt.io/qt-5/qthread.html#sleep)() and [usleep](https://doc.qt.io/qt-5/qthread.html#usleep)().

### [QThread::Priority](https://doc.qt.io/qt-5/qthread.html#Priority-enum) QThread::priority() const

返回正在运行的线程的优先级。如果线程未运行，则此函数返回`InheritPriority`。

该功能在Qt 4.1中引入。

**See also** [Priority](https://doc.qt.io/qt-5/qthread.html#Priority-enum), [setPriority](https://doc.qt.io/qt-5/qthread.html#setPriority)(), and [start](https://doc.qt.io/qt-5/qthread.html#start)().

### `[slot]`void QThread::quit()

告诉线程的事件循环退出并返回代码0（成功）。相当于调用[QThread :: exit](https://doc.qt.io/qt-5/qthread.html#exit)（0）。

如果线程没有事件循环，则此函数不执行任何操作。

**See also** [exit](https://doc.qt.io/qt-5/qthread.html#exit)() and [QEventLoop](https://doc.qt.io/qt-5/qeventloop.html).

### void QThread::requestInterruption()

请求中断线程。该请求是建议性的，由线程上运行的代码来决定它是否以及如何根据此类请求执行操作。此函数不会停止在线程上运行的任何事件循环，也不会以任何方式终止它。

该功能在Qt 5.2中引入。

**See also** [isInterruptionRequested](https://doc.qt.io/qt-5/qthread.html#isInterruptionRequested)().

### `[virtual protected]`void QThread::run()

线程的起点。在调用[start](https://doc.qt.io/qt-5/qthread.html#start)()之后，新创建的线程调用此函数。默认实现只是调用[exec](https://doc.qt.io/qt-5/qthread.html#exec)()。

您可以重新实现此功能以便于高级线程管理。从此方法返回将结束线程的执行。

**See also** [start](https://doc.qt.io/qt-5/qthread.html#start)() and [wait](https://doc.qt.io/qt-5/qthread.html#wait)().

### void QThread::setEventDispatcher([QAbstractEventDispatcher](https://doc.qt.io/qt-5/qabstracteventdispatcher.html) **eventDispatcher*)

将线程的事件调度程序设置为*eventDispatcher*。只有当没有为该线程安装事件调度程序时，才可以执行此操作。也就是说，在使用[start](https://doc.qt.io/qt-5/qthread.html#start)()[启动](https://doc.qt.io/qt-5/qthread.html#start)线程之前，或者在主线程的情况下，在实例化[QCoreApplication](https://doc.qt.io/qt-5/qcoreapplication.html)之前。此方法获取对象的所有权。

该功能在Qt 5.0中引入。

**See also** [eventDispatcher](https://doc.qt.io/qt-5/qthread.html#eventDispatcher)().

### void QThread::setPriority([QThread::Priority](https://doc.qt.io/qt-5/qthread.html#Priority-enum) *priority*)

此函数设置正在运行的线程的*优先级*。如果线程没有运行，则此函数不执行任何操作并立即返回。使用[start](https://doc.qt.io/qt-5/qthread.html#start)()启动具有特定优先级的线程。

该*优先级*参数可以是在任何价值`QThread::Priority`，除了枚举`InheritPriorty`。

*优先级*参数的影响取决于操作系统的调度策略。特别是，*优先*将在不支持线程优先级系统被忽略（如在Linux上，看到http://linux.die.net/man/2/sched_setscheduler了解详细信息）。

该功能在Qt 4.1中引入。

**See also** [Priority](https://doc.qt.io/qt-5/qthread.html#Priority-enum), [priority](https://doc.qt.io/qt-5/qthread.html#priority)(), and [start](https://doc.qt.io/qt-5/qthread.html#start)().

### void QThread::setStackSize([uint](https://doc.qt.io/qt-5/qtglobal.html#uint-typedef) *stackSize*)

将线程的最大堆栈大小设置为*stackSize*。如果*stackSize*大于零，则最大堆栈大小设置为*stackSize*字节，否则最大堆栈大小由操作系统自动确定。

**警告：**大多数操作系统对线程堆栈大小设置了最小和最大限制。如果堆栈大小超出这些限制，则线程将无法启动。

**See also** [stackSize](https://doc.qt.io/qt-5/qthread.html#stackSize)().

### `[static protected]`void QThread::setTerminationEnabled(bool *enabled* = true)

根据*enabled*参数启用或禁用当前线程的终止。该线程必须由[QThread](https://doc.qt.io/qt-5/qthread.html)启动。

如果*enabled*为false，则禁用终止。对[QThread :: terminate](https://doc.qt.io/qt-5/qthread.html#terminate)()的未来调用将立即返回而不起作用。相反，终止延迟直到启用终止。

如果*enabled*为true，则启用终止。对[QThread :: terminate](https://doc.qt.io/qt-5/qthread.html#terminate)()的未来调用将正常终止该线程。如果终止已被延迟（即在终止禁用的情况下调用[QThread :: terminate](https://doc.qt.io/qt-5/qthread.html#terminate)()），则此函数将*立即*终止调用线程。请注意，在这种情况下，此函数不会返回。

**See also** [terminate](https://doc.qt.io/qt-5/qthread.html#terminate)().

### `[static]`void QThread::sleep(unsigned long *secs*)

强制当前线程睡眠*秒*秒。

如果您需要等待给定条件进行更改，请避免使用此功能。相反，将一个槽连接到指示更改的信号或使用事件处理程序（请参阅[QObject :: event](https://doc.qt.io/qt-5/qobject.html#event)()）。

**注意：**此功能不保证准确性。在重负载条件下，应用程序可能会睡眠时间超过*秒*。

**See also** [msleep](https://doc.qt.io/qt-5/qthread.html#msleep)() and [usleep](https://doc.qt.io/qt-5/qthread.html#usleep)().

### [uint](https://doc.qt.io/qt-5/qtglobal.html#uint-typedef) QThread::stackSize() const

返回线程的最大堆栈大小（如果使用[setStackSize](https://doc.qt.io/qt-5/qthread.html#setStackSize)()设置）; 否则返回零。

**See also** [setStackSize](https://doc.qt.io/qt-5/qthread.html#setStackSize)().

### `[slot]`void QThread::start([QThread::Priority](https://doc.qt.io/qt-5/qthread.html#Priority-enum) *priority* = InheritPriority)

通过调用[run](https://doc.qt.io/qt-5/qthread.html#run)()开始执行该线程。操作系统将根据*优先级*参数调度线程。如果线程已在运行，则此函数不执行任何操作。

*优先级*参数的影响取决于操作系统的调度策略。特别是，*优先*将在不支持线程优先级（如在Linux上，请参阅系统忽视[sched_setscheduler](http://linux.die.net/man/2/sched_setscheduler)更多细节文档）。

**See also** [run](https://doc.qt.io/qt-5/qthread.html#run)() and [terminate](https://doc.qt.io/qt-5/qthread.html#terminate)().

### `[signal]`void QThread::started()

在调用[run](https://doc.qt.io/qt-5/qthread.html#run)()函数之前，此信号在开始执行时从关联的线程发出。

**注意：**这是一个私人信号。它可以用于信号连接，但不能由用户发出。

**See also** [finished](https://doc.qt.io/qt-5/qthread.html#finished)().

### `[slot]`void QThread::terminate()

终止线程的执行。根据操作系统的调度策略，线程可能会立即终止也可能不会立即终止。在terminate()之后使用[QThread :: wait](https://doc.qt.io/qt-5/qthread.html#wait)()，以确保。

当线程终止时，等待线程完成的所有线程都将被唤醒。

**警告：**此功能很危险，不鼓励使用。线程可以在其代码路径中的任何位置终止。修改数据时可以终止线程。线程无法自行清理，解锁任何保持的互斥锁等。简而言之，只有在绝对必要时才使用此功能。

可以通过调用[QThread :: setTerminationEnabled](https://doc.qt.io/qt-5/qthread.html#setTerminationEnabled)()显式启用或禁用终止。在终止被禁用时调用此函数会导致终止延迟，直到重新启用终止。有关更多信息，请参阅[QThread :: setTerminationEnabled](https://doc.qt.io/qt-5/qthread.html#setTerminationEnabled)()的文档。

**See also** [setTerminationEnabled](https://doc.qt.io/qt-5/qthread.html#setTerminationEnabled)().

### `[static]`void QThread::usleep(unsigned long *usecs*)

强制当前线程睡眠*微秒（usecs）*微秒。

如果您需要等待给定条件进行更改，请避免使用此功能。相反，将一个槽连接到指示更改的信号或使用事件处理程序（请参阅[QObject :: event](https://doc.qt.io/qt-5/qobject.html#event)()）。

**注意：**此功能不保证准确性。在重负载条件下，应用程序可能比*usecs*睡眠时间更长。某些操作系统可能会*使用*最多10毫秒或15毫秒的*usecs* ; 在Windows上，它将向上舍入为1毫秒的倍数。

**See also** [sleep](https://doc.qt.io/qt-5/qthread.html#sleep)() and [msleep](https://doc.qt.io/qt-5/qthread.html#msleep)().

### bool QThread::wait(unsigned long *time* = ULONG_MAX)

阻止线程，直到满足以下任一条件：

- 与此[QThread](https://doc.qt.io/qt-5/qthread.html)对象关联的线程已完成执行（即从[run](https://doc.qt.io/qt-5/qthread.html#run)()返回时）。如果线程已完成，此函数将返回true。如果线程尚未启动，它也会返回true。
- *时间*毫秒已过。如果*时间*是ULONG_MAX（默认值），那么等待将永远不会超时（线程必须从[run](https://doc.qt.io/qt-5/qthread.html#run)()返回）。如果等待超时，此函数将返回false。

这提供了与POSIX `pthread_join()`功能类似的功能。

**See also** [sleep](https://doc.qt.io/qt-5/qthread.html#sleep)() and [terminate](https://doc.qt.io/qt-5/qthread.html#terminate)().

### `[static]`void QThread::yieldCurrentThread()

如果有的话，将当前线程的执行产生到另一个可运行的线程。请注意，操作系统决定切换到哪个线程。