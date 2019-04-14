# Separator Controls



Qt Quick Controls 2提供多种分离器。

|                                                              |                                    |
| ------------------------------------------------------------ | ---------------------------------- |
| [MenuSeparator](https://doc.qt.io/qt-5/qml-qtquick-controls2-menuseparator.html) | 将菜单中的一组项目与相邻项目分开   |
| [ToolSeparator](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolseparator.html) | 将工具栏中的一组项目与相邻项目分开 |

每种类型的分隔符都有自己的特定用例。以下部分提供了根据用例选择适当类型的分隔符的指南。



## MenuSeparator Control

![img](QtQuick分隔符控件.assets/qtquickcontrols2-menuseparator.png)

[[MenuSeparator](https://doc.qt.io/qt-5/qml-qtquick-controls2-menuseparator.html)应该用于分隔菜单中的项目（通常是[MenuItem](https://doc.qt.io/qt-5/qml-qtquick-controls2-menuitem.html)控件）。将相关菜单项分组在一起使用户更容易与菜单交互。例如，一个典型的桌面用户界面可能会`Undo`和`Redo`一组项目，并且`Cut`，`Copy`和`Paste`在另一个。



## ToolSeparator Control

![img](QtQuick分隔符控件.assets/qtquickcontrols2-toolseparator.png)

[[ToolSeparator](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolseparator.html)应该用于在[ToolBar中](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbar.html)分隔项目（通常是[ToolButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbutton.html)控件）。它可以用于水平或垂直工具栏。

