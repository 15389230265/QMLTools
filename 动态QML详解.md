[TOC]



# 动态QML( Dynamic QML)

​	通过使用QML和JavaScript以更加动态的方式，进一步的扩大灵活性。组件可以在运行时加载和实例化，元素能够被销毁。动态创建的用户界面能够被存储在磁盘上，并且恢复。



## 动态加载组件（Loading Components Dynamically）

动态加载QML不同组成部分最简单的方法是使用加载元素项（Loader element）。它作为一个占位符项用来加载项。项的加载通过资源属性（source property）或者资源组件（sourceCompontent）属性控制。加载元素项通过给定的URL链接加载项，然后实例化一个组件。

加载元素项（loader）作为一个占位符用于被加载项的加载。它的大小基于被加载项的大小而定，反之亦然。如果加载元素定义了大小，或者通过锚定（anchoring）定义了宽度和高度，被加载项将会被设置为加载元素项的大小。如果加载元素项没有设置大小，它将会根据被加载项的大小而定。

下面例子演示了使用加载元素项（Loader Element）将两个分离的用户界面部分加载到一个相同的空间。这个主意是我们有一个快速拨号界面，可以是数字界面或模拟界面。如下面插图所示。表盘周围的数字不受被加载项影响。

![img](动态QML详解.assets/loader-analog.png)

![img](动态QML详解.assets/loader-digital.png)

首先，在应用程序中声明一个加载元素项（Loader element）。注意，资源属性（source property）已经被忽略。这是因为资源取决于当前用户界面状态。

```
Loader {
    id: dialLoader

    anchors.fill: parent
}
```

拨号加载器（dialLoader）的父项的状态属性改变元素驱动根据不同状态加载不同的QML文件。在这个例子中，资源属性（source property）是一个相对文件路径，但是它可以是一个完整的URL链接，通过网络获取加载项。

```
    states: [
        State {
            name: "analog"
            PropertyChanges { target: analogButton; color: "green"; }
            PropertyChanges { target: dialLoader; source: "Analog.qml"; }
        },
        State {
            name: "digital"
            PropertyChanges { target: digitalButton; color: "green"; }
            PropertyChanges { target: dialLoader; source: "Digital.qml"; }
        }
    ]
```

为了使被加载项更加生动，它的速度属性必须根项的速度属性绑定。不能够使用直接绑定来绑定属性，因为项不是总在加载，并且这会随着时间而改变。需要使用一个东西来替换绑定元素（Binding Element）。绑定目标属性（target property），每次加载元素项改变时会触发已加载完成（onLoaded）信号。

```
    Loader {
        id: dialLoader

        anchors.left: parent.left
        anchors.right: parent.right
        anchors.top: parent.top
        anchors.bottom: analogButton.top

        onLoaded: {
            binder.target = dialLoader.item;
        }
    }
    Binding {
        id: binder

        property: "speed"
        value: speed
    }
```

当被加载项加载完成后，加载完成信号（onLoaded）会触发加载QML的动作。类似的，QML完成加载以来与组建构建完成（Component.onCompleted）信号。所有组建都可以使用这个信号，无论它们何时加载。例如，当整个用户界面完成加载后，整个应用程序的根组建可以使用它来启动自己。



### 间接连接（Connecting Indirectly）

动态创建QML元素时，无法使用onSignalName静态配置来连接信号。必须使用连接元素（Connection element）来完成连接信号。它可以连接一个目标元素任意数量的信号。

通过设置连接元素（Connection element）的目标属性，信号可以像正常的方法连接。也就是使用onSignalName方法。不管怎样，通过改变目标属性可以在不同的时间监控不同的元素。

![img](动态QML详解.assets/connections.png)

在上面这个例子中，用户界面由两个可点击区域组成。当其中一个区域点击后，会使用一个闪烁的动画。左边区域的代码段如下所示。在鼠标区域（MouseArea）中，左点击动画（leftClickedAnimation）被触发，导致区域闪烁。

```
        Rectangle {
            id: leftRectangle

            width: 290
            height: 200

            color: "green"

            MouseArea {
                id: leftMouseArea
                anchors.fill: parent
                onClicked: leftClickedAnimation.start();
            }

            Text {
                anchors.centerIn: parent
                font.pixelSize: 30
                color: "white"
                text: "Click me!"
            }
        }
```

除了两个可点击区域，还使用了一个连接元素（Connection element）。当状态为激活时会触发第三个动画，即元素的目标。

```
    Connections {
        id: connections
        onClicked: activeClickedAnimation.start();
    }
```

为了确定鼠标区域的目标，定义了两种状态。注意我们无法使用属性改变元素（PropertyChanges element）来设置目标属性，因为它已经包含了一个目标属性。利用状态改变脚本（StateChangeScript）来完成。

```
    states: [
        State {
            name: "left"
            StateChangeScript {
                script: connections.target = leftMouseArea
            }
        },
        State {
            name: "right"
            StateChangeScript {
                script: connections.target = rightMouseArea
            }
        }
    ]
```

当尝试运行这个例子时，需要注意当多个信号被处理调用所有操作时，执行的顺序是未定义的。

当创建一个连接元素（Connection element）未指定目标属性时，默认的属性是父对象。这意味着需要显式的设置NULL来避免捕获来自父对象的信号，直到目标被设置。这种行为使得基于连接元素（Connection element）创建自定义信号处理组件成为可能。使用这种方式可以将信号的处理代码封装和再使用。

在下面这个例子中，闪烁组件能够被放在任何的鼠标区域（MouseArea）中.点击后会触发动画，导致父对象闪烁。在同一个鼠标区域（MouseArea）的实际任务被触发时也可以被调用。这从实际的动作中，分离了标准的用户反馈，闪烁。

```
import QtQuick 2.0

Connections {
    onClicked: {
        // Automatically targets the parent
    }
}
```

只需要简单的在每个鼠标区域（MouseArea）实例化一个闪烁组件来实现闪烁。

```
import QtQuick 2.0

Item {
    // A background flasher that flashes the background of any parent MouseArea
}
```

当你使用一个连接元素（Connection element）来监控不同类型的目标元素的信号时，你可能会发现在在某些场景下会有来自不同目标的可用信号。这将导致连接元素（Connections element）由于丢失信号输出运行错误（run-time errors）。为了避免这个问题，设置忽略未知信号（ignoreUnknownSignal）属性为true，可以忽略这些错误。