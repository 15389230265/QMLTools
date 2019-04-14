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