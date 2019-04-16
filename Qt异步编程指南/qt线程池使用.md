# QThreadPool Class

该[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)类管理QThreads的集合. [More...](https://doc.qt.io/qt-5/qthreadpool.html#details)

|           |                                                |
| --------- | ---------------------------------------------- |
| Header:   | #include < QThreadPool >                       |
| qmake:    | QT += core                                     |
| Since:    | Qt 4.4                                         |
| Inherits: | [QObject](https://doc.qt.io/qt-5/qobject.html) |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qthreadpool-members.html)
- [Obsolete members](https://doc.qt.io/qt-5/qthreadpool-obsolete.html)

**Note:** All functions in this class are [thread-safe](https://doc.qt.io/qt-5/threads-reentrancy.html).





## Properties

- **activeThreadCount** : const int
- **expiryTimeout** : int
- **maxThreadCount** : int
- **stackSize** : uint

- 1 property inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#properties)





## Public Functions

|         | **QThreadPool**(QObject **parent* = nullptr)         |
| ------- | ---------------------------------------------------- |
| virtual | **~QThreadPool**()                                   |
| int     | **activeThreadCount**() const                        |
| void    | **clear**()                                          |
| int     | **expiryTimeout**() const                            |
| int     | **maxThreadCount**() const                           |
| void    | **releaseThread**()                                  |
| void    | **reserveThread**()                                  |
| void    | **setExpiryTimeout**(int *expiryTimeout*)            |
| void    | **setMaxThreadCount**(int *maxThreadCount*)          |
| void    | **setStackSize**(uint *stackSize*)                   |
| uint    | **stackSize**() const                                |
| void    | **start**(QRunnable **runnable*, int *priority* = 0) |
| bool    | **tryStart**(QRunnable **runnable*)                  |
| bool    | **tryTake**(QRunnable **runnable*)                   |
| bool    | **waitForDone**(int *msecs* = -1)                    |

- 32 public functions inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#public-functions)





## Static Public Members

| QThreadPool *     | **globalInstance**() |
| ----------------- | -------------------- |
| const QMetaObject | **staticMetaObject** |

- 10 static public members inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#static-public-members)

### Additional Inherited Members

- 1 public slot inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#public-slots)
- 2 signals inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#signals)
- 9 protected functions inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#protected-functions)



## Detailed Description

该[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)类管理QThreads的集合。

[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)管理和重新处理各个[QThread](https://doc.qt.io/qt-5/qthread.html)对象，以帮助减少使用线程的程序中的线程创建成本。每个Qt应用程序都有一个全局[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)对象，可以通过调用[globalInstance](https://doc.qt.io/qt-5/qthreadpool.html#globalInstance)()来访问它。

要使用其中一个[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)线程，请[继承QRunnable](https://doc.qt.io/qt-5/qrunnable.html)并实现run()虚函数。然后创建该类的对象并将其传递给[QThreadPool :: start](https://doc.qt.io/qt-5/qthreadpool.html#start)()。

```c++
class HelloWorldTask : HelloWorldTask : public QRunnable
{{
    void run() override() override
    {{
        qDebug()  << "Hello world from thread" << QThread::currentThread();();
    }}
};};

HelloWorldTask HelloWorldTask *hello = new HelloWorldTask();
HelloWorldTask();
// QThreadPool takes ownership and deletes 'hello' automatically
QThreadPool::globalInstance()()->start(hello);(hello);
```

[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)默认自动删除[QRunnable](https://doc.qt.io/qt-5/qrunnable.html)。使用[QRunnable :: setAutoDelete](https://doc.qt.io/qt-5/qrunnable.html#setAutoDelete)()更改自动删除标志。

[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)支持通过从[QRunnable :: run](https://doc.qt.io/qt-5/qrunnable.html#run)()中调用[tryStart](https://doc.qt.io/qt-5/qthreadpool.html#tryStart)（this）多次执行相同的[QRunnable](https://doc.qt.io/qt-5/qrunnable.html)。如果启用了autoDelete，则当最后一个线程退出运行功能时，将删除[QRunnable](https://doc.qt.io/qt-5/qrunnable.html)。启用autoDelete时，使用相同的[QRunnable](https://doc.qt.io/qt-5/qrunnable.html)多次调用[start](https://doc.qt.io/qt-5/qthreadpool.html#start)()会创建竞争条件，不建议这样做。

在一段时间内未使用的线程将过期。默认的到期超时为30000毫秒（30秒）。这可以使用[setExpiryTimeout](https://doc.qt.io/qt-5/qthreadpool.html#expiryTimeout-prop)()更改。设置负到期超时会禁用到期机制。

调用[maxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()以查询要使用的最大线程数。如果需要，您可以使用[setMaxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()更改限制。默认的[maxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()是[QThread :: idealThreadCount](https://doc.qt.io/qt-5/qthread.html#idealThreadCount)()。该[activeThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#activeThreadCount-prop)()函数返回目前做的线程数工作。

该[reserveThread](https://doc.qt.io/qt-5/qthreadpool.html#reserveThread)()函数保留外用线程。完成线程后使用[releaseThread](https://doc.qt.io/qt-5/qthreadpool.html#releaseThread)()，以便可以重用它。实质上，这些函数会临时增加或减少活动线程数，在实现[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)不可见的耗时操作时非常有用。

请注意，[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)是用于管理线程的低级类，请参阅Qt Concurrent模块以获取更高级别的替代方案。

**See also** [QRunnable](https://doc.qt.io/qt-5/qrunnable.html).



## Property Documentation



### activeThreadCount : const int

此属性表示线程池中活动线程的数量。

**注意：**此函数可以返回大于[maxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()的值。有关更多详细信息，请参阅[reserveThread](https://doc.qt.io/qt-5/qthreadpool.html#reserveThread)()。

**Access functions:**

|      |                               |
| ---- | ----------------------------- |
| int  | **activeThreadCount**() const |

**See also** [reserveThread](https://doc.qt.io/qt-5/qthreadpool.html#reserveThread)() and [releaseThread](https://doc.qt.io/qt-5/qthreadpool.html#releaseThread)().



### expiryTimeout : int

未使用*expiryTimeout*毫秒的线程被视为已过期并将退出。这些线程将根据需要重新启动。默认的*expiryTimeout*是30000毫秒（30秒）。如果*expiryTimeout*为负，则新创建的线程将不会到期，例如，在线程池被销毁之前它们不会退出。

请注意，设置*expiryTimeout*对已经运行的线程没有影响。只有新创建的线程才会使用新的*expiryTimeout*。我们建议在创建线程池之后立即设置*expiryTimeout*，但在调用[start](https://doc.qt.io/qt-5/qthreadpool.html#start)()之前。

**Access functions:**

| int  | **expiryTimeout**() const                 |
| ---- | ----------------------------------------- |
| void | **setExpiryTimeout**(int *expiryTimeout*) |



### maxThreadCount : int

此属性表示线程池使用的最大线程数。

**注意：**即使*maxThreadCount*限制为零或为负，线程池也将始终使用至少1个线程。

默认的*maxThreadCount*是[QThread :: idealThreadCount](https://doc.qt.io/qt-5/qthread.html#idealThreadCount)()。

**Access functions:**

| int  | **maxThreadCount**() const                  |
| ---- | ------------------------------------------- |
| void | **setMaxThreadCount**(int *maxThreadCount*) |



### stackSize : [uint](https://doc.qt.io/qt-5/qtglobal.html#uint-typedef)

此属性包含线程池工作线程的堆栈大小。

仅在线程池创建新线程时使用该属性的值。更改它对已创建或正在运行的线程没有影响。

默认值为0，这使[QThread](https://doc.qt.io/qt-5/qthread.html)使用操作系统默认堆栈大小。

该属性在Qt 5.10中引入。

**Access functions:**

| uint | **stackSize**() const              |
| ---- | ---------------------------------- |
| void | **setStackSize**(uint *stackSize*) |



## Member Function Documentation



### QThreadPool::QThreadPool([QObject](https://doc.qt.io/qt-5/qobject.html#QObject) **parent* = nullptr)

使用给定*父级*构造一个线程池。



### `[virtual]`QThreadPool::~QThreadPool()

销毁[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)。此函数将阻塞，直到所有runnables都已完成。



### void QThreadPool::clear()

删除尚未从队列中启动的runnable。在可运行于其中[runnable->自动删除()](https://doc.qt.io/qt-5/qrunnable.html#autoDelete)返回`true`被删除。

该功能在Qt 5.2中引入。

**See also** [start](https://doc.qt.io/qt-5/qthreadpool.html#start)().



### `[static]`[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html#QThreadPool) *QThreadPool::globalInstance()

返回全局[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)实例。



### void QThreadPool::releaseThread()

释放先前通过调用[reserveThread](https://doc.qt.io/qt-5/qthreadpool.html#reserveThread)()保留的线程。

**注意：**调用此函数而不事先保留线程会暂时增加[maxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()。当线程进入休眠状态等待更多工作时，这很有用，允许其他线程继续。确保在完成等待时调用[reserveThread](https://doc.qt.io/qt-5/qthreadpool.html#reserveThread)()，以便线程池可以正确维护[activeThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#activeThreadCount-prop)()。

**See also** [reserveThread](https://doc.qt.io/qt-5/qthreadpool.html#reserveThread)().



### void QThreadPool::reserveThread()

保留一个线程，忽略[activeThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#activeThreadCount-prop)()和[maxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()。

完成线程后，调用[releaseThread](https://doc.qt.io/qt-5/qthreadpool.html#releaseThread)()以允许重用它。

**注意：**此函数将始终增加活动线程的数量。这意味着通过使用此函数，[activeThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#activeThreadCount-prop)()可以返回大于[maxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()的值。

**See also** [releaseThread](https://doc.qt.io/qt-5/qthreadpool.html#releaseThread)().



### void QThreadPool::start([QRunnable](https://doc.qt.io/qt-5/qrunnable.html) **runnable*, int *priority* = 0)

保留一个线程并使用它来运行*runnable*，除非该线程使当前线程数超过[maxThreadCount](https://doc.qt.io/qt-5/qthreadpool.html#maxThreadCount-prop)()。在这种情况下，*runnable*会添加到运行队列中。该*优先级*参数可以用来控制执行的运行队列的顺序。

需要注意的是线程池采取的所有权*可运行*如果[runnable->自动删除()](https://doc.qt.io/qt-5/qrunnable.html#autoDelete)返回`true`，以及*可运行*会自动线程池后删除[runnable-> run()的](https://doc.qt.io/qt-5/qrunnable.html#run)回报。如果[runnable-> autoDelete()](https://doc.qt.io/qt-5/qrunnable.html#autoDelete)返回`false`，则*runnable的*所有权将保留在调用者处。请注意，在调用此函数后更改*runnable*上的自动删除会导致未定义的行为。



### bool QThreadPool::tryStart([QRunnable](https://doc.qt.io/qt-5/qrunnable.html) **runnable*)

尝试保留线程以运行*runnable*。

如果在调用时没有可用的线程，则此函数不执行任何操作并返回`false`。否则，使用一个可用线程立即*运行runnable，*并返回此函数`true`。

需要注意的是线程池采取的所有权*可运行*如果[runnable->自动删除()](https://doc.qt.io/qt-5/qrunnable.html#autoDelete)返回`true`，以及*可运行*会自动线程池后删除[runnable-> run()的](https://doc.qt.io/qt-5/qrunnable.html#run)回报。如果[runnable-> autoDelete()](https://doc.qt.io/qt-5/qrunnable.html#autoDelete)返回`false`，则*runnable的*所有权将保留在调用者处。请注意，在调用此函数后更改*runnable*上的自动删除会导致未定义的行为。



### bool QThreadPool::tryTake([QRunnable](https://doc.qt.io/qt-5/qrunnable.html) **runnable*)

尝试从队列中删除指定的*runnable*（如果尚未启动）。如果runnable尚未启动，则返回`true`，*runnable的*所有权将转移给调用者（即使是`runnable->autoDelete() == true`）。否则返回`false`。

**注意：**如果`runnable->autoDelete() == true`，此函数可能会删除错误的runnable。这称为[ABA问题](https://en.wikipedia.org/wiki/ABA_problem)：原始*runnable*可能已经执行并且已被删除。内存被重新用于另一个runnable，然后被删除而不是预期的。因此，我们建议仅对不能自动删除的runnable调用此函数。

该功能在Qt 5.9中引入。

**See also** [start](https://doc.qt.io/qt-5/qthreadpool.html#start)() and [QRunnable::autoDelete](https://doc.qt.io/qt-5/qrunnable.html#autoDelete)().



### bool QThreadPool::waitForDone(int *msecs* = -1)

等待所有线程退出的最多*msecs*毫秒，并从线程池中删除所有线程。返回`true`所有线程都被删除; 否则它会返回`false`。如果*msecs*为-1（默认值），则忽略超时（等待最后一个线程退出）。