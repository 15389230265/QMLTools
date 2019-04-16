[TOC]



# Text QML Type





指定如何将格式化文本添加到场景。 [More...](https://doc.qt.io/qt-5/qml-qtquick-text.html#details)

| Import Statement: | import QtQuick 2.12                                  |
| ----------------- | ---------------------------------------------------- |
| Inherits:         | [Item](https://doc.qt.io/qt-5/qml-qtquick-item.html) |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qml-qtquick-text-members.html)
- [Obsolete members](https://doc.qt.io/qt-5/qml-qtquick-text-obsolete.html)





## Properties

- **advance** : size
- **baseUrl** : url
- **bottomPadding** : real
- **clip** : bool
- **color** : color
- **contentHeight** : real
- **contentWidth** : real
- **effectiveHorizontalAlignment** : enumeration
- **elide** : enumeration
- **font.bold** : bool
- **font.capitalization** : enumeration
- **font.family** : string
- **font.hintingPreference** : enumeration
- **font.italic** : bool
- **font.kerning** : bool
- **font.letterSpacing** : real
- **font.pixelSize** : int
- **font.pointSize** : real
- **font.preferShaping** : bool
- **font.strikeout** : bool
- **font.styleName** : string
- **font.underline** : bool
- **font.weight** : enumeration
- **font.wordSpacing** : real
- **fontInfo.bold** : bool
- **fontInfo.family** : string
- **fontInfo.italic** : bool
- **fontInfo.pixelSize** : string
- **fontInfo.pointSize** : real
- **fontInfo.styleName** : string
- **fontInfo.weight** : int
- **fontSizeMode** : enumeration
- **horizontalAlignment** : enumeration
- **hoveredLink** : string
- **leftPadding** : real
- **lineCount** : int
- **lineHeight** : real
- **lineHeightMode** : enumeration
- **linkColor** : color
- **maximumLineCount** : int
- **minimumPixelSize** : int
- **minimumPointSize** : int
- **padding** : real
- **renderType** : enumeration
- **rightPadding** : real
- **style** : enumeration
- **styleColor** : color
- **text** : string
- **textFormat** : enumeration
- **topPadding** : real
- **truncated** : bool
- **verticalAlignment** : enumeration
- **wrapMode** : enumeration





## Signals

- **lineLaidOut**(object *line*)
- **linkActivated**(string *link*)
- **linkHovered**(string *link*)





## Methods

- **forceLayout**()
- **linkAt**(real *x*, real *y*)





## Detailed Description

文本项可以显示纯文本和富文本。例如，具有特定字体和大小的红色文本可以像这样定义：

```c++
Text {
    text: "Hello World!"
    font.family: "Helvetica"
    font.pointSize: 24
    color: "red"
}
```

使用HTML样式标记定义富文本：

```c++
Text {
    text: "<b>Hello</b> <i>World!</i>"
}
```

![img](QML文本输入组件.assets/declarative-text.png)

如果未明确设置高度和宽度，Text将尝试确定需要多少空间并相应地进行设置。除非设置了[wrapMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#wrapMode-prop)，否则它总是更喜欢宽度到高度（所有文本都放在一行上）。

或者，[elide](https://doc.qt.io/qt-5/qml-qtquick-text.html#elide-prop)属性可用于将单行纯文本拟合到设定宽度。

请注意，[受支持的HTML子集](https://doc.qt.io/qt-5/richtext-html-subset.html)是有限的。此外，如果文本包含加载远程图像的HTML img标记，则会重新加载文本。

文本提供只读文本。有关可编辑的文本，请参阅[TextEdit](https://doc.qt.io/qt-5/qml-qtquick-textedit.html)。

**See also** [Fonts example](https://doc.qt.io/qt-5/qtquick-text-example.html#fonts).

## Property Documentation



#### advance : [size](https://doc.qt.io/qt-5/qml-size.html)

从文本项的第一个字符的基线原点到文本项中直接在此文本项之后发生的文本项中第一个字符的基线原点的距离（以像素为单位）。

请注意，如果文本从右向左流动，则前进可以为负。

该属性在Qt 5.10中引入。



#### baseUrl : [url](https://doc.qt.io/qt-5/qml-url.html)

此属性指定基本URL，用于解析文本中的相对URL。

网址被解析为与基本URL的目标位于同一目录中，这意味着将忽略最后一个“/”之后路径的任何部分。

| Base URL                                 | Relative URL       | Resolved URL                                  |
| ---------------------------------------- | ------------------ | --------------------------------------------- |
| http://qt-project.org/                   | images/logo.png    | http://qt-project.org/images/logo.png         |
| http://qt-project.org/index.html         | images/logo.png    | http://qt-project.org/images/logo.png         |
| http://qt-project.org/content            | images/logo.png    | http://qt-project.org/content/images/logo.png |
| http://qt-project.org/content/           | images/logo.png    | http://qt-project.org/content/images/logo.png |
| http://qt-project.org/content/index.html | images/logo.png    | http://qt-project.org/content/images/logo.png |
| http://qt-project.org/content/index.html | ../images/logo.png | http://qt-project.org/images/logo.png         |
| http://qt-project.org/content/index.html | /images/logo.png   | http://qt-project.org/images/logo.png         |

默认值是实例化Text项的QML文件的url。



#### bottomPadding : [real](https://doc.qt.io/qt-5/qml-real.html)

这些属性包含内容周围的填充。除了[contentWidth](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentWidth-prop)和[contentHeight](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentHeight-prop)之外，还保留此空间。

该属性在Qt 5.6中引入。



#### clip : [bool](https://doc.qt.io/qt-5/qml-bool.html)

此属性保存文本是否被剪裁。

请注意，如果文本不适合边界矩形，它将突然切断。

如果要在有限的空间中显示可能的长文本，可能需要使用`elide`。



#### color : [color](https://doc.qt.io/qt-5/qml-color.html)

文字颜色。

使用十六进制表示法定义的绿色文本示例：

```
Text {
    color: "#00FF00"
    text: "green text"
}
```

使用SVG颜色名称定义的钢蓝色文本示例：

```
Text {
    color: "steelblue"
    text: "blue text"
}
```



#### contentHeight : [real](https://doc.qt.io/qt-5/qml-real.html)

返回文本的高度，包括由于文本多于设置高度中的文本而覆盖的高度。



#### contentWidth : [real](https://doc.qt.io/qt-5/qml-real.html)

返回文本的宽度，包括宽度超过宽度，如果设置了WrapMode，则由于包装不足而覆盖宽度。



#### effectiveHorizontalAlignment : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置文本项宽度和高度内文本的水平和垂直对齐方式。默认情况下，文本垂直对齐顶部。水平对齐遵循文本的自然对齐，例如，从左到右读取的文本将与左对齐。

为有效值`horizontalAlignment`是`Text.AlignLeft`，`Text.AlignRight`，`Text.AlignHCenter`和`Text.AlignJustify`。为有效值`verticalAlignment`是`Text.AlignTop`，`Text.AlignBottom`和`Text.AlignVCenter`。

请注意，对于单行文本，文本的大小是文本的区域。在这种常见情况下，所有对齐都是等效的。如果您希望文本以其父对象为中心，则需要修改[Item :: anchors](https://doc.qt.io/qt-5/qml-qtquick-item.html#anchors-prop)，或将[horizontalAlignment](https://doc.qt.io/qt-5/qml-qtquick-text.html#horizontalAlignment-prop)设置为Text.AlignHCenter并将宽度绑定到父级的宽度。

使用附加属性[LayoutMirroring :: enabled](https://doc.qt.io/qt-5/qml-qtquick-layoutmirroring.html#enabled-prop)镜像应用程序布局时，文本的水平对齐也将被镜像。但是，该物业`horizontalAlignment`将保持不变。要查询Text的有效水平对齐方式，请使用只读属性`effectiveHorizontalAlignment`。



#### elide : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

将此属性设置为适合Text项宽度的文本的elide部分。如果已设置显式宽度，则文本将仅删除。

此属性不能与富文本一起使用。

Eliding can be:

- Text.ElideNone - the default
- Text.ElideLeft
- Text.ElideMiddle
- Text.ElideRight

如果此属性设置为Text.ElideRight，则可以将其与[包装文本](https://doc.qt.io/qt-5/qml-qtquick-text.html#wrapMode-prop)一起使用。只有在设置`maximumLineCount`或`height`已设置的情况下，文本才会被忽略。如果同时`maximumLineCount`和`height`设置，`maximumLineCount`将适用，除非线不容许高度适应。

如果文本是多长度字符串，而模式不是`Text.ElideNone`，则将使用第一个适合的字符串，否则将删除最后一个字符串。

多长度字符串从最长到最短排序，由Unicode“字符串终结符”字符分隔`U009C`（用QML用`"\u009C"`或写入`"\x9C"`）。



#### font.bold : [bool](https://doc.qt.io/qt-5/qml-bool.html)

设置字体粗细是否为粗体。



#### font.capitalization : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置文本的大小写。

- Font.MixedCase - 这是普通的文本呈现选项，不应用大写更改。
- Font.AllUppercase - 这会改变以全大写类型呈现的文本。
- Font.AllLowercase - 这会改变以全小写字体呈现的文本。
- Font.SmallCaps -  这会改变以小型大写字母呈现的文本。
- Font.Capitalize - 这会将要呈现的文本更改为每个单词的第一个字符作为大写字符。

```
Text { text: "Hello"; font.capitalization: Font.AllLowercase }
```



#### font.family : [string](https://doc.qt.io/qt-5/qml-string.html)

设置字体的系列名称。

字体类型不区分大小写，可以选择包括类型名称，例如“Helvetica [Cronyx]”。如果字体可以从多个字体库获得并且未指定字体库，则选择任意字体库。如果字体类型不可用，则将使用字体匹配算法设置字体。



#### font.hintingPreference : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置文本的首选提示。这是对底层文本呈现系统使用一定级别的提示的暗示，并且跨平台具有不同的支持。有关详细信息，请参阅[QFont :: HintingPreference](https://doc.qt.io/qt-5/qfont.html#HintingPreference-enum)文档中的表。

**注意：**此属性仅在与渲染类型Text.NativeRendering一起使用时才有效。

Font.PreferDefaultHinting - 使用目标平台的默认提示级别。

Font.PreferNoHinting - 如果可能，渲染文本而不暗示字形的轮廓。文本布局在印刷上将是准确的，使用与例如打印时相同的度量。

Font.PreferVerticalHinting - 如果可能，渲染文本时不进行水平提示，但将字形与垂直方向的像素网格对齐。在密度太低而无法准确呈现字形的显示器上，文本将显得更加清晰。但由于字形的水平度量未明确，因此文本的布局可以扩展到更高密度的设备（如打印机），而不会影响换行等细节。

Font.PreferFullHinting - 如果可能，使用水平和垂直方向的提示渲染文本。文本将被更改以优化目标设备的易读性，但由于度量将取决于文本的目标大小，因此字形，换行符和其他印刷细节的位置将无法缩放，这意味着文本布局可能看起来在具有不同像素密度的设备上不同。

```c++
Text { text: "Hello"; renderType: Text.NativeRendering; font.hintingPreference: Font.PreferVerticalHinting }
```

This property was introduced in Qt 5.8.



#### font.italic : [bool](https://doc.qt.io/qt-5/qml-bool.html)

设置字体是否具有斜体样式。



#### font.kerning : [bool](https://doc.qt.io/qt-5/qml-bool.html)

在对文本进行整形时启用或禁用字距调整OpenType功能。禁用此功能可能会在创建或更改文本时提高性能，但会牺牲一些美观功能。默认值是true。

```
Text { text: "OATS FLAVOUR WAY"; font.kerning: false }
```

This property was introduced in Qt 5.10.



#### font.letterSpacing : [real](https://doc.qt.io/qt-5/qml-real.html)

设置字体的字母间距。

字母间距会更改字体中各个字母之间的默认间距。正值会使字母间距增加相应的像素; 负值会减小间距。



#### font.pixelSize : [int](https://doc.qt.io/qt-5/qml-int.html)

以像素为单位设置字体大小。

使用此功能可使字体设备相关。用于`pointSize`以与设备无关的方式设置字体大小。



#### font.pointSize : [real](https://doc.qt.io/qt-5/qml-real.html)

以磅为单位设置字体大小。点大小必须大于零。



#### font.preferShaping : [bool](https://doc.qt.io/qt-5/qml-bool.html)

有时，字体会将复杂的规则应用于一组字符，以便正确显示它们。在一些书写系统中，例如Brahmic脚本，这是为了使文本清晰易读，但在例如拉丁文字中，它仅仅是一个美观的特征。将`preferShaping`属性设置为false将在不需要时禁用所有此类功能，这将在大多数情况下提高性能。

默认值是true。

```
Text { text: "Some text"; font.preferShaping: false }
```

This property was introduced in Qt 5.10.



#### font.strikeout : [bool](https://doc.qt.io/qt-5/qml-bool.html)

设置字体是否具有删除线样式。



#### font.styleName : [string](https://doc.qt.io/qt-5/qml-string.html)

设置字体的样式名称。

样式名称不区分大小写。如果设置，则字体将与样式名称匹配，而不是字体属性[font.weight](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.weight-prop)，[font.bold](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.bold-prop)和[font.italic](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.italic-prop)。

This property was introduced in Qt 5.6.



#### font.underline : [bool](https://doc.qt.io/qt-5/qml-bool.html)

设置文本是否加下划线。



#### font.weight : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置字体的粗细。

粗细可以是以下之一：

- Font.Thin
- Font.Light
- Font.ExtraLight
- Font.Normal - the default
- Font.Medium
- Font.DemiBold
- Font.Bold
- Font.ExtraBold
- Font.Black

```
Text { text: "Hello"; font.weight: Font.DemiBold }
```



#### font.wordSpacing : [real](https://doc.qt.io/qt-5/qml-real.html)

设置字体的字间距。

字间距更改单个单词之间的默认间距。正值使字间隔增加相应的像素量，而负值相应地减小字间间隔。



#### fontInfo.bold : [bool](https://doc.qt.io/qt-5/qml-bool.html)

已为当前字体和[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)解析的字体信息的粗体状态。如果已解析字体的粗细为粗体或更高，则为true。

This property was introduced in Qt 5.9.



#### fontInfo.family : [string](https://doc.qt.io/qt-5/qml-string.html)

已为当前字体和[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)解析的字体的系列名称。

This property was introduced in Qt 5.9.



#### fontInfo.italic : [bool](https://doc.qt.io/qt-5/qml-bool.html)

已为当前字体和[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)解析的字体信息的斜体状态。

This property was introduced in Qt 5.9.



#### fontInfo.pixelSize : [string](https://doc.qt.io/qt-5/qml-string.html)

已为当前字体和[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)解析的字体信息的像素大小。

This property was introduced in Qt 5.9.



#### fontInfo.pointSize : [real](https://doc.qt.io/qt-5/qml-real.html)

已为当前字体和[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)解析的字体信息的[pointSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)。

This property was introduced in Qt 5.9.



#### fontInfo.styleName : [string](https://doc.qt.io/qt-5/qml-string.html)

已为当前字体和[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)解析的字体信息的样式名称。

This property was introduced in Qt 5.9.



#### fontInfo.weight : [int](https://doc.qt.io/qt-5/qml-int.html)

已为当前字体和[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)解析的字体信息的粗细。

This property was introduced in Qt 5.9.



#### fontSizeMode : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

此属性指定如何确定显示文本的字体大小。可能的值是：

- Text.FixedSize（默认） - 使用[font.pixelSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.pixelSize-prop)或[font.pointSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.pointSize-prop)指定的大小。
- Text.HorizontalFit - 使用最大尺寸，最大尺寸，适合项目宽度而不包装。
- Text.VerticalFit - 使用最大尺寸，指定适合项目高度的指定尺寸。
- Text.Fit - 使用最大尺寸，最大尺寸，适合项目的宽度和高度。

拟合的文本的字体大小具有由指定的约束的最小[minimumPointSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#minimumPointSize-prop)或[minimumPixelSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#minimumPixelSize-prop)属性和最大结合通过任一指定的[font.pointSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.pointSize-prop)或[font.pixelSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.pixelSize-prop)性质。

```
Text { text: "Hello"; fontSizeMode: Text.Fit; minimumPixelSize: 10; font.pixelSize: 72 }
```

如果文本不适合具有最小字体大小的项目边界，则将根据[elide](https://doc.qt.io/qt-5/qml-qtquick-text.html#elide-prop)属性省略文本。



#### horizontalAlignment : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置文本项宽度和高度内文本的水平和垂直对齐方式。默认情况下，文本垂直对齐顶部。水平对齐遵循文本的自然对齐，例如，从左到右读取的文本将与左对齐。

为有效值`horizontalAlignment`是`Text.AlignLeft`，`Text.AlignRight`，`Text.AlignHCenter`和`Text.AlignJustify`。为有效值`verticalAlignment`是`Text.AlignTop`，`Text.AlignBottom`和`Text.AlignVCenter`。

请注意，对于单行文本，文本的大小是文本的区域。在这种常见情况下，所有对齐都是等效的。如果您希望文本以其父对象为中心，则需要修改[Item :: anchors](https://doc.qt.io/qt-5/qml-qtquick-item.html#anchors-prop)，或将horizontalAlignment设置为Text.AlignHCenter并将宽度绑定到父级的宽度。

使用附加属性[LayoutMirroring :: enabled](https://doc.qt.io/qt-5/qml-qtquick-layoutmirroring.html#enabled-prop)镜像应用程序布局时，文本的水平对齐也将被镜像。但是，该物业`horizontalAlignment`将保持不变。要查询Text的有效水平对齐方式，请使用只读属性`effectiveHorizontalAlignment`。



#### hoveredLink : [string](https://doc.qt.io/qt-5/qml-string.html)

当用户悬停文本中嵌入的链接时，此属性包含链接字符串。链接必须是富文本格式或HTML格式，*hoveredLink*字符串提供对特定链接的访问。

This property was introduced in Qt 5.2.

**See also** [linkHovered](https://doc.qt.io/qt-5/qml-qtquick-text.html#linkHovered-signal) and [linkAt()](https://doc.qt.io/qt-5/qml-qtquick-text.html#linkAt-method).



#### leftPadding : [real](https://doc.qt.io/qt-5/qml-real.html)

这些属性包含内容周围的填充。除了[contentWidth](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentWidth-prop)和[contentHeight](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentHeight-prop)之外，还保留此空间。

This property was introduced in Qt 5.6.



#### lineCount : [int](https://doc.qt.io/qt-5/qml-int.html)

返回文本项中可见的行数。

富文本不支持此属性。

**See also** [maximumLineCount](https://doc.qt.io/qt-5/qml-qtquick-text.html#maximumLineCount-prop).



#### lineHeight : [real](https://doc.qt.io/qt-5/qml-real.html)

设置文本的行高。该值可以是像素或乘数，具体取决于[lineHeightMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#lineHeightMode-prop)。

默认值是乘数1.0。线高必须是正值。



#### lineHeightMode : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

此属性确定如何指定行高。可能的值是：

- Text.ProportionalHeight（默认值） - 设置与线条成比例的间距（作为乘数）。例如，设置为2表示双倍间距。
- Text.FixedHeight - 将行高设置为固定行高（以像素为单位）。



#### linkColor : [color](https://doc.qt.io/qt-5/qml-color.html)

文本中链接的颜色。

此属性适用于StyledText [textFormat](https://doc.qt.io/qt-5/qml-qtquick-text.html#textFormat-prop)，但不适用于RichText。可以通过在文本中包含CSS样式标记来指定RichText中的链接颜色。



#### maximumLineCount : [int](https://doc.qt.io/qt-5/qml-int.html)

设置此属性以限制文本项将显示的行数。如果将elide设置为Text.ElideRight，则将正确地省略该文本。默认情况下，这是最大可能整数的值。

富文本不支持此属性。

**See also** [lineCount](https://doc.qt.io/qt-5/qml-qtquick-text.html#lineCount-prop) and [elide](https://doc.qt.io/qt-5/qml-qtquick-text.html#elide-prop).



#### minimumPixelSize : [int](https://doc.qt.io/qt-5/qml-int.html)

此属性指定由[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)属性缩放的文本的最小字体像素大小。

如果[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)是Text.FixedSize或[font.pixelSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.pixelSize-prop)是-1，则忽略此属性。



#### minimumPointSize : [int](https://doc.qt.io/qt-5/qml-int.html)

该属性指定最小字体点[大小](https://doc.qt.io/qt-5/qml-size.html)通过缩放文本[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)财产。

如果[fontSizeMode](https://doc.qt.io/qt-5/qml-qtquick-text.html#fontSizeMode-prop)是Text.FixedSize或[font.pointSize](https://doc.qt.io/qt-5/qml-qtquick-text.html#font.pointSize-prop)是-1，则忽略此属性。



#### padding : [real](https://doc.qt.io/qt-5/qml-real.html)

这些属性包含内容周围的填充。除了[contentWidth](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentWidth-prop)和[contentHeight](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentHeight-prop)之外，还保留此空间。

This property was introduced in Qt 5.6.



#### renderType : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

覆盖此组件的默认呈现类型。

支持的渲染类型是：

- Text.QtRendering
- Text.NativeRendering

如果您希望文本在目标平台上看起来是原生的，并且不需要高级功能（如文本转换），请选择Text.NativeRendering。将这些特征与NativeRendering渲染类型结合使用将会带来较差且有时像素化的结果。

默认呈现类型由[QQuickWindow :: textRenderType()](https://doc.qt.io/qt-5/qquickwindow.html#textRenderType)确定。



#### rightPadding : [real](https://doc.qt.io/qt-5/qml-real.html)

这些属性包含内容周围的填充。除了[contentWidth](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentWidth-prop)和[contentHeight](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentHeight-prop)之外，还保留此空间。

This property was introduced in Qt 5.6.



#### style : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置其他文本样式。

支持的文本样式是：

- Text.Normal - the default
- Text.Outline
- Text.Raised
- Text.Sunken

```c++
Row {
    Text { font.pointSize: 24; text: "Normal" }
    Text { font.pointSize: 24; text: "Raised"; style: Text.Raised; styleColor: "#AAAAAA" }
    Text { font.pointSize: 24; text: "Outline";style: Text.Outline; styleColor: "red" }
    Text { font.pointSize: 24; text: "Sunken"; style: Text.Sunken; styleColor: "#AAAAAA" }
}
```

![img](QML文本输入组件.assets/declarative-textstyle.png)



#### styleColor : [color](https://doc.qt.io/qt-5/qml-color.html)

定义文本样式使用的辅助颜色。

`styleColor`用作轮廓文本的轮廓颜色，用作凸起或凹陷文本的阴影颜色。如果没有设置样式，则根本不使用它。

```
Text { font.pointSize: 18; text: "hello"; style: Text.Raised; styleColor: "gray" }
```

**See also** [style](https://doc.qt.io/qt-5/qml-qtquick-text.html#style-prop).



#### text : [string](https://doc.qt.io/qt-5/qml-string.html)

要显示的文字。Text支持纯文本字符串和富文本字符串。

该项目将尝试自动确定是否应将文本视为样式文本。使用[Qt :: mightBeRichText()](https://doc.qt.io/qt-5/qt-sub-qtgui.html#mightBeRichText)进行此确定。



#### textFormat : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

应显示text属性的方式。

支持的文本格式为：

- Text.AutoText（默认）
- Text.PlainText
- Text.StyledText
- Text.RichText

如果文本格式是`Text.AutoText`文本项将自动确定是否应将文本视为样式文本。使用[Qt :: mightBeRichText()](https://doc.qt.io/qt-5/qt-sub-qtgui.html#mightBeRichText)进行此确定，该[Qt :: mightBeRichText()](https://doc.qt.io/qt-5/qt-sub-qtgui.html#mightBeRichText)使用快速且因此简单的启发式算法。它主要检查在第一个换行符之前是否有看起来像标签的东西。虽然结果可能对于普通情况是正确的，但是不能保证。

Text.StyledText是一种优化格式，支持HTML 3.2风格的一些基本文本样式标记：

```html
<b></b> - bold
<del></del> - strike out (removed content)
<s></s> - strike out (no longer accurate or no longer relevant content)
<strong></strong> - bold
<i></i> - italic
<br> - new line
<p> - paragraph
<u> - underlined text
<font color="color_name" size="1-7"></font>
<h1> to <h6> - headers
<a href=""> - anchor
<img src="" align="top,middle,bottom" width="" height=""> - inline images
<ol type="">, <ul type=""> and <li> - ordered and unordered lists
<pre></pre> - preformatted
&gt; &lt; &amp;
```

`Text.StyledText` 解析器是严格的，要求标记正确嵌套。

```
Column {
    Text {
        font.pointSize: 24
        text: "<b>Hello</b> <i>World!</i>"
    }
    Text {
        font.pointSize: 24
        textFormat: Text.RichText
        text: "<b>Hello</b> <i>World!</i>"
    }
    Text {
        font.pointSize: 24
        textFormat: Text.PlainText
        text: "<b>Hello</b> <i>World!</i>"
    }
}
```

![img](QML文本输入组件.assets/declarative-textformat-1554968414936.png)

Text.RichText支持更大的HTML 4子集，如[支持的HTML子集](https://doc.qt.io/qt-5/richtext-html-subset.html)页面所述。您应该更喜欢使用Text.PlainText或Text.StyledText，因为它们可以提供更好的性能。



#### topPadding : [real](https://doc.qt.io/qt-5/qml-real.html)

这些属性包含内容周围的填充。除了[contentWidth](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentWidth-prop)和[contentHeight](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentHeight-prop)之外，还保留此空间。

该属性在Qt 5.6中引入。



#### truncated : [bool](https://doc.qt.io/qt-5/qml-bool.html)

如果由于[maximumLineCount](https://doc.qt.io/qt-5/qml-qtquick-text.html#maximumLineCount-prop)或[elide](https://doc.qt.io/qt-5/qml-qtquick-text.html#elide-prop)而截断文本，则返回true 。

富文本不支持此属性。

**See also** [maximumLineCount](https://doc.qt.io/qt-5/qml-qtquick-text.html#maximumLineCount-prop) and [elide](https://doc.qt.io/qt-5/qml-qtquick-text.html#elide-prop).



#### verticalAlignment : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置文本项宽度和高度内文本的水平和垂直对齐方式。默认情况下，文本垂直对齐顶部。水平对齐遵循文本的自然对齐，例如，从左到右读取的文本将与左对齐。

为有效值`horizontalAlignment`是`Text.AlignLeft`，`Text.AlignRight`，`Text.AlignHCenter`和`Text.AlignJustify`。为有效值`verticalAlignment`是`Text.AlignTop`，`Text.AlignBottom`和`Text.AlignVCenter`。

请注意，对于单行文本，文本的大小是文本的区域。在这种常见情况下，所有对齐都是等效的。如果您希望文本以其父对象为中心，则需要修改[Item :: anchors](https://doc.qt.io/qt-5/qml-qtquick-item.html#anchors-prop)，或将[horizontalAlignment](https://doc.qt.io/qt-5/qml-qtquick-text.html#horizontalAlignment-prop)设置为Text.AlignHCenter并将宽度绑定到父级的宽度。

使用附加属性[LayoutMirroring :: enabled](https://doc.qt.io/qt-5/qml-qtquick-layoutmirroring.html#enabled-prop)镜像应用程序布局时，文本的水平对齐也将被镜像。但是，该物业`horizontalAlignment`将保持不变。要查询Text的有效水平对齐方式，请使用只读属性`effectiveHorizontalAlignment`。



#### wrapMode : [enumeration](https://doc.qt.io/qt-5/qml-enumeration.html)

设置此属性以将文本换行到Text项的宽度。如果已设置显式宽度，则文本将仅换行。wrapMode可以是以下之一：

- Text.NoWrap（默认） - 不执行包装。如果文本包含的换行符不足，则[contentWidth](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentWidth-prop)将超过设置的宽度。
- Text.WordWrap - 包装仅在字边界上完成。如果单词太长，[contentWidth](https://doc.qt.io/qt-5/qml-qtquick-text.html#contentWidth-prop)将超过设置的宽度。
- Text.WrapAnywhere - 包装在一行的任何一点完成，即使它出现在一个单词的中间。
- Text.Wrap - 如果可能，包装发生在单词边界; 否则它会发生在线上的适当位置，即使在一个单词的中间。





## Signal Documentation



#### lineLaidOut(object *line*)

为布局过程中布置的每行文本发出此信号。指定的*线*对象提供有关当前布局的线的更多详细信息。

这样就有机会在布线时定位和调整线条的大小。例如，它可用于创建列或在对象周围布置文本。

指定的*行*对象的属性是：

- number (read-only)
- x
- y
- width
- height

例如，这会将文本项的前5行向右移动100个像素：

```
onLineLaidOut: {
    if (line.number < 5) {
        line.x = line.x + 100
        line.width = line.width - 100
    }
}
```

相应的处理程序是`onLineLaidOut`。



#### linkActivated([string](https://doc.qt.io/qt-5/qml-string.html) *link*)

当用户单击文本中嵌入的链接时，将发出此信号。链接必须是富文本或HTML格式，并且*链接*字符串提供对特定链接的访问。

```
    Text {
            textFormat: Text.RichText
            text: "See the <a href=\"http://qt-project.org\">Qt Project website</a>."
            onLinkActivated: console.log(link + " link activated")
    }
```

示例代码将显示文本“查看[Qt项目网站”](http://qt-project.org/)。

单击突出显示的链接将输出`http://qt-project.org link activated`到控制台。

相应的处理程序是`onLinkActivated`。



#### linkHovered([string](https://doc.qt.io/qt-5/qml-string.html) *link*)

当用户悬停文本中嵌入的链接时，将发出此信号。链接必须是富文本或HTML格式，并且*链接*字符串提供对特定链接的访问。

相应的处理程序是`onLinkHovered`。

This signal was introduced in Qt 5.2.

**See also** [hoveredLink](https://doc.qt.io/qt-5/qml-qtquick-text.html#hoveredLink-prop) and [linkAt()](https://doc.qt.io/qt-5/qml-qtquick-text.html#linkAt-method).





## Method Documentation



#### forceLayout()

触发显示文本的重新布局。

This method was introduced in Qt 5.9.



#### linkAt([real](https://doc.qt.io/qt-5/qml-real.html) *x*, [real](https://doc.qt.io/qt-5/qml-real.html) *y*)

返回内容坐标中*x*，*y*点的链接字符串，如果该点不存在链接，则返回空字符串。

This method was introduced in Qt 5.3.

**See also** [hoveredLink](https://doc.qt.io/qt-5/qml-qtquick-text.html#hoveredLink-prop).