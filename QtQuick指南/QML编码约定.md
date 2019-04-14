# QML Coding Conventions



本文档包含我们在文档和示例中遵循的QML编码约定，并建议其他人遵循。



## QML Object Declarations

在整个文档和示例中，[QML对象属性](https://doc.qt.io/qt-5/qtqml-syntax-objectattributes.html)始终按以下顺序构建：

- id
- property declarations
- signal declarations
- JavaScript functions
- object properties
- child objects
- states
- transitions

为了更好的可读性，我们用空行分隔这些不同的部分。

例如，假设的*照片* QML对象看起来像这样：

```c++
Rectangle {
    id: photo                                               // id on the first line makes it easy to find an object

    property bool thumbnail: false                          // property declarations
    property alias image: photoImage.source

    signal clicked                                          // signal declarations

    function doSomething(x)                                 // javascript functions
    {
        return x + photoImage.width
    }

    color: "gray"                                           // object properties
    x: 20                                                   // try to group related properties together
    y: 20
    height: 150
    width: {                                                // large bindings
        if (photoImage.width > 200) {
            photoImage.width;
        } else {
            200;
        }
    }

    Rectangle {                                             // child objects
        id: border
        anchors.centerIn: parent; color: "white"

        Image {
            id: photoImage
            anchors.centerIn: parent
        }
    }

    states: State {                                         // states
        name: "selected"
        PropertyChanges { target: border; color: "red" }
    }

    transitions: Transition {                               // transitions
        from: ""
        to: "selected"
        ColorAnimation { target: border; duration: 200 }
    }
}
```



## Grouped Properties

如果使用一组属性中的多个属性，请考虑使用*组表示法*而不是*点表示法（*如果它提高了可读性）。

例如，这个：

```
Rectangle {
    anchors.left: parent.left; anchors.top: parent.top; anchors.right: parent.right; anchors.leftMargin: 20
}

Text {
    text: "hello"
    font.bold: true; font.italic: true; font.pixelSize: 20; font.capitalization: Font.AllUppercase
}
```

可以像这样写：

```
Rectangle {
    anchors { left: parent.left; top: parent.top; right: parent.right; leftMargin: 20 }
}

Text {
    text: "hello"
    font { bold: true; italic: true; pixelSize: 20; capitalization: Font.AllUppercase }
}
```



## Lists

如果列表只包含一个元素，我们通常会省略方括号。

例如，组件通常只有一个状态是很常见的。

在这种情况下，而不是：

```
states: [
    State {
        name: "open"
        PropertyChanges { target: container; width: 200 }
    }
]
```

我们会写这个：

```
states: State {
    name: "open"
    PropertyChanges { target: container; width: 200 }
}
```



## JavaScript Code

如果脚本是单个表达式，我们建议将其内联编写：

```
Rectangle { color: "blue"; width: parent.width / 3 }
```

如果脚本只有几行，我们通常使用一个块：

```
Rectangle {
    color: "blue"
    width: {
        var w = parent.width / 3
        console.debug(w)
        return w
    }
}
```

如果脚本长度超过几行或者可以被不同的对象使用，我们建议创建一个函数并调用它：

```
function calculateWidth(object)
{
    var w = object.width / 3
    // ...
    // more javascript code
    // ...
    console.debug(w)
    return w
}

Rectangle { color: "blue"; width: calculateWidth(parent) }
```

对于长脚本，我们将把这些函数放在他们自己的JavaScript文件中并像这样导入：

```
import "myscript.js" as Script

Rectangle { color: "blue"; width: Script.calculateWidth(parent) }
```

如果代码长于一行并因此在一个块内，我们使用分号来表示每个语句的结尾：

```
MouseArea {
    anchors.fill: parent
    onClicked: {
        var scenePos = mapToItem(null, mouseX, mouseY);
        console.log("MouseArea was clicked at scene pos " + scenePos);
    }
}
```