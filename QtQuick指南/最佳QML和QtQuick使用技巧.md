# Best Practices for QML and Qt Quick



尽管QML和Qt Quick提供了所有好处，但在某些情况下它们可能具有挑战性。以下部分详细介绍了一些可帮助您在开发应用程序时获得更好结果的最佳实践。



## Custom UI Controls

流畅的现代UI是任何应用程序在当今世界取得成功的关键，而QML对于设计人员或开发人员来说非常有意义。Qt提供了最基本的UI控件，这些控件是创建流畅和现代外观UI所必需的。建议在创建自己的自定义UI控件之前浏览此UI控件列表。

除了Qt Quick本身提供的这些基本UI控件外，Qt Quick Controls 2还提供了丰富的UI控件。它们可以满足最常见的用例而无需任何更改，并且可以通过自定义选项提供更多可能性。特别是，Qt Quick Controls 2提供符合最新UI设计趋势的样式选项。如果这些UI控件不能满足您的应用程序需求，则建议您仅创建自定义控件。



### Related Information

- [Qt Quick Controls 2](https://doc.qt.io/qt-5/qtquickcontrols-index.html)
- [Qt Quick](https://doc.qt.io/qt-5/qtquick-index.html)



## Coding Conventions

See [QML Coding Conventions](https://doc.qt.io/qt-5/qml-codingconventions.html).



## Bundle Application Resources

大多数应用程序依赖于图像和图标等资源来提供丰富的用户体验。无论目标操作系统如何，将这些资源提供给应用程序通常都是一项挑战。最流行的操作系统采用更严格的安全策略来限制对文件系统的访问，从而加载这些资源变得更加困难。作为替代方案，Qt提供了自己的[资源系统](https://doc.qt.io/qt-5/resources.html)，该[系统](https://doc.qt.io/qt-5/resources.html)内置于应用程序二进制文件中，无论目标操作系统如何，都可以访问应用程序的资源。

例如，请考虑以下项目目录结构：

```
project
├── images
│   ├── image1.png
│   └── image2.png
├── project.pro
└── qml
    └── main.qml
```

以下条目`project.pro`确保将资源内置到应用程序二进制文件中，以便在需要时使用它们：

```
RESOURCES += \
    qml/main.qml \
    images/image1.png \
    images/image2.png
```

更方便的方法是使用[通配符语法](https://doc.qt.io/qt-5/qmake-function-reference.html#files-pattern-recursive-false)一次选择多个文件：

```
RESOURCES += \
    $$files(qml/*.qml) \
    $$files(images/*.png)
```

这种方法对于依赖于有限数量资源的应用程序很方便。但是，每当`RESOURCES`使用此方法添加新文件时，它都会导致重新编译*所有*其他文件`RESOURCES`。这可能效率低下，尤其是对于大型文件集。在这种情况下，更好的方法是将每种类型的资源分成它自己的[.qrc](https://doc.qt.io/qt-5/resources.html#resource-collection-files)文件。例如，上面的代码段可以更改为以下内容：

```
qml.files = $$files(*.qml)
qml.prefix = /qml
RESOURCES += qml

images.files = $$files(*.png)
images.prefix = /images
RESOURCES += images
```

现在，每当更改QML文件时，只需重新编译QML文件。

有时可能需要对资源系统管理的特定文件的路径进行更多控制。例如，如果我们想要`image2.png`一个别名，我们需要切换到一个显式`.qrc`文件。[创建资源文件](https://doc.qt.io/qtcreator/creator-project-creating.html#creating-resource-files)详细说明了如何执行此操作。



### Related Information

- [The Qt Resource System](https://doc.qt.io/qt-5/resources.html)



## Separate UI from Logic

大多数应用程序开发人员希望实现的关键目标之一是创建可维护的应用程序。实现此目标的方法之一是将用户界面与业务逻辑分开。以下是应用程序的UI应该用QML编写的几个原因：

- 声明性语言非常适合定义UI。
- QML代码编写起来比较简单，因为它比C ++更简洁，并且不是强类型的。这也使它成为原型的优秀语言，例如，与设计师合作时至关重要的品质。
- JavaScript可以很容易地在QML中用于响应事件。

作为一种强类型语言，C ++最适合应用程序的逻辑。通常，此类代码执行复杂计算或数据处理等任务，这些任务在C ++中比QML更快。

Qt提供了各种在应用程序中集成QML和C ++代码的方法。典型的用例是在用户界面中显示数据列表。如果数据集是静态的，简单的和/或小的，则用QML编写的模型就足够了。

以下代码段演示了使用QML编写的模型示例：

```
model: ListModel {
    ListElement { name: "Item 1" }
    ListElement { name: "Item 2" }
    ListElement { name: "Item 3" }
}

model: [ "Item 1", "Item 2", "Item 3" ]

model: 10
```

Use [C++](https://doc.qt.io/qt-5/qtquick-modelviewsdata-cppmodels.html#qabstractitemmodel-subclass) for dynamic data sets that are large or frequently modified.



### Interacting with QML from C++

虽然Qt允许您从C ++操作QML，但不建议这样做。为了解释原因，让我们看一个简化的例子。



#### Pulling References from QML

假设我们正在为设置页面编写UI：

```
import QtQuick 2.12
import QtQuick.Controls 2.12

Page {
    Button {
        text: qsTr("Restore default settings")
    }
}
```

我们希望按钮在单击时使用C ++执行某些操作。我们知道QML中的对象可以像在C ++中那样发出变化信号，因此我们给按钮一个[objectName，](https://doc.qt.io/qt-5/qml-qtqml-qtobject.html#objectName-prop)以便我们可以从C ++中找到它：

```
Button {
    objectName: "restoreDefaultsButton"
    text: qsTr("Restore default settings")
}
```

然后，在C ++中，我们找到该对象并连接到其变化信号：

```c++
#include <QGuiApplication>
#include <QQmlApplicationEngine>
#include <QSettings>

class Backend : public QObject
{
    Q_OBJECT

public:
    Backend() {}

public slots:
    void restoreDefaults() {
        settings.setValue("loadLastProject", QVariant(false));
    }

private:
    QSettings settings;
};

int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);

    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));
    if (engine.rootObjects().isEmpty())
        return -1;

    Backend backend;

    QObject *rootObject = engine.rootObjects().first();
    QObject *restoreDefaultsButton = rootObject->findChild<QObject*>("restoreDefaultsButton");
    QObject::connect(restoreDefaultsButton, SIGNAL(clicked()),
        &backend, SLOT(restoreDefaults()));

    return app.exec();
}

#include "main.moc"
```

通过这种方法，可以从QML“拉出”对象的引用。这个问题是C ++逻辑层依赖于QML表示层。如果我们以这样的方式重构QML，使得`objectName`更改或其他一些更改破坏了C ++找到QML对象的能力，那么我们的工作流程变得更加复杂和繁琐。



#### Pushing References to QML

重构QML比重构C ++容易得多，因此为了使维护无痛，我们应该尽可能地保持C ++类型不了解QML。这可以通过将对C ++类型的引用“推送”到QML中来实现：

```c++
int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);

    Backend backend;

    QQmlApplicationEngine engine;
    engine.rootContext()->setContextProperty("backend", &backend);
    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));
    if (engine.rootObjects().isEmpty())
        return -1;

    return app.exec();
}
```

然后QML直接调用C ++插槽：

```
import QtQuick 2.12
import QtQuick.Controls 2.12

Page {
    Button {
        text: qsTr("Restore default settings")
        onClicked: backend.restoreDefaults()
    }
}
```

使用这种方法，如果将来需要重构QML，C ++将保持不变。

在上面的示例中，我们在根上下文中设置了一个context属性，以将C ++对象公开给QML。这意味着该属性可供引擎加载的每个组件使用。对于必须在加载QML时必须可用且不能在QML中实例化的对象，上下文属性非常有用。

有关选择将C ++类型公开给QML的正确方法的快速指南，请参阅[Choosing the Correct Integration Method Between C++ and QML](https://doc.qt.io/qt-5/qtqml-cppintegration-overview.html#choosing-the-correct-integration-method-between-c-and-qml).



### Related Information

- [Integrating QML and C++](https://doc.qt.io/qt-5/qtqml-cppintegration-topic.html)
- [Chat application tutorial](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html)



## Using Qt Quick Layouts

Qt提供Qt快速布局，以便在布局中直观地排列Qt Quick项目。与其替代项目定位器不同，Qt快速布局也可以在窗口调整大小时调整其子项的大小。虽然Qt Quick的布局往往是大多数使用情况所需要的选项，下面的*DOS*和*注意事项*必须同时使用它们来考虑：



### DOS

- 使用[anchors](https://doc.qt.io/qt-5/qml-qtquick-item.html#anchors-prop)或[width](https://doc.qt.io/qt-5/qml-qtquick-item.html#width-prop)和[height](https://doc.qt.io/qt-5/qml-qtquick-item.html#height-prop)属性可以根据非布局父项指定布局的大小。
- 使用[Layout](https://doc.qt.io/qt-5/qml-qtquick-layouts-layout.html)附加属性设置布局的直接子项的大小和对齐属性。



### Don'ts

- 不要为提供implicitWidth和implicitHeight的项定义首选大小，除非它们的隐式大小不令人满意。

- 不要在作为布局的直接子项的项目上使用锚点。相反，使用`Layout.preferredWidth`和`Layout.preferredHeight`:

  ```
  RowLayout {
      id: layout
      anchors.fill: parent
      spacing: 6
      Rectangle {
          color: 'azure'
          Layout.fillWidth: true
          Layout.minimumWidth: 50
          Layout.preferredWidth: 100
          Layout.maximumWidth: 300
          Layout.minimumHeight: 150
          Text {
              anchors.centerIn: parent
              text: parent.width + 'x' + parent.height
          }
      }
      Rectangle {
          color: 'plum'
          Layout.fillWidth: true
          Layout.minimumWidth: 100
          Layout.preferredWidth: 200
          Layout.preferredHeight: 100
          Text {
              anchors.centerIn: parent
              text: parent.width + 'x' + parent.height
          }
      }
  }
  ```

**注意：**布局和锚点都是需要更多内存和实例化时间的对象类型。当对x，y，width和height属性的简单绑定足够时，避免使用它们（特别是在列表和表委托中，以及控件的样式）。



### Related Information

- [Item Positioners](https://doc.qt.io/qt-5/qtquick-positioning-layouts.html)
- [Qt Quick Layouts Overview](https://doc.qt.io/qt-5/qtquicklayouts-overview.html)



## Type Safety

在QML中声明属性时，使用“var”类型很简单方便：

```
property var name
property var size
property var optionsMenu
```

但是，这种方法有几个缺点：

- 如果指定了类型错误的值，则报告的错误将指向属性声明的位置，而不是指定属性的位置。这会使跟踪错误变得更加困难，从而减慢了开发过程。
- 静态分析以捕捉上述错误是不可能的。
- 读者并不总是立即清楚该属性的实际基础类型。

相反，请尽可能使用实际类型：

```
property string name
property int size
property MyMenu optionsMenu
```



## Performance

有关QML和Qt Quick中的性能的信息，请参阅 [Performance Considerations And Suggestions](https://doc.qt.io/qt-5/qtquick-performance.html).



## Tools and Utilities

有关使QML和Qt Quick更容易使用的有用工具和实用程序的信息，请参阅 [Qt Quick Tools and Utilities](https://doc.qt.io/qt-5/qtquick-tools-and-utilities.html).



## Scene Graph

有关Qt Quick场景图的信息，请参阅 [Qt Quick Scene Graph](https://doc.qt.io/qt-5/qtquick-visualcanvas-scenegraph.html).



## Scalable User Interfaces

随着显示分辨率的提高，可伸缩的应用程序UI变得越来越重要。实现此目的的方法之一是为不同的屏幕分辨率维护UI的多个副本，并根据可用的分辨率加载适当的UI。虽然这很好用，但它增加了维护开销。

Qt为此问题提供了更好的解决方案，并建议应用程序开发人员遵循以下提示：

- 使用锚点或Qt快速布局模块布置可视项目。
- 不要为可视项指定显式宽度和高度。
- 为应用程序支持的每种显示分辨率提供UI资源，如图像和图标。Qt的快速控制2像册例如通过提供演示此井`qt-logo.png`为`@2x`，`@3x`，和`@4x`分辨率，使应用程序以满足高分辨率显示器。如果明确启用了高DPI缩放功能，Qt会自动选择适合给定显示的图像。
- 将SVG图像用于小图标。虽然较大的SVG渲染速度较慢，但较小的SVG运行良好。矢量图像避免了提供图像的多个版本的需要，这对于位图图像是必要的。
- 使用基于字体的图标，例如Font Awesome。它们可以扩展到任何显示分辨率，也可以进行着色。Qt Quick Controls 2文本编辑器示例演示了这一点。

有了这个，您的应用程序的UI应根据提供的显示分辨率进行缩放。

![img](最佳QML和QtQuick使用技巧.assets/qtquickcontrols2-gallery-welcome.png)



### Related Information

- [Gallery example](https://doc.qt.io/qt-5/qtquickcontrols-gallery-example.html)
- [Text Editor example](https://doc.qt.io/qt-5/qtquickcontrols-texteditor-example.html)
- [Font Awesome](https://fontawesome.com/)
- [Scalability](https://doc.qt.io/qt-5/scalability.html)
- [High DPI Displays](https://doc.qt.io/qt-5/highdpi.html#high-dpi-displays)