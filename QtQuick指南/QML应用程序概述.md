# QML Applications



QML是一种声明性语言，允许用户界面根据其可视组件以及它们如何相互作用和相互关联来描述。它是一种高度可读的语言，旨在使组件以动态方式互连，并允许在用户界面中轻松地重用和自定义组件。使用该`QtQuick`模块，设计人员和开发人员可以轻松地在QML中构建流畅的动画用户界面，并可以选择将这些用户界面连接到任何后端C ++库。



### What is QML?

QML是一种用户界面规范和编程语言。它允许开发人员和设计人员创建高性能，流畅动画和视觉吸引力的应用程序。QML提供了一种高度可读，声明性，类JSON语法，支持命令式JavaScript表达式和动态属性绑定。

[Qt](https://doc.qt.io/qt-5/qtqml-index.html)语言和引擎基础结构由[Qt QML](https://doc.qt.io/qt-5/qtqml-index.html)模块提供。有关QML语言的详细信息，请参阅[Qt QML](https://doc.qt.io/qt-5/qtqml-index.html)模块文档。

以下页面包含有关QML的更多信息：

- [First Steps with QML](https://doc.qt.io/qt-5/qmlfirststeps.html) - begin using QML with these examples
- [Creating Qt Quick Projects in Qt Creator](http://doc.qt.io/qtcreator/quick-projects.html)
- [The QML Reference](https://doc.qt.io/qt-5/qmlreference.html) - reference about the QML constructs and features
- [QML Coding Conventions](https://doc.qt.io/qt-5/qml-codingconventions.html)
- [Glossary of QML Terms](https://doc.qt.io/qt-5/qml-glossary.html)



### What is Qt Quick?

Qt Quick是QML的标准类型和功能库。它包括视觉类型，交互式类型，动画，模型和视图，粒子效果和着色器效果。QML应用程序开发人员可以使用单个import语句访问所有该功能。

该`QtQuick`QML库由提供[Qt Quick的](https://doc.qt.io/qt-5/qtquick-index.html)模块。有关Qt Quick提供的各种QML类型和其他功能的详细信息，请参阅[Qt Quick](https://doc.qt.io/qt-5/qtquick-index.html)模块文档。



## QML User Interfaces

为了创建或自定义图形用户界面，除了Qt QML的标准QML类型之外，Qt Quick还添加了可视类型，动画类型和其他QML类型。Qt Quick Designer集成在Qt Creator中，并支持`QtQuick 2`Qt Creator 2.7及更高版本。

- [Visual types in QML](https://doc.qt.io/qt-5/qtquick-usecase-visual.html)
- [Responding to User Input in QML](https://doc.qt.io/qt-5/qtquick-usecase-userinput.html)
- [Animations in QML](https://doc.qt.io/qt-5/qtquick-usecase-animations.html)
- [Displaying Text in QML](https://doc.qt.io/qt-5/qtquick-usecase-text.html)
- [Layouts in QML](https://doc.qt.io/qt-5/qtquick-usecase-layouts.html)
- [Style and Theme Support](https://doc.qt.io/qt-5/qtquick-usecase-styling.html)
- [Integrating JavaScript in QML](https://doc.qt.io/qt-5/qtquick-usecase-integratingjs.html)
- [Scalability](https://doc.qt.io/qt-5/scalability.html)



### Buttons, Menus, and other Controls

对于一组基本UI控件，[Qt Quick Controls](https://doc.qt.io/qt-5/qtquickcontrols-index.html)模块实现了多个控件，如按钮，菜单和视图。这些控件带有几种可以使用的内置样式，并且还支持自定义样式的创建。

- [Qt Quick Controls 2 Guidelines](https://doc.qt.io/qt-5/qtquickcontrols2-guidelines.html)
- [Qt Quick Templates 2](https://doc.qt.io/qt-5/qtquicktemplates2-index.html)



### Special Effects

几个Qt模块提供了在应用程序中创建特殊效果的类型。它们各自的页面包含有关特定用途的更多信息

- [Particle Effects](https://doc.qt.io/qt-5/qtquick-effects-particles.html)
- [Graphical Effects](https://doc.qt.io/qt-5/graphicaleffects.html) - for creating image composition effects.



### Viewing Web Content in QML Applications

QML类型[WebEngineView](https://doc.qt.io/qt-5/qml-qtwebengine-webengineview.html)和[WebView](https://doc.qt.io/qt-5/qml-qtwebview-webview.html)呈现和显示动态Web内容。两者之间的唯一区别是后者使用本机Web视图（如果可用）。这两种类型都可以加载URL或HTML字符串。要阅读有关它们的更多信息并查看代码示例，请参阅[Qt WebEngine](https://doc.qt.io/qt-5/qtwebengine-index.html)和[Qt WebView](https://doc.qt.io/qt-5/qtwebview-index.html)。



### Sensors, Gestures, and Touch Interfaces

在[Qt的传感器](https://doc.qt.io/qt-5/qtsensors-index.html)模块允许应用程序能够读取从传感器，诸如加速计和倾斜传感器的信息。有一个适用于不同平台的通用QML API，可以在C ++中进行扩展。

- [Qt Sensors QML Types](https://doc.qt.io/qt-5/qtsensors-qmlmodule.html)
- [Qt Sensors Examples](https://doc.qt.io/qt-5/qtsensors-examples.html)
- [Compatibility Map](https://doc.qt.io/qt-5/compatmap.html) - lists support level for different mobile platforms



## Multimedia Content

在[Qt的多媒体](https://doc.qt.io/qt-5/qtmultimedia-index.html)模块使应用程序能够处理各种媒体内容的便利的一套QML类型。这些QML类型可以用C ++扩展。

- Multimedia
  - [Audio Overview](https://doc.qt.io/qt-5/audiooverview.html)
  - [Video Overview](https://doc.qt.io/qt-5/videooverview.html)
  - [Camera Overview](https://doc.qt.io/qt-5/cameraoverview.html)
  - [Radio Overview](https://doc.qt.io/qt-5/radiooverview.html)
  - [Qt Audio Engine](https://doc.qt.io/qt-5/qtaudioengine-qmlmodule.html) - for 3D positional audio playback and content management.



## Mobile Devices

几个Qt模块为联网和移动设备提供QML API。QML类型提供对蓝牙，近场通信（NFC）和支持GPS的设备的访问。

- [Qt Positioning](https://doc.qt.io/qt-5/qtpositioning-index.html)
- [Qt Location](https://doc.qt.io/qt-5/qtlocation-index.html)
- [Qt Bluetooth](https://doc.qt.io/qt-5/qtbluetooth-index.html)
- [Qt NFC](https://doc.qt.io/qt-5/qtnfc-index.html)

For more information, visit the [Networking and Connectivity](https://doc.qt.io/qt-5/topics-network-connectivity.html) and [Mobile APIs](https://doc.qt.io/qt-5/mobiledevelopment.html) pages.



## Code Samples and Demos

要了解有关QML代码使用的更多信息，有几个代码示例显示了如何使用QML类型。此外，还有几个演示，展示了如何在应用程序中使用QML代码。

- [Getting Started Programming with Qt Quick](https://doc.qt.io/qt-5/qtdoc-tutorials-alarms-example.html) - a tutorial showing the creation of a simple QML text editor.
- [Qt Quick Examples and Tutorials](https://doc.qt.io/qt-5/qtquick-codesamples.html)



## Advanced Application Development Topics

- [Integrating QML and C++](https://doc.qt.io/qt-5/qtqml-cppintegration-topic.html)
- [Deploying QML Applications](https://doc.qt.io/qt-5/qtquick-deployment.html)
- [Best Practices for QML and Qt Quick](https://doc.qt.io/qt-5/qtquick-bestpractices.html)
- [Performance Considerations and Suggestions](https://doc.qt.io/qt-5/qtquick-performance.html)
- [Internationalization and Localization](https://doc.qt.io/qt-5/qtquick-internationalization.html)
- Testing and Debugging
  - [Prototyping with qmlscene](https://doc.qt.io/qt-5/qtquick-qmlscene.html)
  - [Debugging QML Applications](https://doc.qt.io/qt-5/qtquick-debugging.html)
  - [Qt Quick Test: QML Unit Testing Framework](https://doc.qt.io/qt-5/qtquicktest-index.html)



## Other QML Modules

Qt Quick仅提供基本的视觉类型，而Qt的大部分功能都通过其他模块暴露给QML。如果您需要这些模块的功能，则应浏览其QML文档。

The [All QML APIs by Module](https://doc.qt.io/qt-5/modules-qml.html) contains a list of all QML modules in Qt 5.



## Related Topics

- [Porting QML Applications to Qt 5](https://doc.qt.io/qt-5/qtquick-porting-qt5.html)