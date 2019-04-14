# Menu Controls



|                                                              |                                        |
| ------------------------------------------------------------ | -------------------------------------- |
| [Menu](https://doc.qt.io/qt-5/qml-qtquick-controls2-menu.html) | 可以用作上下文菜单或弹出菜单的弹出窗口 |
| [MenuBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-menubar.html) | 提供窗口菜单栏                         |
| [MenuBarItem](https://doc.qt.io/qt-5/qml-qtquick-controls2-menubaritem.html) | 在MenuBar中显示一个下拉菜单            |
| [MenuItem](https://doc.qt.io/qt-5/qml-qtquick-controls2-menuitem.html) | 在菜单中显示项目                       |

每种类型的菜单控件都有自己特定的目标用例。以下部分提供了根据用例选择适当类型菜单控件的指南。



## Menu Control

![img](QtQuick菜单控件.assets/qtquickcontrols2-menu.png)

[Menu](https://doc.qt.io/qt-5/qml-qtquick-controls2-menu.html)控件可用于上下文菜单; 例如，右键单击后。它也可以用于弹出菜单; 例如，单击按钮后显示的菜单。

[MenuItem](https://doc.qt.io/qt-5/qml-qtquick-controls2-menuitem.html)是Menu控件中的一个项目。菜单中的每个项目：

- 向用户显示文本
- 允许检查/取消选中
- 突出显示（例如，在键盘导航上）
- 对激活执行一些操作



## MenuBar Control

![img](QtQuick菜单控件.assets/qtquickcontrols2-menubar.png)

[MenuBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-menubar.html)控件可用于窗口菜单栏。

[MenuBarItem](https://doc.qt.io/qt-5/qml-qtquick-controls2-menubaritem.html)是[MenuBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-menubar.html)控件中的一个项目。菜单栏中的每个项目：

- 向用户显示文本
- 突出显示（例如，在键盘导航上）
- 在激活时弹出相应的菜单