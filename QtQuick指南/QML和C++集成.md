**技巧对应于qt长期支持版本5.12.0**

------

[TOC]

------




# QML与C++交互的注意事项

[参考：QtQuick 系列教程之 QML 与 C++ 交互](<https://gitbook.cn/books/5acf2d97573c31562789c16a/index.html>) （强烈推荐阅读）


## 关于C++对象注册问题

### 方式1 ：C++ 对象注册到元对象系统

```c++
// main.cpp
QQmlApplicationEngine::rootContext()->setContextProperty(“name”, &Demo);
```

​	使用这个方法可以快速将C++类注册到qml元对象系统中，是方式二的简化版，效果类似于注册一个单例模块，这种方法简单，但没有版本号控制的功能，对于后续开发不利，不建议使用。

ps：建议使用智能指针的方式创建指向类的实例，可以避免内存错误

```c++
// QScopedPointer模版类可以指向一个动态分配内存的对象
QScopedPointer<Demo> myclass(new Demo());
```



### 方式2：C++ 对象注册到 Qml 系统

```c++
// demo.h
Class Demo : public QObject
{
    Q_OBJECT
    ...
}
```
```c++
//  main.cpp
qmlRegisterType＜Demo＞("com.mycompany", 1, 0, "name");
```

​	所用的C++类必须继承QObject，这种注册对象的方法带有版本控制，推荐使用。



### 方式3：C++对象注册一个单例组件到Qml系统

```c++
// main.cpp 使用匿名函数方式
qmlRegisterSingletonType<SingletonTypeExample>("Qt.example.qjsvalueApi", 1, 0, "MyApi", [](QQmlEngine *engine, QJSEngine *scriptEngine) -> QObject * {
    Q_UNUSED(engine)
    Q_UNUSED(scriptEngine)

    SingletonTypeExample *example = new SingletonTypeExample();
    return example;
});
```

```html
// main.qml
import QtQuick 2.0
import Qt.example.qobjectSingleton 1.0
Item {
    id: root
    property int someValue: MyApi.someProperty

    Component.onCompleted: {
        someValue = MyApi.doSomething()
    }
}
```

​	单例组件作用于整个程序中，当我们不需要多个实例化我们的自定义类时，这个方式十分有用。



![img](QML和C++集成.assets/cpp-qml-integration-flowchart.png)


