# Navigation Controls



Qt Quick Controls 2提供多种导航模型。

|                                                              |                                      |
| ------------------------------------------------------------ | ------------------------------------ |
| [Drawer](https://doc.qt.io/qt-5/qml-qtquick-controls2-drawer.html) | 侧面板可以使用滑动手势打开和关闭     |
| [StackView](https://doc.qt.io/qt-5/qml-qtquick-controls2-stackview.html) | 提供基于堆栈的导航模型               |
| [SwipeView](https://doc.qt.io/qt-5/qml-qtquick-controls2-swipeview.html) | 允许用户通过横向滑动来浏览页面       |
| [TabBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-tabbar.html) | 允许用户在不同的视图或子任务之间切换 |
| [TabButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-tabbutton.html) | 按钮看起来适合TabBar                 |

以下部分提供了根据用例选择适当类型的导航模型的指南。



## StackView Control

![img](QtQuick导航控件.assets/qtquickcontrols2-stackview-wireframe.png)

[StackView](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#stackview)提供了一个基于堆栈的导航模型，可以与一组相互关联的页面一起使用。[StackView](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#stackview)按照[后进](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#stackview)先出原则工作：最后在堆栈上推送的页面是可见的。弹出页面会删除最后一页并使前一页可见。



## SwipeView Control

![img](QtQuick导航控件.assets/qtquickcontrols2-swipeview-wireframe.png)

[[SwipeView](https://doc.qt.io/qt-5/qml-qtquick-controls2-swipeview.html)提供了一个简化水平分页滚动的导航模型。底部的页面指示器显示当前活动页面。



## TabBar

![img](QtQuick导航控件.assets/qtquickcontrols2-tabbar-wireframe.png)

[TabBar](https://doc.qt.io/qt-5/qtquickcontrols2-navigation.html#tabbar)是一个带有图标或文本的栏，允许用户在不同的子任务，视图或模式之间切换。



## TabButton Control

![img](QtQuick导航控件.assets/qtquickcontrols2-tabbutton.png)

[TabButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-tabbutton.html)是一个具有适合[TabBar](https://doc.qt.io/qt-5/qtquickcontrols2-navigation.html#tabbar)控件的布局的按钮。