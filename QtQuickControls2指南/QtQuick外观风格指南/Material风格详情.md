# Material Style



The Material Style is based on the Google Material Design Guidelines. [More...](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#detailed-desc-material)

| Import Statement: | import QtQuick.Controls.Material 2.12 |
| ----------------- | ------------------------------------- |
| Since:            | Qt 5.7                                |



## Attached Properties

- [**accent**](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-accent-attached-prop) : color
- [**background**](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-background-attached-prop) : color
- [**elevation**](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-elevation-attached-prop) : int
- [**foreground**](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-foreground-attached-prop) : color
- [**primary**](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-primary-attached-prop) : color
- [**theme**](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-theme-attached-prop) : enumeration





## Attached Methods

- color [**color**](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-color-attached-method)(enumeration predefined, enumeration shade)



## Detailed Description



材质样式基于[Google Material Design Guidelines](https://www.google.com/design/spec/material-design/introduction.html)。它允许跨平台和设备大小的统一体验。

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![img](Material风格详情.assets/qtquickcontrols2-material-light.png)明亮主题 | ![img](Material风格详情.assets/qtquickcontrols2-material-dark.png)黑暗主题 |

要使用“材质”样式运行应用程序，请参阅[在Qt快速控制中使用样式2](https://doc.qt.io/qt-5/qtquickcontrols2-styles.html#using-styles-in-qt-quick-controls-2)。

**注意：** Material样式不是原生Android样式。Material样式是遵循Google Material Design Guidelines的100％跨平台Qt Quick Controls 2样式实现。该风格可在任何平台上运行，并且在各处看起来或多或少相同。由于可用系统字体和字体呈现引擎的差异，可能会出现细微差别。





### Customization

The Material style allows customizing five attributes, [theme](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-theme-attached-prop), [primary](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-primary-attached-prop), [accent](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-accent-attached-prop), [foreground](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-foreground-attached-prop), and [background](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-background-attached-prop).

![img](Material风格详情.assets/qtquickcontrols2-material-attributes.png)

可以为任何窗口或项目指定所有属性，它们会以与[字体](https://doc.qt.io/qt-5/qml-qtquick-controls2-control.html#font-prop)相同的方式自动传播到子项。在以下示例中，窗口和所有三个单选按钮都使用紫色强调颜色显示在黑暗主题中：

```
import QtQuick 2.12
import QtQuick.Controls 2.12
import QtQuick.Controls.Material 2.12

ApplicationWindow {
    visible: true

    Material.theme: Material.Dark
    Material.accent: Material.Purple

    Column {
        anchors.centerIn: parent

        RadioButton { text: qsTr("Small") }
        RadioButton { text: qsTr("Medium");  checked: true }
        RadioButton { text: qsTr("Large") }
    }
}
```

![img](Material风格详情.assets/qtquickcontrols2-material-purple-1555143533194.png)

除了在QML中指定属性外，还可以通过环境变量或配置文件指定它们。QML中指定的属性优先于所有其他方法。



#### Configuration File

| Variable     | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| `Theme`      | 指定默认的[材质主题](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-theme-attached-prop)。例如，该值可以是可用主题之一`"Dark"`。 |
| `Variant`    | 指定材料变体。Material Design有两种变体：为触摸设备设计的普通变体，以及用于桌面的密集变体。密集型变体使用较小的尺寸来控制及其字体。值可以是`"Normal"`或`"Dense"`。 |
| `Accent`     | 指定默认的[材质重音颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-accent-attached-prop)。该值可以是任何[颜色](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs)，但推荐使用的一个[预先定义的材料的颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)，例如`"Teal"`。 |
| `Primary`    | 指定默认的[材质主要颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-primary-attached-prop)。该值可以是任何[颜色](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs)，但推荐使用的一个[预先定义的材料的颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)，例如`"BlueGrey"`。 |
| `Foreground` | 指定默认的[材质前景色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-foreground-attached-prop)。例如，该值可以是任何[颜色](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs)，或者是[预定义的材料颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)之一`"Brown"`。 |
| `Background` | 指定默认的[材质背景颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-background-attached-prop)。例如，该值可以是任何[颜色](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs)，或者是[预定义的材料颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)之一`"Grey"`。 |

See [Qt Quick Controls 2 Configuration File](https://doc.qt.io/qt-5/qtquickcontrols2-configuration.html) for more details about the configuration file.





#### Environment Variables

| Variable                                | Description                                                  |
| --------------------------------------- | ------------------------------------------------------------ |
| `QT_QUICK_CONTROLS_MATERIAL_THEME`      | Specifies the default [Material theme](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-theme-attached-prop). The value can be one of the available themes, for example `"Dark"`. |
| `QT_QUICK_CONTROLS_MATERIAL_VARIANT`    | Specifies the Material variant. The Material Design has two variants: a normal variant designed for touch devices, and a dense variant for desktop. The dense variant uses smaller sizes for controls and their fonts.The value can be `"Normal"` or `"Dense"`. |
| `QT_QUICK_CONTROLS_MATERIAL_ACCENT`     | Specifies the default [Material accent color](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-accent-attached-prop). The value can be any [color](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs), but it is recommended to use one of the [pre-defined Material colors](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors), for example `"Teal"`. |
| `QT_QUICK_CONTROLS_MATERIAL_PRIMARY`    | Specifies the default [Material primary color](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-primary-attached-prop). The value can be any [color](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs), but it is recommended to use one of the [pre-defined Material colors](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors), for example `"BlueGrey"`. |
| `QT_QUICK_CONTROLS_MATERIAL_FOREGROUND` | Specifies the default [Material foreground color](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-foreground-attached-prop). The value can be any [color](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs), or one of the [pre-defined Material colors](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors), for example `"Brown"`. |
| `QT_QUICK_CONTROLS_MATERIAL_BACKGROUND` | Specifies the default [Material background color](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-background-attached-prop). The value can be any [color](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs), or one of the [pre-defined Material colors](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors), for example `"Grey"`. |

See [Supported Environment Variables in Qt Quick Controls 2](https://doc.qt.io/qt-5/qtquickcontrols2-environment.html) for the full list of supported environment variables.





### Dependency

必须单独导入材质样式才能访问特定于材质样式的属性。应该注意的是，无论对Material样式的引用如何，相同的应用程序代码都与任何其他样式一起运行。特定于材质的属性仅在使用“材质”样式运行应用程序时才有效。

如果在始终加载的QML文件中导入材质样式，则必须使用应用程序部署材质样式，以便能够运行应用程序，而不管应用程序运行的样式。通过使用[文件选择器](https://doc.qt.io/qt-5/qtquickcontrols2-fileselectors.html)，可以应用特定于样式的调整，而不会创建对样式的硬依赖性。





### Pre-defined Material Colors

Even though primary and accent can be any [color](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs), it is recommended to use one of the pre-defined colors that have been designed to work well with the rest of the Material style palette:

可用的预定义颜色：

![1555143915182](Material风格详情.assets/1555143915182.png)

When the dark theme is in use, different [shades](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-shades) of the pre-defined colors are used by default:

![1555143965459](Material风格详情.assets/1555143965459.png)



### Pre-defined Shades

There are several different [shades](https://material.google.com/style/color.html#color-color-palette) of each [pre-defined color](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors) that can be passed to the [Material.color()](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#material-color-attached-method) function:

| Constant             | Value |
| -------------------- | ----- |
| `Material.Shade50`   |       |
| `Material.Shade100`  |       |
| `Material.Shade200`  |       |
| `Material.Shade300`  |       |
| `Material.Shade400`  |       |
| `Material.Shade500`  |       |
| `Material.Shade600`  |       |
| `Material.Shade700`  |       |
| `Material.Shade800`  |       |
| `Material.Shade900`  |       |
| `Material.ShadeA100` |       |
| `Material.ShadeA200` |       |
| `Material.ShadeA400` |       |
| `Material.ShadeA700` |       |

**See also** [Default Style](https://doc.qt.io/qt-5/qtquickcontrols2-default.html), [Universal Style](https://doc.qt.io/qt-5/qtquickcontrols2-universal.html)





### Variants

Material样式还支持密集变体，其中按钮和委托等控件的高度较小，使用较小的字体大小。建议在桌面平台上使用密集版本，鼠标和键盘允许更精确和灵活的用户交互。

要使用密集变体，请将`QT_QUICK_CONTROLS_MATERIAL_VARIANT`环境变量设置为`Dense`，或`Variant=Dense`在[qtquickcontrols2.conf](https://doc.qt.io/qt-5/qtquickcontrols2-configuration.html)文件中指定。两种情况下的默认值都是`Normal`。

以下图像说明了使用普通和密集变体时某些控件之间的差异：

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![img](Material风格详情.assets/qtquickcontrols2-material-variant-normal.png) | ![img](Material风格详情.assets/qtquickcontrols2-material-variant-dense.png) |

请注意，上面显示的高度可能会因平台上的字体差异而有所不同。





## Attached Property Documentation



#### Material.accent : color

此附加属性保持主题的强调色。该物业可以附加到任何窗口或项目。该值将传播给子级。

默认值为`Material.Pink`。

在以下示例中，突出显示按钮的强调颜色更改为`Material.Orange`：

```
Button {
    text: qsTr("Button")
    highlighted: true
    Material.accent: Material.Orange
}
```

![img](Material风格详情.assets/qtquickcontrols2-material-accent-1555144257044.png)

**注意：**尽管强调可以是任何[颜色](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs)，但建议使用其中一种[预定义的材质颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)，这些[材料颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)设计为与材质样式调色板的其余部分配合使用。



#### Material.background : color

此附加属性保存主题的背景颜色。该物业可以附加到任何窗口或项目。该值将传播给子级。

默认值是主题特定的（浅色或深色）。

在以下示例中，按钮的背景颜色更改为`Material.Teal`：

```
Button {
    text: qsTr("Button")
    highlighted: true
    Material.background: Material.Teal
}
```

![img](Material风格详情.assets/qtquickcontrols2-material-background-1555144363262.png)



#### Material.elevation : int

此附加属性保持控件的高程。海拔越高，阴影越深。该属性可以附加到任何控件，但并非所有控件都可以显示高程。

默认值是特定于控件的。

在以下示例中，窗格的高程设置为`6`以实现[高架卡](https://material.google.com/components/cards.html)的外观：

```
Pane {
    width: 120
    height: 120

    Material.elevation: 6

    Label {
        text: qsTr("I'm a card!")
        anchors.centerIn: parent
    }
}
```

![img](Material风格详情.assets/qtquickcontrols2-material-elevation-1555144391155.png)



#### Material.foreground : color

此附加属性保存主题的前景色。该物业可以附加到任何窗口或项目。该值将传播给子级。

默认值是主题特定的（浅色或深色）。

在以下示例中，按钮的前景色设置为`Material.Pink`：

```
Button {
    text: qsTr("Button")
    Material.foreground: Material.Pink
}
```

![img](Material风格详情.assets/qtquickcontrols2-material-foreground-1555144423960.png)



#### Material.primary : color

此附加属性包含主题的主要颜色。该物业可以附加到任何窗口或项目。该值将传播给子级。

默认情况下，主要颜色用作[ToolBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-toolbar.html)的背景颜色。

默认值为`Material.Indigo`。

**注意：**尽管主要[颜色](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs)可以是任何[颜色](https://doc.qt.io/qt-5/qml-color.html#colorbasictypedocs)，但建议使用其中一种[预定义的材质颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)，这些[颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)设计为与材质样式调色板的其余部分配合使用。



#### Material.theme : enumeration

无论主题是浅色还是深色，这个附属属性都可以保存。该物业可以附加到任何窗口或项目。该值将传播给子级。

可用主题：

| Constant          | Description           |
| ----------------- | --------------------- |
| `Material.Light`  | Light theme (default) |
| `Material.Dark`   | Dark theme            |
| `Material.System` | System theme          |

设置主题以`System`根据系统主题颜色选择浅色或深色主题。但是，在读取theme属性的值时，值永远不会`System`是实际主题。

在以下示例中，窗格和按钮的主题都设置为`Material.Dark`：

```
Pane {
    Material.theme: Material.Dark

    Button {
        text: qsTr("Button")
    }
}
```

![img](Material风格详情.assets/qtquickcontrols2-material-theme-1555144483586.png)



## Attached Method Documentation

#### color color(enumeration *predefined*, enumeration *shade*)

此附加方法返回指定[预定义材质颜色](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-material-colors)的有效颜色值以及给定[阴影](https://doc.qt.io/qt-5/qtquickcontrols2-material.html#pre-defined-shades)。如果省略，则shade参数默认为`Material.Shade500`。

```
Rectangle {
    color: Material.color(Material.Red)
}
```