[TOC]



# Qt Concurrent



## Getting Started

该[QtConcurrent](https://doc.qt.io/qt-5/qtconcurrent-module.html)命名空间提供高层次的API，使人们可以编写多线程程序，而无需使用低级线程原语，如互斥，读写锁，等待条件或信号。使用[QtConcurrent](https://doc.qt.io/qt-5/qtconcurrent-module.html)编写的程序会根据可用的处理器核心数自动调整使用的线程数。这意味着今后编写的应用程序将在未来部署在多核系统上时继续扩展。

[QtConcurrent](https://doc.qt.io/qt-5/qtconcurrent-module.html)包括用于并行列表处理的函数式编程样式API，包括用于共享内存（非分布式）系统的MapReduce和FilterReduce实现，以及用于管理GUI应用程序中的异步计算的类：

- Concurrent Map and Map-Reduce
  - [QtConcurrent::map()](https://doc.qt.io/qt-5/qtconcurrent.html#map) 将函数应用于容器中的每个项目，并就地修改项目。
  - [QtConcurrent::mapped()](https://doc.qt.io/qt-5/qtconcurrent.html#mapped) 与map()类似，不同之处在于它返回带有修改的新容器。
  - [QtConcurrent::mappedReduced()](https://doc.qt.io/qt-5/qtconcurrent.html#mappedReduced) 与mapped()类似，只是将修改后的结果缩小或折叠成单个结果。
- Concurrent Filter and Filter-Reduce
  - [QtConcurrent::filter()](https://doc.qt.io/qt-5/qtconcurrent.html#filter) 根据过滤函数的结果从容器中删除所有项目。
  - [QtConcurrent::filtered()](https://doc.qt.io/qt-5/qtconcurrent.html#filtered) 与filter()类似，不同之处在于它返回一个带有过滤结果的新容器。
  - [QtConcurrent::filteredReduced()](https://doc.qt.io/qt-5/qtconcurrent.html#filteredReduced) 与filtered()类似，不同之处在于过滤后的结果被缩小或折叠成单个结果。
- Concurrent Run
  - [QtConcurrent::run()](https://doc.qt.io/qt-5/qtconcurrent.html#run) 在另一个线程中运行一个函数。
- [QFuture](https://doc.qt.io/qt-5/qfuture.html) 表示异步计算的结果。
- [QFutureIterator](https://doc.qt.io/qt-5/qfutureiterator.html) 允许经由可用的结果迭代[QFuture](https://doc.qt.io/qt-5/qfuture.html)。
- [QFutureWatcher](https://doc.qt.io/qt-5/qfuturewatcher.html) 允许使用信号和插槽监控[QFuture](https://doc.qt.io/qt-5/qfuture.html)。
- [QFutureSynchronizer](https://doc.qt.io/qt-5/qfuturesynchronizer.html) 是一个便捷类，可以自动同步多个QFutures。

Qt Concurrent支持几种兼容STL的容器和迭代器类型，但最适用于具有随机访问迭代器的Qt容器，例如[QList](https://doc.qt.io/qt-5/qlist.html)或[QVector](https://doc.qt.io/qt-5/qvector.html)。map和filter函数接受容器和开始/结束迭代器。

STL Iterator support overview:

| Iterator Type          | Example classes                                              | Support status            |
| ---------------------- | ------------------------------------------------------------ | ------------------------- |
| Input Iterator         |                                                              | Not Supported             |
| Output Iterator        |                                                              | Not Supported             |
| Forward Iterator       | std::slist                                                   | Supported                 |
| Bidirectional Iterator | [QLinkedList](https://doc.qt.io/qt-5/qlinkedlist.html), std::list | Supported                 |
| Random Access Iterator | [QList](https://doc.qt.io/qt-5/qlist.html), [QVector](https://doc.qt.io/qt-5/qvector.html), std::vector | Supported and Recommended |

在Qt Concurrent迭代大量轻量级项目的情况下，随机访问迭代器可以更快，因为它们允许跳转到容器中的任何点。此外，使用随机访问迭代器允许Qt Concurrent通过[QFuture :: progressValue](https://doc.qt.io/qt-5/qfuture.html#progressValue)()和[QFutureWatcher :: progressValueChanged](https://doc.qt.io/qt-5/qfuturewatcher.html#progressValueChanged)()提供进度信息。

非就地修改函数（例如mapped()和filtered()）在调用时生成容器的副本。如果您正在使用STL容器，则此复制操作可能需要一些时间，在这种情况下，我们建议为容器指定开始和结束迭代器。





# Concurrent Run



QtConcurrent :: run()函数在一个单独的线程中运行一个函数。该函数的返回值可通过[QFuture](https://doc.qt.io/qt-5/qfuture.html) API获得。

此函数是[Qt Concurrent](https://doc.qt.io/qt-5/qtconcurrent-index.html)框架的一部分。



## Running a Function in a Separate Thread

要在另一个线程中运行函数，请使用QtConcurrent :: run()：

```c++
extern void aFunction();
QFuture<void> future = QtConcurrent::run(aFunction);
```

这将运行*机能缺失*从默认获得一个单独的线程[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)。您可以使用[QFuture](https://doc.qt.io/qt-5/qfuture.html)和[QFutureWatcher](https://doc.qt.io/qt-5/qfuturewatcher.html)类来监视函数的状态。

要使用专用线程池，可以将[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)作为第一个参数传递：

```c++
extern void aFunction();
QThreadPool pool;
QFuture<void> future = QtConcurrent::run(&pool, aFunction);
```



## Passing Arguments to the Function

将参数传递给函数是通过在函数名后立即将它们添加到QtConcurrent :: run()调用来完成的。例如：

```c++
extern void aFunctionWithArguments(int arg1, double arg2, const QString &string);

int integer = ...;
double floatingPoint = ...;
QString string = ...;

QFuture<void> future = QtConcurrent::run(aFunctionWithArguments, integer, floatingPoint, string);
```

每个参数的副本都在调用QtConcurrent :: run()的位置进行，并且这些值在开始执行函数时传递给线程。调用QtConcurrent :: run()后对参数所做的更改对于线程是*不*可见的。



## Returning Values from the Function

函数的任何返回值都可以通过[QFuture获得](https://doc.qt.io/qt-5/qfuture.html)：

```c++
extern QString functionReturningAString();
QFuture<QString> future = QtConcurrent::run(functionReturningAString);
...
QString result = future.result();
```

如上所述，传递参数的方式如下：

```c++
extern QString someFunction(const QByteArray &input);

QByteArray bytearray = ...;

QFuture<QString> future = QtConcurrent::run(someFunction, bytearray);
...
QString result = future.result();
```

请注意，[QFuture :: result](https://doc.qt.io/qt-5/qfuture.html#result)()函数会阻止并等待结果可用。当函数执行完毕且结果可用时，使用[QFutureWatcher](https://doc.qt.io/qt-5/qfuturewatcher.html)获取通知。



## Additional API Features



### Using Member Functions

QtConcurrent :: run()也接受指向成员函数的指针。第一个参数必须是const引用或指向该类实例的指针。在调用const成员函数时，通过const引用传递是很有用的。传递指针对于调用修改实例的非const成员函数很有用。

例如，在单独的线程中调用[QByteArray :: split](https://doc.qt.io/qt-5/qbytearray.html#split)()（一个const成员函数）就像这样：

```c++
// call 'QList<QByteArray>  QByteArray::split(char sep) const' in a separate thread
QByteArray bytearray = "hello world";
QFuture<QList<QByteArray> > future = QtConcurrent::run(bytearray, &QByteArray::split, ',');
...
QList<QByteArray> result = future.result();
```

调用非const成员函数是这样的：

```c++
// call 'void QImage::invertPixels(InvertMode mode)' in a separate thread
QImage image = ...;
QFuture<void> future = QtConcurrent::run(&image, &QImage::invertPixels, QImage::InvertRgba);
...
future.waitForFinished();
// At this point, the pixels in 'image' have been inverted
```



### Using Lambda Functions

调用lambda函数是这样的：

```c++
QFuture<void> future = QtConcurrent::run([=]() {
    // Code in this block will run in another thread
});
...
```







# QtConcurrent Namespace

该[QtConcurrent](https://doc.qt.io/qt-5/qtconcurrent-module.html)命名空间提供高层次的API，使人们可以编写多线程程序，而无需使用低级线程原语。 [More...](https://doc.qt.io/qt-5/qtconcurrent.html#details)

|         |                         |
| ------- | ----------------------- |
| Header: | #include <QtConcurrent> |
| qmake:  | QT += concurrent        |
| Since:  | Qt 4.4                  |

- [Obsolete members](https://doc.qt.io/qt-5/qtconcurrent-obsolete.html)



## Classes

|       |                         |
| ----- | ----------------------- |
| class | **IntermediateResults** |



## Types

|       |                                                              |
| ----- | ------------------------------------------------------------ |
| enum  | **ReduceOption** { UnorderedReduce, OrderedReduce, SequentialReduce } |
| flags | **ReduceOptions**                                            |



## Functions

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| void                                                         | **blockingFilter**(Sequence &*sequence*, KeepFunctor *filterFunction*) |
| Sequence                                                     | **blockingFiltered**(const Sequence &*sequence*, KeepFunctor *filterFunction*) |
| OutputSequence                                               | **blockingFiltered**(Iterator *begin*, Iterator *end*, KeepFunctor *filterFunction*) |
| ResultType                                                   | **blockingFilteredReduced**(const Sequence &*sequence*, KeepFunctor *filterFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| ResultType                                                   | **blockingFilteredReduced**(Iterator *begin*, Iterator *end*, KeepFunctor *filterFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| void                                                         | **blockingMap**(Sequence &*sequence*, MapFunctor *function*) |
| void                                                         | **blockingMap**(Iterator *begin*, Iterator *end*, MapFunctor *function*) |
| OutputSequence                                               | **blockingMapped**(const InputSequence &*sequence*, MapFunctor *function*) |
| Sequence                                                     | **blockingMapped**(Iterator *begin*, Iterator *end*, MapFunctor *function*) |
| ResultType                                                   | **blockingMappedReduced**(const Sequence &*sequence*, MapFunctor *mapFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| ResultType                                                   | **blockingMappedReduced**(Iterator *begin*, Iterator *end*, MapFunctor *mapFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| QFuture<void>                                                | **filter**(Sequence &*sequence*, KeepFunctor *filterFunction*) |
| QFuture<typename Sequence::value_type>                       | **filtered**(const Sequence &*sequence*, KeepFunctor *filterFunction*) |
| QFuture<typename qValueType<Iterator>::value_type>           | **filtered**(Iterator *begin*, Iterator *end*, KeepFunctor *filterFunction*) |
| QFuture<ResultType>                                          | **filteredReduced**(const Sequence &*sequence*, KeepFunctor *filterFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| QFuture<ResultType>                                          | **filteredReduced**(Iterator *begin*, Iterator *end*, KeepFunctor *filterFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| QFuture<void>                                                | **map**(Sequence &*sequence*, MapFunctor *function*)         |
| QFuture<void>                                                | **map**(Iterator *begin*, Iterator *end*, MapFunctor *function*) |
| QFuture<typename QtPrivate::MapResultType<void, MapFunctor>::ResultType> | **mapped**(const Sequence &*sequence*, MapFunctor *function*) |
| QFuture<typename QtPrivate::MapResultType<void, MapFunctor>::ResultType> | **mapped**(Iterator *begin*, Iterator *end*, MapFunctor *function*) |
| QFuture<ResultType>                                          | **mappedReduced**(const Sequence &*sequence*, MapFunctor *mapFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| QFuture<ResultType>                                          | **mappedReduced**(Iterator *begin*, Iterator *end*, MapFunctor *mapFunction*, ReduceFunctor *reduceFunction*, QtConcurrent::ReduceOptions *reduceOptions* = ...) |
| QFuture<T>                                                   | **run**(Function *function*, *...*)                          |
| QFuture<T>                                                   | **run**(QThreadPool **pool*, Function *function*, *...*)     |





## Detailed Description

该[QtConcurrent](https://doc.qt.io/qt-5/qtconcurrent-module.html)命名空间提供高层次的API，使人们可以编写多线程程序，而无需使用低级线程原语。

有关可用功能的概述，请参阅[Qt Concurrent](https://doc.qt.io/qt-5/qtconcurrent-index.html)模块文档，或有关每个功能的详细信息，请参见下文。



## Classes

### class [IntermediateResults](https://doc.qt.io/qt-5/qtconcurrent-intermediateresults.html)



## Type Documentation

### enum QtConcurrent::ReduceOption flags QtConcurrent::ReduceOptions

此枚举指定将map或filter函数的结果传递给reduce函数的顺序。

| Constant                         | Value | Description                                                  |
| -------------------------------- | ----- | ------------------------------------------------------------ |
| `QtConcurrent::UnorderedReduce`  | `0x1` | 减少以任意顺序完成。                                         |
| `QtConcurrent::OrderedReduce`    | `0x2` | 减少按原始序列的顺序进行。                                   |
| `QtConcurrent::SequentialReduce` | `0x4` | 减少按顺序进行：一次只有一个线程进入reduce函数。（Qt Concurrent的未来版本可能支持并行缩减。） |

ReduceOptions类型是[QFlags](https://doc.qt.io/qt-5/qflags.html) <ReduceOption> 的typedef 。它存储ReduceOption值的OR组合。



## Function Documentation



### void QtConcurrent::blockingFilter(Sequence &*sequence*, KeepFunctor *filterFunction*)

按*顺序*为每个项调用*filterFunction*一次。如果*filterFunction*返回`true`，则项目按*顺序*保存; 否则，该项目将从*序列中*删除。

**注意：**此功能将阻止，直到序列中的所有项目都已处理完毕。

**See also** [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### Sequence QtConcurrent::blockingFiltered(const Sequence &*sequence*, KeepFunctor*filterFunction*)

为*序列中的*每个项调用*filterFunction*一次，并返回一个新的保留项序列。如果*filterFunction*返回`true`，则*该项*的副本将放入新的Sequence中。否则，该项目将*不会*出现在新序列中。

**注意：**此功能将阻止，直到序列中的所有项目都已处理完毕。

**See also** [filtered](https://doc.qt.io/qt-5/qtconcurrent.html#filtered)() and [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### OutputSequence QtConcurrent::blockingFiltered(Iterator *begin*, Iterator *end*, KeepFunctor*filterFunction*)

调用*的filterFunction*一次从每个项目*开始*到*结束*，并返回保持项目的一个新的序列。如果*filterFunction*返回`true`，则*该项*的副本将放入新的Sequence中。否则，该项目将*不会*出现在新序列中。

**注意：**此函数将阻塞，直到迭代器到达正在处理的序列的末尾。

**See also** [filtered](https://doc.qt.io/qt-5/qtconcurrent.html#filtered)() and [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### ResultType QtConcurrent::blockingFilteredReduced(const Sequence &*sequence*, KeepFunctor *filterFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*reduceOptions* = ...)

按*顺序*为每个项调用*filterFunction*一次。如果*filterFunction*返回一个项目，那么该项目将传递给*reduceFunction*。换句话说，返回值是`true`的结果*reduceFunction*针对每个项目，其中*的filterFunction*返回`true`

需要注意的是，虽然*的filterFunction*被同时调用，只有一次一个线程将调用*reduceFunction*。如果*reduceOptions*是[QtConcurrent :: ](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*UnorderedReduce，*则调用*reduceFunction*的顺序是不确定的。如果*reduceOptions*是[QtConcurrent :: OrderedReduce](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)，*则按*原始序列的顺序调用*reduceFunction*。

**注意：**此功能将阻止，直到序列中的所有项目都已处理完毕。

**See also** [filteredReduced](https://doc.qt.io/qt-5/qtconcurrent.html#filteredReduced)() and [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### ResultType QtConcurrent::blockingFilteredReduced(Iterator *begin*, Iterator *end*, KeepFunctor*filterFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum) *reduceOptions* = ...)

调用*的filterFunction*一次从每个项目*开始*到*结束*。如果*filterFunction*返回`true`一个项目，那么该项目将传递给*reduceFunction*。换句话说，返回值是的结果*reduceFunction*针对每个项目，其中*的filterFunction*返回`true`。

需要注意的是，虽然*的filterFunction*被同时调用，只有一次一个线程将调用*reduceFunction*。如果*reduceOptions*是[QtConcurrent :: ](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*UnorderedReduce，*则调用*reduceFunction*的顺序是不确定的。如果*reduceOptions*是[QtConcurrent :: OrderedReduce](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)，*则按*原始序列的顺序调用*reduceFunction*。

**注意：**此函数将阻塞，直到迭代器到达正在处理的序列的末尾。

**See also** [filteredReduced](https://doc.qt.io/qt-5/qtconcurrent.html#filteredReduced)() and [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### void QtConcurrent::blockingMap(Sequence &*sequence*, MapFunctor *function*)

按*顺序*为每个项目调用一次*函数*。该*函数*传递对项目的引用，以便对项目所做的任何修改将按*顺序*出现。

**注意：**此功能将阻止，直到序列中的所有项目都已处理完毕。

**See also** [map](https://doc.qt.io/qt-5/qtconcurrent.html#map)() and [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### void QtConcurrent::blockingMap(Iterator *begin*, Iterator *end*, MapFunctor *function*)

呼叫*功能*从每个项目一旦*开始*到*结束*。该*函数*传递对该项的引用，以便对该项执行的任何修改将出现在迭代器所属的序列中。

**注意：**此函数将阻塞，直到迭代器到达正在处理的序列的末尾。

**See also** [map](https://doc.qt.io/qt-5/qtconcurrent.html#map)() and [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### OutputSequence QtConcurrent::blockingMapped(const InputSequence &*sequence*, MapFunctor *function*)

按*顺序*为每个项调用一次*函数*，并返回包含结果的OutputSequence。结果的类型将匹配我的MapFunctor返回的类型。

**注意：**此功能将阻止，直到序列中的所有项目都已处理完毕。

**See also** [mapped](https://doc.qt.io/qt-5/qtconcurrent.html#mapped)() and [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### Sequence QtConcurrent::blockingMapped(Iterator *begin*, Iterator *end*, MapFunctor*function*)

呼叫*功能*从每个项目一旦*开始*到*结束*，并返回结果的容器。将容器类型指定为模板参数，如下所示：

```c++
QList<int> ints = QtConcurrent::blockingMapped<QList<int> >(beginIterator, endIterator, fn);
```

**注意：**此函数将阻塞，直到迭代器到达正在处理的序列的末尾。

**See also** [mapped](https://doc.qt.io/qt-5/qtconcurrent.html#mapped)() and [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### ResultType QtConcurrent::blockingMappedReduced(const Sequence &*sequence*, MapFunctor *mapFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*reduceOptions* = ...)

按*顺序*为每个项调用*mapFunction*一次。每个*mapFunction*的返回值都传递给*reduceFunction*。

请注意，虽然*mapFunction*是并发调用的，但一次只有一个线程会调用*reduceFunction*。*reduceFunction*的调用顺序由*reduceOptions*决定。

**注意：**此功能将阻止，直到序列中的所有项目都已处理完毕。

**See also** [mapped](https://doc.qt.io/qt-5/qtconcurrent.html#mapped)() and [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### ResultType QtConcurrent::blockingMappedReduced(Iterator *begin*, Iterator *end*, MapFunctor*mapFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum) *reduceOptions* = ...)

呼吁*mapFunction*一次从每个项目*开始*到*结束*。每个*mapFunction*的返回值都传递给*reduceFunction*。

请注意，虽然*mapFunction*是并发调用的，但一次只有一个线程会调用*reduceFunction*。调用*reduceFunction*的顺序是未定义的。

**注意：**此函数将阻塞，直到迭代器到达正在处理的序列的末尾。

**See also** [blockingMappedReduced](https://doc.qt.io/qt-5/qtconcurrent.html#blockingMappedReduced)() and [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html) < void > QtConcurrent::filter(Sequence &*sequence*, KeepFunctor *filterFunction*)

按*顺序*为每个项调用*filterFunction*一次。如果*filterFunction*返回`true`，则项目按*顺序*保存; 否则，该项目将从*序列中*删除。

**See also** [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)<typename Sequence::value_type> QtConcurrent::filtered(const Sequence&*sequence*, KeepFunctor *filterFunction*)

为*序列中的*每个项调用*filterFunction*一次，并返回一个新的保留项序列。如果*filterFunction*返回`true`，则*该项*的副本将放入新的Sequence中。否则，该项目将*不会*出现在新序列中。

**See also** [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)<typename qValueType< Iterator >::value_type> QtConcurrent::filtered(Iterator*begin*, Iterator *end*, KeepFunctor *filterFunction*)

调用*的filterFunction*一次从每个项目*开始*到*结束*，并返回保持项目的一个新的序列。如果*filterFunction*返回`true`，则*该项*的副本将放入新的Sequence中。否则，该项目将*不会*出现在新序列中。

**See also** [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< ResultType > QtConcurrent::filteredReduced(const Sequence &*sequence*, KeepFunctor *filterFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*reduceOptions* = ...)

按*顺序*为每个项调用*filterFunction*一次。如果*filterFunction*返回一个项目，那么该项目将传递给*reduceFunction*。换句话说，返回值是的结果*reduceFunction*针对每个项目，其中*的filterFunction*返回。`true``true`

需要注意的是，虽然*的filterFunction*被同时调用，只有一次一个线程将调用*reduceFunction*。如果*reduceOptions*是[QtConcurrent :: ](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*UnorderedReduce，*则调用*reduceFunction*的顺序是不确定的。如果*reduceOptions*是[QtConcurrent :: OrderedReduce](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)，*则按*原始序列的顺序调用*reduceFunction*。

**See also** [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< ResultType > QtConcurrent::filteredReduced(Iterator *begin*, Iterator *end*, KeepFunctor *filterFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*reduceOptions* = ...)

调用*的filterFunction*一次从每个项目*开始*到*结束*。如果*filterFunction*返回`true`一个项目，那么该项目将传递给*reduceFunction*。换句话说，返回值是的结果*reduceFunction*针对每个项目，其中*的filterFunction*返回`true`。

需要注意的是，虽然*的filterFunction*被同时调用，只有一次一个线程将调用*reduceFunction*。如果*reduceOptions*是[QtConcurrent :: ](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*UnorderedReduce，*则调用*reduceFunction*的顺序是不确定的。如果*reduceOptions*是[QtConcurrent :: OrderedReduce](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)，*则按*原始序列的顺序调用*reduceFunction*。

**See also** [Concurrent Filter and Filter-Reduce](https://doc.qt.io/qt-5/qtconcurrentfilter.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< void > QtConcurrent::map(Sequence &*sequence*, MapFunctor *function*)

按*顺序*为每个项目调用一次*函数*。该*函数*传递对项目的引用，以便对项目所做的任何修改将按*顺序*出现。

**See also** [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< void > QtConcurrent::map(Iterator *begin*, Iterator *end*, MapFunctor *function*)

呼叫*功能*从每个项目一旦*开始*到*结束*。该*函数*传递对该项的引用，以便对该项执行的任何修改将出现在迭代器所属的序列中。

**See also** [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)<typename [QtPrivate::MapResultType](https://doc.qt.io/qt-5/qtprivate-mapresulttype.html)<void, MapFunctor>::ResultType> QtConcurrent::mapped(const Sequence &*sequence*, MapFunctor *function*)

对每个项目按*顺序*调用一次*函数*，并返回每个映射项目的未来。您可以使用[QFuture :: const_iterator](https://doc.qt.io/qt-5/qfuture-const-iterator.html)或[QFutureIterator](https://doc.qt.io/qt-5/qfutureiterator.html)迭代结果。

**See also** [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)<typename [QtPrivate::MapResultType](https://doc.qt.io/qt-5/qtprivate-mapresulttype.html)<void, MapFunctor>::ResultType> QtConcurrent::mapped(Iterator *begin*, Iterator *end*, MapFunctor *function*)

呼叫*功能*从每个项目一旦*开始*到*结束*，并与每个映射项作为结果返回未来。您可以使用[QFuture :: const_iterator](https://doc.qt.io/qt-5/qfuture-const-iterator.html)或[QFutureIterator](https://doc.qt.io/qt-5/qfutureiterator.html)迭代结果。

**See also** [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< ResultType> QtConcurrent::mappedReduced(const Sequence &*sequence*, MapFunctor *mapFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*reduceOptions* = ...)

按*顺序*为每个项调用*mapFunction*一次。每个*mapFunction*的返回值都传递给*reduceFunction*。

请注意，虽然*mapFunction*是并发调用的，但一次只有一个线程会调用*reduceFunction*。*reduceFunction*的调用顺序由*reduceOptions*决定。

**See also** [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< ResultType> QtConcurrent::mappedReduced(Iterator *begin*, Iterator *end*, MapFunctor *mapFunction*, ReduceFunctor *reduceFunction*, [QtConcurrent::ReduceOptions](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)*reduceOptions* = ...)

呼吁*mapFunction*一次从每个项目*开始*到*结束*。每个*mapFunction*的返回值都传递给*reduceFunction*。

请注意，虽然*mapFunction*是并发调用的，但一次只有一个线程会调用*reduceFunction*。默认情况下，调用*reduceFunction*的顺序是未定义的。

**注意：**[QtConcurrent :: OrderedReduce](https://doc.qt.io/qt-5/qtconcurrent.html#ReduceOption-enum)导致有序缩减。

**See also** [Concurrent Map and Map-Reduce](https://doc.qt.io/qt-5/qtconcurrentmap.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< T > QtConcurrent::run(Function *function*, *...*)

Equivalent to

```c++
QtConcurrent::run(QThreadPool::globalInstance(), function, ...);
```

在单独的线程中运行*函数*。该线程取自全局[QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html)。请注意，*功能*可能不会立即运行; 只有在线程可用时才会运行*函数*。

T与*函数*的返回值的类型相同。可以通过[QFuture :: result](https://doc.qt.io/qt-5/qfuture.html#result)()函数访问非void返回值。

需要注意的是[QFuture](https://doc.qt.io/qt-5/qfuture.html)通过QtConcurrent返回::运行()不支持取消，暂停或进展的报告。返回的[QFuture](https://doc.qt.io/qt-5/qfuture.html)只能用于查询运行/完成状态和函数的返回值。

**See also** [Concurrent Run](https://doc.qt.io/qt-5/qtconcurrentrun.html).



### [QFuture](https://doc.qt.io/qt-5/qfuture.html)< T > QtConcurrent::run([QThreadPool](https://doc.qt.io/qt-5/qthreadpool.html) **pool*, Function *function*, *...*)

在单独的线程中运行*函数*。该线程来自[QThreadPool ](https://doc.qt.io/qt-5/qthreadpool.html)*池*。请注意，*功能*可能不会立即运行; 只有在线程可用时才会运行*函数*。

T与*函数*的返回值的类型相同。可以通过[QFuture :: result](https://doc.qt.io/qt-5/qfuture.html#result)()函数访问非void返回值。

需要注意的是[QFuture](https://doc.qt.io/qt-5/qfuture.html)通过QtConcurrent::run()返回不支持取消，暂停或进展的报告。返回的[QFuture](https://doc.qt.io/qt-5/qfuture.html)只能用于查询运行/完成状态和函数的返回值。

该功能在Qt 5.4中引入。

**See also** [Concurrent Run](https://doc.qt.io/qt-5/qtconcurrentrun.html).