# Popup Controls



|                                                              |                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [Dialog](https://doc.qt.io/qt-5/qml-qtquick-controls2-dialog.html) | 带有标准按钮和标题的弹出对话框，用于与用户进行短期交互 |
| [Drawer](https://doc.qt.io/qt-5/qml-qtquick-controls2-drawer.html) | 侧面板可以使用滑动手势打开和关闭                       |
| [Menu](https://doc.qt.io/qt-5/qml-qtquick-controls2-menu.html) | 可以用作上下文菜单或弹出菜单的弹出窗口                 |
| [Popup](https://doc.qt.io/qt-5/qml-qtquick-controls2-popup.html) | 基于类型的弹出式用户界面控件                           |
| [ToolTip](https://doc.qt.io/qt-5/qml-qtquick-controls2-tooltip.html) | 提供任何控件的工具提示                                 |

每种类型的弹出控件都有自己特定的目标用例。以下部分提供了有关选择适当类型的弹出控件的指南，具体取决于用例。



## Drawer Control

![img](QtQuick弹出控件.assets/qtquickcontrols2-drawer-expanded-wireframe.png)

[Drawer](https://doc.qt.io/qt-5/qml-qtquick-controls2-drawer.html) 提供基于滑动的侧面板，类似于触摸界面中常用的侧面板，以提供导航的中心位置。

抽屉可以定位在屏幕的四个边缘中的任何一个处。它允许用户添加导航而不占用宝贵的屏幕空间。用户可以通过简单的滑动移动随时显示和隐藏抽屉。



## Menu Control

![img](QtQuick弹出控件.assets/qtquickcontrols2-menu.png)

[Menu](https://doc.qt.io/qt-5/qml-qtquick-controls2-menu.html) 控件显示可以选择的项目的垂直列表。它可用于提供可在给定上下文中执行的操作列表。

**See also** [Drawer Control](https://doc.qt.io/qt-5/qtquickcontrols2-popups.html#drawer-control).



## Popup Control

![img](QtQuick弹出控件.assets/qtquickcontrols2-popup-settings.png)

[Popup](https://doc.qt.io/qt-5/qml-qtquick-controls2-popup.html) 显示的内容比其他的应用程序内容。它会提示用户做出决定或输入信息。

弹出窗口可以是模态的或非模态的。模态弹出窗口阻止用户与应用程序交互，直到他们做出选择并关闭弹出窗口。

弹出窗口可用于：

- 向用户传达他们必须阅读和确认的消息。
- 显示错误消息。
- 提示用户做出选择和/或输入值。



## ToolTip Control

![img](QtQuick弹出控件.assets/qtquickcontrols2-tooltip.png)

[ToolTip](https://doc.qt.io/qt-5/qml-qtquick-controls2-tooltip.html) 显示一小段文本，通知用户控件的功能。它通常位于父对照的上方或下方。

建议：

- 如果控件具有很少或没有描述性文本，或者需要简短说明，请使用工具提示。
- 仅当屏幕上其他位置没有特定控件的信息时才使用工具提示。
- 保持工具提示文本较短，以便在显示时不覆盖其他内容。