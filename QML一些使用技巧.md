# QML与C++交互的注意事项

[参考](<https://gitbook.cn/books/5acf2d97573c31562789c16a/index.html>) QtQuick 系列教程之 QML 与 C++ 交互（强烈推荐阅读）

------



[TOC]

------

### 关于C++对象注册问题

#### 方式1 ：C++ 对象注册到元对象系统

`QQmlApplicationEngine::rootContext()->setContextProperty(“name”, &Class);`

​	使用这个方法可以快速将C++类注册到qml元对象系统中，是方式二的简化版，效果类似于注册一个单例模块，这种方法简单，但没有版本号控制的功能，对于后续开发不利，不建议使用。

ps：建议使用智能指针的方式创建指向类的实例，可以避免内存错误

`//QScopedPointer模版类可以指向一个动态分配内存的对象`

`QScopedPointer<Class> myclass(new Class());`



#### 方式2：C++ 对象注册到 Qml 系统

`qmlRegisterType＜Class＞("com.mycompany", 1, 0, "name");`

