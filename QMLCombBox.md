[TOC]



# ComboBox QML Type





Combined button and popup list for selecting options. [More...](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#details)

| Import Statement: | import QtQuick.Controls 2.5                                  |
| ----------------- | ------------------------------------------------------------ |
| Since:            | Qt 5.7                                                       |
| Inherits:         | [Control](https://doc.qt.io/qt-5/qml-qtquick-controls2-control.html) |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox-members.html)





## Properties

- **acceptableInput** : bool
- **count** : int
- **currentIndex** : int
- **currentText** : string
- **delegate** : Component
- **displayText** : string
- **down** : bool
- **editText** : string
- **editable** : bool
- **flat** : bool
- **highlightedIndex** : int
- **implicitIndicatorHeight** : real
- **implicitIndicatorWidth** : real
- **indicator** : Item
- **inputMethodComposing** : bool
- **inputMethodHints** : flags
- **model** : model
- **popup** : Popup
- **pressed** : bool
- **textRole** : string
- **validator** : Validator





## Signals

- void **accepted**()
- void **activated**(int *index*)
- void **highlighted**(int *index*)





## Methods

- void **decrementCurrentIndex**()
- int **find**(string *text*, flags = *Qt.MatchExactly*)
- void **incrementCurrentIndex**()
- void **selectAll**()
- string **textAt**(int *index*)



## 详细说明



![img](QMLCombBox.assets/qtquickcontrols2-combobox.gif)



[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)是一个组合按钮和弹出列表。它提供了一种以占用最少屏幕空间量的方式向用户显示选项列表的方法。

[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)填充了数据模型。数据模型通常是JavaScript数组，[ListModel](https://doc.qt.io/qt-5/qml-qtqml-models-listmodel.html)或整数，但也支持其他类型的[数据模型](https://doc.qt.io/qt-5/qtquick-modelviewsdata-modelview.html#qml-data-models)。

```c++
ComboBox {
    model: ["First", "Second", "Third"]
}
```





## 可编辑的ComboBox

[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)可以[编辑](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#editable-prop)。可编辑的组合框根据模型中可用的内容自动完成其文本。

以下示例通过对[接受的](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#accepted-signal)信号作出反应，演示将内容附加到可编辑的组合框。

```c++
ComboBox {
    editable: true
    model: ListModel {
        id: model
        ListElement { text: "Banana" }
        ListElement { text: "Apple" }
        ListElement { text: "Coconut" }
    }
    onAccepted: {
        if (find(editText) === -1)
            model.append({text: editText})
    }
}
```





## ComboBox模型角色

[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)能够可视化提供角色的标准[数据模型](https://doc.qt.io/qt-5/qtquick-modelviewsdata-modelview.html#qml-data-models)`modelData`：

- 只有一个角色的模型
- 没有命名角色的模型（JavaScript数组，整数）

使用具有多个命名角色的模型时，必须将[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)配置为对其[显示文本](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#displayText-prop)和[委托](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#delegate)实例使用特定[文本角色](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#textRole-prop)。

```
ComboBox {
    textRole: "key"
    model: ListModel {
        ListElement { key: "First"; value: 123 }
        ListElement { key: "Second"; value: 456 }
        ListElement { key: "Third"; value: 789 }
    }
}
```

**注意：**如果为[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)分配了一个具有多个命名角色的数据模型，但[未定义textRole](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#textRole-prop)，则[ComboBox](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html)无法将其可视化并抛出一个`ReferenceError: modelData is not defined`。

**另请参阅**[Qt快速控制中的](https://doc.qt.io/qt-5/qtquickcontrols2-focus.html)[自定义组合框](https://doc.qt.io/qt-5/qtquickcontrols2-customize.html#customizing-combobox)，[输入控件](https://doc.qt.io/qt-5/qtquickcontrols2-input.html)和[焦点管理2](https://doc.qt.io/qt-5/qtquickcontrols2-focus.html)。





## Property Documentation



#### [read-only]acceptableInput : [bool](https://doc.qt.io/qt-5/qml-bool.html)

------

此属性保存组合框是否包含可编辑文本字段中的可接受文本。

如果已设置验证程序，则`true`仅当验证程序接受当前文本作为最终字符串（而不是中间字符串）时，该值才为。

这个属性是在QtQuick.Controls 2.2（Qt 5.9）中引入的。

**See also** [validator](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#validator-prop) and [accepted](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#accepted-signal).



#### [read-only]count : [int](https://doc.qt.io/qt-5/qml-int.html)

------

此属性保存组合框中的项目数。



#### currentIndex : [int](https://doc.qt.io/qt-5/qml-int.html)

------

此属性保存组合框中当前项的索引。

默认值是`-1`当[计数](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#count-prop)是`0`，和`0`其他。

**See also** [activated()](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#activated-signal), [currentText](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentText-prop), and [highlightedIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#highlightedIndex-prop).



#### [read-only]currentText : [string](https://doc.qt.io/qt-5/qml-string.html)

------

此属性保存组合框中当前项的文本。

**See also** [currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop), [displayText](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#displayText-prop), and [textRole](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#textRole-prop).



#### delegate : [Component](https://doc.qt.io/qt-5/qml-qtqml-component.html)

------

此属性包含一个委托，该委托在组合框弹出窗口中显示项目。

建议使用[ItemDelegate](https://doc.qt.io/qt-5/qml-qtquick-controls2-itemdelegate.html)（或任何其他[AbstractButton](https://doc.qt.io/qt-5/qml-qtquick-controls2-abstractbutton.html)衍生物）作为委托。这可确保交互按预期工作，并且弹出窗口将在适当时自动关闭。当其他类型用作委托时，必须手动关闭弹出窗口。例如，如果使用[MouseArea](https://doc.qt.io/qt-5/qml-qtquick-mousearea.html)：

```
delegate: Rectangle {
    // ...
    MouseArea {
        // ...
        onClicked: comboBox.popup.close()
    }
}
```

**See also** [ItemDelegate](https://doc.qt.io/qt-5/qml-qtquick-controls2-itemdelegate.html) and [Customizing ComboBox](https://doc.qt.io/qt-5/qtquickcontrols2-customize.html#customizing-combobox).



#### displayText : [string](https://doc.qt.io/qt-5/qml-string.html)

------

此属性保存组合框按钮上显示的文本。

默认情况下，显示文本显示当前选择。也就是说，它遵循当前项目的文本。但是，可以使用自定义值覆盖默认显示文本。

```
ComboBox {
    currentIndex: 1
    displayText: "Size: " + currentText
    model: ["S", "M", "L"]
}
```

**See also** [currentText](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentText-prop) and [textRole](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#textRole-prop).



#### down : [bool](https://doc.qt.io/qt-5/qml-bool.html)

------

此属性保存组合框按钮是否在视觉上向下。

除非明确设置，否则此属性为何`true`时`pressed`或者`popup.visible`是`true`。要返回默认值，请将此属性设置为`undefined`。

这个属性是在QtQuick.Controls 2.2（Qt 5.9）中引入的。

**See also** [pressed](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#pressed-prop) and [popup](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#popup-prop).



#### editText : [string](https://doc.qt.io/qt-5/qml-string.html)

------

此属性将文本保存在可编辑组合框的文本字段中。

这个属性是在QtQuick.Controls 2.2（Qt 5.9）中引入的。

**See also** [editable](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#editable-prop).



#### editable : [bool](https://doc.qt.io/qt-5/qml-bool.html)

------

此属性保存组合框是否可编辑。

默认值为`false`。

这个属性是在QtQuick.Controls 2.2（Qt 5.9）中引入的。

**See also** [validator](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#validator-prop).



#### flat : [bool](https://doc.qt.io/qt-5/qml-bool.html)

------

此属性保持组合框按钮是否平坦。

除非与之交互，否则平面组合框按钮不会绘制背景。与普通组合框相比，平面组合框提供的外观使其与UI的其余部分相比更加突出。例如，当将组合框放入工具栏时，可能需要使组合框平坦，以使其与工具按钮的平面外观更好地匹配。

默认值为`false`。

此属性在QtQuick.Controls 2.1（Qt 5.8）中引入。



#### [read-only]highlightedIndex : [int](https://doc.qt.io/qt-5/qml-int.html)

------

此属性保存组合框弹出列表中突出显示项的索引。

激活突出显示的项目时，将关闭弹出窗口，将[currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop)设置为`highlightedIndex`，并将此属性的值重置为`-1`，因为不再有突出显示的项目。

**See also** [highlighted()](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#highlighted-signal) and [currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop).



#### [read-only]implicitIndicatorHeight : [real](https://doc.qt.io/qt-5/qml-real.html)

------

此属性保存隐式指标高度。

该值等于`indicator ? indicator.implicitHeight : 0`.

这通常与[implicitContentHeight](https://doc.qt.io/qt-5/qml-qtquick-controls2-control.html#implicitContentHeight-prop)和[implicitBackgroundHeight](https://doc.qt.io/qt-5/qml-qtquick-controls2-control.html#implicitBackgroundHeight-prop)一起用于计算[implicitHeight](https://doc.qt.io/qt-5/qml-qtquick-item.html#implicitHeight-prop)。

此属性是在QtQuick.Controls 2.5（Qt 5.12）中引入的。

**See also** [implicitIndicatorWidth](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#implicitIndicatorWidth-prop).



#### [read-only]implicitIndicatorWidth : [real](https://doc.qt.io/qt-5/qml-real.html)

------

此属性包含隐式指示符宽度。

该值等于`indicator ? indicator.implicitWidth : 0`.

通常使用它与[implicitContentWidth](https://doc.qt.io/qt-5/qml-qtquick-controls2-control.html#implicitContentWidth-prop)和[implicitBackgroundWidth](https://doc.qt.io/qt-5/qml-qtquick-controls2-control.html#implicitBackgroundWidth-prop)一起计算[implicitWidth](https://doc.qt.io/qt-5/qml-qtquick-item.html#implicitWidth-prop)。

此属性是在QtQuick.Controls 2.5（Qt 5.12）中引入的。

**See also** [implicitIndicatorHeight](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#implicitIndicatorHeight-prop).



#### indicator : [Item](https://doc.qt.io/qt-5/qml-qtquick-item.html)

------

此属性包含拖放指示器项。

**See also** [Customizing ComboBox](https://doc.qt.io/qt-5/qtquickcontrols2-customize.html#customizing-combobox).



#### [read-only]inputMethodComposing : [bool](https://doc.qt.io/qt-5/qml-bool.html)

------

此属性保存可编辑组合框是否具有输入方法的部分文本输入。

在编写时，输入方法可能依赖于组合框中的鼠标或键事件来编辑或提交部分文本。此属性可用于确定何时禁用可能干扰输入方法的正确操作的事件处理程序。

这个属性是在QtQuick.Controls 2.2（Qt 5.9）中引入的。



#### inputMethodHints : flags

------

为输入法提供有关组合框的预期内容及其操作方式的提示。

默认值为`Qt.ImhNoPredictiveText`.

该值是标志的逐位组合，或者`Qt.ImhNone`如果没有设置提示。

Flags that alter behavior are:

- Qt.ImhHiddenText - 字符应该被隐藏，就像输入密码时通常使用的那样。
- Qt.ImhSensitiveData - 活动输入法不应将类型文本存储在任何持久存储中，如预测用户字典。
- Qt.ImhNoAutoUppercase - 当句子结束时，输入法不应尝试自动切换到大写。
- Qt.ImhPreferNumbers - 数字是首选（但不是必需的）。
- Qt.ImhPreferUppercase - 首选大写字母（但不是必需的）。
- Qt.ImhPreferLowercase - 首选小写字母（但不是必需的）。
- Qt.ImhNoPredictiveText - 输入时不要使用预测文本（即字典查找）。
- Qt.ImhDate - 文本编辑器用作日期字段。
- Qt.ImhTime - 文本编辑器用作时间字段。

Flags that restrict input (exclusive flags) are:

- Qt.ImhDigitsOnly - 仅允许数字。
- Qt.ImhFormattedNumbersOnly - 仅允许输入数字。这包括小数点和减号。
- Qt.ImhUppercaseOnly - 仅允许大写字母输入。
- Qt.ImhLowercaseOnly - 仅允许小写字母输入。
- Qt.ImhDialableCharactersOnly - 仅允许适合电话拨号的字符。
- Qt.ImhEmailCharactersOnly - 仅允许适用于电子邮件地址的字符。
- Qt.ImhUrlCharactersOnly - 仅允许适合URL的字符。

Masks:

- Qt.ImhExclusiveInputMask - 如果使用任何独占标志，则此掩码会产生非零值。

这个属性是在QtQuick.Controls 2.2（Qt 5.9）中引入的。



#### model : model

------

此属性保存为组合框提供数据的模型。

```
ComboBox {
    textRole: "key"
    model: ListModel {
        ListElement { key: "First"; value: 123 }
        ListElement { key: "Second"; value: 456 }
        ListElement { key: "Third"; value: 789 }
    }
}
```

**See also** [textRole](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#textRole-prop) and [Data Models](https://doc.qt.io/qt-5/qtquick-modelviewsdata-modelview.html#qml-data-models).



#### popup : [Popup](https://doc.qt.io/qt-5/qml-qtquick-controls2-popup.html)

------

此属性包含弹出窗口。

如有必要，可以手动打开或关闭弹出窗口：

```
onSpecialEvent: comboBox.popup.close()
```

**See also** [Customizing ComboBox](https://doc.qt.io/qt-5/qtquickcontrols2-customize.html#customizing-combobox).



#### pressed : [bool](https://doc.qt.io/qt-5/qml-bool.html)

------

此属性保持是否物理按下组合框按钮。触摸或按键事件可以按下按钮。

**See also** [down](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#down-prop).



#### textRole : [string](https://doc.qt.io/qt-5/qml-string.html)

------

此属性包含用于填充组合框的模型角色。

当模型具有多个角色时，`textRole`可以设置以确定应显示哪个角色。

**See also** [model](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#model), [currentText](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentText-prop), [displayText](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#displayText-prop), and [ComboBox Model Roles](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#combobox-model-roles).



#### validator : Validator

------

此属性包含可编辑组合框的输入文本验证程序。

设置验证程序时，文本字段仅接受使text属性处于中间状态的输入。如果文本是在可接受的状态下，当信号将仅在**返回**或**回车**键被按下发射[接受](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#accepted-signal)信号。

当前支持的验证器是[IntValidator](https://doc.qt.io/qt-5/qml-qtquick-intvalidator.html)，[DoubleValidator](https://doc.qt.io/qt-5/qml-qtquick-doublevalidator.html)和[RegExpValidator](https://doc.qt.io/qt-5/qml-qtquick-regexpvalidator.html)。使用验证器的一个例子如下所示，这允许之间的整数的输入`0`和`10`到文本字段中：

```
ComboBox {
    model: 10
    editable: true
    validator: IntValidator {
        top: 9
        bottom: 0
    }
}
```

这个属性是在QtQuick.Controls 2.2（Qt 5.9）中引入的。

**See also** [acceptableInput](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#acceptableInput-prop), [accepted](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#accepted-signal), and [editable](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#editable-prop).





## Signal Documentation



#### void accepted()

------

在[可编辑的](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#editable-prop)组合框上按下**Return**或**Enter**键时会发出此信号。如果确认的字符串当前不在模型中，则[currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop)将设置为`-1`，并且将相应地更新`currentText`

**注意：**如果组合框上设置了[验证器](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#validator-prop)，则只有在输入处于可接受状态时才会发出信号。

该信号在QtQuick.Controls 2.2（Qt 5.9）中引入。



#### void activated([int](https://doc.qt.io/qt-5/qml-int.html) *index*)

------

当用户激活*索引*处的项目时，将发出此信号。

在弹出窗口打开时选择项目时会激活项目，导致弹出窗口关闭（和[currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop)更改），或者弹出窗口关闭并且组合框通过键盘导航，导致[currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop)更改。该[CURRENTINDEX](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop)属性设置为*索引*。

**See also** [currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop).



#### void highlighted([int](https://doc.qt.io/qt-5/qml-int.html) *index*)

------

当用户突出显示弹出列表中*索引*处的项目时，将发出此信号。

突出显示的信号仅在弹出窗口打开且项目突出显示时发出，但不一定[激活](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#activated-signal)。

**See also** [highlightedIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#highlightedIndex-prop).





## Method Documentation



#### void decrementCurrentIndex()

------

如果弹出列表可见，则递减组合框的当前索引或突出显示的索引。

**See also** [currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop) and [highlightedIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#highlightedIndex-prop).



#### [int](https://doc.qt.io/qt-5/qml-int.html) find([string](https://doc.qt.io/qt-5/qml-string.html) *text*, flags = *Qt.MatchExactly*)

------

返回指定*文本*的索引，或者`-1`如果未找到匹配项。

执行搜索的方式由指定的匹配*标志*定义。默认情况下，组合框执行区分大小写的完全匹配(`Qt.MatchExactly`)。除非`Qt.MatchCaseSensitive`还指定了标志，否则所有其他匹配类型都不区分大小写。

| Constant                | Description                |
| ----------------------- | -------------------------- |
| `Qt.MatchExactly`       | 搜索词完全匹配（默认）。   |
| `Qt.MatchRegExp`        | 搜索词匹配为正则表达式。   |
| `Qt.MatchWildcard`      | 搜索词匹配使用通配符。     |
| `Qt.MatchFixedString`   | 搜索词匹配为固定字符串。   |
| `Qt.MatchStartsWith`    | 搜索字词与项目的开头匹配。 |
| `Qt.MatchEndsWidth`     | 搜索字词与项目的结尾匹配。 |
| `Qt.MatchContains`      | 搜索项包含在项目中。       |
| `Qt.MatchCaseSensitive` | 搜索区分大小写。           |

**See also** [textRole](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#textRole-prop).



#### void incrementCurrentIndex()

------

如果弹出列表可见，则增加组合框的当前索引或突出显示的索引。

**See also** [currentIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#currentIndex-prop) and [highlightedIndex](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#highlightedIndex-prop).



#### void selectAll()

------

选择组合框的可编辑文本字段中的所有文本。

这种方法在QtQuick.Controls 2.2（Qt 5.9）中引入。

**See also** [editText](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#editText-prop).



#### [string](https://doc.qt.io/qt-5/qml-string.html) textAt([int](https://doc.qt.io/qt-5/qml-int.html) *index*)

------

返回指定*索引*的文本，如果索引超出范围，则返回空字符串。

**See also** [textRole](https://doc.qt.io/qt-5/qml-qtquick-controls2-combobox.html#textRole-prop).