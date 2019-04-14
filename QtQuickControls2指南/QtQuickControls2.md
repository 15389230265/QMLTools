[TOC]



# Qt Quick Controls



Qt Quick Controls 2提供了一组控件，可用于在Qt Quick中构建完整的界面。该模块在Qt 5.7中引入。

![img](QtQuickControls2.assets/qtquickcontrols2-styles.png)

Qt Quick Controls 2提供可选择的可定制样式。 See [Styling Qt Quick Controls 2](https://doc.qt.io/qt-5/qtquickcontrols2-styles.html) for more details.



## Prerequisites

The [QML types](https://doc.qt.io/qt-5/qtquick-controls2-qmlmodule.html) can be imported into your application using the following import statement in your `.qml` file:

```
import QtQuick.Controls 2.12
```

The [C++ classes](https://doc.qt.io/qt-5/qtquickcontrols2-module.html) can be included into your application using the following include statement:

```
#include <QtQuickControls2>
```

To link against the corresponding C++ libraries, add the following to your qmake project file:

```
QT += quickcontrols2
```

For more details, see [Getting Started with Qt Quick Controls 2](https://doc.qt.io/qt-5/qtquickcontrols2-gettingstarted.html).



### Building From Source

从源代码构建时，请确保Qt图形效果模块也已构建，因为Qt Quick Controls 2需要它。

建议使用[Qt Image Formats](https://doc.qt.io/qt-5/license-changes.html#qt-image-formats)模块，但不是必需的。它支持[Imagine style](https://doc.qt.io/qt-5/qtquickcontrols2-imagine.html#animated-images)的`.webp`格式。



## Versions

Qt Quick Controls 2.0在Qt 5.7中引入。随后的次要Qt版本将Qt Quick Controls 2模块的导入版本增加1，直到Qt 5.12，其中导入版本与Qt的次要版本匹配。实验性Qt Labs模块使用导入版本1.0。

| `Qt` | [`QtQuick`](https://doc.qt.io/qt-5/qtquick-qmlmodule.html) | [`QtQuick.Controls`](https://doc.qt.io/qt-5/qtquick-controls2-qmlmodule.html), [`QtQuick.Controls.Material`](https://doc.qt.io/qt-5/qtquickcontrols2-material.html), [`QtQuick.Controls.Universal`](https://doc.qt.io/qt-5/qtquickcontrols2-universal.html), [`QtQuick.Templates`](https://doc.qt.io/qt-5/qtquick-templates2-qmlmodule.html) | [`Qt.labs.calendar`](https://doc.qt.io/qt-5/qt-labs-calendar-qmlmodule.html), [`Qt.labs.platform`](https://doc.qt.io/qt-5/qt-labs-platform-qmlmodule.html) |
| ---- | ---------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 5.7  | 2.7                                                        | 2.0                                                          | 1.0                                                          |
| 5.8  | 2.8                                                        | 2.1                                                          | 1.0                                                          |
| 5.9  | 2.9                                                        | 2.2                                                          | 1.0                                                          |
| 5.10 | 2.10                                                       | 2.3                                                          | 1.0                                                          |
| 5.11 | 2.11                                                       | 2.4                                                          | 1.0                                                          |
| 5.12 | 2.12                                                       | 2.12                                                         | 1.0                                                          |
| ...  | ...                                                        | ...                                                          | ...                                                          |



## License and Attributions

Qt Quick Controls 2可从[The Qt Company](http://www.qt.io/about-us/)获得商业许可。此外，它可以在[GNU宽通用公共许可证版本3](http://www.gnu.org/licenses/lgpl-3.0.html)或[GNU通用公共许可证版本2下获得](http://www.gnu.org/licenses/gpl-2.0.html)。有关详细信息，请参阅[Qt Licensing](https://doc.qt.io/qt-5/licensing.html)。

此外，Qt Quick Controls 2可能包含以下许可许可下的第三方模块：



## Topics

- [Guidelines](https://doc.qt.io/qt-5/qtquickcontrols2-guidelines.html)
- [Styling](https://doc.qt.io/qt-5/qtquickcontrols2-styles.html)
- [Icons](https://doc.qt.io/qt-5/qtquickcontrols2-icons.html)
- [Customization](https://doc.qt.io/qt-5/qtquickcontrols2-customize.html)
- [High-DPI Support](https://doc.qt.io/qt-5/qtquickcontrols2-highdpi.html)
- [Using File Selectors](https://doc.qt.io/qt-5/qtquickcontrols2-fileselectors.html)
- [Deployment](https://doc.qt.io/qt-5/qtquickcontrols2-deployment.html)
- [Configuration File](https://doc.qt.io/qt-5/qtquickcontrols2-configuration.html)
- [Environment Variables](https://doc.qt.io/qt-5/qtquickcontrols2-environment.html)
- [Differences between Qt Quick Controls 1](https://doc.qt.io/qt-5/qtquickcontrols2-differences.html)



## Reference

- [QML Types](https://doc.qt.io/qt-5/qtquick-controls2-qmlmodule.html)
- [C++ Classes](https://doc.qt.io/qt-5/qtquickcontrols2-module.html)



## Examples

- [Gallery](https://doc.qt.io/qt-5/qtquickcontrols-gallery-example.html)
- [Chat Tutorial](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html)
- [Text Editor](https://doc.qt.io/qt-5/qtquickcontrols-texteditor-example.html)
- [Wearable Demo](https://doc.qt.io/qt-5/qtquickcontrols-wearable-example.html)
- [Automotive Example](https://doc.qt.io/qt-5/qtquickcontrols-imagine-automotive-example.html)
- [Music Player Example](https://doc.qt.io/qt-5/qtquickcontrols-imagine-musicplayer-example.html)
- [All Examples](https://doc.qt.io/qt-5/qtquickcontrols2-examples.html)



## Related Modules

- [Qt Quick](https://doc.qt.io/qt-5/qtquick-index.html)
- [Qt Quick Layouts](https://doc.qt.io/qt-5/qtquicklayouts-index.html)
- [Qt Quick Templates 2](https://doc.qt.io/qt-5/qtquicktemplates2-index.html)
- [Qt Labs Calendar](https://doc.qt.io/qt-5/qtlabscalendar-index.html)
- [Qt Labs Platform](https://doc.qt.io/qt-5/whatsnew512.html#qt-labs-platform)





# Getting Started with Qt Quick Controls 2



此处显示了使用控件的QML文件的基本示例：

```c++
import QtQuick 2.12
import QtQuick.Controls 2.12

ApplicationWindow {
    title: "My Application"
    width: 640
    height: 480
    visible: true

    Button {
        text: "Push Me"
        anchors.centerIn: parent
    }
}
```



## Setting Up Controls from C++

虽然[QQuickView](https://doc.qt.io/qt-5/qquickview.html)传统上用于在C ++应用程序中显示QML文件，但这样做意味着您只能从C ++设置窗口属性。

使用Qt Quick Controls 2，将[ApplicationWindow](https://doc.qt.io/qt-5/qml-qtquick-controls2-applicationwindow.html)声明为[应用程序](https://doc.qt.io/qt-5/qml-qtquick-controls2-applicationwindow.html)的根项，然后使用[QQmlApplicationEngine](https://doc.qt.io/qt-5/qqmlapplicationengine.html)启动它。这可确保您可以从QML控制顶级窗口属性。

此处显示了使用控件的源文件的基本示例：

```c++
#include <QGuiApplication>
#include <QQmlApplicationEngine>

int main(int argc, char *argv[])
{
    QGuiApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
    QGuiApplication app(argc, argv);
    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));
    return app.exec();
}
```



### Using C++ Data From QML

如果需要注册要在QML中使用的C ++类，可以在声明[QQmlApplicationEngine](https://doc.qt.io/qt-5/qqmlapplicationengine.html)之前调用[qmlRegisterType](https://doc.qt.io/qt-5/qqmlengine.html#qmlRegisterType)（）。有关更多信息，请参阅[从C ++定义QML类型](https://doc.qt.io/qt-5/qtqml-cppintegration-overview.html#defining-qml-types-from-c)。

如果需要向QML组件公开数据，则需要使它们可用于当前QML引擎的上下文。有关更多信息，请参阅[QQmlContext](https://doc.qt.io/qt-5/qqmlcontext.html)。