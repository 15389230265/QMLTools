# Qt Quick



Qt Quick模块是编写QML应用程序的标准库。虽然[Qt QML](https://doc.qt.io/qt-5/qtqml-index.html)模块提供QML引擎和语言基础结构，但Qt Quick模块提供了使用QML创建用户界面所需的所有基本类型。它提供了一个可视画布，包括用于创建和动画可视组件，接收用户输入，创建数据模型和视图以及延迟对象实例化的类型。

Qt Quick模块提供了一个[QML API](https://doc.qt.io/qt-5/qtquick-qmlmodule.html)，它提供了用于使用QML语言创建用户界面的QML类型，以及一个用于使用C ++代码扩展QML应用程序的[C ++ API](https://doc.qt.io/qt-5/qtquick-module.html)。

**注意：**还可以使用一组基于Qt Quick的UI控件来创建用户界面。有关更多信息，请参阅[Qt Quick Controls 2](https://doc.qt.io/qt-5/qtquickcontrols-index.html)。

对于那些刚接触QML和Qt Quick的人，请参阅[QML应用程序](https://doc.qt.io/qt-5/qmlapplications.html)，了解编写QML应用程序的介绍。



## Important Concepts in Qt Quick

Qt Quick提供了创建具有流畅和动态用户界面的丰富应用程序所需的一切。它允许用户界面围绕用户界面组件的行为以及它们如何彼此连接构建，并且它提供具有其自己的坐标系和渲染引擎的可视画布。动画和过渡效果是Qt Quick中的第一类概念，视觉效果可以通过粒子和着色器效果的专用组件来补充。

- [The Visual Canvas](https://doc.qt.io/qt-5/qtquick-visualcanvas-topic.html)
- [User Input](https://doc.qt.io/qt-5/qtquick-input-topic.html)
- [Positioning](https://doc.qt.io/qt-5/qtquick-positioning-topic.html)
- [States, Transitions And Animations](https://doc.qt.io/qt-5/qtquick-statesanimations-topic.html)
- [Data - Models, Views and Data Storage](https://doc.qt.io/qt-5/qtquick-modelviewsdata-topic.html)
- [Particles And Graphical Effects](https://doc.qt.io/qt-5/qtquick-effects-topic.html)
- [Convenience Types](https://doc.qt.io/qt-5/qtquick-convenience-topic.html)

使用该`QtQuick`模块时，您需要知道如何使用QML语言编写QML应用程序。特别是[QML应用程序](https://doc.qt.io/qt-5/qmlapplications.html)页面中的QML Basics和QML Essentials 。

要了解有关使用QML语言的更多信息，请参阅[Qt QML](https://doc.qt.io/qt-5/qtqml-index.html)模块文档。



## C++ Extension Points

- C++ Extension Points
  - [Creating User-Defined QQuickItem-Derived Types](https://doc.qt.io/qt-5/qtquick-cppextensionpoints.html#user-defined-qquickitem-derived-types)
  - [Scene Graph-Related Classes](https://doc.qt.io/qt-5/qtquick-cppextensionpoints.html#scene-graph-related-classes)
  - [Pixmap and Threaded Image Support](https://doc.qt.io/qt-5/qtquick-cppextensionpoints.html#pixmap-and-threaded-image-support)



## Licenses and Attributions

Qt Quick可在[Qt公司的](http://www.qt.io/about-us/)商业许可下获得。此外，它还可以通过免费软件许可证获得。从Qt 5.4开始，这些免费软件许可证是[GNU较宽松通用公共许可证版本3](http://www.gnu.org/licenses/lgpl-3.0.html)或[GNU通用公共许可证版本2](http://www.gnu.org/licenses/gpl-2.0.html)。有关详细信息，请参阅[Qt Licensing](https://doc.qt.io/qt-5/licensing.html)。



## Reference Documentation

Additional Qt Quick information:

- [Qt Quick C++ Classes](https://doc.qt.io/qt-5/qtquick-module.html) - the C++ API provided by the Qt Quick module

- Qt Quick QML Types - a list of QML types provided by the Qt Quick import

  - [XML List Model](https://doc.qt.io/qt-5/qtquick-xmllistmodel-qmlmodule.html) - contains types for creating models from XML data
  - [Local Storage](https://doc.qt.io/qt-5/qtquick-localstorage-qmlmodule.html) - a submodule containing a JavaScript interface for an [SQLite](https://doc.qt.io/qt-5/qtsql-attribution-sqlite.html#sqlite) database
  - [Particles](https://doc.qt.io/qt-5/qtquick-particles-qmlmodule.html) - provides a particle system for Qt Quick
  - [Layouts](https://doc.qt.io/qt-5/qtquicklayouts-index.html) - provides layouts for arranging Qt Quick items
  - [Window](https://doc.qt.io/qt-5/qtquick-window-qmlmodule.html) - contains types for creating top-level windows and accessing screen information
  - [Dialogs](https://doc.qt.io/qt-5/qtquickdialogs-index.html) - contains types for creating and interacting with system dialogs
  - [Tests](https://doc.qt.io/qt-5/qttest-qmlmodule.html) - contains types for writing unit test for a QML application

- [Qt Quick Examples and Tutorials](https://doc.qt.io/qt-5/qtquick-codesamples.html)

- [Qt Quick Guidelines](https://doc.qt.io/qt-5/qtquick-bestpractices.html)

Further information for writing QML applications:

- [QML Applications](https://doc.qt.io/qt-5/qmlapplications.html) - essential information for application development with QML and Qt Quick
- [Qt QML](https://doc.qt.io/qt-5/qtqml-index.html) - documentation for the Qt QML module, which provides the QML engine and language infrastructure