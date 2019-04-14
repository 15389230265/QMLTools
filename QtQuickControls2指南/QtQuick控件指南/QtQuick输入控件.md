[TOC]

# Input Controls



Qt Quick Controls 2为数字和文本输入提供了各种输入控件。

|                                                              |                                          |
| ------------------------------------------------------------ | ---------------------------------------- |
| [ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html) | 用于选择选项的组合按钮和弹出列表         |
| [Dial](https://doc.qt.io/qt-5/qml-qtquick-controls2-dial.html) | 旋转的圆形刻度盘设置值                   |
| [RangeSlider](https://doc.qt.io/qt-5/qml-qtquick-controls2-rangeslider.html) | 用于通过沿轨道滑动两个手柄来选择一系列值 |
| [Slider](https://doc.qt.io/qt-5/qml-qtquick-controls2-slider.html) | 用于通过沿轨道滑动手柄来选择值           |
| [TextArea](https://doc.qt.io/qt-5/qml-qtquick-controls2-textarea.html) | 多行文字输入区                           |
| [TextField](https://doc.qt.io/qt-5/qml-qtquick-controls2-textfield.html) | 单行文本输入字段                         |
| [Tumbler](https://doc.qt.io/qt-5/qml-qtquick-controls2-tumbler.html) | 可选择项目的可旋转轮                     |

每种类型的输入控件都有自己特定的目标用例。以下部分提供了根据用例选择适当类型的输入控件的指南。



## ComboBox Control



![img](QtQuick输入控件.assets/qtquickcontrols2-combobox.gif)



[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)用于从静态多行下拉列表中选择值。无法添加新值，只能选择一个选项。

Recommendations:

- 如果值的数量非常大，请考虑应用过滤器。
- 如果列表非常有限，请考虑使用[RadioButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-radiobutton.html)。这具有用户可以同时看到所有选项的优点。
- 选择一个默认值，即最常选择的值。

**See also** [CheckBox Control](https://doc.qt.io/qt-5/qtquickcontrols2-buttons.html#checkbox-control), [Tumbler Control](https://doc.qt.io/qt-5/qtquickcontrols2-input.html#tumbler-control).



## Dial Control

![img](QtQuick输入控件.assets/qtquickcontrols2-dial.png)

[Dial](https://doc.qt.io/qt-5/qml-qtquick-controls2-dial.html) 类似于传统的拨号旋钮，可在立体声或工业设备等设备上找到。

通过单击并拖动来旋转拨盘，手柄指示拨盘的值。

对于快速输入很重要的应用，圆形[输入模式](https://doc.qt.io/qt-5/qml-qtquick-controls2-dial.html#inputMode-prop)很有用，因为单击拨盘会将其直接移动到该位置。

对于精确输入很重要的应用，建议使用水平和垂直输入模式，因为这些模式允许相对于单击拨盘的位置进行小幅调整。对于大值跳跃可能不安全的拨号盘，这些模式也更好，例如控制音量的拨号盘。

**See also** [Tumbler Control](https://doc.qt.io/qt-5/qtquickcontrols2-input.html#tumbler-control).



## TextArea Control

![img](QtQuick输入控件.assets/qtquickcontrols2-textarea.png)

[TextArea](https://doc.qt.io/qt-5/qml-qtquick-controls2-textarea.html)是一个多行文本编辑器。



## TextField Control

![img](QtQuick输入控件.assets/qtquickcontrols2-textfield.png)

[TextField](https://doc.qt.io/qt-5/qml-qtquick-controls2-textfield.html) 是单行文本编辑器。

**See also** [Tumbler Control](https://doc.qt.io/qt-5/qtquickcontrols2-input.html#tumbler-control).



## Slider Control



![img](QtQuick输入控件.assets/qtquickcontrols2-slider.gif)



[Slider](https://doc.qt.io/qt-5/qml-qtquick-controls2-slider.html) 用于通过沿轨道滑动手柄来选择值。



## RangeSlider Control



![img](QtQuick输入控件.assets/qtquickcontrols2-rangeslider.gif)



[RangeSlider](https://doc.qt.io/qt-5/qml-qtquick-controls2-rangeslider.html)用于通过沿轨道滑动每个手柄来选择由两个值指定的范围。

**See also** [Slider Control](https://doc.qt.io/qt-5/qtquickcontrols2-input.html#slider-control).



## Tumbler Control

![img](QtQuick输入控件.assets/qtquickcontrols2-tumbler.png)

[Tumbler](https://doc.qt.io/qt-5/qml-qtquick-controls2-tumbler.html) 是可以选择的可旋转轮子。

**See also** [ComboBox Control](https://doc.qt.io/qt-5/qtquickcontrols2-input.html#combobox-control).



## Related Information

- [Qt Quick Controls 2 Guidelines](https://doc.qt.io/qt-5/qtquickcontrols2-guidelines.html)