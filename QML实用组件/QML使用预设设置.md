[TOC]



# Settings QML Type



提供与平台无关的持久应用程序设置 [More...](https://doc.qt.io/qt-5/qml-qt-labs-settings-settings.html#details)

|                   |                             |
| ----------------- | --------------------------- |
| Import Statement: | import Qt.labs.settings 1.0 |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qml-qt-labs-settings-settings-members.html)



## Properties

- **category** : string
- **fileName** : string



## Methods

- **setValue**(string *key*, var *value*)
- var **value**(string *key*, var *defaultValue*)



## Detailed Description

“设置”类型提供与平台无关的持久应用程序设置

**注意：**通过导入**Qt.labs.settings**模块可以使用此类型。*Qt.labs模块中的类型不保证在将来的版本中保持兼容。*

用户通常希望应用程序在会话中记住其设置（窗口大小和位置，选项等）。通过“设置”类型，您可以轻松地保存和恢复此类应用程序设置。

通过在Settings元素中声明属性来指定单个设置值。支持所有[基本类型](https://doc.qt.io/qt-5/qtqml-typesystem-basictypes.html)属性。建议的方法是使用属性别名，以便双向获取自动属性更新。以下示例显示如何使用“设置”存储和还原窗口的几何图形。

```c++
import QtQuick.Window 2.1
import Qt.labs.settings 1.0

Window {
    id: window

    width: 800
    height: 600

    Settings {
        property alias x: window.x
        property alias y: window.y
        property alias width: window.width
        property alias height: window.height
    }
}
```

首次应用程序启动时，窗口的默认尺寸指定为800x600。请注意，没有指定默认位置 - 我们让窗口管理器处理它。稍后当窗口几何变化时，新值将自动存储到持久设置中。第二个应用程序运行将从持久设置中获取初始值，使窗口返回到先前的位置和大小。

通过使用属性别名实现的完全声明性语法，只要别名属性的值发生更改，就会以存储持久性设置为代价。可以使用正常属性来获得对存储持久性设置的更细粒度的控制。以下示例说明如何保存组件销毁的设置。

```
import QtQuick 2.1
import Qt.labs.settings 1.0

Item {
    id: page

    state: settings.state

    states: [
        State {
            name: "active"
            // ...
        },
        State {
            name: "inactive"
            // ...
        }
    ]

    Settings {
        id: settings
        property string state: "active"
    }

    Component.onDestruction: {
        settings.state = page.state
    }
}
```

请注意现在如何在持久设置属性中指定默认值，并将实际属性绑定到该设置，以便从持久设置中获取初始值。





## Application Identifiers

通过提供应用程序[名称](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)，[组织](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)和[域](https://doc.qt.io/qt-5/qcoreapplication.html#organizationDomain-prop)或通过指定[fileName](https://doc.qt.io/qt-5/qml-qt-labs-settings-settings.html#fileName-prop)来标识特定于应用程序的设置。

```c++
#include <QGuiApplication>
#include <QQmlApplicationEngine>

int main(int argc, char *argv[])
{
    QGuiApplication app(argc, argv);
    app.setOrganizationName("Some Company");
    app.setOrganizationDomain("somecompany.com");
    app.setApplicationName("Amazing Application");

    QQmlApplicationEngine engine("main.qml");
    return app.exec();
}
```

这些通常在C ++的开头指定`main()`，但也可以通过以下属性在QML中控制：

- [Qt.application.name](https://doc.qt.io/qt-5/qml-qtqml-qt.html#application-prop),
- [Qt.application.organization](https://doc.qt.io/qt-5/qml-qtqml-qt.html#application-prop) 
- [Qt.application.domain](https://doc.qt.io/qt-5/qml-qtqml-qt.html#application-prop).





## Categories

通过[类别](https://doc.qt.io/qt-5/qml-qt-labs-settings-settings.html#category-prop)属性指定类别名称，可以将应用程序设置划分为逻辑类别。使用逻辑类别不仅可以提供更清晰的设置结构，还可以防止设置键之间可能发生的冲突。

如果需要多个类别，请使用多个“设置”对象，每个对象都有自己的类别：

```
Item {
    id: panel

    visible: true

    Settings {
        category: "OutputPanel"
        property alias visible: panel.visible
        // ...
    }

    Settings {
        category: "General"
        property alias fontSize: fontSizeSpinBox.value
        // ...
    }
}
```

不是确保应用程序中的所有设置都具有唯一名称，而是可以将设置划分为唯一类别，然后可以包含使用其他类别中使用的相同名称的设置 - 没有冲突。



## Notes

当前的实现基于[QSettings](https://doc.qt.io/qt-5/qsettings.html)。这带来了某些限制，例如缺少更改通知。使用一个“设置”实例写入设置值不会更新另一个“设置”实例中的值，即使它们指的是同一类别中的相同设置。

信息存储在Windows上的系统注册表中，以及[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)上的XML首选项文件中。在其他Unix系统上，如果没有标准，则使用INI文本文件。有关更多详细信息，请参阅[QSettings](https://doc.qt.io/qt-5/qsettings.html)文档。

**另请参见**[QSettings](https://doc.qt.io/qt-5/qsettings.html)。



## Property Documentation



#### category : [string](https://doc.qt.io/qt-5/qml-string.html)

此属性包含设置类别的名称。

类别可用于将相关设置组合在一起。



#### fileName : [string](https://doc.qt.io/qt-5/qml-string.html)

此属性保存设置文件的路径。如果该文件尚不存在，则会创建该文件。

该属性在Qt 5.12中引入。

**See also** [QSettings::fileName](https://doc.qt.io/qt-5/qsettings.html#fileName) and [QSettings::IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum).



## Method Documentation



#### setValue([string](https://doc.qt.io/qt-5/qml-string.html) *key*, [var](https://doc.qt.io/qt-5/qml-var.html) *value*)

将setting键的值设置为value。如果密钥已存在，则覆盖先前的值。

该方法在Qt 5.12中引入。

**See also** [value()](https://doc.qt.io/qt-5/qml-qt-labs-settings-settings.html#value-method) and [QSettings::setValue](https://doc.qt.io/qt-5/qsettings.html#setValue).



#### [var](https://doc.qt.io/qt-5/qml-var.html) value([string](https://doc.qt.io/qt-5/qml-string.html) *key*, [var](https://doc.qt.io/qt-5/qml-var.html) *defaultValue*)

返回设置*键*的值。如果该设置不存在，则返回*defaultValue*。

该方法在Qt 5.12中引入。

**See also** [setValue()](https://doc.qt.io/qt-5/qml-qt-labs-settings-settings.html#setValue-method) and [QSettings::value](https://doc.qt.io/qt-5/qsettings.html#value).





# QSettings Class

该[QSettings](https://doc.qt.io/qt-5/qsettings.html)类提供持久的平台无关的应用程序设置。 [More...](https://doc.qt.io/qt-5/qsettings.html#details)

|           |                                                |
| --------- | ---------------------------------------------- |
| Header:   | #include < QSettings >                         |
| qmake:    | QT += core                                     |
| Inherits: | [QObject](https://doc.qt.io/qt-5/qobject.html) |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qsettings-members.html)
- [Obsolete members](https://doc.qt.io/qt-5/qsettings-obsolete.html)

**Note:** All functions in this class are [reentrant](https://doc.qt.io/qt-5/threads-reentrancy.html).

**Note:** These functions are also [thread-safe](https://doc.qt.io/qt-5/threads-reentrancy.html):

- [registerFormat](https://doc.qt.io/qt-5/qsettings.html#registerFormat)(const QString &extension, QSettings::ReadFunc readFunc, QSettings::WriteFunc writeFunc, Qt::CaseSensitivity caseSensitivity)





## Public Types

| enum    | **Format** { NativeFormat, Registry32Format, Registry64Format, IniFormat, InvalidFormat } |
| ------- | ------------------------------------------------------------ |
| typedef | **ReadFunc**                                                 |
| enum    | **Scope** { UserScope, SystemScope }                         |
| typedef | **SettingsMap**                                              |
| enum    | **Status** { NoError, AccessError, FormatError }             |
| typedef | **WriteFunc**                                                |





## Public Functions

|                   | **QSettings**(const QString &*organization*, const QString &*application* = QString(), QObject **parent* = nullptr) |
| ----------------- | ------------------------------------------------------------ |
|                   | **QSettings**(QSettings::Scope *scope*, const QString &*organization*, const QString &*application* = QString(), QObject **parent* = nullptr) |
|                   | **QSettings**(QSettings::Format *format*, QSettings::Scope *scope*, const QString &*organization*, const QString &*application* = QString(), QObject **parent* = nullptr) |
|                   | **QSettings**(const QString &*fileName*, QSettings::Format *format*, QObject **parent* = nullptr) |
|                   | **QSettings**(QObject **parent* = nullptr)                   |
| virtual           | **~QSettings**()                                             |
| QStringList       | **allKeys**() const                                          |
| QString           | **applicationName**() const                                  |
| void              | **beginGroup**(const QString &*prefix*)                      |
| int               | **beginReadArray**(const QString &*prefix*)                  |
| void              | **beginWriteArray**(const QString &*prefix*, int *size* = -1) |
| QStringList       | **childGroups**() const                                      |
| QStringList       | **childKeys**() const                                        |
| void              | **clear**()                                                  |
| bool              | **contains**(const QString &*key*) const                     |
| void              | **endArray**()                                               |
| void              | **endGroup**()                                               |
| bool              | **fallbacksEnabled**() const                                 |
| QString           | **fileName**() const                                         |
| QSettings::Format | **format**() const                                           |
| QString           | **group**() const                                            |
| QTextCodec *      | **iniCodec**() const                                         |
| bool              | **isAtomicSyncRequired**() const                             |
| bool              | **isWritable**() const                                       |
| QString           | **organizationName**() const                                 |
| void              | **remove**(const QString &*key*)                             |
| QSettings::Scope  | **scope**() const                                            |
| void              | **setArrayIndex**(int *i*)                                   |
| void              | **setAtomicSyncRequired**(bool *enable*)                     |
| void              | **setFallbacksEnabled**(bool *b*)                            |
| void              | **setIniCodec**(QTextCodec **codec*)                         |
| void              | **setIniCodec**(const char **codecName*)                     |
| void              | **setValue**(const QString &*key*, const QVariant &*value*)  |
| QSettings::Status | **status**() const                                           |
| void              | **sync**()                                                   |
| QVariant          | **value**(const QString &*key*, const QVariant &*defaultValue* = QVariant()) const |

- 32 public functions inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#public-functions)





## Static Public Members

| QSettings::Format | **defaultFormat**()                                          |
| ----------------- | ------------------------------------------------------------ |
| QSettings::Format | **registerFormat**(const QString &*extension*, QSettings::ReadFunc *readFunc*, QSettings::WriteFunc *writeFunc*, Qt::CaseSensitivity *caseSensitivity* = Qt::CaseSensitive) |
| void              | **setDefaultFormat**(QSettings::Format *format*)             |
| void              | **setPath**(QSettings::Format *format*, QSettings::Scope *scope*, const QString &*path*) |
| const QMetaObject | **staticMetaObject**                                         |

- 10 static public members inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#static-public-members)





## Reimplemented Protected Functions

| virtual bool | **event**(QEvent **event*) override |
| ------------ | ----------------------------------- |
|              |                                     |

- 9 protected functions inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#protected-functions)

### Additional Inherited Members

- 1 property inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#properties)
- 1 public slot inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#public-slots)
- 2 signals inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#signals)
- 9 protected functions inherited from [QObject](https://doc.qt.io/qt-5/qobject.html#protected-functions)





## Detailed Description

该[QSettings](https://doc.qt.io/qt-5/qsettings.html)类提供持久的平台无关的应用程序设置。

用户通常希望应用程序在会话中记住其设置（窗口大小和位置，选项等）。此信息通常存储在Windows上的系统注册表中，以及[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS 上的属性列表文件中。在Unix系统上，在没有标准的情况下，许多应用程序（包括KDE应用程序）使用INI文本文件。

[QSettings](https://doc.qt.io/qt-5/qsettings.html)是围绕这些技术的抽象，使您能够以可移植的方式保存和恢复应用程序设置。它还支持[自定义存储格式](https://doc.qt.io/qt-5/qsettings.html#registerFormat)。

[QSettings](https://doc.qt.io/qt-5/qsettings.html)的API基于[QVariant](https://doc.qt.io/qt-5/qvariant.html)，允许您以最少的工作量保存大多数基于值的类型，例如[QString](https://doc.qt.io/qt-5/qstring.html)，[QRect](https://doc.qt.io/qt-5/qrect.html)和[QImage](https://doc.qt.io/qt-5/qimage.html)。

如果您只需要一个非持久性的基于内存的结构，请考虑使用[QMap](https://doc.qt.io/qt-5/qmap.html) < [QString](https://doc.qt.io/qt-5/qstring.html)，[QVariant](https://doc.qt.io/qt-5/qvariant.html) >。



### Basic Usage

创建[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象时，必须传递公司或组织的名称以及应用程序的名称。例如，如果您的产品名为Star Runner，而您的公司名为MySoft，则您将构建[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象，如下所示：

```c++
    QSettings settings("MySoft", "Star Runner");
```

[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象可以在堆栈上或堆上创建（即使用`new`）。构造和销毁[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象非常快。

如果在应用程序的许多位置使用[QSettings](https://doc.qt.io/qt-5/qsettings.html)，则可能需要使用[QCoreApplication :: setOrganizationName](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)()和[QCoreApplication :: setApplicationName](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)()指定组织名称和应用程序名称，然后使用默认的[QSettings](https://doc.qt.io/qt-5/qsettings.html)构造函数：

```c++
    QCoreApplication::setOrganizationName("MySoft");
    QCoreApplication::setOrganizationDomain("mysoft.com");
    QCoreApplication::setApplicationName("Star Runner");
    ...
    QSettings settings;
```

（这里，我们还指定了组织的Internet域。当设置Internet域时，它用于[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS而不是组织名称，因为[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS应用程序通常使用Internet域来标识自己。如果没有设置域，伪域名是从组织名称派生的。有关详细信息，请参阅下面的[平台特定说明](https://doc.qt.io/qt-5/qsettings.html#platform-specific-notes)。）

[QSettings](https://doc.qt.io/qt-5/qsettings.html)存储设置。每个设置都包含一个指定设置名称（*密钥*）的[QString](https://doc.qt.io/qt-5/qstring.html)和一个存储与密钥关联的数据的[QVariant](https://doc.qt.io/qt-5/qvariant.html)。要编写设置，请使用[setValue](https://doc.qt.io/qt-5/qsettings.html#setValue)()。例如：

```c++
    settings.setValue("editor/wrapMargin", 68);
```

如果已存在具有相同键的设置，则新值将覆盖现有值。为了提高效率，可能无法立即将更改保存到永久存储中。（您始终可以调用[sync](https://doc.qt.io/qt-5/qsettings.html#sync)()来提交更改。）

您可以使用[value](https://doc.qt.io/qt-5/qsettings.html#value)()获取设置的值：

```c++
    int margin = settings.value("editor/wrapMargin").toInt();
```

如果没有具有指定名称的设置，[QSettings](https://doc.qt.io/qt-5/qsettings.html)将返回空[QVariant](https://doc.qt.io/qt-5/qvariant.html)（可以转换为整数0）。您可以通过将第二个参数传递给[value](https://doc.qt.io/qt-5/qsettings.html#value)()来指定另一个默认值：

```c++
    int margin = settings.value("editor/wrapMargin", 80).toInt();
```

要测试给定密钥是否存在，请调用[contains](https://doc.qt.io/qt-5/qsettings.html#contains)()。要删除与键关联的设置，请调用[remove](https://doc.qt.io/qt-5/qsettings.html#remove)()。要获取所有键的列表，请调用[allKeys](https://doc.qt.io/qt-5/qsettings.html#allKeys)()。要删除所有键，请调用[clear](https://doc.qt.io/qt-5/qsettings.html#clear)()。



### QVariant and GUI Types

因为[的QVariant](https://doc.qt.io/qt-5/qvariant.html)是Qt核心模块的一部分，所以不能将数据类型，例如提供转换功能[的QColor](https://doc.qt.io/qt-5/qcolor.html)，[QImage的](https://doc.qt.io/qt-5/qimage.html)和[的QPixmap](https://doc.qt.io/qt-5/qpixmap.html)，这是Qt的GUI的一部分。换句话说，不存在`toColor()`，`toImage()`或`toPixmap()`在功能[的QVariant](https://doc.qt.io/qt-5/qvariant.html)。

相反，您可以使用[QVariant :: value](https://doc.qt.io/qt-5/qvariant.html#value)（）或qVariantValue（）模板函数。例如：

```c++
QSettings settings("MySoft", "Star Runner");
QColor color = settings.value("DataPump/bgcolor").value<QColor>();
```

逆变换（例如，从[而QColor](https://doc.qt.io/qt-5/qcolor.html)到[的QVariant](https://doc.qt.io/qt-5/qvariant.html)）是自动的用于通过支持的所有数据类型[的QVariant](https://doc.qt.io/qt-5/qvariant.html)，包括GUI相关类型：

```c++
QSettings settings("MySoft", "Star Runner");
QColor color = palette().background().color();
settings.setValue("DataPump/bgcolor", color);
```

可以使用[QSettings](https://doc.qt.io/qt-5/qsettings.html)存储使用[qRegisterMetaType](https://doc.qt.io/qt-5/qmetatype.html#qRegisterMetaType-1)（）和[qRegisterMetaTypeStreamOperators](https://doc.qt.io/qt-5/qmetatype.html#qRegisterMetaTypeStreamOperators)（）注册的自定义类型。



### Section and Key Syntax

设置键可以包含任何Unicode字符。Windows注册表和INI文件使用不区分大小写的键，而[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS 上的CFPreferences API 使用区分大小写的键。要避免可移植性问题，请遵循以下简单规则：

1. 始终使用相同的外壳参考相同的键。例如，如果您在代码中的某个位置将某个键称为“文本字体”，请不要在其他位置将其称为“文本字体”。
2. 除了案例之外，避免使用相同的密钥名称。例如，如果您有一个名为“ [MainWindow](https://doc.qt.io/qt-5/designer-to-know.html#mainwindow) ” 的密钥，请不要尝试将另一个密钥保存为“mainwindow”。
3. 不要在部分或键名中使用斜杠（'/'和'\'）; 反斜杠字符用于分隔子键（见下文）。在窗口'\'由[QSettings](https://doc.qt.io/qt-5/qsettings.html)转换为'/'，这使它们相同。

您可以使用'/'字符作为分隔符来形成分层键，类似于Unix文件路径。例如：

```
    settings.setValue("mainwindow/size", win->size());
    settings.setValue("mainwindow/fullScreen", win->isFullScreen());
    settings.setValue("outputpanel/visible", panel->isVisible());
```

如果要保存或恢复具有相同前缀的许多设置，可以使用[beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)（）指定前缀，并在末尾调用[endGroup](https://doc.qt.io/qt-5/qsettings.html#endGroup)（）。这是同样的例子，但这次使用的是组机制：

```
    settings.beginGroup("mainwindow");
    settings.setValue("size", win->size());
    settings.setValue("fullScreen", win->isFullScreen());
    settings.endGroup();

    settings.beginGroup("outputpanel");
    settings.setValue("visible", panel->isVisible());
    settings.endGroup();
```

如果使用[beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)（）设置组，则大多数函数的行为会因此而更改。可以递归设置组。

除了组，[QSettings](https://doc.qt.io/qt-5/qsettings.html)还支持“数组”概念。有关详细信息，请参阅[beginReadArray](https://doc.qt.io/qt-5/qsettings.html#beginReadArray)（）和[beginWriteArray](https://doc.qt.io/qt-5/qsettings.html#beginWriteArray)（）。



### Fallback Mechanism

假设您已创建了一个[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象，其组织名称为MySoft，应用程序名称为Star Runner。查找值时，将按顺序搜索最多四个位置：

1. Star Runner应用程序的用户特定位置
2. MySoft为所有应用程序提供的特定于用户的位置
3. Star Runner应用程序的系统范围位置
4. MySoft为所有应用程序提供系统范围的位置

（有关Qt支持的不同平台上这些位置的信息，请参阅下面的[平台特定说明](https://doc.qt.io/qt-5/qsettings.html#platform-specific-notes)。）

如果在第一个位置找不到密钥，则搜索在第二个位置继续，依此类推。这使您可以存储系统范围或组织范围的设置，并基于每个用户或每个应用程序覆盖它们。要关闭此机制，请调用[setFallbacksEnabled](https://doc.qt.io/qt-5/qsettings.html#setFallbacksEnabled)（false）。

虽然来自所有四个位置的密钥都可供读取，但只有第一个文件（手头应用程序的用户特定位置）可供写入。要写入任何其他文件，请省略应用程序名称和/或指定[QSettings :: SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum)（与[QSettings :: UserScope相反](https://doc.qt.io/qt-5/qsettings.html#Scope-enum)，默认值）。

让我们看一个例子：

```c++
    QSettings obj1("MySoft", "Star Runner");
    QSettings obj2("MySoft");
    QSettings obj3(QSettings::SystemScope, "MySoft", "Star Runner");
    QSettings obj4(QSettings::SystemScope, "MySoft");
```

下表总结了哪些[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象访问哪个位置。“ **X** ”表示该位置是与[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象关联的主要位置，用于读取和写入; “o”表示该位置在阅读时用作后备。

| Locations               | `obj1` | `obj2` | `obj3` | `obj4` |
| ----------------------- | ------ | ------ | ------ | ------ |
| 1. User, Application    | **X**  |        |        |        |
| 2. User, Organization   | o      | **X**  |        |        |
| 3. System, Application  | o      |        | **X**  |        |
| 4. System, Organization | o      | o      | o      | **X**  |

这种机制的优点在于它可以在Qt支持的所有平台上运行，并且它仍然为您提供了很大的灵活性，而无需您指定任何文件名或注册表路径。

如果要在所有平台上使用INI文件而不是本机API，可以将[QSettings :: IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)作为[QSettings](https://doc.qt.io/qt-5/qsettings.html)构造函数的第一个参数传递，然后是作用域，组织名称和应用程序名称：

```c++
    QSettings settings(QSettings::IniFormat, QSettings::UserScope,
                       "MySoft", "Star Runner");
```

通过[“设置编辑器”](https://doc.qt.io/qt-5/qtwidgets-tools-settingseditor-example.html)示例，您可以尝试使用不同的设置位置并打开或关闭回退。



### Restoring the State of a GUI Application

[QSettings](https://doc.qt.io/qt-5/qsettings.html)通常用于存储GUI应用程序的状态。以下示例说明如何使用[QSettings](https://doc.qt.io/qt-5/qsettings.html)保存和恢复应用程序主窗口的几何图形。

```c++
void MainWindow::writeSettings()
{
    QSettings settings("Moose Soft", "Clipper");

    settings.beginGroup("MainWindow");
    settings.setValue("size", size());
    settings.setValue("pos", pos());
    settings.endGroup();
}

void MainWindow::readSettings()
{
    QSettings settings("Moose Soft", "Clipper");

    settings.beginGroup("MainWindow");
    resize(settings.value("size", QSize(400, 400)).toSize());
    move(settings.value("pos", QPoint(200, 200)).toPoint());
    settings.endGroup();
}
```

请参阅[Window Geometry](https://doc.qt.io/qt-5/application-windows.html#window-geometry)，了解为什么最好调用[QWidget :: resize](https://doc.qt.io/qt-5/qwidget.html#size-prop)（）和[QWidget :: move](https://doc.qt.io/qt-5/qwidget.html#pos-prop)（）而不是[QWidget :: setGeometry](https://doc.qt.io/qt-5/qwidget.html#geometry-prop)（）来恢复窗口的几何。

该`readSettings()`和`writeSettings()`功能必须从主窗口的构造函数和关闭事件处理函数中调用如下：

```c++
MainWindow::MainWindow()
{
    ...
    readSettings();
}

void MainWindow::closeEvent(QCloseEvent *event)
{
    if (userReallyWantsToQuit()) {
        writeSettings();
        event->accept();
    } else {
        event->ignore();
    }
}
```

有关使用[QSettings](https://doc.qt.io/qt-5/qsettings.html)的自包含示例，请参阅[应用程序](https://doc.qt.io/qt-5/qtwidgets-mainwindows-application-example.html)示例。



### Accessing Settings from Multiple Threads or Processes Simultaneously

[QSettings](https://doc.qt.io/qt-5/qsettings.html)是[可重入的](https://doc.qt.io/qt-5/threads-reentrancy.html)。这意味着您可以同时在不同的线程中使用不同的[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象。即使[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象引用磁盘上的相同文件（或系统注册表中的相同条目），此保证仍然有效。如果通过一个[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象修改了设置，则更改将立即在任何其他[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象中可见，这些对象在同一位置上运行并且位于同一进程中。

[如果](https://doc.qt.io/qt-5/qsettings.html)满足某些条件，[QSettings](https://doc.qt.io/qt-5/qsettings.html)可以安全地用于不同的进程（可以是同时运行的应用程序的不同实例或完全不同的应用程序）来读取和写入相同的系统位置。对于[QSettings :: IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)，它使用建议文件锁定和智能合并算法来确保数据完整性。可行的条件是可写配置文件必须是常规文件，并且必须驻留在当前用户可以创建新临时文件的目录中。如果不是这样，则必须使用[setAtomicSyncRequired](https://doc.qt.io/qt-5/qsettings.html#setAtomicSyncRequired)（）关掉安全装置。

请注意，[sync](https://doc.qt.io/qt-5/qsettings.html#sync)（）会导入其他进程所做的更改（除了从此[QSettings](https://doc.qt.io/qt-5/qsettings.html)写入更改）。



### Platform-Specific Notes



#### Locations Where Application Settings Are Stored

如“ [回退机制”](https://doc.qt.io/qt-5/qsettings.html#fallback-mechanism)部分所述，[QSettings](https://doc.qt.io/qt-5/qsettings.html)将应用程序的设置存储在最多四个位置，具体取决于设置是特定于用户还是系统范围，以及设置是特定于应用程序还是组织范围。为简单起见，我们假设该组织名为MySoft，该应用程序名为Star Runner。

在Unix系统上，如果文件格式为[NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)，则默认使用以下文件：

1. `$HOME/.config/MySoft/Star Runner.conf` (Qt for Embedded Linux: `$HOME/Settings/MySoft/Star Runner.conf`)
2. `$HOME/.config/MySoft.conf` (Qt for Embedded Linux: `$HOME/Settings/MySoft.conf`)
3. for each directory <dir> in $XDG_CONFIG_DIRS: `<dir>/MySoft/Star Runner.conf`
4. for each directory <dir> in $XDG_CONFIG_DIRS: `<dir>/MySoft.conf`

**Note:** If XDG_CONFIG_DIRS is unset, the default value of `/etc/xdg` is used.

在[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)版本10.2和10.3上，默认使用这些文件：

1. `$HOME/Library/Preferences/com.MySoft.Star Runner.plist`
2. `$HOME/Library/Preferences/com.MySoft.plist`
3. `/Library/Preferences/com.MySoft.Star Runner.plist`
4. `/Library/Preferences/com.MySoft.plist`

在Windows上，[NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)设置存储在以下注册表路径中：

1. `HKEY_CURRENT_USER\Software\MySoft\Star Runner`
2. `HKEY_CURRENT_USER\Software\MySoft\OrganizationDefaults`
3. `HKEY_LOCAL_MACHINE\Software\MySoft\Star Runner`
4. `HKEY_LOCAL_MACHINE\Software\MySoft\OrganizationDefaults`

**Note:** On Windows, for 32-bit programs running in WOW64 mode, settings are stored in the following registry path: `HKEY_LOCAL_MACHINE\Software\WOW6432node`.

如果文件格式是[NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)，则这是应用程序主目录中的“Settings / MySoft / Star Runner.conf”。

如果文件格式为[IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)，则在Unix，[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS 上使用以下文件：

1. `$HOME/.config/MySoft/Star Runner.ini` (Qt for Embedded Linux: `$HOME/Settings/MySoft/Star Runner.ini`)
2. `$HOME/.config/MySoft.ini` (Qt for Embedded Linux: `$HOME/Settings/MySoft.ini`)
3. for each directory <dir> in $XDG_CONFIG_DIRS: `<dir>/MySoft/Star Runner.ini`
4. for each directory <dir> in $XDG_CONFIG_DIRS: `<dir>/MySoft.ini`

**Note:** If XDG_CONFIG_DIRS is unset, the default value of `/etc/xdg` is used.

在Windows上，使用以下文件：

1. `FOLDERID_RoamingAppData\MySoft\Star Runner.ini`
2. `FOLDERID_RoamingAppData\MySoft.ini`
3. `FOLDERID_ProgramData\MySoft\Star Runner.ini`
4. `FOLDERID_ProgramData\MySoft.ini`

The identifiers prefixed by `FOLDERID_` are special item ID lists to be passed to the Win32 API function `SHGetKnownFolderPath()`to obtain the corresponding path.

`FOLDERID_RoamingAppData` usually points to `C:\Users\*User Name*\AppData\Roaming`, also shown by the environment variable `%APPDATA%`.

`FOLDERID_ProgramData` usually points to `C:\ProgramData`.

If the file format is [IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), this is "Settings/MySoft/Star Runner.ini" in the application's home directory.

The paths for the `.ini` and `.conf` files can be changed using [setPath](https://doc.qt.io/qt-5/qsettings.html#setPath)(). On Unix, [macOS](https://doc.qt.io/qt-5/internationalization.html#macos), and iOS the user can override them by setting the `XDG_CONFIG_HOME` environment variable; see [setPath](https://doc.qt.io/qt-5/qsettings.html#setPath)() for details.



#### Accessing INI and .plist Files Directly

有时您确实希望访问存储在特定文件或注册表路径中的设置。在所有平台上，如果要直接读取INI文件，可以使用[QSettings](https://doc.qt.io/qt-5/qsettings.html)构造函数，该构造函数将文件名作为第一个参数，并将[QSettings :: IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)作为第二个参数传递。例如：

```c++
QSettings settings("/home/petra/misc/myapp.ini", QSettings::IniFormat);
```

然后，您可以使用[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象来读取和写入文件中的设置。

在[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS上，您可以`.plist`通过传递[QSettings :: NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)作为第二个参数来访问属性列表文件。例如：

```c++
QSettings settings("/Users/petra/misc/myapp.plist", QSettings::NativeFormat);
```



#### Accessing the Windows Registry Directly

在Windows上，[QSettings](https://doc.qt.io/qt-5/qsettings.html)允许您访问在系统注册表中使用[QSettings](https://doc.qt.io/qt-5/qsettings.html)（或支持格式的设置，例如字符串数据）编写的设置。这是通过在注册表和[QSettings :: NativeFormat中](https://doc.qt.io/qt-5/qsettings.html#Format-enum)使用路径构造[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象来完成的。

例如：

```c++
QSettings settings("HKEY_CURRENT_USER\\Software\\Microsoft\\Office",
                   QSettings::NativeFormat);
```

可以像往常一样通过[QSettings](https://doc.qt.io/qt-5/qsettings.html)对象读取或写入在指定路径下出现的所有注册表项（使用正斜杠而不是反斜杠）。例如：

```c++
settings.setValue("11.0/Outlook/Security/DontTrustInstalledFiles", 0);
```

请注意，如上所述，[QSettings](https://doc.qt.io/qt-5/qsettings.html)使用反斜杠字符来分隔子键。因此，您无法读取或写入包含斜杠或反斜杠的Windows注册表项; 如果需要，您应该使用本机Windows API。



#### Accessing Common Registry Settings on Windows

在Windows上，密钥可以同时包含值和子密钥。使用“默认”或“。”访问其默认值。代替子键：

```c++
settings.setValue("HKEY_CURRENT_USER\\MySoft\\Star Runner\\Galaxy", "Milkyway");
settings.setValue("HKEY_CURRENT_USER\\MySoft\\Star Runner\\Galaxy\\Sun", "OurStar");
settings.value("HKEY_CURRENT_USER\\MySoft\\Star Runner\\Galaxy\\Default"); // returns "Milkyway"
```

On other platforms than Windows, "Default" and "." would be treated as regular subkeys.



#### Platform Limitations

虽然[QSettings](https://doc.qt.io/qt-5/qsettings.html)试图平滑不同支持平台之间的差异，但在移植应用程序时仍应注意一些差异：

- Windows系统注册表具有以下限制：子项不能超过255个字符，条目的值不能超过16,383个字符，并且键的所有值不能超过65,535个字符。要解决这些限制的一种方法是使用存储设置[IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)而不是[NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum)。

- 在Windows上，使用Windows系统注册表时，[QSettings](https://doc.qt.io/qt-5/qsettings.html)不会保留值的原始类型。因此，设置新值时，值的类型可能会更改。例如，类型的值`REG_EXPAND_SZ`将更改为`REG_SZ`。

- 在[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS上，[allKeys](https://doc.qt.io/qt-5/qsettings.html#allKeys)（）将为适用于所有应用程序的全局设置返回一些额外的键。这些键可以使用[value](https://doc.qt.io/qt-5/qsettings.html#value)（）读取，但不能更改，只能被遮蔽。调用[setFallbacksEnabled](https://doc.qt.io/qt-5/qsettings.html#setFallbacksEnabled)（false）将隐藏这些全局设置。

- 上[的MacOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS，所使用的API CFPreferences [QSettings](https://doc.qt.io/qt-5/qsettings.html)预计互联网域名，而不是组织名称。为了提供统一的API，[QSettings](https://doc.qt.io/qt-5/qsettings.html)从组织名称中获取虚假域名（除非组织名称已经是域名，例如OpenOffice.org）。该算法将“.com”附加到公司名称，并用连字符替换空格和其他非法字符。如果要指定其他域名，请在函数中调用[QCoreApplication :: setOrganizationDomain](https://doc.qt.io/qt-5/qcoreapplication.html#organizationDomain-prop)（），[QCoreApplication :: setOrganizationName](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)（）和[QCoreApplication :: setApplicationName](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)（）`main()`，然后使用默认的[QSettings](https://doc.qt.io/qt-5/qsettings.html)构造函数。另一种解决方案是使用预处理程序指令，例如：

  ```
  #ifdef Q_OS_MAC
      QSettings settings("grenoullelogique.fr", "Squash");
  #else
      QSettings settings("Grenoulle Logique", "Squash");
  #endif
  ```

- 在[macOS上](https://doc.qt.io/qt-5/internationalization.html#macos)，访问不属于当前用户（即[SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum)）的设置的权限已更改为10.7（Lion）。在该版本之前，具有管理员权限的用户可以访问这些权限。对于10.7和10.8（Mountain Lion），只有root可以。但是，10.9（Mavericks）再次更改了该规则，但仅限于原生格式（plist文件）。

**See also** [QVariant](https://doc.qt.io/qt-5/qvariant.html), [QSessionManager](https://doc.qt.io/qt-5/qsessionmanager.html), [Settings Editor Example](https://doc.qt.io/qt-5/qtwidgets-tools-settingseditor-example.html), and [Application Example](https://doc.qt.io/qt-5/qtwidgets-mainwindows-application-example.html).



## Member Type Documentation

### enum QSettings::Format

This enum type specifies the storage format used by [QSettings](https://doc.qt.io/qt-5/qsettings.html).

| Constant                      | Value | Description                                                  |
| ----------------------------- | ----- | ------------------------------------------------------------ |
| `QSettings::NativeFormat`     | `0`   | 使用最适合平台的存储格式存储设置。在Windows上，这意味着系统注册表; 在[macOS](https://doc.qt.io/qt-5/internationalization.html#macos)和iOS上，这意味着CFPreferences API; 在Unix上，这意味着INI格式的文本配置文件。 |
| `QSettings::Registry32Format` | `2`   | 仅限Windows：从64位Windows上运行的64位应用程序显式访问32位系统注册表。在32位Windows或64位Windows上的32位应用程序中，这与指定NativeFormat相同。此枚举值已添加到Qt 5.7中。 |
| `QSettings::Registry64Format` | `3`   | 仅限Windows：从64位Windows上运行的32位应用程序显式访问64位系统注册表。在32位Windows上或64位Windows上的64位应用程序中，这与指定NativeFormat的工作方式相同。此枚举值已添加到Qt 5.7中。 |
| `QSettings::IniFormat`        | `1`   | 将设置存储在INI文件中。                                      |
| `QSettings::InvalidFormat`    | `16`  | [registerFormat](https://doc.qt.io/qt-5/qsettings.html#registerFormat)（）返回的特殊值。 |

在Unix上，NativeFormat和IniFormat意味着相同的事情，除了文件扩展名不同（`.conf`对于NativeFormat，`.ini`对于IniFormat）。

INI文件格式是Qt在所有平台上支持的Windows文件格式。在没有INI标准的情况下，我们尝试遵循Microsoft的做法，但以下情况除外：

- 如果存储[QVariant](https://doc.qt.io/qt-5/qvariant.html)无法转换为[QString的类型](https://doc.qt.io/qt-5/qstring.html)（例如，[QPoint](https://doc.qt.io/qt-5/qpoint.html)，[QRect](https://doc.qt.io/qt-5/qrect.html)和[QSize](https://doc.qt.io/qt-5/qsize.html)），Qt将使用`@`基于语法的语法对该类型进行编码。例如：

  ```
  pos = @Point(100 100)
  ```

  为了尽量减少兼容性问题，任何`@`不在值的第一位置出现或者不跟一个Qt类型（`Point`，`Rect`，`Size`等）被视为普通的字符。

- 虽然反斜杠是INI文件中的特殊字符，但大多数Windows应用程序不会`\`在文件路径中转义反斜杠（）：

  ```
  windir = C:\Windows
  ```

  [QSettings](https://doc.qt.io/qt-5/qsettings.html)始终将反斜杠视为特殊字符，并且不提供用于读取或写入此类条目的API。

- INI文件格式对密钥的语法有严格的限制。Qt通过使用`%`键中的转义字符来解决这个问题。此外，如果您保存顶级设置（其中没有斜杠的键，例如“someKey”），它将出现在INI文件的“常规”部分中。为避免覆盖其他密钥，如果使用“General / someKey”等密钥保存，则密钥将位于“％General”部分，*而不是* “General”部分。

- 遵循我们应该接受的原则和我们生成的保守主义的理念，[QSettings](https://doc.qt.io/qt-5/qsettings.html)将接受Latin-1编码的INI文件，但生成纯ASCII文件，其中非ASCII值使用标准INI转义序列进行编码。要使INI文件更具可读性（但可能不太兼容），请调用[setIniCodec](https://doc.qt.io/qt-5/qsettings.html#setIniCodec)（）。

**See also** [registerFormat](https://doc.qt.io/qt-5/qsettings.html#registerFormat)() and [setPath](https://doc.qt.io/qt-5/qsettings.html#setPath)().



### typedef QSettings::ReadFunc

Typedef指向具有以下签名的函数的指针：

```c++
bool myReadFunc(QIODevice &device, QSettings::SettingsMap &map);
```

`ReadFunc`用作`registerFormat()`指向读取一组键/值对的函数的指针。`ReadFunc`应该读取一遍中的所有选项，并返回`SettingsMap`容器中的所有设置，该设置最初为空。

**See also** [WriteFunc](https://doc.qt.io/qt-5/qsettings.html#WriteFunc-typedef) and [registerFormat](https://doc.qt.io/qt-5/qsettings.html#registerFormat)().

### enum QSettings::Scope

此枚举指定设置是特定于用户还是由同一系统的所有用户共享。

| Constant                 | Value | Description                                                  |
| ------------------------ | ----- | ------------------------------------------------------------ |
| `QSettings::UserScope`   | `0`   | 将设置存储在特定于当前用户的位置（例如，在用户的主目录中）。 |
| `QSettings::SystemScope` | `1`   | 将设置存储在全局位置，以便同一台计算机上的所有用户访问同一组设置。 |

**See also** [setPath](https://doc.qt.io/qt-5/qsettings.html#setPath)().

### typedef QSettings::SettingsMap

Typedef for [QMap](https://doc.qt.io/qt-5/qmap.html)<[QString](https://doc.qt.io/qt-5/qstring.html), [QVariant](https://doc.qt.io/qt-5/qvariant.html)>.

**See also** [registerFormat](https://doc.qt.io/qt-5/qsettings.html#registerFormat)().

### enum QSettings::Status

The following status values are possible:

| Constant                 | Value | Description                                                  |
| ------------------------ | ----- | ------------------------------------------------------------ |
| `QSettings::NoError`     | `0`   | No error occurred.                                           |
| `QSettings::AccessError` | `1`   | An access error occurred (e.g. trying to write to a read-only file). |
| `QSettings::FormatError` | `2`   | A format error occurred (e.g. loading a malformed INI file). |

**See also** [status](https://doc.qt.io/qt-5/qsettings.html#status)().

### typedef QSettings::WriteFunc

Typedef for a pointer to a function with the following signature:

```
bool myWriteFunc(QIODevice &device, const QSettings::SettingsMap &map);
```

`WriteFunc` is used in `registerFormat()` as a pointer to a function that writes a set of key/value pairs. `WriteFunc` is only called once, so you need to output the settings in one go.

**See also** [ReadFunc](https://doc.qt.io/qt-5/qsettings.html#ReadFunc-typedef) and [registerFormat](https://doc.qt.io/qt-5/qsettings.html#registerFormat)().

## Member Function Documentation

### QSettings::QSettings(const [QString](https://doc.qt.io/qt-5/qstring.html) &*organization*, const [QString](https://doc.qt.io/qt-5/qstring.html) &*application* = QString(), [QObject](https://doc.qt.io/qt-5/qobject.html#QObject) **parent* = nullptr)

Constructs a [QSettings](https://doc.qt.io/qt-5/qsettings.html) object for accessing settings of the application called *application* from the organization called *organization*, and with parent *parent*.

Example:

```
QSettings settings("Moose Tech", "Facturo-Pro");
```

The scope is set to [QSettings::UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum), and the format is set to [QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) (i.e. calling [setDefaultFormat](https://doc.qt.io/qt-5/qsettings.html#setDefaultFormat)() before calling this constructor has no effect).

**See also** [setDefaultFormat](https://doc.qt.io/qt-5/qsettings.html#setDefaultFormat)() and [Fallback Mechanism](https://doc.qt.io/qt-5/qsettings.html#fallback-mechanism).

### QSettings::QSettings([QSettings::Scope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) *scope*, const [QString](https://doc.qt.io/qt-5/qstring.html) &*organization*, const [QString](https://doc.qt.io/qt-5/qstring.html)&*application* = QString(), [QObject](https://doc.qt.io/qt-5/qobject.html#QObject) **parent* = nullptr)

Constructs a [QSettings](https://doc.qt.io/qt-5/qsettings.html) object for accessing settings of the application called *application* from the organization called *organization*, and with parent *parent*.

If *scope* is [QSettings::UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum), the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object searches user-specific settings first, before it searches system-wide settings as a fallback. If *scope* is [QSettings::SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum), the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object ignores user-specific settings and provides access to system-wide settings.

The storage format is set to [QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) (i.e. calling [setDefaultFormat](https://doc.qt.io/qt-5/qsettings.html#setDefaultFormat)() before calling this constructor has no effect).

If no application name is given, the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object will only access the organization-wide [locations](https://doc.qt.io/qt-5/qsettings.html#fallback-mechanism).

**See also** [setDefaultFormat](https://doc.qt.io/qt-5/qsettings.html#setDefaultFormat)().

### QSettings::QSettings([QSettings::Format](https://doc.qt.io/qt-5/qsettings.html#Format-enum) *format*, [QSettings::Scope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) *scope*, const [QString](https://doc.qt.io/qt-5/qstring.html)&*organization*, const [QString](https://doc.qt.io/qt-5/qstring.html) &*application* = QString(), [QObject](https://doc.qt.io/qt-5/qobject.html#QObject) **parent* = nullptr)

Constructs a [QSettings](https://doc.qt.io/qt-5/qsettings.html) object for accessing settings of the application called *application* from the organization called *organization*, and with parent *parent*.

If *scope* is [QSettings::UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum), the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object searches user-specific settings first, before it searches system-wide settings as a fallback. If *scope* is [QSettings::SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum), the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object ignores user-specific settings and provides access to system-wide settings.

If *format* is [QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), the native API is used for storing settings. If *format* is [QSettings::IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), the INI format is used.

If no application name is given, the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object will only access the organization-wide [locations](https://doc.qt.io/qt-5/qsettings.html#fallback-mechanism).

### QSettings::QSettings(const [QString](https://doc.qt.io/qt-5/qstring.html) &*fileName*, [QSettings::Format](https://doc.qt.io/qt-5/qsettings.html#Format-enum) *format*, [QObject](https://doc.qt.io/qt-5/qobject.html#QObject) **parent* = nullptr)

Constructs a [QSettings](https://doc.qt.io/qt-5/qsettings.html) object for accessing the settings stored in the file called *fileName*, with parent *parent*. If the file doesn't already exist, it is created.

If *format* is [QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), the meaning of *fileName* depends on the platform. On Unix, *fileName* is the name of an INI file. On [macOS](https://doc.qt.io/qt-5/internationalization.html#macos) and iOS, *fileName* is the name of a `.plist` file. On Windows, *fileName* is a path in the system registry.

If *format* is [QSettings::IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), *fileName* is the name of an INI file.

**Warning:** This function is provided for convenience. It works well for accessing INI or `.plist` files generated by Qt, but might fail on some syntaxes found in such files originated by other programs. In particular, be aware of the following limitations:

- [QSettings](https://doc.qt.io/qt-5/qsettings.html) provides no way of reading INI "path" entries, i.e., entries with unescaped slash characters. (This is because these entries are ambiguous and cannot be resolved automatically.)
- In INI files, [QSettings](https://doc.qt.io/qt-5/qsettings.html) uses the `@` character as a metacharacter in some contexts, to encode Qt-specific data types (e.g., `@Rect`), and might therefore misinterpret it when it occurs in pure INI files.

**See also** [fileName](https://doc.qt.io/qt-5/qsettings.html#fileName)().

### QSettings::QSettings([QObject](https://doc.qt.io/qt-5/qobject.html#QObject) **parent* = nullptr)

Constructs a [QSettings](https://doc.qt.io/qt-5/qsettings.html) object for accessing settings of the application and organization set previously with a call to [QCoreApplication::setOrganizationName](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)(), [QCoreApplication::setOrganizationDomain](https://doc.qt.io/qt-5/qcoreapplication.html#organizationDomain-prop)(), and [QCoreApplication::setApplicationName](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)().

The scope is [QSettings::UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) and the format is [defaultFormat](https://doc.qt.io/qt-5/qsettings.html#defaultFormat)() ([QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) by default). Use [setDefaultFormat](https://doc.qt.io/qt-5/qsettings.html#setDefaultFormat)() before calling this constructor to change the default format used by this constructor.

The code

```
QSettings settings("Moose Soft", "Facturo-Pro");
```

is equivalent to

```
QCoreApplication::setOrganizationName("Moose Soft");
QCoreApplication::setApplicationName("Facturo-Pro");
QSettings settings;
```

If [QCoreApplication::setOrganizationName](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)() and [QCoreApplication::setApplicationName](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)() has not been previously called, the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object will not be able to read or write any settings, and [status](https://doc.qt.io/qt-5/qsettings.html#status)() will return [AccessError](https://doc.qt.io/qt-5/qsettings.html#Status-enum).

On [macOS](https://doc.qt.io/qt-5/internationalization.html#macos) and iOS, if both a name and an Internet domain are specified for the organization, the domain is preferred over the name. On other platforms, the name is preferred over the domain.

**See also** [QCoreApplication::setOrganizationName](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)(), [QCoreApplication::setOrganizationDomain](https://doc.qt.io/qt-5/qcoreapplication.html#organizationDomain-prop)(), [QCoreApplication::setApplicationName](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)(), and [setDefaultFormat](https://doc.qt.io/qt-5/qsettings.html#setDefaultFormat)().

### `[virtual]`QSettings::~QSettings()

Destroys the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object.

Any unsaved changes will eventually be written to permanent storage.

**See also** [sync](https://doc.qt.io/qt-5/qsettings.html#sync)().

### [QStringList](https://doc.qt.io/qt-5/qstringlist.html) QSettings::allKeys() const

Returns a list of all keys, including subkeys, that can be read using the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object.

Example:

```
QSettings settings;
settings.setValue("fridge/color", QColor(Qt::white));
settings.setValue("fridge/size", QSize(32, 96));
settings.setValue("sofa", true);
settings.setValue("tv", false);

QStringList keys = settings.allKeys();
// keys: ["fridge/color", "fridge/size", "sofa", "tv"]
```

If a group is set using [beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)(), only the keys in the group are returned, without the group prefix:

```
settings.beginGroup("fridge");
keys = settings.allKeys();
// keys: ["color", "size"]
```

**See also** [childGroups](https://doc.qt.io/qt-5/qsettings.html#childGroups)() and [childKeys](https://doc.qt.io/qt-5/qsettings.html#childKeys)().

### [QString](https://doc.qt.io/qt-5/qstring.html) QSettings::applicationName() const

Returns the application name used for storing the settings.

This function was introduced in Qt 4.4.

**See also** [QCoreApplication::applicationName](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)(), [format](https://doc.qt.io/qt-5/qsettings.html#format)(), [scope](https://doc.qt.io/qt-5/qsettings.html#scope)(), and [organizationName](https://doc.qt.io/qt-5/qsettings.html#organizationName)().

### void QSettings::beginGroup(const [QString](https://doc.qt.io/qt-5/qstring.html) &*prefix*)

Appends *prefix* to the current group.

The current group is automatically prepended to all keys specified to [QSettings](https://doc.qt.io/qt-5/qsettings.html). In addition, query functions such as [childGroups](https://doc.qt.io/qt-5/qsettings.html#childGroups)(), [childKeys](https://doc.qt.io/qt-5/qsettings.html#childKeys)(), and [allKeys](https://doc.qt.io/qt-5/qsettings.html#allKeys)() are based on the group. By default, no group is set.

Groups are useful to avoid typing in the same setting paths over and over. For example:

```
settings.beginGroup("mainwindow");
settings.setValue("size", win->size());
settings.setValue("fullScreen", win->isFullScreen());
settings.endGroup();

settings.beginGroup("outputpanel");
settings.setValue("visible", panel->isVisible());
settings.endGroup();
```

This will set the value of three settings:

- `mainwindow/size`
- `mainwindow/fullScreen`
- `outputpanel/visible`

Call [endGroup](https://doc.qt.io/qt-5/qsettings.html#endGroup)() to reset the current group to what it was before the corresponding beginGroup() call. Groups can be nested.

**See also** [endGroup](https://doc.qt.io/qt-5/qsettings.html#endGroup)() and [group](https://doc.qt.io/qt-5/qsettings.html#group)().

### int QSettings::beginReadArray(const [QString](https://doc.qt.io/qt-5/qstring.html) &*prefix*)

Adds *prefix* to the current group and starts reading from an array. Returns the size of the array.

Example:

```
struct Login {
    QString userName;
    QString password;
};
QList<Login> logins;
...

QSettings settings;
int size = settings.beginReadArray("logins");
for (int i = 0; i < size; ++i) {
    settings.setArrayIndex(i);
    Login login;
    login.userName = settings.value("userName").toString();
    login.password = settings.value("password").toString();
    logins.append(login);
}
settings.endArray();
```

Use [beginWriteArray](https://doc.qt.io/qt-5/qsettings.html#beginWriteArray)() to write the array in the first place.

**See also** [beginWriteArray](https://doc.qt.io/qt-5/qsettings.html#beginWriteArray)(), [endArray](https://doc.qt.io/qt-5/qsettings.html#endArray)(), and [setArrayIndex](https://doc.qt.io/qt-5/qsettings.html#setArrayIndex)().

### void QSettings::beginWriteArray(const [QString](https://doc.qt.io/qt-5/qstring.html) &*prefix*, int *size* = -1)

Adds *prefix* to the current group and starts writing an array of size *size*. If *size* is -1 (the default), it is automatically determined based on the indexes of the entries written.

If you have many occurrences of a certain set of keys, you can use arrays to make your life easier. For example, let's suppose that you want to save a variable-length list of user names and passwords. You could then write:

```
struct Login {
    QString userName;
    QString password;
};
QList<Login> logins;
...

QSettings settings;
settings.beginWriteArray("logins");
for (int i = 0; i < logins.size(); ++i) {
    settings.setArrayIndex(i);
    settings.setValue("userName", list.at(i).userName);
    settings.setValue("password", list.at(i).password);
}
settings.endArray();
```

The generated keys will have the form

- `logins/size`
- `logins/1/userName`
- `logins/1/password`
- `logins/2/userName`
- `logins/2/password`
- `logins/3/userName`
- `logins/3/password`
- ...

To read back an array, use [beginReadArray](https://doc.qt.io/qt-5/qsettings.html#beginReadArray)().

**See also** [beginReadArray](https://doc.qt.io/qt-5/qsettings.html#beginReadArray)(), [endArray](https://doc.qt.io/qt-5/qsettings.html#endArray)(), and [setArrayIndex](https://doc.qt.io/qt-5/qsettings.html#setArrayIndex)().

### [QStringList](https://doc.qt.io/qt-5/qstringlist.html) QSettings::childGroups() const

Returns a list of all key top-level groups that contain keys that can be read using the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object.

Example:

```
QSettings settings;
settings.setValue("fridge/color", QColor(Qt::white));
settings.setValue("fridge/size", QSize(32, 96));
settings.setValue("sofa", true);
settings.setValue("tv", false);

QStringList groups = settings.childGroups();
// groups: ["fridge"]
```

If a group is set using [beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)(), the first-level keys in that group are returned, without the group prefix.

```
settings.beginGroup("fridge");
groups = settings.childGroups();
// groups: []
```

You can navigate through the entire setting hierarchy using [childKeys](https://doc.qt.io/qt-5/qsettings.html#childKeys)() and childGroups() recursively.

**See also** [childKeys](https://doc.qt.io/qt-5/qsettings.html#childKeys)() and [allKeys](https://doc.qt.io/qt-5/qsettings.html#allKeys)().

### [QStringList](https://doc.qt.io/qt-5/qstringlist.html) QSettings::childKeys() const

Returns a list of all top-level keys that can be read using the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object.

Example:

```
QSettings settings;
settings.setValue("fridge/color", QColor(Qt::white));
settings.setValue("fridge/size", QSize(32, 96));
settings.setValue("sofa", true);
settings.setValue("tv", false);

QStringList keys = settings.childKeys();
// keys: ["sofa", "tv"]
```

If a group is set using [beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)(), the top-level keys in that group are returned, without the group prefix:

```
settings.beginGroup("fridge");
keys = settings.childKeys();
// keys: ["color", "size"]
```

You can navigate through the entire setting hierarchy using childKeys() and [childGroups](https://doc.qt.io/qt-5/qsettings.html#childGroups)() recursively.

**See also** [childGroups](https://doc.qt.io/qt-5/qsettings.html#childGroups)() and [allKeys](https://doc.qt.io/qt-5/qsettings.html#allKeys)().

### void QSettings::clear()

Removes all entries in the primary location associated to this [QSettings](https://doc.qt.io/qt-5/qsettings.html) object.

Entries in fallback locations are not removed.

If you only want to remove the entries in the current [group](https://doc.qt.io/qt-5/qsettings.html#group)(), use remove("") instead.

**See also** [remove](https://doc.qt.io/qt-5/qsettings.html#remove)() and [setFallbacksEnabled](https://doc.qt.io/qt-5/qsettings.html#setFallbacksEnabled)().

### bool QSettings::contains(const [QString](https://doc.qt.io/qt-5/qstring.html) &*key*) const

Returns `true` if there exists a setting called *key*; returns false otherwise.

If a group is set using [beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)(), *key* is taken to be relative to that group.

Note that the Windows registry and INI files use case-insensitive keys, whereas the CFPreferences API on [macOS](https://doc.qt.io/qt-5/internationalization.html#macos) and iOS uses case-sensitive keys. To avoid portability problems, see the [Section and Key Syntax](https://doc.qt.io/qt-5/qsettings.html#section-and-key-syntax) rules.

**See also** [value](https://doc.qt.io/qt-5/qsettings.html#value)() and [setValue](https://doc.qt.io/qt-5/qsettings.html#setValue)().

### `[static]`[QSettings::Format](https://doc.qt.io/qt-5/qsettings.html#Format-enum) QSettings::defaultFormat()

Returns default file format used for storing settings for the [QSettings](https://doc.qt.io/qt-5/qsettings.html)([QObject](https://doc.qt.io/qt-5/qobject.html) *) constructor. If no default format is set, [QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) is used.

This function was introduced in Qt 4.4.

**See also** [setDefaultFormat](https://doc.qt.io/qt-5/qsettings.html#setDefaultFormat)() and [format](https://doc.qt.io/qt-5/qsettings.html#format)().

### void QSettings::endArray()

Closes the array that was started using [beginReadArray](https://doc.qt.io/qt-5/qsettings.html#beginReadArray)() or [beginWriteArray](https://doc.qt.io/qt-5/qsettings.html#beginWriteArray)().

**See also** [beginReadArray](https://doc.qt.io/qt-5/qsettings.html#beginReadArray)() and [beginWriteArray](https://doc.qt.io/qt-5/qsettings.html#beginWriteArray)().

### void QSettings::endGroup()

Resets the group to what it was before the corresponding [beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)() call.

Example:

```
settings.beginGroup("alpha");
// settings.group() == "alpha"

settings.beginGroup("beta");
// settings.group() == "alpha/beta"

settings.endGroup();
// settings.group() == "alpha"

settings.endGroup();
// settings.group() == ""
```

**See also** [beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)() and [group](https://doc.qt.io/qt-5/qsettings.html#group)().

### `[override virtual protected]`bool QSettings::event([QEvent](https://doc.qt.io/qt-5/qevent.html) **event*)

Reimplemented from [QObject::event](https://doc.qt.io/qt-5/qobject.html#event)().

### bool QSettings::fallbacksEnabled() const

Returns `true` if fallbacks are enabled; returns `false` otherwise.

By default, fallbacks are enabled.

**See also** [setFallbacksEnabled](https://doc.qt.io/qt-5/qsettings.html#setFallbacksEnabled)().

### [QString](https://doc.qt.io/qt-5/qstring.html) QSettings::fileName() const

Returns the path where settings written using this [QSettings](https://doc.qt.io/qt-5/qsettings.html) object are stored.

On Windows, if the format is [QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), the return value is a system registry path, not a file path.

**See also** [isWritable](https://doc.qt.io/qt-5/qsettings.html#isWritable)() and [format](https://doc.qt.io/qt-5/qsettings.html#format)().

### [QSettings::Format](https://doc.qt.io/qt-5/qsettings.html#Format-enum) QSettings::format() const

Returns the format used for storing the settings.

This function was introduced in Qt 4.4.

**See also** [defaultFormat](https://doc.qt.io/qt-5/qsettings.html#defaultFormat)(), [fileName](https://doc.qt.io/qt-5/qsettings.html#fileName)(), [scope](https://doc.qt.io/qt-5/qsettings.html#scope)(), [organizationName](https://doc.qt.io/qt-5/qsettings.html#organizationName)(), and [applicationName](https://doc.qt.io/qt-5/qsettings.html#applicationName)().

### [QString](https://doc.qt.io/qt-5/qstring.html) QSettings::group() const

Returns the current group.

**See also** [beginGroup](https://doc.qt.io/qt-5/qsettings.html#beginGroup)() and [endGroup](https://doc.qt.io/qt-5/qsettings.html#endGroup)().

### [QTextCodec](https://doc.qt.io/qt-5/qtextcodec.html) *QSettings::iniCodec() const

Returns the codec that is used for accessing INI files. By default, no codec is used, so a null pointer is returned.

This function was introduced in Qt 4.5.

**See also** [setIniCodec](https://doc.qt.io/qt-5/qsettings.html#setIniCodec)().

### bool QSettings::isAtomicSyncRequired() const

Returns `true` if [QSettings](https://doc.qt.io/qt-5/qsettings.html) is only allowed to perform atomic saving and reloading (synchronization) of the settings. Returns `false` if it is allowed to save the settings contents directly to the configuration file.

The default is `true`.

This function was introduced in Qt 5.10.

**See also** [setAtomicSyncRequired](https://doc.qt.io/qt-5/qsettings.html#setAtomicSyncRequired)() and [QSaveFile](https://doc.qt.io/qt-5/qsavefile.html).

### bool QSettings::isWritable() const

Returns `true` if settings can be written using this [QSettings](https://doc.qt.io/qt-5/qsettings.html) object; returns `false` otherwise.

One reason why isWritable() might return false is if [QSettings](https://doc.qt.io/qt-5/qsettings.html) operates on a read-only file.

**Warning:** This function is not perfectly reliable, because the file permissions can change at any time.

**See also** [fileName](https://doc.qt.io/qt-5/qsettings.html#fileName)(), [status](https://doc.qt.io/qt-5/qsettings.html#status)(), and [sync](https://doc.qt.io/qt-5/qsettings.html#sync)().

### [QString](https://doc.qt.io/qt-5/qstring.html) QSettings::organizationName() const

Returns the organization name used for storing the settings.

This function was introduced in Qt 4.4.

**See also** [QCoreApplication::organizationName](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)(), [format](https://doc.qt.io/qt-5/qsettings.html#format)(), [scope](https://doc.qt.io/qt-5/qsettings.html#scope)(), and [applicationName](https://doc.qt.io/qt-5/qsettings.html#applicationName)().

### `[static]`[QSettings::Format](https://doc.qt.io/qt-5/qsettings.html#Format-enum) QSettings::registerFormat(const [QString](https://doc.qt.io/qt-5/qstring.html) &*extension*, [QSettings::ReadFunc](https://doc.qt.io/qt-5/qsettings.html#ReadFunc-typedef) *readFunc*, [QSettings::WriteFunc](https://doc.qt.io/qt-5/qsettings.html#WriteFunc-typedef) *writeFunc*, [Qt::CaseSensitivity](https://doc.qt.io/qt-5/qt.html#CaseSensitivity-enum)*caseSensitivity* = Qt::CaseSensitive)

Registers a custom storage format. On success, returns a special Format value that can then be passed to the [QSettings](https://doc.qt.io/qt-5/qsettings.html) constructor. On failure, returns [InvalidFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum).

The *extension* is the file extension associated to the format (without the '.').

The *readFunc* and *writeFunc* parameters are pointers to functions that read and write a set of key/value pairs. The [QIODevice](https://doc.qt.io/qt-5/qiodevice.html)parameter to the read and write functions is always opened in binary mode (i.e., without the [QIODevice::Text](https://doc.qt.io/qt-5/qiodevice.html#OpenModeFlag-enum) flag).

The *caseSensitivity* parameter specifies whether keys are case sensitive or not. This makes a difference when looking up values using [QSettings](https://doc.qt.io/qt-5/qsettings.html). The default is case sensitive.

By default, if you use one of the constructors that work in terms of an organization name and an application name, the file system locations used are the same as for [IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum). Use [setPath](https://doc.qt.io/qt-5/qsettings.html#setPath)() to specify other locations.

Example:

```
bool readXmlFile(QIODevice &device, QSettings::SettingsMap &map);
bool writeXmlFile(QIODevice &device, const QSettings::SettingsMap &map);

int main(int argc, char *argv[])
{
    const QSettings::Format XmlFormat =
            QSettings::registerFormat("xml", readXmlFile, writeXmlFile);

    QSettings settings(XmlFormat, QSettings::UserScope, "MySoft",
                       "Star Runner");

    ...
}
```

**Note:** This function is [thread-safe](https://doc.qt.io/qt-5/threads-reentrancy.html).

This function was introduced in Qt 4.1.

**See also** [setPath](https://doc.qt.io/qt-5/qsettings.html#setPath)().

### void QSettings::remove(const [QString](https://doc.qt.io/qt-5/qstring.html) &*key*)

Removes the setting *key* and any sub-settings of *key*.

Example:

```
QSettings settings;
settings.setValue("ape");
settings.setValue("monkey", 1);
settings.setValue("monkey/sea", 2);
settings.setValue("monkey/doe", 4);

settings.remove("monkey");
QStringList keys = settings.allKeys();
// keys: ["ape"]
```

Be aware that if one of the fallback locations contains a setting with the same key, that setting will be visible after calling remove().

If *key* is an empty string, all keys in the current [group](https://doc.qt.io/qt-5/qsettings.html#group)() are removed. For example:

```
QSettings settings;
settings.setValue("ape");
settings.setValue("monkey", 1);
settings.setValue("monkey/sea", 2);
settings.setValue("monkey/doe", 4);

settings.beginGroup("monkey");
settings.remove("");
settings.endGroup();

QStringList keys = settings.allKeys();
// keys: ["ape"]
```

Note that the Windows registry and INI files use case-insensitive keys, whereas the CFPreferences API on [macOS](https://doc.qt.io/qt-5/internationalization.html#macos) and iOS uses case-sensitive keys. To avoid portability problems, see the [Section and Key Syntax](https://doc.qt.io/qt-5/qsettings.html#section-and-key-syntax) rules.

**See also** [setValue](https://doc.qt.io/qt-5/qsettings.html#setValue)(), [value](https://doc.qt.io/qt-5/qsettings.html#value)(), and [contains](https://doc.qt.io/qt-5/qsettings.html#contains)().

### [QSettings::Scope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) QSettings::scope() const

Returns the scope used for storing the settings.

This function was introduced in Qt 4.4.

**See also** [format](https://doc.qt.io/qt-5/qsettings.html#format)(), [organizationName](https://doc.qt.io/qt-5/qsettings.html#organizationName)(), and [applicationName](https://doc.qt.io/qt-5/qsettings.html#applicationName)().

### void QSettings::setArrayIndex(int *i*)

Sets the current array index to *i*. Calls to functions such as [setValue](https://doc.qt.io/qt-5/qsettings.html#setValue)(), [value](https://doc.qt.io/qt-5/qsettings.html#value)(), [remove](https://doc.qt.io/qt-5/qsettings.html#remove)(), and [contains](https://doc.qt.io/qt-5/qsettings.html#contains)() will operate on the array entry at that index.

You must call [beginReadArray](https://doc.qt.io/qt-5/qsettings.html#beginReadArray)() or [beginWriteArray](https://doc.qt.io/qt-5/qsettings.html#beginWriteArray)() before you can call this function.

### void QSettings::setAtomicSyncRequired(bool *enable*)

Configures whether [QSettings](https://doc.qt.io/qt-5/qsettings.html) is required to perform atomic saving and reloading (synchronization) of the settings. If the *enable*argument is `true` (the default), [sync](https://doc.qt.io/qt-5/qsettings.html#sync)() will only perform synchronization operations that are atomic. If this is not possible, [sync](https://doc.qt.io/qt-5/qsettings.html#sync)() will fail and [status](https://doc.qt.io/qt-5/qsettings.html#status)() will be an error condition.

Setting this property to `false` will allow [QSettings](https://doc.qt.io/qt-5/qsettings.html) to write directly to the configuration file and ignore any errors trying to lock it against other processes trying to write at the same time. Because of the potential for corruption, this option should be used with care, but is required in certain conditions, like a [QSettings::IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) configuration file that exists in an otherwise non-writeable directory or NTFS Alternate Data Streams.

See [QSaveFile](https://doc.qt.io/qt-5/qsavefile.html) for more information on the feature.

This function was introduced in Qt 5.10.

**See also** [isAtomicSyncRequired](https://doc.qt.io/qt-5/qsettings.html#isAtomicSyncRequired)() and [QSaveFile](https://doc.qt.io/qt-5/qsavefile.html).

### `[static]`void QSettings::setDefaultFormat([QSettings::Format](https://doc.qt.io/qt-5/qsettings.html#Format-enum) *format*)

Sets the default file format to the given *format*, which is used for storing settings for the [QSettings](https://doc.qt.io/qt-5/qsettings.html)([QObject](https://doc.qt.io/qt-5/qobject.html) *) constructor.

If no default format is set, [QSettings::NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) is used. See the documentation for the [QSettings](https://doc.qt.io/qt-5/qsettings.html) constructor you are using to see if that constructor will ignore this function.

This function was introduced in Qt 4.4.

**See also** [defaultFormat](https://doc.qt.io/qt-5/qsettings.html#defaultFormat)() and [format](https://doc.qt.io/qt-5/qsettings.html#format)().

### void QSettings::setFallbacksEnabled(bool *b*)

Sets whether fallbacks are enabled to *b*.

By default, fallbacks are enabled.

**See also** [fallbacksEnabled](https://doc.qt.io/qt-5/qsettings.html#fallbacksEnabled)().

### void QSettings::setIniCodec([QTextCodec](https://doc.qt.io/qt-5/qtextcodec.html) **codec*)

Sets the codec for accessing INI files (including `.conf` files on Unix) to *codec*. The codec is used for decoding any data that is read from the INI file, and for encoding any data that is written to the file. By default, no codec is used, and non-ASCII characters are encoded using standard INI escape sequences.

**Warning:** The codec must be set immediately after creating the [QSettings](https://doc.qt.io/qt-5/qsettings.html) object, before accessing any data.

This function was introduced in Qt 4.5.

**See also** [iniCodec](https://doc.qt.io/qt-5/qsettings.html#iniCodec)().

### void QSettings::setIniCodec(const char **codecName*)

This is an overloaded function.

Sets the codec for accessing INI files (including `.conf` files on Unix) to the [QTextCodec](https://doc.qt.io/qt-5/qtextcodec.html) for the encoding specified by *codecName*. Common values for `codecName` include "ISO 8859-1", "UTF-8", and "UTF-16". If the encoding isn't recognized, nothing happens.

This function was introduced in Qt 4.5.

**See also** [QTextCodec::codecForName](https://doc.qt.io/qt-5/qtextcodec.html#codecForName)().

### `[static]`void QSettings::setPath([QSettings::Format](https://doc.qt.io/qt-5/qsettings.html#Format-enum) *format*, [QSettings::Scope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) *scope*, const [QString](https://doc.qt.io/qt-5/qstring.html) &*path*)

Sets the path used for storing settings for the given *format* and *scope*, to *path*. The *format* can be a custom format.

The table below summarizes the default values:

| Platform                                                     | Format                                                       | Scope                                                        | Path                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------- |
| Windows                                                      | [IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) | [UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `FOLDERID_RoamingAppData` |
| [SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `FOLDERID_ProgramData`                                       |                                                              |                           |
| Unix                                                         | [NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), [IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) | [UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `$HOME/.config`           |
| [SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `/etc/xdg`                                                   |                                                              |                           |
| Qt for Embedded Linux                                        | [NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum), [IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) | [UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `$HOME/Settings`          |
| [SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `/etc/xdg`                                                   |                                                              |                           |
| [macOS](https://doc.qt.io/qt-5/internationalization.html#macos) and iOS | [IniFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) | [UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `$HOME/.config`           |
| [SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) | `/etc/xdg`                                                   |                                                              |                           |

The default [UserScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) paths on Unix, [macOS](https://doc.qt.io/qt-5/internationalization.html#macos), and iOS (`$HOME/.config` or $HOME/Settings) can be overridden by the user by setting the `XDG_CONFIG_HOME` environment variable. The default [SystemScope](https://doc.qt.io/qt-5/qsettings.html#Scope-enum) paths on Unix, [macOS](https://doc.qt.io/qt-5/internationalization.html#macos), and iOS (`/etc/xdg`) can be overridden when building the Qt library using the `configure` script's `-sysconfdir` flag (see [QLibraryInfo](https://doc.qt.io/qt-5/qlibraryinfo.html) for details).

Setting the [NativeFormat](https://doc.qt.io/qt-5/qsettings.html#Format-enum) paths on Windows, [macOS](https://doc.qt.io/qt-5/internationalization.html#macos), and iOS has no effect.

**Warning:** This function doesn't affect existing [QSettings](https://doc.qt.io/qt-5/qsettings.html) objects.

This function was introduced in Qt 4.1.

**See also** [registerFormat](https://doc.qt.io/qt-5/qsettings.html#registerFormat)().

### void QSettings::setValue(const [QString](https://doc.qt.io/qt-5/qstring.html) &*key*, const [QVariant](https://doc.qt.io/qt-5/qvariant.html) &*value*)

Sets the value of setting *key* to *value*. If the *key* already exists, the previous value is overwritten.

Note that the Windows registry and INI files use case-insensitive keys, whereas the CFPreferences API on [macOS](https://doc.qt.io/qt-5/internationalization.html#macos) and iOS uses case-sensitive keys. To avoid portability problems, see the [Section and Key Syntax](https://doc.qt.io/qt-5/qsettings.html#section-and-key-syntax) rules.

Example:

```
QSettings settings;
settings.setValue("interval", 30);
settings.value("interval").toInt();     // returns 30

settings.setValue("interval", 6.55);
settings.value("interval").toDouble();  // returns 6.55
```

**See also** [value](https://doc.qt.io/qt-5/qsettings.html#value)(), [remove](https://doc.qt.io/qt-5/qsettings.html#remove)(), and [contains](https://doc.qt.io/qt-5/qsettings.html#contains)().

### [QSettings::Status](https://doc.qt.io/qt-5/qsettings.html#Status-enum) QSettings::status() const

Returns a status code indicating the first error that was met by [QSettings](https://doc.qt.io/qt-5/qsettings.html), or [QSettings::NoError](https://doc.qt.io/qt-5/qsettings.html#Status-enum) if no error occurred.

Be aware that [QSettings](https://doc.qt.io/qt-5/qsettings.html) delays performing some operations. For this reason, you might want to call [sync](https://doc.qt.io/qt-5/qsettings.html#sync)() to ensure that the data stored in [QSettings](https://doc.qt.io/qt-5/qsettings.html) is written to disk before calling status().

**See also** [sync](https://doc.qt.io/qt-5/qsettings.html#sync)().

### void QSettings::sync()

Writes any unsaved changes to permanent storage, and reloads any settings that have been changed in the meantime by another application.

This function is called automatically from [QSettings](https://doc.qt.io/qt-5/qsettings.html)'s destructor and by the event loop at regular intervals, so you normally don't need to call it yourself.

**See also** [status](https://doc.qt.io/qt-5/qsettings.html#status)().

### [QVariant](https://doc.qt.io/qt-5/qvariant.html) QSettings::value(const [QString](https://doc.qt.io/qt-5/qstring.html) &*key*, const [QVariant](https://doc.qt.io/qt-5/qvariant.html) &*defaultValue* = QVariant()) const

Returns the value for setting *key*. If the setting doesn't exist, returns *defaultValue*.

If no default value is specified, a default [QVariant](https://doc.qt.io/qt-5/qvariant.html) is returned.

Note that the Windows registry and INI files use case-insensitive keys, whereas the CFPreferences API on [macOS](https://doc.qt.io/qt-5/internationalization.html#macos) and iOS uses case-sensitive keys. To avoid portability problems, see the [Section and Key Syntax](https://doc.qt.io/qt-5/qsettings.html#section-and-key-syntax) rules.

Example:

```
QSettings settings;
settings.setValue("animal/snake", 58);
settings.value("animal/snake", 1024).toInt();   // returns 58
settings.value("animal/zebra", 1024).toInt();   // returns 1024
settings.value("animal/zebra").toInt();         // returns 0
```

**See also** [setValue](https://doc.qt.io/qt-5/qsettings.html#setValue)(), [contains](https://doc.qt.io/qt-5/qsettings.html#contains)(), and [remove](https://doc.qt.io/qt-5/qsettings.html#remove)().