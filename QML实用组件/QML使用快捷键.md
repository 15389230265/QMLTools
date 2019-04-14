# Shortcut QML Type



提供键盘快捷键. [More...](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#details)

|                   |                     |
| ----------------- | ------------------- |
| Import Statement: | import QtQuick 2.12 |
| Since:            | Qt 5.5              |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qml-qtquick-shortcut-members.html)



## Properties

- **autoRepeat** : bool
- **context** : enumeration
- **enabled** : bool
- **nativeText** : string
- **portableText** : string
- **sequence** : keysequence
- **sequences** : list<keysequence>



## Signals

- **activated**()
- **activatedAmbiguously**()



## Detailed Description

快捷方式类型提供了一种处理键盘快捷方式的方法。快捷方式可以设置为[标准键盘快捷方式之一](https://doc.qt.io/qt-5/qkeysequence.html#StandardKey-enum)，也可以使用包含[激活](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#activated-signal)快捷方式所需的最多四个按键序列的字符串进行描述。

```
Item {
    id: view

    property int currentIndex

    Shortcut {
        sequence: StandardKey.NextChild
        onActivated: view.currentIndex++
    }
}
```

还可以设置多个快捷方式[序列](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#sequences-prop)，以便可以通过几个不同的按键序列[激活](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#activated-signal)快捷方式。

**See also** [Keys](https://doc.qt.io/qt-5/qml-qtquick-keys.html) and [shortcutOverride()](https://doc.qt.io/qt-5/qml-qtquick-keys.html#shortcutOverride-signal).



## Property Documentation



#### autoRepeat : [bool](https://doc.qt.io/qt-5/qml-bool.html)

此属性保存快捷方式是否可以自动重复。

默认值为`true`。



#### context : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

此属性包含[快捷方式上下文](https://doc.qt.io/qt-5/qt.html#ShortcutContext-enum)。

支持的值是：

- `Qt.WindowShortcut` （默认） - 当其父项位于活动顶级窗口中时，快捷方式处于活动状态。
- `Qt.ApplicationShortcut` - 当应用程序的某个窗口处于活动状态时，该快捷方式处于活动状态。

```
Shortcut {
    sequence: StandardKey.Quit
    context: Qt.ApplicationShortcut
    onActivated: Qt.quit()
}
```



#### enabled : [bool](https://doc.qt.io/qt-5/qml-bool.html)

此属性保存是否启用了快捷方式。

默认值为`true`。



#### nativeText : [string](https://doc.qt.io/qt-5/qml-string.html)

此属性提供快捷方式的键序列作为特定于平台的字符串。这意味着它将被显示为已翻译，并且在[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)上它将类似于菜单栏中的键序列。最好将此文本显示给用户（例如，在工具提示上）。

该属性在Qt 5.6中引入。

**See also** [sequence](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#sequence-prop) and [portableText](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#portableText-prop).



#### portableText : [string](https://doc.qt.io/qt-5/qml-string.html)

此属性将快捷键的键序列作为“可移植”格式的字符串提供，适用于读取和写入文件。在许多情况下，它看起来与Windows和X11上的本机文本类似。

该属性在Qt 5.6中引入。

**See also** [sequence](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#sequence-prop) and [nativeText](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#nativeText-prop).



#### sequence : keysequence

此属性包含快捷键的键序列。键序列可以设置为[标准键盘快捷键之一](https://doc.qt.io/qt-5/qkeysequence.html#StandardKey-enum)，也可以使用包含[激活](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#activated-signal)快捷键所需的最多四次按键序列的字符串进行描述。

默认值为空键序列。

```
Shortcut {
    sequence: "Ctrl+E,Ctrl+W"
    onActivated: edit.wrapMode = TextEdit.Wrap
}
```

**See also** [sequences](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#sequences-prop).



#### sequences : [list](https://doc.qt.io/qt-5/qml-list.html) < keysequence >

此属性包含快捷方式的多个键序列。键序列可以设置为[标准键盘快捷键之一](https://doc.qt.io/qt-5/qkeysequence.html#StandardKey-enum)，也可以使用包含[激活](https://doc.qt.io/qt-5/qml-qtquick-shortcut.html#activated-signal)快捷键所需的最多四次按键序列的字符串来描述。

```
Shortcut {
    sequences: [StandardKey.Cut, "Ctrl+X", "Shift+Del"]
    onActivated: edit.cut()
}
```

This property was introduced in Qt 5.9.



## Signal Documentation



#### activated()

激活快捷方式时会发出此信号。

相应的处理程序是`onActivated`。



#### activatedAmbiguously()

当快捷方式被激活时，会发出此信号，这意味着它与多个快捷方式的开头匹配。

相应的处理程序是`onActivatedAmbiguously`。