[TOC]



# Button Controls



Qt Quick Controls 2提供一系列按钮式控制。

|                                                              |                                      |
| ------------------------------------------------------------ | ------------------------------------ |
| [AbstractButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-abstractbutton.html) | 抽象基类型提供按钮共有的功能         |
| [Button](https://doc.qt.io/qt-5/qml-qtquick-controls2-button.html) | 可以单击按钮执行命令或回答问题的按钮 |
| [CheckBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html) | 检查可以打开或关闭的按钮             |
| [DelayButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-delaybutton.html) | 按住足够长时间后触发的按钮           |
| [RadioButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-radiobutton.html) | 可以打开或关闭的专用单选按钮         |
| [RoundButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-roundbutton.html) | 带有圆角的按钮控制，可由用户点击     |
| [Switch](https://doc.qt.io/qt-5/qml-qtquick-controls2-switch.html) | 可以打开或关闭的按钮                 |
| [ToolButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbutton.html) | 具有适合ToolBar的外观的按钮          |

每种类型的按钮都有自己的特定用例。以下部分提供了根据用例选择适当类型按钮的指南。



## Button Control

[Button](https://doc.qt.io/qt-5/qml-qtquick-controls2-button.html) 是一个可单击的控件，用于启动操作，或打开或关闭弹出窗口。按钮通常具有文本标签，但也可以包含图标。

当弹出窗口或对话框需要执行操作时，Button是一个非常合适的控件。最常见的示例是应用，取消，保存，关闭和帮助。



![img](QtQuick按钮控件.assets/qtquickcontrols2-button.gif)



Recommendations:

- 按钮的文本应该是描述动作的动词，或者是与将要打开的弹出窗口的标题匹配的名词。
- 不要使用按钮来设置状态。[Switch](https://doc.qt.io/qt-5/qml-qtquick-controls2-switch.html)更适合这种情况。
- 除非您具有指定的UI准则，否则请使用默认字体。
- 如果文本已本地化，请考虑较长文本对布局的影响。

**See also** [Button](https://doc.qt.io/qt-5/qml-qtquick-controls2-button.html) and [AbstractButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-abstractbutton.html)



## CheckBox Control



![img](QtQuick按钮控件.assets/qtquickcontrols2-checkbox.gif)



[CheckBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html) 用于构建多选选项列表。可以选择任意数量的选项，包括无选项，但选项不应相互排斥。

使用单个[CheckBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html)进行是/否选择，例如当您必须接受表单中的服务条款协议时。

对于单个是/否选择，也可以使用开关。如果选择涉及一个选项，最好使用[CheckBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html)。如果涉及要采取的措施，建议使用开关。

可以对选项进行分组时，可以使用部分选中的[CheckBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html)来表示整个组。当用户选择组中的一些（但不是全部）子项时，使用复选框的[部分检查状态](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html#checkState-prop)。

三种可用的检查状态包括：已检查，部分检查和未选中。

可检查选项通常是垂直列出的。

Recommendations:

- 复选框标签应该是复选标记为true的语句，并且复选标记的缺失应为false。
- 复选框标签不应包含否定语句。
- 除非您具有指定的UI准则，否则请使用默认字体。
- 如果文本已本地化，请考虑较长文本对布局的影响。

**See also** [CheckBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html)



## DelayButton Control

[DelayButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-delaybutton.html) 是一个在触发动作之前包含延迟的按钮。此延迟可防止意外按下。



![img](QtQuick按钮控件.assets/qtquickcontrols2-delaybutton.gif)



Recommendations:

- 在触摸用户界面中使用。
- 用于必须小心触发的操作。

**See also** [Button](https://doc.qt.io/qt-5/qml-qtquick-controls2-button.html) and [AbstractButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-abstractbutton.html)



## RadioButton Control



![img](QtQuick按钮控件.assets/qtquickcontrols2-radiobutton.gif)



[RadioButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-radiobutton.html) 用于从一组选项中仅选择一个选项。选择一个选项会自动取消选择之前选择的选项。

如果只有两个互斥选项，请将它们组合成一个复选框或一个开关。

Recommendations:

- 将标签文本限制为一行。
- 确保选中合理的默认选项。
- 垂直列出[RadioButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-radiobutton.html)选项。
- 如果文本已本地化，请考虑较长文本对布局的影响。
- 除非您具有指定的UI准则，否则请使用默认字体。
- 就像使用[CheckBox一样](https://doc.qt.io/qt-5/qml-qtquick-controls2-checkbox.html)，不要使列表太大。
- 为避免混淆，请勿将两组单选按钮放在一起。

**See also** [RadioButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-radiobutton.html)



## RoundButton Control

[RoundButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-roundbutton.html) 是一个可单击的控件，用于启动操作，或打开或关闭弹出窗口。带有方形图标图标或单字母字体图标的圆形按钮是圆形的。圆形[RoundButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-roundbutton.html)比普通[Button](https://doc.qt.io/qt-5/qml-qtquick-controls2-button.html)占用更少的空间，也可以用作浮动操作按钮。

![img](QtQuick按钮控件.assets/qtquickcontrols2-roundbutton.png)

Recommendations:

- 保持标签简洁明了。
- 如果文本已本地化，请考虑较长文本对布局的影响。

**See also** [RoundButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-roundbutton.html)



## Switch Control

![img](QtQuick按钮控件.assets/qtquickcontrols2-switch.png)

[Switch](https://doc.qt.io/qt-5/qml-qtquick-controls2-switch.html)代表一个物理开关，允许用户在“开”或“关”状态之间进行选择。使用开关进行二进制操作，该操作在打开后立即生效。例如，打开或关闭WIFI的开关。

Recommendations:

- 保持标签简洁明了。
- 如果文本已本地化，请考虑较长文本对布局的影响。

**See also** [Switch](https://doc.qt.io/qt-5/qml-qtquick-controls2-switch.html)



## ToolButton Control

![img](QtQuick按钮控件.assets/qtquickcontrols2-toolbutton.png)

[ToolButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbutton.html) 几乎与[Button](https://doc.qt.io/qt-5/qml-qtquick-controls2-button.html)相同，但它具有图形外观，使其更适合插入[ToolBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbar.html)。

**See also** [ToolButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbutton.html)



## Related Information

- [Qt Quick Controls 2 Guidelines](https://doc.qt.io/qt-5/qtquickcontrols2-guidelines.html)