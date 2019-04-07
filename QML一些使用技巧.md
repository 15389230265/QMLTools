**技巧对应于qt长期支持版本5.12.0**

------

[TOC]

------


# QML与C++交互的注意事项

[参考：QtQuick 系列教程之 QML 与 C++ 交互](<https://gitbook.cn/books/5acf2d97573c31562789c16a/index.html>) （强烈推荐阅读）


### 关于C++对象注册问题

#### 方式1 ：C++ 对象注册到元对象系统

`QQmlApplicationEngine::rootContext()->setContextProperty(“name”, &Class);`

​	使用这个方法可以快速将C++类注册到qml元对象系统中，是方式二的简化版，效果类似于注册一个单例模块，这种方法简单，但没有版本号控制的功能，对于后续开发不利，不建议使用。

ps：建议使用智能指针的方式创建指向类的实例，可以避免内存错误

`//QScopedPointer模版类可以指向一个动态分配内存的对象`

`QScopedPointer<Class> myclass(new Class());`



#### 方式2：C++ 对象注册到 Qml 系统

`qmlRegisterType＜Class＞("com.mycompany", 1, 0, "name");`

------

# QML的MVC设计模式

### 说明

**Model-View-Controller**[(详情见qt文档)](<https://doc-snapshots.qt.io/qt5-5.11/qtquick-modelviewsdata-modelview.html>)

![](C:\Users\50397\Documents\QML一些使用技巧\modelview-overview.png)

- **Model** - contains the data and its structure. There are several QML types for creating models.

- **View** - a container that displays the data. The view might display the data in a list or a grid.

- **Delegate** - dictates how the data should appear in the view. The delegate takes each data in the model and encapsulates it. The data is accessible through the delegate.

  ​	简单来说，MVC设计模式将数据本身的数据和格式于如何显示数据，控制数据的方法分开。模型便是我们要存放数据和格式的地方，QML提供了ListModel来存放一个链表模型，但大多数时候我们需要使用更高级的模型，因此使用QT提供的模型接口重写自己的模型类。视图便是显示数据模型的容器，定义了一些和模型相关的显示规则，QML中提供了ListView、GridView、PathView三种基于链表、网格和路径的显示方式，满足大部分显示需求，当然QT也提供了视图的重写接口来自定义视图。委托主要负责UI交互和数据模型的封装显示，委托中可以使用QML中的所有组件决定数据如何显示在视图中，也可以使用自定义的组件。

### Model

​	QML中提供的模型就不细说了，文档中说的十分清楚，这里主要说一下自定义模型类





### View

### Delegate

