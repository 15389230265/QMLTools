[TOC]



# Container Controls



Qt Quick Controls 2提供一系列容器式控件。

|                                                              |                                      |
| ------------------------------------------------------------ | ------------------------------------ |
| [ApplicationWindow](https://doc.qt.io/qt-5/qml-qtquick-controls2-applicationwindow.html) | 风格顶级窗口，支持页眉和页脚         |
| [Container](https://doc.qt.io/qt-5/qml-qtquick-controls2-container.html) | 抽象基类型提供容器通用的功能         |
| [Frame](https://doc.qt.io/qt-5/qml-qtquick-controls2-frame.html) | 逻辑控件组的可视框架                 |
| [GroupBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-groupbox.html) | 逻辑控件组的可视框架和标题           |
| [Page](https://doc.qt.io/qt-5/qml-qtquick-controls2-page.html) | 样式化页面控件，支持页眉和页脚       |
| [Pane](https://doc.qt.io/qt-5/qml-qtquick-controls2-pane.html) | 提供与应用程序样式和主题匹配的背景   |
| [ScrollView](https://doc.qt.io/qt-5/qml-qtquick-controls2-scrollview.html) | 可滚动视图                           |
| [StackView](https://doc.qt.io/qt-5/qml-qtquick-controls2-stackview.html) | 提供基于堆栈的导航模型               |
| [SwipeView](https://doc.qt.io/qt-5/qml-qtquick-controls2-swipeview.html) | 允许用户通过横向滑动来浏览页面       |
| [TabBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-tabbar.html) | 允许用户在不同的视图或子任务之间切换 |
| [ToolBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbar.html) | 用于上下文相关控件的容器             |

每种类型的容器都可用于将一组控件组合在一起。以下部分提供了根据用例选择适当类型容器的指南。



## ApplicationWindow Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-applicationwindow-wireframe.png)

[ApplicationWindow](https://doc.qt.io/qt-5/qml-qtquick-controls2-applicationwindow.html) 创建应用程序的根窗口，并可以轻松地向该窗口添加可选的页眉和页脚。



## Frame Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-frame.png)

[Frame](https://doc.qt.io/qt-5/qml-qtquick-controls2-frame.html) 用于在可视框架内将逻辑控件组布局在一起。



## GroupBox Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-groupbox.png)

[GroupBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-groupbox.html) 用于在标题视觉框架内将逻辑控件组布局在一起。



## Page Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-page-wireframe.png)

[Page](https://doc.qt.io/qt-5/qml-qtquick-controls2-page.html) 提供特定于页面的页眉和页脚项。完全可以使用[ApplicationWindow](https://doc.qt.io/qt-5/qml-qtquick-controls2-applicationwindow.html)来设置页眉和页脚，但如果你的页眉和页脚因屏幕而异，那么最好使用[Page](https://doc.qt.io/qt-5/qml-qtquick-controls2-page.html)。



## Pane Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-pane.png)

[Pane](https://doc.qt.io/qt-5/qml-qtquick-controls2-pane.html) 提供与应用程序样式和主题匹配的背景颜色。窗格不提供自己的布局，但要求您定位其内容，例如使用[RowLayout](https://doc.qt.io/qt-5/qml-qtquick-layouts-rowlayout.html)或[ColumnLayout](https://doc.qt.io/qt-5/qml-qtquick-layouts-columnlayout.html)。



## ScrollView Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-scrollview-wireframe.png)

[ScrollView](https://doc.qt.io/qt-5/qml-qtquick-controls2-scrollview.html) 为用户定义的内容提供滚动。



## StackView Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-stackview-wireframe.png)

[StackView](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#stackview) 使用[后进](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#stackview)先出原则将内容页面组织成堆栈：要被“推”到堆栈上的最后一项是第一个要删除的项目，最顶层的项目始终是可见的项目。



## SwipeView Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-swipeview-wireframe.png)

[SwipeView](https://doc.qt.io/qt-5/qml-qtquick-controls2-swipeview.html) 内容页面组织成可刷新的条带。



## TabBar Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-tabbar-wireframe.png)

[TabBar](https://doc.qt.io/qt-5/qtquickcontrols2-navigation.html#tabbar) 内容页面组织到选项卡中。



## ToolBar Control

![img](QtQuick窗口容器控件.assets/qtquickcontrols2-toolbar.png)

[ToolBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbar.html) 是应用程序范围和上下文敏感的操作和控件的容器。