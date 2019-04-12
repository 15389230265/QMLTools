[TOC]



# FileDialog QML Type



用于从本地文件系统中选择文件的Dialog组件。 [More...](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#details)

|                   |                            |
| ----------------- | -------------------------- |
| Import Statement: | import QtQuick.Dialogs 1.3 |
| Since:            | Qt 5.1                     |

- [List of all members, including inherited members](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog-members.html)





## Properties

- **defaultSuffix** : string
- **fileUrl** : url
- **fileUrls** : list<url>
- **folder** : url
- **modality** : Qt::WindowModality
- **nameFilters** : list<string>
- **selectExisting** : bool
- **selectFolder** : bool
- **selectMultiple** : bool
- **selectedNameFilter** : string
- **shortcuts** : Object
- **sidebarVisible** : bool
- **title** : string
- **visible** : bool





## Methods

- void **close**()
- void **open**()





## Detailed Description

[FileDialog](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html)提供了一个基本文件选择器：它允许用户选择现有文件和/或目录，或创建新文件名。该对话框最初是不可见的。您需要先根据需要设置属性，然后将[visible](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#visible-prop)设置为true或调用[open()](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#open-method)。

以下是打开文件对话框并在用户选择文件后退出的最小示例：

```
import QtQuick 2.2
import QtQuick.Dialogs 1.0

FileDialog {
    id: fileDialog
    title: "Please choose a file"
    folder: shortcuts.home
    onAccepted: {
        console.log("You chose: " + fileDialog.fileUrls)
        Qt.quit()
    }
    onRejected: {
        console.log("Canceled")
        Qt.quit()
    }
    Component.onCompleted: visible = true
}
```

一个[FileDialog的](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html)窗口会自动瞬变它的父窗口。因此，无论是在[Item内](https://doc.qt.io/qt-5/qml-qtquick-item.html)还是在[Window](https://doc.qt.io/qt-5/qml-qtquick-window-window.html)内声明对话框，对话框都将显示在包含该项的窗口的中心，或者在您声明的Window上。

如果可能，[FileDialog](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html)的实现将是一个平台文件对话框。如果那是不可能的，那么它将尝试实例化[QFileDialog](https://doc.qt.io/qt-5/qfiledialog.html)。如果这也不可能，那么它将回退到QML实现DefaultFileDialog.qml。在这种情况下，您可以通过编辑此文件来自定义外观。DefaultFileDialog.qml包含一个Rectangle来保存对话框的内容，因为某些嵌入式系统不支持多个顶级窗口。当对话框变为可见时，如果可能，它将自动包装在窗口中，或者如果只能有一个窗口，则只需在主窗口的顶部进行重新设置。

QML实现有一个侧边栏，包含通用平台特定位置的[快捷方式](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#shortcuts-prop)和用户可修改的收藏夹。它使用特定于应用程序的[设置](https://doc.qt.io/qt-5/qt-labs-settings-qmlmodule.html)来存储用户的收藏夹以及其他用户可修改的状态，例如是否显示侧边栏，分割器的位置以及对话框大小。这些设置存储在一个名为`QQControlsFileDialog`特定于应用程序的[QSettings的部分中](https://doc.qt.io/qt-5/qsettings.html)。例如，在使用qml工具测试应用程序时，`QQControlsFileDialog`将在`Qml Runtime`设置文件（或注册表项）中创建该部分。如果通过自定义C ++ main（）函数启动应用程序，建议设置[名称](https://doc.qt.io/qt-5/qcoreapplication.html#applicationName-prop)，[组织](https://doc.qt.io/qt-5/qcoreapplication.html#organizationName-prop)和[域](https://doc.qt.io/qt-5/qcoreapplication.html#organizationDomain-prop)以控制应用程序设置的位置。如果在应用程序的其他部分中使用[“设置”](https://doc.qt.io/qt-5/qt-labs-settings-qmlmodule.html)对象，则它们将存储在同一文件的其他部分中。

[QFileDialog](https://doc.qt.io/qt-5/qfiledialog.html)全局存储其设置而不是每个应用程序。平台本机文件对话框可能会或可能不会以各种与平台相关的方式存储设置。

## Property Documentation



#### defaultSuffix : [string](https://doc.qt.io/qt-5/qml-string.html)

如果未指定其他后缀，则此属性保留添加到文件名的后缀。

此属性指定一个字符串，如果它没有后缀，将添加到文件名中。后缀通常用于指示文件类型（例如“txt”表示文本文件）。

如果第一个字符是点（'.'），则将其删除。

该属性在Qt 5.10中引入。



#### fileUrl : [url](https://doc.qt.io/qt-5/qml-url.html)

用户选择的文件路径。

**注意：**仅当选择了一个文件时才设置此属性。在所有其他情况下，它将是空的。

**See also** [fileUrls](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#fileUrls-prop).



#### fileUrls : [list](https://doc.qt.io/qt-5/qml-list.html) < [url](https://doc.qt.io/qt-5/qml-url.html) >

用户选择的文件路径列表。



#### folder : [url](https://doc.qt.io/qt-5/qml-url.html)

当前所选文件夹的路径。在调用[open（）](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#open-method)之前设置此属性将导致文件浏览器最初位于指定的文件夹上。

关闭对话框后，还会更新此属性的值。

默认情况下，网址为空。

**注意：**在iOS上，如果将*文件夹*设置为[shortcuts.pictures](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#shortcuts-prop)，则将使用本机图像选取器对话框来访问用户的相册。返回的URL可以设置为[Image的](https://doc.qt.io/qt-5/qml-qtquick-image.html)[源](https://doc.qt.io/qt-5/qml-qtquick-image.html#source-prop)。要启用此功能，分配给项目文件中的QMAKE_INFO_PLIST的Info.plist必须包含密钥NSPhotoLibraryUsageDescription。有关此密钥的更多信息，请参阅Apple的Info.plist文档。此功能已在Qt 5.5中添加。

**See also** [shortcuts](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#shortcuts-prop).



#### modality : Qt::WindowModality

是否应该相对于包含对话框的父项，相对于整个应用程序的模态或非模态的窗口模式显示对话框。
默认情况下是`Qt.WindowModal`。
模态并不意味着有任何阻塞调用等待接受或拒绝对话; 只是防止用户同时与父窗口和/或应用程序窗口进行交互。您可能需要编写onAccepted处理程序来实际加载或保存所选文件。



#### nameFilters : [list](https://doc.qt.io/qt-5/qml-list.html)< [string](https://doc.qt.io/qt-5/qml-string.html) >

要用作文件名过滤器的字符串列表。每个字符串可以是以空格分隔的过滤器列表; 过滤器可能包括？和*通配符。过滤器列表也可以括在括号中，并且可以提供过滤器的文本描述。

例如：

```
FileDialog {
    nameFilters: [ "Image files (*.jpg *.png)", "All files (*)" ]
}
```

**注意：**过滤器不会排除目录。

**See also** [selectedNameFilter](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#selectedNameFilter-prop).



#### selectExisting : [bool](https://doc.qt.io/qt-5/qml-bool.html)

是否只能选择现有文件或目录。

默认情况下，此属性为true。在打开对话框之前，必须将此属性设置为所需的值。将此属性设置为false意味着该对话框用于命名要保存内容的文件，或命名要创建的文件夹; 因此[selectMultiple](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#selectMultiple-prop)必须为false。



#### selectFolder : [bool](https://doc.qt.io/qt-5/qml-bool.html)

所选项目是否应为文件夹。

默认情况下，此属性为false。在打开对话框之前，必须将此属性设置为所需的值。将此属性设置为true意味着[selectMultiple](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#selectMultiple-prop)必须为false，并且[selectExisting](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#selectExisting-prop)必须为true。



#### selectMultiple : [bool](https://doc.qt.io/qt-5/qml-bool.html)

是否可以选择多个文件名。

默认情况下，此属性为false。在打开对话框之前，必须将此属性设置为所需的值。将此属性设置为true意味着[selectExisting](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#selectExisting-prop)必须为true。



#### selectedNameFilter : [string](https://doc.qt.io/qt-5/qml-string.html)

目前选择了哪个[nameFilters](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#nameFilters-prop)。

可以在对话框可见之前设置此属性，设置默认名称过滤器，也可以在对话框可见时设置此属性以设置当前名称过滤器。当用户选择不同的过滤器时，它也会更新。



#### shortcuts : Object

从[QStandardPaths](https://doc.qt.io/qt-5/whatsnew50.html#qstandardpaths)到其URL 的一些有用路径的映射。在创建[FileDialog](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html)时，每个路径都会在添加到此列表之前验证是否存在于用户的计算机上。

|             |                                                              |                                                              |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `desktop`   | [QStandardPaths::DesktopLocation](https://doc.qt.io/qt-5/qstandardpaths.html#StandardLocation-enum) | 用户的桌面目录。                                             |
| `documents` | [QStandardPaths::DocumentsLocation](https://doc.qt.io/qt-5/qstandardpaths.html#StandardLocation-enum) | 包含用户文档文件的目录。                                     |
| `home`      | [QStandardPaths::HomeLocation](https://doc.qt.io/qt-5/qstandardpaths.html#StandardLocation-enum) | 用户的主目录。                                               |
| `music`     | [QStandardPaths::MusicLocation](https://doc.qt.io/qt-5/qstandardpaths.html#StandardLocation-enum) | 包含用户音乐或其他音频文件的目录。                           |
| `movies`    | [QStandardPaths::MoviesLocation](https://doc.qt.io/qt-5/qstandardpaths.html#StandardLocation-enum) | 包含用户电影和视频的目录。                                   |
| `pictures`  | [QStandardPaths::PicturesLocation](https://doc.qt.io/qt-5/qstandardpaths.html#StandardLocation-enum) | 包含用户图片或照片的目录。它始终是一种`file`:URL; 但在某些平台上，它将是专用的，因此[FileDialog](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html)将被实现为图库浏览器对话框。 |

例如，`shortcuts.home`将提供用户主目录的URL。

该属性在Qt 5.5中引入。



#### sidebarVisible : [bool](https://doc.qt.io/qt-5/qml-bool.html)

此属性保存包含快捷方式和书签的对话框中的侧边栏是否可见。默认情况下，它取决于`QQControlsFileDialog`应用程序[设置](https://doc.qt.io/qt-5/qt-labs-settings-qmlmodule.html)部分中存储的[设置](https://doc.qt.io/qt-5/qt-labs-settings-qmlmodule.html)。

该属性在Qt 5.4中引入。



#### title : [string](https://doc.qt.io/qt-5/qml-string.html)

对话框窗口的标题。



#### visible : [bool](https://doc.qt.io/qt-5/qml-bool.html)

此属性保存对话框是否可见。默认情况下，是false。

**See also** [modality](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#modality-prop).



## Method Documentation



#### void close()

关闭对话框。



#### void open()

显示用户的对话框。它相当于将[visible](https://doc.qt.io/qt-5/qml-qtquick-dialogs-filedialog.html#visible-prop)设置为true。





# FileDialog QML Type



本机文件对话框。[More...](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#details)

|                   |                                                              |
| ----------------- | ------------------------------------------------------------ |
| Import Statement: | import Qt.labs.platform 1.0                                  |
| Since:            | Qt 5.8                                                       |
| Inherits:         | [Dialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html) |

- [List of all members, including inherited members](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog-members.html)



## Properties

- **acceptLabel** : string
- **currentFile** : url
- **currentFiles** : list<url>
- **defaultSuffix** : string
- **file** : url
- **fileMode** : enumeration
- **files** : list<url>
- **folder** : url
- **nameFilters** : list<string>
- **options** : flags
- **rejectLabel** : string
- **selectedNameFilter**
  - **selectedNameFilter.index** : int
  - **selectedNameFilter.name** : string
  - **selectedNameFilter.extensions** : list<string>





## Detailed Description

该[FileDialog的](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html)类型提供本机平台文件对话框一个QML API。

![img](QML打开文件对话框.assets/qtlabsplatform-filedialog-gtk.png)

要显示文件对话框，请构造[FileDialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html)的实例，设置所需的属性，然后调用[open()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#open-method)。所述[currentFile](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFile-prop)或[currentFiles](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFiles-prop)属性可以被用来确定在对话框中当前选中的文件（多个）。该[文件](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#file-prop)与[多文件](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#files-prop)最后的选择已经通过接受对话仅取得后属性更新。

```c++
MenuItem {
    text: "Open..."
    onTriggered: fileDialog.open()
}

FileDialog {
    id: fileDialog
    currentFile: document.source
    folder: StandardPaths.writableLocation(StandardPaths.DocumentsLocation)
}

MyDocument {
    id: document
    source: fileDialog.file
}
```



### Availability

目前，以下平台上提供了本机平台文件对话框：

- iOS
- Linux (when running with the GTK+ platform theme)
- macOS
- Windows
- [WinRT](https://doc.qt.io/qt-5.9/build-sources.html#winrt)

Qt实验室平台模块使用Qt Widgets作为没有可用本机实现的平台的后备。因此，使用Qt Labs Platform模块中的类型的应用程序应链接到[QtWidgets](https://doc.qt.io/qt-5.9/qtwidgets-module.html)并使用[QApplication](https://doc.qt.io/qt-5.9/qapplication.html)而不是[QGuiApplication](https://doc.qt.io/qt-5.9/qguiapplication.html)。

要链接到[QtWidgets](https://doc.qt.io/qt-5.9/qtwidgets-module.html)库，请将以下内容添加到qmake项目文件中：

```
QT += widgets
```

创建实例[的QApplication](https://doc.qt.io/qt-5.9/qapplication.html)在`main()`：

```c++
#include <QApplication>
#include <QQmlApplicationEngine>

int main(int argc, char *argv[])
{
    QApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
    QApplication app(argc, argv);
    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));
    return app.exec();
}
```

**注意：**Qt.labs模块中的类型不保证在将来的版本中保持兼容。

**See also** [FolderDialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html) and [StandardPaths](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-standardpaths.html).



## Property Documentation



#### acceptLabel : string

此属性保存接受对话框的按钮上显示的标签文本。

设置为空字符串时，将使用基础平台的默认标签。默认标签通常是“ **打开”**或“ **保存”，**具体取决于使用该对话框的[fileMode](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#fileMode-prop)。

默认值为空字符串。

**See also** [rejectLabel](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#rejectLabel-prop).



#### currentFile : [url](https://doc.qt.io/qt-5.9/qml-url.html)

此属性保存对话框中当前选定的文件。

与[文件](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#file-prop)属性不同`currentFile`，即使在最终选择之前，用户在对话框中选择文件时也会更新属性。

**See also** [file](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#file-prop) and [currentFiles](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFiles-prop).



#### currentFiles : list< [url](https://doc.qt.io/qt-5.9/qml-url.html) >

此属性保存对话框中当前选定的多文件。

与[files](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#files-prop)属性不同`currentFiles`，即使在最终选择之前，用户在对话框中选择文件时也会更新属性。

**See also** [files](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#files-prop) and [currentFile](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFile-prop).



#### defaultSuffix : string

此属性包含一个后缀，该后缀将添加到未指定后缀的选定文件中。后缀通常用于指示文件类型（例如“txt”表示文本文件）。

如果第一个字符是点（'.'），则将其删除。



#### file : [url](https://doc.qt.io/qt-5.9/qml-url.html)

此属性包含最终接受的文件。

与[currentFile](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFile-prop)属性不同，`file`当用户在对话框中选择文件时，属性不会更新，但只有在最终选择完成后才会更新。也就是说，当用户单击“ **确定”**接受文件时。或者，可以处理[accepted()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#accepted-signal)信号以获得最终选择。

**See also** [currentFile](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFile-prop) and [accepted()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#accepted-signal).



#### fileMode : enumeration

此属性保存对话框的模式。

可用值：

| Constant               | Description                                |
| ---------------------- | ------------------------------------------ |
| `FileDialog.OpenFile`  | 该对话框用于选择现有文件（默认）。         |
| `FileDialog.OpenFiles` | 该对话框用于选择多个现有文件。             |
| `FileDialog.SaveFile`  | 该对话框用于选择任何文件。该文件不必存在。 |



#### files : list< [url](https://doc.qt.io/qt-5.9/qml-url.html) >

此属性包含最终接受的多文件。

与[currentFiles](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFiles-prop)属性不同，`files`当用户在对话框中选择文件时，属性不会更新，但只有在最终选择完成后才会更新。也就是说，当用户单击“ **确定”**接受文件时。或者，可以处理[accepted()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#accepted-signal)信号以获得最终选择。

**See also** [currentFiles](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#currentFiles-prop) and [accepted()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#accepted-signal).



#### folder : [url](https://doc.qt.io/qt-5.9/qml-url.html)

此属性包含选择文件的文件夹。要选择文件夹，请改用[FolderDialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html)。

**See also** [FolderDialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html).



#### nameFilters : list< string >

此属性包含限制可以选择的文件类型的过滤器。

```
FileDialog {
    nameFilters: ["Text files (*.txt)", "HTML files (*.html *.htm)"]
}
```

**Note:** ***.\*** 不是便携式过滤器，因为文件扩展名确定文件类型的历史假设在每个操作系统上都不一致。可以使文件名中没有点（例如，`Makefile`）。在本机Windows文件对话框中，***.\***将匹配此类文件，而在其他类型的文件对话框中，它可能不匹配。因此，如果您要选择任何文件，最好使用*****。

**See also** [selectedNameFilter](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#selectedNameFilter-prop).



#### options : flags

此属性包含影响对话框外观的各种选项。

默认情况下，禁用所有选项。

应在显示对话框之前设置选项。在对话框可见时设置它们不能保证立即对对话框产生影响（取决于选项和平台）。

Available options:

| Constant                           | Description                                                  |
| ---------------------------------- | ------------------------------------------------------------ |
| `FileDialog.DontResolveSymlinks`   | 不要在文件对话框中解析符号链接。默认情况下，符号链接已解析。 |
| `FileDialog.DontConfirmOverwrite`  | 如果选择了现有文件，请勿要求确认。默认情况下，请求确认。     |
| `FileDialog.ReadOnly`              | 表示该对话框不允许创建目录。                                 |
| `FileDialog.HideNameFilterDetails` | 指示是否隐藏文件名过滤器详细信息。                           |



#### rejectLabel : string

此属性保存拒绝对话框的按钮上显示的标签文本。

设置为空字符串时，将使用基础平台的默认标签。默认标签通常为“ **取消”**。

默认值为空字符串。

**See also** [acceptLabel](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#acceptLabel-prop).



#### selectedNameFilter group 
selectedNameFilter.index : int
selectedNameFilter.name : string
selectedNameFilter.extensions : list<string>

这些属性包含当前选定的名称过滤器。

| Name                                      | Description                                                  |
| :---------------------------------------- | :----------------------------------------------------------- |
| **index** : int                           | 此属性确定选择了哪个[名称过滤器](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#nameFilters-prop)。打开对话框时，将选择指定的过滤器。当用户选择另一个过滤器时，该值会更新。 |
| [read-only] **name** : string             | 此属性包含所选过滤器的名称。在下面的示例中，第一个过滤器的名称是`"Text files"`，第二个过滤器的名称是`"HTML files"`。 |
| [read-only] **extensions** : list<string> | 此属性包含所选过滤器的扩展名列表。在下面的示例中，第一个过滤器的扩展名列表是`["txt"]`第二个过滤器的扩展名列表`["html", "htm"]`。 |

```c++
FileDialog {
    id: fileDialog
    selectedNameFilter.index: 1
    nameFilters: ["Text files (*.txt)", "HTML files (*.html *.htm)"]
}

MyDocument {
    id: document
    fileType: fileDialog.selectedNameFilter.extensions[0]
}
```

**See also** [nameFilters](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html#nameFilters-prop).





------

# FolderDialog QML Type



本机文件夹对话框。 [More...](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#details)

| Import Statement: | import Qt.labs.platform 1.0                                  |
| ----------------- | ------------------------------------------------------------ |
| Since:            | Qt 5.8                                                       |
| Inherits:         | [Dialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html) |

- [List of all members, including inherited members](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog-members.html)





## Properties

- **acceptLabel** : string
- **currentFolder** : url
- **folder** : url
- **options** : flags
- **rejectLabel** : string





## Detailed Description

该[FolderDialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html)类型提供了原生的平台文件夹对话框一个QML API。

![img](QML打开文件对话框.assets/qtlabsplatform-folderdialog-gtk.png)

要显示文件夹对话框，请构造[FolderDialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html)的实例，设置所需的属性，然后调用[open()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#open-method)。该[currentFolder](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#currentFolder-prop)属性可以用来确定在对话框中当前选定的文件夹中。该[文件夹](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#folder-prop)最后的选择已经通过接受对话仅取得后属性更新。

```c++
MenuItem {
    text: "Open..."
    onTriggered: folderDialog.open()
}

FolderDialog {
    id: folderDialog
    currentFolder: viewer.folder
    folder: StandardPaths.standardLocations(StandardPaths.PicturesLocation)[0]
}

MyViewer {
    id: viewer
    folder: folderDialog.folder
}
```





### Availability

目前，以下平台上提供了本机平台文件夹对话框：

- iOS
- Linux (when running with the GTK+ platform theme)
- macOS
- Windows
- [WinRT](https://doc.qt.io/qt-5.9/build-sources.html#winrt)

Qt实验室平台模块使用Qt Widgets作为没有可用本机实现的平台的后备。因此，使用Qt Labs Platform模块中的类型的应用程序应链接到[QtWidgets](https://doc.qt.io/qt-5.9/qtwidgets-module.html)并使用[QApplication](https://doc.qt.io/qt-5.9/qapplication.html)而不是[QGuiApplication](https://doc.qt.io/qt-5.9/qguiapplication.html)。

要链接到[QtWidgets](https://doc.qt.io/qt-5.9/qtwidgets-module.html)库，请将以下内容添加到qmake项目文件中：

```
QT += widgets
```

创建实例[的QApplication](https://doc.qt.io/qt-5.9/qapplication.html)在`main()`：

```c++
#include <QApplication>
#include <QQmlApplicationEngine>

int main(int argc, char *argv[])
{
    QApplication::setAttribute(Qt::AA_EnableHighDpiScaling);
    QApplication app(argc, argv);
    QQmlApplicationEngine engine;
    engine.load(QUrl(QStringLiteral("qrc:/main.qml")));
    return app.exec();
}
```

**注意：** *Qt.labs模块中的类型不保证在将来的版本中保持兼容。*

**See also** [FileDialog](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-filedialog.html) and [StandardPaths](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-standardpaths.html).

## Property Documentation



#### acceptLabel : string

此属性保存接受对话框的按钮上显示的标签文本。

设置为空字符串时，将使用基础平台的默认标签。默认标签通常是“ **打开”**。

默认值为空字符串。

**See also** [rejectLabel](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#rejectLabel-prop).



#### currentFolder : [url](https://doc.qt.io/qt-5.9/qml-url.html)

此属性保存对话框中当前选定的文件夹。

与[文件夹](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#folder-prop)属性不同`currentFolder`，即使在最终选择之前，用户在对话框中选择文件夹时也会更新属性。

**See also** [folder](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#folder-prop).



#### folder : [url](https://doc.qt.io/qt-5.9/qml-url.html)

此属性包含最终接受的文件夹。

与[currentFolder](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#currentFolder-prop)属性不同，`folder`当用户在对话框中选择文件夹时，属性不会更新，但只有在最终选择完成后才会更新。也就是说，当用户单击“ **确定”**接受文件夹时。或者，可以处理[接受的()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#accepted-signal)信号以获得最终选择。

**See also** [currentFolder](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#currentFolder-prop) and [accepted()](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-dialog.html#accepted-signal).



#### options : flags

此属性包含影响对话框外观的各种选项。

默认情况下，禁用所有选项。

应在显示对话框之前设置选项。在对话框可见时设置它们不能保证立即对对话框产生影响（取决于选项和平台）。

Available options:

| Constant                           | Description                                                  |
| ---------------------------------- | ------------------------------------------------------------ |
| `FolderDialog.ShowDirsOnly`        | 仅显示文件夹对话框中的目录。默认情况下，会显示文件夹和目录。 |
| `FolderDialog.DontResolveSymlinks` | 不要在文件夹对话框中解析符号链接。默认情况下，符号链接已解析。 |
| `FolderDialog.ReadOnly`            | 表示该对话框不允许创建目录。                                 |



#### rejectLabel : string

此属性保存拒绝对话框的按钮上显示的标签文本。

设置为空字符串时，将使用基础平台的默认标签。默认标签通常为“ **取消”**。

默认值为空字符串。

**See also** [acceptLabel](https://doc.qt.io/qt-5.9/qml-qt-labs-platform-folderdialog.html#acceptLabel-prop).