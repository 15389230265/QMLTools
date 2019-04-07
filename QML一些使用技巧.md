**技巧对应于qt长期支持版本5.12.0**

------

[TOC]

------




# QML与C++交互的注意事项

[参考：QtQuick 系列教程之 QML 与 C++ 交互](<https://gitbook.cn/books/5acf2d97573c31562789c16a/index.html>) （强烈推荐阅读）


## 关于C++对象注册问题

### 方式1 ：C++ 对象注册到元对象系统

```c++
// main.cpp
QQmlApplicationEngine::rootContext()->setContextProperty(“name”, &Demo);
```

​	使用这个方法可以快速将C++类注册到qml元对象系统中，是方式二的简化版，效果类似于注册一个单例模块，这种方法简单，但没有版本号控制的功能，对于后续开发不利，不建议使用。

ps：建议使用智能指针的方式创建指向类的实例，可以避免内存错误

```c++
// QScopedPointer模版类可以指向一个动态分配内存的对象
QScopedPointer<Demo> myclass(new Demo());
```



### 方式2：C++ 对象注册到 Qml 系统

```c++
// demo.h
Class Demo : public QObject
{
    Q_OBJECT
    ...
}
```
```c++
//  main.cpp
qmlRegisterType＜Demo＞("com.mycompany", 1, 0, "name");
```

​	所用的C++类必须继承QObject，这种注册对象的方法带有版本控制，推荐使用。



### 方式3：C++对象注册一个单例组件到Qml系统

```c++
// main.cpp 使用匿名函数方式
qmlRegisterSingletonType<SingletonTypeExample>("Qt.example.qjsvalueApi", 1, 0, "MyApi", [](QQmlEngine *engine, QJSEngine *scriptEngine) -> QObject * {
    Q_UNUSED(engine)
    Q_UNUSED(scriptEngine)

    SingletonTypeExample *example = new SingletonTypeExample();
    return example;
});
```

```html
// main.qml
import QtQuick 2.0
import Qt.example.qobjectSingleton 1.0
Item {
    id: root
    property int someValue: MyApi.someProperty

    Component.onCompleted: {
        someValue = MyApi.doSomething()
    }
}
```

​	单例组件作用于整个程序中，当我们不需要多个实例化我们的自定义类时，这个方式十分有用。

------




# QML的MVC设计模式

## 说明

**Model-View-Controller**[(详情见qt文档)](<https://doc-snapshots.qt.io/qt5-5.11/qtquick-modelviewsdata-modelview.html>) [(文档2)](<https://doc.qt.io/qt-5/model-view-programming.html>)

![](assets/modelview-overview.png)

- **Model** - contains the data and its structure. There are several QML types for creating models.

- **View** - a container that displays the data. The view might display the data in a list or a grid.

- **Delegate** - dictates how the data should appear in the view. The delegate takes each data in the model and encapsulates it. The data is accessible through the delegate.

  ​	简单来说，MVC设计模式将数据本身的数据和格式于如何显示数据，控制数据的方法分开。模型便是我们要存放数据和格式的地方，QML提供了ListModel来存放一个链表模型，但大多数时候我们需要使用更高级的模型，因此使用QT提供的模型接口重写自己的模型类。视图便是显示数据模型的容器，定义了一些和模型相关的显示规则，QML中提供了ListView、GridView、PathView三种基于链表、网格和路径的显示方式，满足大部分显示需求，当然QT也提供了视图的重写接口来自定义视图。委托主要负责UI交互和数据模型的封装显示，委托中可以使用QML中的所有组件决定数据如何显示在视图中，也可以使用自定义的组件。

------



## Model

​	QML中提供的模型就不细说了，文档中说的十分清楚，这里主要说一下自定义模型类。所有的模型基于[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)类。该类定义视图和委托用于访问数据的接口。数据本身不必存储在模型中；它可以保存在由单独的类，文件，数据库或某些其他应用程序组件提供的数据结构或存储库中。

Qt提供了一些可用于处理数据项的现成模型：

- [QStringListModel](https://doc.qt.io/qt-5/qstringlistmodel.html)用于存储[QString](https://doc.qt.io/qt-5/qstring.html)项的简单列表。
- [QStandardItemModel](https://doc.qt.io/qt-5/qstandarditemmodel.html)管理更复杂的项目树结构，每个项目都可以包含任意数据。
- [QFileSystemModel](https://doc.qt.io/qt-5/qfilesystemmodel.html)提供有关本地文件系统中的文件和目录的信息。
- [QSqlQueryModel](https://doc.qt.io/qt-5/qsqlquerymodel.html)，[QSqlTableModel](https://doc.qt.io/qt-5/qsqltablemodel.html)和[QSqlRelationalTableModel](https://doc.qt.io/qt-5/qsqlrelationaltablemodel.html)用于使用模型/视图约定访问数据库。



### 模型类

​	在模型/视图体系结构中，模型提供了一个标准接口，用于查看和委托用于访问数据。在Qt中，标准接口由[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)类定义。无论数据项如何存储在任何底层数据结构中，[QAbstractItemModel的](https://doc.qt.io/qt-5/qabstractitemmodel.html)所有子类都将数据表示为包含项表的层次结构。视图使用此*约定*来访问模型中的数据项，但它们不会以向用户显示此信息的方式进行限制。

![](assets/modelview-models.png)

#### 模型索引

​	为了确保数据的表示与其访问方式分开，引入了*模型索引*的概念。可以通过模型获得的每条信息由模型索引表示。视图和委托使用这些索引来请求显示的数据项。

​	因此，只有模型需要知道如何获取数据，并且可以相当普遍地定义模型管理的数据类型。模型索引包含指向创建它们的模型的指针，这可以防止在处理多个模型时出现混淆。

```c++
QAbstractItemModel *model = index.model();
```

​	模型索引提供对信息的*临时*引用，并可用于通过模型检索或修改数据。由于模型可能会不时重新组织其内部结构，因此模型索引可能会变得无效，并且*不应存储*。如果需要对一条信息进行长期引用，则必须创建*持久模型索引*。这提供了模型保持最新信息的参考。临时模型索引由[QModelIndex](https://doc.qt.io/qt-5/qmodelindex.html)类提供，持久模型索引由[QPersistentModelIndex](https://doc.qt.io/qt-5/qpersistentmodelindex.html)类提供。

​	要获取与数据项对应的模型索引，必须为模型指定三个属性：行号，列号和父项的模型索引。以下部分详细描述和解释这些属性。

#### 行和列

​	在最基本的形式中，模型可以作为简单的表格访问，其中项目按行和列编号定位。*这并不意味着底层数据存储在数组结构中* ; 行号和列号的使用只是允许组件相互通信的惯例。我们可以通过为模型指定行和列号来检索有关任何给定项的信息，并且我们会收到表示该项的索引：

```c++
QModelIndex index = model->index(row, column, ...);
```

​	为链表和表格等简单的单级数据结构提供接口的模型不需要提供任何其他信息，但正如上面的代码所示，我们需要在获取模型索引时提供更多信息。

![](assets/modelview-tablemodel.png)

该图显示了基本表格模型，其中每个项由一对行号和列号定位。我们通过将相关的行和列号传递给模型来获得引用数据项的模型索引。

```c++
QModelIndex indexA = model->index(0, 0, QModelIndex());
QModelIndex indexB = model->index(1, 1, QModelIndex());
QModelIndex indexC = model->index(2, 1, QModelIndex());
```

通过指定`QModelIndex()`作为父项来始终引用模型中的顶级项。这将在下一节中讨论。

#### items的父项

​	当在表格或列表视图中使用数据时，模型提供的项目数据的表格式界面是理想的; 行和列编号系统完全映射到视图显示项目的方式。但是，树视图等结构要求模型为其中的项目公开更灵活的界面。因此，每个项目也可以是另一个项目表的父项，这与树视图中的顶级项目可以包含另一个项目列表的方式非常相似。

​	在请求模型项的索引时，我们必须提供有关项的父项的一些信息。在模型之外，引用项目的唯一方法是通过模型索引，因此还必须给出父模型索引：

```c++
QModelIndex index = model->index(row, column, parent);
```

![](assets/modelview-treemodel.png)

该图显示了树模型的表示，其中每个项由父项，行号和列号引用。

项目“A”和“C”表示为模型中的顶级兄弟：

```c++
QModelIndex indexA = model->index(0, 0, QModelIndex());
QModelIndex indexC = model->index(2, 1, QModelIndex());
```

项目“A”有许多孩子。使用以下代码获得项“B”的模型索引：

```c++
QModelIndex indexB = model->index(1, 0, indexA);
```

#### items的role(角色 ps:重要)

​	模型中的项目可以为其他组件执行各种*角色*，允许为不同的情况提供不同类型的数据。例如，[Qt :: DisplayRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)用于访问可在视图中显示为文本的字符串。通常，项包含许多不同角色的数据，标准角色由[Qt :: ItemDataRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)定义。

​	我们可以通过向模型传递与项目对应的模型索引，并通过指定角色来获取我们想要的数据类型来向模型询问项目的数据：

```c++
// QVariant是集合了大部分数据类型的类 QVariant类型的放入和取出必须是相对应的
QVariant value = model->data(index, role);
```

![](assets/modelview-roles.png)

该角色向模型指示正在引用哪种类型的数据。视图可以以不同方式显示角色，因此为每个角色提供适当的信息非常重要。

​	项目数据的最常见用途由[Qt :: ItemDataRole中](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)定义的标准角色涵盖。通过为每个角色提供适当的项目数据，模型可以向视图和代表提供有关如何向用户呈现项目的提示。不同类型的视图可以根据需要自由地解释或忽略此信息。还可以为特定于应用程序的目的定义其他角色。

#### 摘要

- 模型索引以独立于任何底层数据结构的方式提供有关模型提供的项目位置的视图和委托信息。
- 项目由行和列编号以及其父项的模型索引引用。
- 模型索引由模型根据其他组件的请求构建，例如视图和委托。
- 如果在使用[index()](https://doc.qt.io/qt-5/qabstractitemmodel.html#index)请求[索引](https://doc.qt.io/qt-5/qabstractitemmodel.html#index)时为父项指定了有效的模型索引，则返回的索引将引用模型中该父项下面的项。获得的索引是指该项目的子项。
- 如果在使用[index()](https://doc.qt.io/qt-5/qabstractitemmodel.html#index)请求[索引](https://doc.qt.io/qt-5/qabstractitemmodel.html#index)时为父项指定了无效的模型索引，则返回的索引将引用模型中的顶级项。
- 该[角色](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)区分与项目关联的不同类型的数据。

#### 使用模型索引

​	为了演示如何使用模型索引从模型中检索数据，我们设置了一个没有视图的[QFileSystemModel](https://doc.qt.io/qt-5/qfilesystemmodel.html)，并在窗口小部件中显示文件和目录的名称。虽然这没有显示使用模型的常规方法，但它演示了模型在处理模型索引时使用的约定。

​	我们通过以下方式构建文件系统模型：

```c++
    QFileSystemModel *model = new QFileSystemModel;
    QModelIndex parentIndex = model->index(QDir::currentPath());
    int numRows = model->rowCount(parentIndex);
```

​	在这种情况下，我们设置一个默认的[QFileSystemModel](https://doc.qt.io/qt-5/qfilesystemmodel.html)，使用该模型提供的[index()](https://doc.qt.io/qt-5/qfilesystemmodel.html#index)的特定实现获取父索引，并使用[rowCount()](https://doc.qt.io/qt-5/qfilesystemmodel.html#rowCount)函数计算模型中的行数。

​	为简单起见，我们只对模型第一列中的项目感兴趣。我们依次检查每一行，获取每行中第一项的模型索引，并读取模型中为该项存储的数据。

```c++
    for (int row = 0; row < numRows; ++row) {
        QModelIndex index = model->index(row, 0, parentIndex);
```

​	要获取模型索引，我们指定行号，列号（第一列为零）以及我们想要的所有项的父级的相应模型索引。使用模型的[data()](https://doc.qt.io/qt-5/qfilesystemmodel.html#data)函数检索存储在每个项目中的文本。我们指定模型索引和[DisplayRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)以字符串的形式获取项目的数据。

```c++
        QString text = model->data(index, Qt::DisplayRole).toString();
        // Display the text in a widget.

    }
```

上面的示例演示了用于从模型中检索数据的基本原则：

- 可以使用[rowCount()](https://doc.qt.io/qt-5/qabstractitemmodel.html#rowCount)和[columnCount()](https://doc.qt.io/qt-5/qabstractitemmodel.html#columnCount)找到模型的维度。这些函数通常需要指定父模型索引。
- 模型索引用于访问模型中的项目。指定项目需要行，列和父模型索引。
- 要访问模型中的顶级项，请将空模型索引指定为父索引`QModelIndex()`。
- 项目包含不同角色的数据。要获取特定角色的数据，必须将模型索引和角色都提供给模型。

------



## View

​	为不同类型的视图提供了完整的实现：[QListView](https://doc.qt.io/qt-5/qlistview.html)显示项目列表，[QTableView](https://doc.qt.io/qt-5/qtableview.html)显示表格中模型的数据，[QTreeView](https://doc.qt.io/qt-5/qtreeview.html)显示分层列表中的数据模型项目。这些类中的每一个都基于[QAbstractItemView](https://doc.qt.io/qt-5/qabstractitemview.html)抽象基类。虽然这些类是即用型实现，但它们也可以进行子类化以提供自定义视图。



### 概念

​	在模型/视图体系结构中，视图从模型中获取数据项并将其呈现给用户。呈现数据的方式不需要类似于模型提供的数据的表示，并且可以与用于存储数据项的基础数据结构*完全不同*。

内容和表示的分离是通过使用所提供的一种标准模型接口的实现化[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)，所提供的标准视图interface [QAbstractItemView](https://doc.qt.io/qt-5/qabstractitemview.html)，以及使用表示在一般的方式数据项模型索引。视图通常管理从模型获得的数据的整体布局。它们可以自己呈现单个数据项，或使用[委托](https://doc.qt.io/qt-5/model-view-programming.html#delegate-classes)来处理呈现和编辑功能。

​	除了显示数据外，视图还处理项目之间的导航以及项目选择的某些方面。视图还实现了基本的用户界面功能，例如上下文菜单和拖放。视图可以为项提供默认编辑工具，也可以与[委托](https://doc.qt.io/qt-5/model-view-programming.html#delegate-classes)一起提供自定义编辑器。

​	可以在没有模型的情况下构建视图，但必须先提供模型才能显示有用信息。视图通过使用[选项](https://doc.qt.io/qt-5/model-view-programming.html#handling-selections-in-item-views)来跟踪用户已选择的项目，这些[选择](https://doc.qt.io/qt-5/model-view-programming.html#handling-selections-in-item-views)可以针对每个视图单独维护，或者在多个视图之间共享。

​	某些视图（如[QTableView](https://doc.qt.io/qt-5/qtableview.html)和[QTreeView](https://doc.qt.io/qt-5/qtreeview.html)）显示标题和项目。这些也由视图类[QHeaderView实现](https://doc.qt.io/qt-5/qheaderview.html)。标头通常访问与包含它们的视图相同的模型。它们使用[QAbstractItemModel :: headerData](https://doc.qt.io/qt-5/qabstractitemmodel.html#headerData)（）函数从模型中检索数据，并且通常以标签的形式显示标题信息。可以从[QHeaderView](https://doc.qt.io/qt-5/qheaderview.html)类[继承](https://doc.qt.io/qt-5/qheaderview.html)新标头，以便为视图提供更专业的标签。



### 使用现有视图

​	Qt提供了三个即用型视图类，它们以大多数用户熟悉的方式呈现模型中的数据。[QListView](https://doc.qt.io/qt-5/qlistview.html)可以将模型中的项目显示为简单列表，也可以以经典图标视图的形式显示。[QTreeView](https://doc.qt.io/qt-5/qtreeview.html)将模型中的项显示为列表层次结构，允许以紧凑的方式表示深层嵌套结构。[QTableView](https://doc.qt.io/qt-5/qtableview.html)以表格的形式呈现模型中的项目，非常类似于电子表格应用程序的布局。

![img](assets/standard-views.png)

​	对于大多数应用程序，上面显示的标准视图的默认行为应该足够了。它们提供基本的编辑功能，并且可以进行定制以满足更专业的用户界面的需求。

#### 使用模型

​	我们将[创建字符串列表模型作为示例模型](https://doc.qt.io/qt-5/model-view-programming.html#creating-new-models)，使用一些数据进行设置，并构造一个视图以显示模型的内容。这可以在一个函数中执行：

```c++
int main(int argc, char *argv[])
{
    QApplication app(argc, argv);

// Unindented for quoting purposes:
QStringList numbers;
numbers << "One" << "Two" << "Three" << "Four" << "Five";

QAbstractItemModel *model = new StringListModel(numbers);
```

​	请注意，`StringListModel`被声明为[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)。这允许我们使用模型的抽象接口，并确保代码仍然有效，即使我们用不同的模型替换字符串列表模型。

​	[QListView](https://doc.qt.io/qt-5/qlistview.html)提供的列表视图足以显示字符串列表模型中的项目。我们构造视图，并使用以下代码行设置模型：

```c++
QListView *view = new QListView;
view->setModel(model);
```

​	视图以正常方式显示：

```c++
    view->show();
    return app.exec();
}
```

​	视图呈现模型的内容，通过模型的界面访问数据。当用户尝试编辑项目时，视图使用默认委托来提供编辑器窗口小部件。

![img](assets/stringlistmodel.png)

​	上图显示了[QListView](https://doc.qt.io/qt-5/qlistview.html)如何表示字符串列表模型中的数据。由于模型是可编辑的，因此视图会自动允许使用默认委托编辑列表中的每个项目。

#### 使用模型的多个视图

​	在同一模型上提供多个视图只需为每个视图设置相同的模型。在下面的代码中，我们创建了两个表视图，每个视图使用我们为此示例创建的相同的简单表模型：

```c++
    QTableView *firstTableView = new QTableView;
    QTableView *secondTableView = new QTableView;

    firstTableView->setModel(model);
    secondTableView->setModel(model);  
       
```

​	在模型/视图体系结构中使用信号和插槽意味着可以将对模型的更改传播到所有附加视图，从而确保无论使用何种视图，我们始终可以访问相同的数据。

![img](assets/sharedmodel-tableviews.png)

上图显示了同一模型的两个不同视图，每个视图包含许多选定的项目。虽然模型中的数据在视图中一致显示，但每个视图都维护自己的内部选择模型。这在某些情况下可能很有用，但对于许多应用程序，需要共享选择模型。



### 处理项目的选择

​	处理视图中项目选择的机制由[QItemSelectionModel](https://doc.qt.io/qt-5/qitemselectionmodel.html)类提供。默认情况下，所有标准视图都构建自己的选择模型，并以正常方式与它们进行交互。视图使用的选择模型可以通过[selectionModel()](https://doc.qt.io/qt-5/qabstractitemview.html#selectionModel)函数获得，并且可以使用[setSelectionModel()](https://doc.qt.io/qt-5/qabstractitemview.html#setSelectionModel)指定替换选择模型。当我们想要在同一模型数据上提供多个一致视图时，控制视图使用的选择模型的能力非常有用。

​	通常，除非您是模型或视图的子类，否则不需要直接操作选择的内容。但是，如果需要，可以访问选择模型的界面，这将在[项目视图中的处理选择](https://doc.qt.io/qt-5/model-view-programming.html#handling-selections-in-item-views)进行探讨。

#### 在视图之间共享选择

​	虽然默认情况下视图类提供自己的选择模型很方便，但是当我们在同一模型上使用多个视图时，通常希望模型的数据和用户的选择在所有视图中一致地显示。由于视图类允许替换其内部选择模型，因此我们可以使用以下行在视图之间实现统一选择：

```c++
    secondTableView->setSelectionModel(firstTableView->selectionModel());
```

​	第二个视图给出了第一个视图的选择模型。现在，两个视图都在相同的选择模型上运行，同时保持数据和所选项目的同步。

![img](assets/sharedselection-tableviews.png)

​	在上面显示的示例中，使用两个相同类型的视图来显示相同模型的数据。但是，如果使用两种不同类型的视图，则在每个视图中可以非常不同地表示所选项目; 例如，表视图中的连续选择可以表示为树视图中的一组分段突出显示的项目。

------



## Delegate

​	[QAbstractItemDelegate](https://doc.qt.io/qt-5/qabstractitemdelegate.html)是模型/视图框架中委托的抽象基类。默认委托实现由[QStyledItemDelegate](https://doc.qt.io/qt-5/qstyleditemdelegate.html)提供，这被Qt的标准视图用作默认委托。但是，[QStyledItemDelegate](https://doc.qt.io/qt-5/qstyleditemdelegate.html)和[QItemDelegate](https://doc.qt.io/qt-5/qitemdelegate.html)是绘制和为视图中的项目提供编辑器的独立替代方法。它们之间的区别在于[QStyledItemDelegate](https://doc.qt.io/qt-5/qstyleditemdelegate.html)使用当前样式来绘制其项目。因此，我们建议在实现自定义委托或使用Qt样式表时使用[QStyledItemDelegate](https://doc.qt.io/qt-5/qstyleditemdelegate.html)作为基类。



### 概念

​	与模型 - 视图 - 控制器模式不同，模型/视图设计不包括用于管理与用户的交互的完全独立的组件。通常，视图负责向用户呈现模型数据，以及处理用户输入。为了在获得此输入的方式中允许一些灵活性，交互由代表执行。这些组件提供输入功能，还负责在某些视图中呈现单个项目。用于控制委托的标准接口在[QAbstractItemDelegate](https://doc.qt.io/qt-5/qabstractitemdelegate.html)类中定义。

​	期望代表能够通过实现[paint()](https://doc.qt.io/qt-5/qitemdelegate.html#paint)和[sizeHint()](https://doc.qt.io/qt-5/qitemdelegate.html#sizeHint)函数来自己呈现它们的内容。但是，基于小部件的简单委托可以[继承QItemDelegate](https://doc.qt.io/qt-5/qitemdelegate.html)而不是[QAbstractItemDelegate](https://doc.qt.io/qt-5/qabstractitemdelegate.html)，并利用这些函数的默认实现。

​	可以通过使用小部件来管理编辑过程或直接处理事件来实现代表的编辑器。第一种方法将在本节后面介绍，它也会显示在[Spin Box Delegate](https://doc.qt.io/qt-5/qtwidgets-itemviews-spinboxdelegate-example.html)示例中。

​	该[Pixelator](https://doc.qt.io/qt-5/qtwidgets-itemviews-pixelator-example.html)示例演示如何创建执行专门用于渲染表视图自定义委托。



### 使用现有委托

​	Qt提供的标准视图使用[QItemDelegate的](https://doc.qt.io/qt-5/qitemdelegate.html)实例来提供编辑工具。委托接口的这个默认实现为每个标准视图呈现通常样式的项：[QListView](https://doc.qt.io/qt-5/qlistview.html)，[QTableView](https://doc.qt.io/qt-5/qtableview.html)和[QTreeView](https://doc.qt.io/qt-5/qtreeview.html)。

​	所有标准角色都由标准视图使用的默认委托处理。解释这些的方式在[QItemDelegate](https://doc.qt.io/qt-5/qitemdelegate.html)文档中描述。

​	视图使用的委托由[itemDelegate()](https://doc.qt.io/qt-5/qabstractitemview.html#itemDelegate)函数返回。该[setItemDelegate()](https://doc.qt.io/qt-5/qabstractitemview.html#setItemDelegate)功能可让您安装自定义委托的标准视图，并且有必要设置为代表的自定义视图时使用此功能。 



### 一个简单的代表

​	此处实现的委托使用[QSpinBox](https://doc.qt.io/qt-5/qspinbox.html)提供编辑功能，主要用于显示整数的模型。虽然我们为此目的设置了一个基于自定义整数的表模型，但我们可以轻松地使用[QStandardItemModel](https://doc.qt.io/qt-5/qstandarditemmodel.html)，因为自定义委托控制数据输入。我们构造一个表视图来显示模型的内容，这将使用自定义委托进行编辑。

![img](assets/spinboxdelegate-example.png)

我们从[QStyledItemDelegate继承](https://doc.qt.io/qt-5/qstyleditemdelegate.html)了委托，因为我们不想编写自定义显示函数。但是，我们仍然必须提供管理编辑器小部件的功能：

```c++
class SpinBoxDelegate : public QStyledItemDelegate
{
    Q_OBJECT

public:
    SpinBoxDelegate(QObject *parent = 0);

    QWidget *createEditor(QWidget *parent, const QStyleOptionViewItem &option,
                          const QModelIndex &index) const override;

    void setEditorData(QWidget *editor, const QModelIndex &index) const override;
    void setModelData(QWidget *editor, QAbstractItemModel *model,
                      const QModelIndex &index) const override;

    void updateEditorGeometry(QWidget *editor,
        const QStyleOptionViewItem &option, const QModelIndex &index) const override;
};         
```

请注意，构造委托时不会设置任何编辑器小部件。我们只在需要时构造一个编辑器小部件。

#### 提供编辑

​	在此示例中，当表视图需要提供编辑器时，它会要求委托者提供适合于正在修改的项的编辑器窗口小部件。所述[createEditor()](https://doc.qt.io/qt-5/qabstractitemdelegate.html#createEditor)函数与该委托需要能够设置一个合适的插件提供的一切：

```c++
QWidget *SpinBoxDelegate::createEditor(QWidget *parent,
    const QStyleOptionViewItem &/* option */,
    const QModelIndex &/* index */) const
{
    QSpinBox *editor = new QSpinBox(parent);
    editor->setFrame(false);
    editor->setMinimum(0);
    editor->setMaximum(100);

    return editor;
}
```

​	请注意，我们不需要保留指向编辑器小部件的指针，因为视图负责在不再需要时销毁它。

​	我们在编辑器上安装委托的默认事件过滤器，以确保它提供用户期望的标准编辑快捷方式。可以在编辑器中添加其他快捷方式，以允许更复杂的行为; 这些将在[编辑提示](https://doc.qt.io/qt-5/model-view-programming.html#editinghints)一节中讨论。

​	该视图通过调用稍后为此目的定义的函数来确保正确设置编辑器的数据和几何。我们可以根据视图提供的模型索引创建不同的编辑器。例如，如果我们有一列整数和一列字符串，我们可以返回一个`QSpinBox`或一个 `QLineEdit`，具体取决于正在编辑的列。

​	委托必须提供将模型数据复制到编辑器中的功能。在此示例中，我们读取存储在[显示角色中](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)的数据，并相应地设置复选框中的值。

```c++
void SpinBoxDelegate::setEditorData(QWidget *editor,
                                    const QModelIndex &index) const
{
    int value = index.model()->data(index, Qt::EditRole).toInt();

    QSpinBox *spinBox = static_cast<QSpinBox*>(editor);
    spinBox->setValue(value);
}
```

​	在这个例子中，我们知道编辑器小部件是一个复选框，但我们可以为模型中的不同类型的数据提供不同的编辑器，在这种情况下，我们需要在访问其成员函数之前将小部件转换为适当的类型。

#### 将数据提交给模型

​	当用户在复选框中完成编辑值后，视图会要求委托通过调用[setModelData()](https://doc.qt.io/qt-5/qabstractitemdelegate.html#setModelData)函数将编辑后的值存储在模型中。

```c++
void SpinBoxDelegate::setModelData(QWidget *editor, QAbstractItemModel *model,
                                   const QModelIndex &index) const
{
    QSpinBox *spinBox = static_cast<QSpinBox*>(editor);
    spinBox->interpretText();
    int value = spinBox->value();

    model->setData(index, value, Qt::EditRole);
}
```

​	由于视图管理委托的编辑器小部件，我们只需要使用提供的编辑器内容更新模型。在这种情况下，我们确保复选框是最新的，并使用指定的索引使用它包含的值更新模型。

​	标准[QItemDelegate](https://doc.qt.io/qt-5/qitemdelegate.html)类通过发出[closeEditor()](https://doc.qt.io/qt-5/qabstractitemdelegate.html#closeEditor)信号在完成编辑时通知视图。该视图可确保关闭和销毁编辑器窗口小部件。在这个例子中，我们只提供简单的编辑工具，所以我们不需要发出这个信号。

​	对数据的所有操作都是通过[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)提供的接口执行的。这使得委托主要独立于它操作的数据类型，但必须做出一些假设才能使用某些类型的编辑器小部件。在此示例中，我们假设模型始终包含整数值，但我们仍然可以将此委托用于不同类型的模型，因为[QVariant](https://doc.qt.io/qt-5/qvariant.html)为意外数据提供合理的默认值。

#### 更新编辑器的几何体

代表负责管理编辑器的几何体。创建编辑器时以及更改项目在视图中的大小或位置时，必须设置几何。幸运的是，视图在[视图选项](https://doc.qt.io/qt-5/qstyleoptionviewitem.html)对象中提供了所有必需的几何信息。

```c++
void SpinBoxDelegate::updateEditorGeometry(QWidget *editor,
    const QStyleOptionViewItem &option, const QModelIndex &/* index */) const
{
    editor->setGeometry(option.rect);
}
          
```

在这种情况下，我们只使用项目矩形中的视图选项提供的几何信息。呈现具有多个元素的项目的委托不会直接使用项目矩形。它将编辑器与项目中的其他元素相关联。

#### 编辑提示

​	编辑后，代理应该向其他组件提供有关编辑过程结果的提示，并提供有助于任何后续编辑操作的提示。这是通过用适当的提示发出[closeEditor()](https://doc.qt.io/qt-5/qabstractitemdelegate.html#closeEditor)信号来实现的。这由我们在构造时在复选框上安装的默认[QItemDelegate](https://doc.qt.io/qt-5/qitemdelegate.html)事件过滤器来处理。

​	可以调整复选框的行为以使其更加用户友好。在[QItemDelegate](https://doc.qt.io/qt-5/qitemdelegate.html)提供的默认事件过滤器中，如果用户点击**Return**以在复选框中确认他们的选择，则委托将值提交给模型并关闭复选框。我们可以通过在复选框上安装我们自己的事件过滤器来改变这种行为，并提供适合我们需求的编辑提示; 例如，我们可以使用[EditNextItem](https://doc.qt.io/qt-5/qabstractitemdelegate.html#EndEditHint-enum)提示发出[closeEditor()](https://doc.qt.io/qt-5/qabstractitemdelegate.html#closeEditor)，以自动开始编辑视图中的下一个项目。

​	另一种不需要使用事件过滤器的方法是提供我们自己的编辑器小部件，为了方便起见，可能是子类化[QSpinBox](https://doc.qt.io/qt-5/qspinbox.html)。这种替代方法可以让我们更好地控制编辑器窗口小部件的行为，但代价是编写其他代码。如果需要自定义标准Qt编辑器窗口小部件的行为，通常更容易在委托中安装事件过滤器。

​	代理不必发出这些提示，但那些不会发布这些提示的内容将更少地集成到应用程序中，并且不如那些发出提示来支持常见编辑操作的那些提示。

------



## 处理项目视图中的选择

### 概念

项视图类中使用的选择模型基于模型/视图体系结构的设施提供选择的一般描述。虽然操作选择的标准类对于提供的项目视图已足够，但选择模型允许您创建专门的选择模型以满足您自己的项目模型和视图的要求。

有关视图中所选项目的信息存储在[QItemSelectionModel](https://doc.qt.io/qt-5/qitemselectionmodel.html)类的实例中。这维护单个模型中项目的模型索引，并且独立于任何视图。由于模型上可以有很多视图，因此可以在视图之间共享选择，允许应用程序以一致的方式显示多个视图。

选择由*选择范围组成*。通过仅记录每个所选项目范围的起始和结束模型索引，这些有效地维护关于项目的大选择的信息。通过使用多个选择范围来描述选择来构造非连续的项目选择。

选择应用于由选择模型持有的模型索引的集合。应用的最新项目选择称为*当前选择*。即使在通过使用某些类型的选择命令进行应用之后，也可以修改该选择的效果。这些将在本节后面讨论。

#### 当前项目和所选项目

在视图中，始终存在当前项目和所选项目 - 两个独立状态。项目可以是当前项目并同时选择。该视图负责确保始终存在当前项目作为键盘导航，例如，需要当前项目。

下表突出显示了当前项目和所选项目之间的差异。

| 当前项目                                                     | 精选项目                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 只能有一个当前项目。                                         | 可以有多个选定的项目。                                       |
| 将使用键导航或鼠标按钮单击更改当前项目。                     | 当用户与项目交互时，取决于若干预定义模式（例如，单个选择，多个选择等），设置或取消设置所选项目状态。 |
| 如果按下编辑键**F2**，或者双击该项目（如果已启用编辑），则将编辑当前项目。 | 当前项目可以与锚点一起使用，以指定应该选择或取消选择的范围（或两者的组合）。 |
| 当前项目由焦点矩形指示。                                     | 所选项目用选择矩形表示。                                     |

在操作选择时，将[QItemSelectionModel](https://doc.qt.io/qt-5/qitemselectionmodel.html)视为项目模型中所有项目的选择状态的记录通常是有帮助的。一旦设置了选择模型，就可以选择，取消选择项目集合，或者可以切换它们的选择状态而无需知道已经选择了哪些项目。可以随时检索所有选定项目的索引，并且可以通过信号和槽机制向其他组件通知选择模型的变化。





### 使用选择模型

标准视图类提供可在大多数应用程序中使用的默认选择模型。可以使用视图的[selectionModel（）](https://doc.qt.io/qt-5/qabstractitemview.html#selectionModel)函数获取属于一个视图的选择模型，并使用[setSelectionModel（）](https://doc.qt.io/qt-5/qabstractitemview.html#setSelectionModel)在许多视图之间共享，因此通常不需要构造新的选择模型。

通过为[QItemSelection](https://doc.qt.io/qt-5/qitemselection.html)指定模型和一对模型索引来创建[选择](https://doc.qt.io/qt-5/qitemselection.html)。这使用索引来引用给定模型中的项目，并将它们解释为所选项目块中的左上角和右下角项目。要将选择应用于模型中的项目，需要将选择提交给选择模型; 这可以通过多种方式实现，每种方式对选择模型中已存在的选择具有不同的影响。

#### 选择项目

为了演示选择的一些主要特性，我们构造了一个包含总共32个项目的自定义表模型的实例，并在其数据上打开一个表视图：

```c++
    TableModel *model = new TableModel(8, 4, &app);

    QTableView *table = new QTableView(0);
    table->setModel(model);

    QItemSelectionModel *selectionModel = table->selectionModel();
```

检索表视图的默认选择模型以供以后使用。我们不修改模型中的任何项目，而是选择视图将显示在表格左上角的几个项目。为此，我们需要检索与要选择的区域中的左上角和右下角项对应的模型索引：

```c++
    QModelIndex topLeft;
    QModelIndex bottomRight;

    topLeft = model->index(0, 0, QModelIndex());
    bottomRight = model->index(5, 2, QModelIndex());
```

要在模型中选择这些项，并在表视图中查看相应的更改，我们需要构造一个选择对象，然后将其应用于选择模型：

```c++
    QItemSelection selection(topLeft, bottomRight);
    selectionModel->select(selection, QItemSelectionModel::Select);
```

使用由[选择标志](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)的组合定义的命令将选择应用于选择模型。在这种情况下，所使用的标志使得选择对象中记录的项目被包括在选择模型中，而不管它们之前的状态如何。视图显示结果选择。

![img](assets/selected-items1.png)

可以使用由选择标志定义的各种操作来修改项目的选择。由这些操作产生的选择可能具有复杂的结构，但它由选择模型有效地表示。当我们检查如何更新选择时，将描述使用不同的选择标志来操作所选项。

#### 阅读选择状态

可以使用[selectedIndexes()](https://doc.qt.io/qt-5/qitemselectionmodel.html#selectedIndexes)函数读取存储在选择模型中的模型索引。这将返回一个未排序的模型索引列表，只要我们知道它们适用于哪个模型，我们就可以迭代它们：

```c++
    QModelIndexList indexes = selectionModel->selectedIndexes();
    QModelIndex index;

    foreach(index, indexes) {
        QString text = QString("(%1,%2)").arg(index.row()).arg(index.column());
        model->setData(index, text);
    }
```

上面的代码使用Qt方便的[foreach关键字](https://doc.qt.io/qt-5/containers.html)来迭代和修改与选择模型返回的索引相对应的项。

选择模型发出信号以指示选择的变化。这些通知其他组件有关整个选择和项目模型中当前关注项目的更改。我们可以将[selectionChanged()](https://doc.qt.io/qt-5/qitemselectionmodel.html#selectionChanged)信号连接到一个槽，并在选择更改时检查模型中被选中或取消选择的项。使用两个[QItemSelection](https://doc.qt.io/qt-5/qitemselection.html)对象调用该槽：一个包含与新选择的项对应的索引列表; 另一个包含与新取消选择的项对应的索引。

在下面的代码中，我们提供了一个接收[selectionChanged（）](https://doc.qt.io/qt-5/qitemselectionmodel.html#selectionChanged)信号的槽，用字符串填充所选项，并清除取消选择的项的内容。

```c++
void MainWindow::updateSelection(const QItemSelection &selected,
    const QItemSelection &deselected)
{
    QModelIndex index;
    QModelIndexList items = selected.indexes();

    foreach (index, items) {
        QString text = QString("(%1,%2)").arg(index.row()).arg(index.column());
        model->setData(index, text);
    }

    items = deselected.indexes();

    foreach (index, items)
        model->setData(index, "");
}
```

我们可以通过将[currentChanged（）](https://doc.qt.io/qt-5/qitemselectionmodel.html#currentChanged)信号连接到使用两个模型索引调用的插槽来跟踪当前关注的项目。这些对应于先前关注的项目和当前关注的项目。

在下面的代码中，我们提供了一个接收[currentChanged（）](https://doc.qt.io/qt-5/qitemselectionmodel.html#currentChanged)信号的槽，并使用提供的信息来更新[QMainWindow](https://doc.qt.io/qt-5/qmainwindow.html)的状态栏：

```c++
void MainWindow::changeCurrent(const QModelIndex &current,
    const QModelIndex &previous)
{
    statusBar()->showMessage(
        tr("Moved from (%1,%2) to (%3,%4)")
            .arg(previous.row()).arg(previous.column())
            .arg(current.row()).arg(current.column()));
}
```

使用这些信号监视用户所做的选择很简单，但我们也可以直接更新选择模型。

#### 更新选择

选择命令由[QItemSelectionModel :: SelectionFlag](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)定义的选择标志的组合提供。每个选择标志告诉选择模型在调用任一[select()](https://doc.qt.io/qt-5/qitemselection.html#select)函数时如何更新其所选项的内部记录。最常用的标志是[Select](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)标志，它指示选择模型将指定的项目记录为被选中。在[切换](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)标志导致选择模型来反转规定的项目，状态，选择给出的任何取消选择的项目，并取消选择任何当前所选项目。该[取消](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)标志取消选择所有指定项目。

通过创建项目选择并将其应用于选择模型来更新选择模型中的各个项目。在下面的代码中，我们将第二个项目选择应用于上面显示的表模型，使用[Toggle](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)命令反转给定项目的选择状态。

```c++
    QItemSelection toggleSelection;

    topLeft = model->index(2, 1, QModelIndex());
    bottomRight = model->index(7, 3, QModelIndex());
    toggleSelection.select(topLeft, bottomRight);

    selectionModel->select(toggleSelection, QItemSelectionModel::Toggle);
```

此操作的结果显示在表格视图中，提供了一种方便的方式来可视化我们已实现的目标：

![img](assets/selected-items2.png)

默认情况下，选择命令仅对模型索引指定的各个项进行操作。但是，用于描述选择命令的标志可以与其他标志组合以更改整个行和列。例如，如果仅使用一个索引调用[select()](https://doc.qt.io/qt-5/qitemselectionmodel.html#select-1)，但使用的是[Select](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)和[Rows](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)组合的命令，则会[选择](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)包含所引用项的整个行。以下代码演示了[行](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)和[列](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)标志的使用：

```c++
    QItemSelection columnSelection;

    topLeft = model->index(0, 1, QModelIndex());
    bottomRight = model->index(0, 2, QModelIndex());

    columnSelection.select(topLeft, bottomRight);

    selectionModel->select(columnSelection,
        QItemSelectionModel::Select | QItemSelectionModel::Columns);

    QItemSelection rowSelection;

    topLeft = model->index(0, 0, QModelIndex());
    bottomRight = model->index(1, 0, QModelIndex());

    rowSelection.select(topLeft, bottomRight);

    selectionModel->select(rowSelection,
        QItemSelectionModel::Select | QItemSelectionModel::Rows);
```

虽然只有四个索引提供给选择模型，但使用[Columns](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)和[Rows](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)选择标志意味着选择了两列和两行。下图显示了这两个选项的结果：

![img](assets/selected-items3.png)

在示例模型上执行的命令都涉及在模型中累积选择的项目。也可以清除选择，或用新的选择替换当前选择。

要用新选择替换当前选择，请将其他选择标志与[Current](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)标志组合。使用此标志的命令指示选择模型将其当前模型索引集合替换为[select（）](https://doc.qt.io/qt-5/qitemselectionmodel.html#select-1)调用中指定的那些。要在开始添加新选项之前清除所有选择，请将其他选择标志与[清除](https://doc.qt.io/qt-5/qitemselectionmodel.html#SelectionFlag-enum)标志组合。这具有重置选择模型的模型索引集合的效果。

#### 选择模型中的所有项目

要选择模型中的所有项目，必须为模型的每个级别创建一个选择，以覆盖该级别中的所有项目。我们通过使用给定的父索引检索与左上角和右下角项对应的索引来执行此操作：

```c++
    QModelIndex topLeft = model->index(0, 0, parent);
    QModelIndex bottomRight = model->index(model->rowCount(parent)-1,
        model->columnCount(parent)-1, parent);
    
```

使用这些索引和模型构建选择。然后在选择模型中选择相应的项目：

```c++
    QItemSelection selection(topLeft, bottomRight);
    selectionModel->select(selection, QItemSelectionModel::Select);
```

这需要在模型中的所有级别执行。对于顶级项目，我们将以通常的方式定义父索引：

```c++
    QModelIndex parent = QModelIndex();
```

对于分层模型，[hasChildren()](https://doc.qt.io/qt-5/qabstractitemmodel.html#hasChildren)函数用于确定任何给定项是否是另一级项的父项。

------





## 创建新模型

模型/视图组件之间的功能分离允许创建可以利用现有视图的模型。这种方法允许我们使用标准图形用户界面组件（如[QListView](https://doc.qt.io/qt-5/qlistview.html)，[QTableView](https://doc.qt.io/qt-5/qtableview.html)和[QTreeView）](https://doc.qt.io/qt-5/qtreeview.html)显示来自各种来源的数据。

[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)类提供一个接口，具有足够的灵活性来支持数据源安排在分层结构的信息，从而允许该数据将被插入，移除，修改，或以某种方式分类的可能性。它还为拖放操作提供支持。

该[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)和[QAbstractTableModel](https://doc.qt.io/qt-5/qabstracttablemodel.html)类简单的非分层数据结构提供支持接口，并且更容易为出发点，进行简单的列表，表格使用。

在本节中，我们创建一个简单的只读模型来探索模型/视图体系结构的基本原理。在本节的后面，我们调整了这个简单的模型，以便用户可以修改项目。

有关更复杂模型的示例，请参阅[Simple Tree Model](https://doc.qt.io/qt-5/qtwidgets-itemviews-simpletreemodel-example.html)示例。

[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)子类的要求在[Model Subclassing Reference](https://doc.qt.io/qt-5/model-view-programming.html#model-subclassing-reference)文档中有更详细的描述。



### 设计模型

在为现有数据结构创建新模型时，重要的是要考虑应该使用哪种类型的模型来为数据提供接口。如果数据结构可以表示为列表或项目表，则可以子类化[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)或[QAbstractTableModel，](https://doc.qt.io/qt-5/qabstracttablemodel.html)因为这些类为许多函数提供了合适的默认实现。

但是，如果底层数据结构只能由分层树结构表示，则必须子类化[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)。此方法在[Simple Tree Model](https://doc.qt.io/qt-5/qtwidgets-itemviews-simpletreemodel-example.html)示例中采用。

在本节中，我们基于字符串列表实现了一个简单的模型，因此[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)提供了一个理想的基类来构建。

无论底层数据结构采用何种形式，在专用模型中补充标准[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html) API 通常是一个好主意，它允许更自然地访问底层数据结构。这样可以更轻松地使用数据填充模型，但仍然允许其他通用模型/视图组件使用标准API与其进行交互。下面描述的模型为此提供了一个自定义构造函数。



### 只读模型示例

这里实现的模型是一个基于标准[QStringListModel](https://doc.qt.io/qt-5/qstringlistmodel.html)类的简单，非分层，只读数据模型。它有一个[QStringList](https://doc.qt.io/qt-5/qstringlist.html)作为其内部数据源，并且只实现生成模型所需的内容。为了简化实现，我们将[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)子类化，因为它为列表模型定义了合理的默认行为，并且它公开了比[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)类更简单的接口。

在实现模型时，重要的是要记住[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)本身不存储任何数据，它仅提供视图用于访问数据的接口。对于最小的只读模型，只需要实现一些函数，因为大多数接口都有默认实现。类声明如下：

```c++
class StringListModel : public QAbstractListModel
{
    Q_OBJECT

public:
    StringListModel(const QStringList &strings, QObject *parent = nullptr)
        : QAbstractListModel(parent), stringList(strings) {}

    int rowCount(const QModelIndex &parent = QModelIndex()) const override;
    QVariant data(const QModelIndex &index, int role) const override;
    QVariant headerData(int section, Qt::Orientation orientation,
                        int role = Qt::DisplayRole) const override;

private:
    QStringList stringList;
};
```

除了模型的构造函数之外，我们只需要实现两个函数：[rowCount()](https://doc.qt.io/qt-5/qabstractitemmodel.html#rowCount)返回模型中的行数，[data()](https://doc.qt.io/qt-5/qabstractitemmodel.html#data)返回与指定模型索引对应的数据项。

表现良好的模型还实现了[headerData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#headerData)，以便在其标题中显示树和表视图。

请注意，这是一个非分层模型，因此我们不必担心父子关系。如果我们的模型是分层的，我们还必须实现[index()](https://doc.qt.io/qt-5/qabstractitemmodel.html#index)和[parent()](https://doc.qt.io/qt-5/qabstractitemmodel.html#parent)函数。

字符串列表存储在`stringList`私有成员变量内部。

#### 模型的尺寸

我们希望模型中的行数与字符串列表中的字符串数相同。我们实现了[rowCount（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#rowCount)函数，并考虑到这一点：

```c++
int StringListModel::rowCount(const QModelIndex &parent) const
{
    return stringList.count();
}
```

由于模型是非分层的，我们可以安全地忽略与父项对应的模型索引。默认情况下，从[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)派生的模型只包含一列，因此我们不需要重新实现[columnCount()](https://doc.qt.io/qt-5/qabstractitemmodel.html#columnCount)函数。

#### 模型标题和数据

对于视图中的项目，我们希望返回字符串列表中的字符串。[data()](https://doc.qt.io/qt-5/qabstractitemmodel.html#data)函数负责返回对应于所述索引参数的数据的项：

```c++
QVariant StringListModel::data(const QModelIndex &index, int role) const
{
    if (!index.isValid())
        return QVariant();

    if (index.row() >= stringList.size())
        return QVariant();

    if (role == Qt::DisplayRole)
        return stringList.at(index.row());
    else
        return QVariant();
}
```

如果提供的模型索引有效，行号在字符串列表中的项目范围内，并且请求的角色是我们支持的角色，则只返回有效的[QVariant](https://doc.qt.io/qt-5/qvariant.html)。

某些视图（如[QTreeView](https://doc.qt.io/qt-5/qtreeview.html)和[QTableView](https://doc.qt.io/qt-5/qtableview.html)）能够显示标题以及项目数据。如果我们的模型显示在带标题的视图中，我们希望标题显示行号和列号。我们可以通过[继承headerData（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#headerData)函数来提供有关头的信息：

```c++
QVariant StringListModel::headerData(int section, Qt::Orientation orientation,
                                     int role) const
{
    if (role != Qt::DisplayRole)
        return QVariant();

    if (orientation == Qt::Horizontal)
        return QStringLiteral("Column %1").arg(section);
    else
        return QStringLiteral("Row %1").arg(section);
}
```

同样，只有当角色是我们支持的角色时，我们才会返回有效的[QVariant](https://doc.qt.io/qt-5/qvariant.html)。在确定要返回的确切数据时，还会考虑标题的方向。

并非所有视图都显示带有项目数据的标题，而那些视图可以配置为隐藏它们。尽管如此，建议您实现[headerData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#headerData)函数以提供有关模型提供的数据的相关信息。

一个项目可以有多个角色，根据指定的角色给出不同的数据。我们模型中的项目只有一个角色[DisplayRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)，因此无论指定的角色如何，我们都会返回项目的数据。但是，我们可以在其他角色中重用我们为[DisplayRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)提供的数据，例如视图可用于显示工具提示中项目信息的[ToolTipRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)。



### 一个可编辑的模型

只读模型显示了如何向用户呈现简单的选择，但对于许多应用程序，可编辑列表模型更有用。我们可以修改只读模型，通过更改我们为只读实现的data（）函数，并通过实现两个额外的函数：[flags（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#flags)和[setData（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#setData)来使项目可编辑。以下函数声明将添加到类定义中：

```c++
    Qt::ItemFlags flags(const QModelIndex &index) const override;
    bool setData(const QModelIndex &index, const QVariant &value,
                 int role = Qt::EditRole) override;
```

#### 使模型可编辑

委托在创建编辑器之前检查项目是否可编辑。该模型必须让代表知道其项目是可编辑的。我们通过为模型中的每个项返回正确的标志来完成此操作; 在这种情况下，我们启用所有项目并使其可选择和可编辑：

```c++
Qt::ItemFlags StringListModel::flags(const QModelIndex &index) const
{
    if (!index.isValid())
        return Qt::ItemIsEnabled;

    return QAbstractItemModel::flags(index) | Qt::ItemIsEditable;
}
```

请注意，我们不必知道委托如何执行实际的编辑过程。我们只需要为委托提供一种在模型中设置数据的方法。这是通过[setData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#setData)函数实现的：

```c++
bool StringListModel::setData(const QModelIndex &index,
                              const QVariant &value, int role)
{
    if (index.isValid() && role == Qt::EditRole) {

        stringList.replace(index.row(), value.toString());
        emit dataChanged(index, index, {role});
        return true;
    }
    return false;
}
```

在此模型中，字符串列表中与模型索引对应的项目将替换为提供的值。但是，在我们修改字符串列表之前，我们必须确保索引有效，项目类型正确，并且支持该角色。按照惯例，我们坚持认为角色是[EditRole，](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)因为这是标准项委托使用的角色。但是，对于布尔值，您可以使用[Qt :: CheckStateRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)并设置[Qt :: ItemIsUserCheckable](https://doc.qt.io/qt-5/qt.html#ItemFlag-enum)标志; 然后使用复选框来编辑值。此模型中的基础数据对于所有角色都是相同的，因此这个细节使得更容易将模型与标准组件集成。

设置数据后，模型必须让视图知道某些数据已更改。这是通过发出[dataChanged（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#dataChanged)信号来完成的。由于只有一项数据发生了变化，因此信号中指定的项目范围仅限于一个模型索引。

还需要更改data()函数以添加[Qt :: EditRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)测试：

```c++
QVariant StringListModel::data(const QModelIndex &index, int role) const
{
    if (!index.isValid())
        return QVariant();

    if (index.row() >= stringList.size())
        return QVariant();

    if (role == Qt::DisplayRole || role == Qt::EditRole)
        return stringList.at(index.row());
    else
        return QVariant();
}
```

#### 插入和删除行

可以更改模型中的行数和列数。在字符串列表模型中，只更改行数是有意义的，因此我们只重新实现插入和删除行的功能。这些在类定义中声明：

```c++
    bool insertRows(int position, int rows, const QModelIndex &index = QModelIndex()) override;
    bool removeRows(int position, int rows, const QModelIndex &index = QModelIndex()) override;
```

由于此模型中的行对应于列表中的字符串，因此该`insertRows()`函数会在指定位置之前将多个空字符串插入字符串列表中。插入的字符串数等于指定的行数。

父索引通常用于确定模型中应添加行的位置。在这种情况下，我们只有一个顶级字符串列表，所以我们只需在该列表中插入空字符串。

```c++
bool StringListModel::insertRows(int position, int rows, const QModelIndex &parent)
{
    beginInsertRows(QModelIndex(), position, position+rows-1);

    for (int row = 0; row < rows; ++row) {
        stringList.insert(position, "");
    }

    endInsertRows();
    return true;
}
```

该模型首先调用[beginInsertRows（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginInsertRows)函数以通知其他组件行数即将更改。该函数指定要插入的第一个和最后一个新行的行号，以及其父项的模型索引。更改字符串列表后，它调用[endInsertRows（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#endInsertRows)来完成操作并通知其他组件模型的尺寸已更改，返回true表示成功。

从模型中删除行的功能也很容易编写。要从模型中删除的行由给定的位置和行数指定。我们忽略父索引来简化我们的实现，只需从字符串列表中删除相应的项。

```c++
bool StringListModel::removeRows(int position, int rows, const QModelIndex &parent)
{
    beginRemoveRows(QModelIndex(), position, position+rows-1);

    for (int row = 0; row < rows; ++row) {
        stringList.removeAt(position);
    }

    endRemoveRows();
    return true;
}
```

所述[beginRemoveRows（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginRemoveRows)除去任何潜在的数据之前，功能总是被调用，并指定要删除的第一行和最后一行。这允许其他组件在数据变得不可用之前访问它。删除行后，模型将发出[endRemoveRows（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#endRemoveRows)以完成操作，并让其他组件知道模型的尺寸已更改。

### 下一步

我们可以使用[QListView](https://doc.qt.io/qt-5/qlistview.html)类显示此模型或任何其他模型提供的数据，以垂直列表的形式显示模型的项目。对于字符串列表模型，此视图还提供默认编辑器，以便可以操作项目。我们研究了[View Classes中](https://doc.qt.io/qt-5/model-view-programming.html#view-classes)标准视图类提供的可能性。

所述[模型子类参考](https://doc.qt.io/qt-5/model-view-programming.html#model-subclassing-reference)文件讨论的要求[化QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)更详细的子类，和提供了指导必须被实现为使得在不同类型的模型各种特征的虚拟功能。





## 项目视图便利类

基于项目的小部件具有反映其用途的名称：`QListWidget`提供项目列表，`QTreeWidget`显示多级树结构，并`QTableWidget`提供单元格项目表。每个类都继承了类的行为，`QAbstractItemView`该类实现了项目选择和标题管理的常见行为。

### 列表小部件

单项物品清单通常使用一个`QListWidget`和多个`QListWidgetItem`s 来显示。列表小部件的构造方式与任何其他小部件相同：

```c++
    QListWidget *listWidget = new QListWidget(this);
```

列表项可以在构造时直接添加到列表小部件：

```c++
    new QListWidgetItem(tr("Sycamore"), listWidget);
    new QListWidgetItem(tr("Chestnut"), listWidget);
    new QListWidgetItem(tr("Mahogany"), listWidget);
```

它们也可以在没有父列表小部件的情况下构建，并在以后添加到列表中：

```c++
    QListWidgetItem *newItem = new QListWidgetItem;
    newItem->setText(itemText);
    listWidget->insertItem(row, newItem);
```

列表中的每个项目都可以显示文本标签和图标。可以更改用于呈现文本的颜色和字体，以便为项目提供自定义外观。工具提示，状态提示和“这是什么？” 帮助都可以轻松配置，以确保列表正确集成到应用程序中。

```c++
    newItem->setToolTip(toolTipText);
    newItem->setStatusTip(toolTipText);
    newItem->setWhatsThis(whatsThisText);
```

默认情况下，列表中的项目按其创建顺序显示。可以根据[Qt :: SortOrder中](https://doc.qt.io/qt-5/qt.html#SortOrder-enum)给出的条件对项目列表进行排序，以生成按正向或反向字母顺序排序的项目列表：

```c++
    listWidget->sortItems(Qt::AscendingOrder);
    listWidget->sortItems(Qt::DescendingOrder);
```

### 树小部件

树和层次的项目列表由`QTreeWidget`和`QTreeWidgetItem`类提供。树窗口小部件中的每个项目都可以具有自己的子项目，并且可以显示多个信息列。树窗口小部件的创建方式与其他窗口小部件一样：

```c++
    QTreeWidget *treeWidget = new QTreeWidget(this);
```

在将项目添加到树窗口小部件之前，必须设置列数。例如，我们可以定义两列，并创建一个标题以在每列的顶部提供标签：

```c++
    treeWidget->setColumnCount(2);
    QStringList headers;
    headers << tr("Subject") << tr("Default");
    treeWidget->setHeaderLabels(headers);
```

为每个部分设置标签的最简单方法是提供字符串列表。对于更复杂的标题，您可以构造一个树项，根据需要进行装饰，并将其用作树小部件的标题。

树窗口小部件中的顶级项目是以树窗口小部件作为其父窗口小部件构建的。它们可以按任意顺序插入，或者您可以通过在构造每个项目时指定前一个项目来确保它们按特定顺序列出：

```c++
    QTreeWidgetItem *cities = new QTreeWidgetItem(treeWidget);
    cities->setText(0, tr("Cities"));
    QTreeWidgetItem *osloItem = new QTreeWidgetItem(cities);
    osloItem->setText(0, tr("Oslo"));
    osloItem->setText(1, tr("Yes"));

    QTreeWidgetItem *planets = new QTreeWidgetItem(treeWidget, cities);
```

树窗口小部件处理顶级项目与树中更深层次的其他项目略有不同。可以通过调用树窗口小部件的[takeTopLevelItem()](https://doc.qt.io/qt-5/qtreewidget.html#takeTopLevelItem)函数从树的[顶层](https://doc.qt.io/qt-5/qtreewidget.html#takeTopLevelItem)删除项目，但通过调用其父项目的[takeChild()](https://doc.qt.io/qt-5/qtreewidgetitem.html#takeChild)函数来删除较低级别的项目。使用[insertTopLevelItem()](https://doc.qt.io/qt-5/qtreewidget.html#insertTopLevelItem)函数将项插入树的[顶层](https://doc.qt.io/qt-5/qtreewidget.html#insertTopLevelItem)。在树的较低级别，使用父项的[insertChild()](https://doc.qt.io/qt-5/qtreewidgetitem.html#insertChild)函数。

在树中的顶层和底层之间移动项目很容易。我们只需要检查项目是否是顶级项目，这些信息由每个项目的`parent()`功能提供。例如，我们可以删除树小部件中的当前项，而不管其位置如何：

```c++
    QTreeWidgetItem *parent = currentItem->parent();
    int index;

    if (parent) {
        index = parent->indexOfChild(treeWidget->currentItem());
        delete parent->takeChild(index);
    } else {
        index = treeWidget->indexOfTopLevelItem(treeWidget->currentItem());
        delete treeWidget->takeTopLevelItem(index);
    }
```

在树窗口小部件中的其他位置插入项目遵循相同的模式：

```c++
    QTreeWidgetItem *parent = currentItem->parent();
    QTreeWidgetItem *newItem;
    if (parent)
        newItem = new QTreeWidgetItem(parent, treeWidget->currentItem());
    else
        newItem = new QTreeWidgetItem(treeWidget, treeWidget->currentItem());
```

### 表格小部件

类似于在电子表格应用程序找到的项目表构造与`QTableWidget`和`QTableWidgetItem`。这些提供了一个滚动表小部件，其中包含要在其中使用的标题和项目。

可以使用一定数量的行和列创建表，也可以根据需要将这些表添加到未计算的表中。

```c++
    QTableWidget *tableWidget;
    tableWidget = new QTableWidget(12, 3, this);
```

在将表添加到所需位置的表之前，将在表外构建项：

```c++
    QTableWidgetItem *newItem = new QTableWidgetItem(tr("%1").arg(
        pow(row, column+1)));
    tableWidget->setItem(row, column, newItem);
```

通过构造表外的项并将它们用作标题，可以将水平和垂直标题添加到表中：

```c++
    QTableWidgetItem *valuesHeaderItem = new QTableWidgetItem(tr("Values"));
    tableWidget->setHorizontalHeaderItem(0, valuesHeaderItem);
```

请注意，表中的行和列从零开始。

### 共同特征

每个便捷类都有许多共同的基于项目的功能，这些功能可通过每个类中的相同接口获得。我们在以下部分中介绍了这些内容以及不同小部件的一些示例。查看每个小部件的[模型/视图类](https://doc.qt.io/qt-5/model-view-programming.html#model-view-classes)列表，以获取有关使用每个函数的更多详细信息。

#### 隐藏的项目

有时可以隐藏项目视图小部件中的项目而不是删除它们。可以隐藏所有上述小部件的项目，然后再次显示。您可以通过调用isItemHidden（）函数来确定项是否被隐藏，并且可以隐藏项`setItemHidden()`。

由于此操作是基于项目的，因此所有三个便利类都可以使用相同的功能。

#### 选择方式

选择项的方式由小部件的选择模式（[QAbstractItemView :: SelectionMode](https://doc.qt.io/qt-5/qabstractitemview.html#SelectionMode-enum)）控制。此属性控制用户是否可以选择一个或多个项目，并且在多项目选择中，选择是否必须是连续范围的项目。对于所有上述小部件，选择模式以相同的方式工作。

|                                       |                                                              |
| ------------------------------------- | :----------------------------------------------------------- |
| ![img](assets/selection-single.png)   | **单项选择**：如果用户需要从窗口小部件中选择单个项目，则默认`SingleSelection`模式最合适。在此模式下，当前项目和所选项目是相同的。 |
| ![img](assets/selection-multi.png)    | **多项选择：**在此模式下，用户可以切换窗口小部件中任何项目的选择状态，而无需更改现有选择，就像非独占复选框可以独立切换的方式一样。 |
| ![img](assets/selection-extended.png) | **扩展选择：**通常需要**选择**许多相邻项目的小部件（例如电子表格中的小部件）需要该`ExtendedSelection`模式。在此模式下，可以使用鼠标和键盘选择窗口小部件中的连续项目范围。如果使用修饰键，也可以创建涉及与窗口小部件中其他所选项不相邻的许多项的复杂选择。如果用户在不使用修改键的情况下选择项目，则清除现有选择。 |

使用该`selectedItems()`函数读取窗口小部件中的所选项，提供可以迭代的相关项列表。例如，我们可以使用以下代码找到所选项目列表中所有数值的总和：

```c++
    QList<QTableWidgetItem *> selected = tableWidget->selectedItems();
    QTableWidgetItem *item;
    int number = 0;
    double total = 0;

    foreach (item, selected) {
        bool ok;
        double value = item->text().toDouble(&ok);

        if (ok && !item->text().isEmpty()) {
            total += value;
            number++;
        }
    }
```

请注意，对于单选模式，当前项目将在选择中。在多选和扩展选择模式中，当前项目可能不在选择范围内，这取决于用户形成选择的方式。

#### 搜索

能够在项目视图窗口小部件中查找项目通常很有用，无论是作为开发人员还是作为向用户呈现的服务。所有三个项目视图便利类都提供了一个通用`findItems()`功能，使其尽可能一致和简单。

根据[Qt :: MatchFlags中](https://doc.qt.io/qt-5/qt.html#MatchFlag-enum)选择的值指定的条件，通过它们包含的文本搜索项目。我们可以通过`findItems()`函数获得匹配项的列表：

```c++
    QTreeWidgetItem *item;
    QList<QTreeWidgetItem *> found = treeWidget->findItems(
        itemText, Qt::MatchWildcard);

    foreach (item, found) {
        treeWidget->setItemSelected(item, true);
        // Show the item->text(0) for each item.
    }
```

如果上面的代码包含搜索字符串中给出的文本，则会导致选择树窗口小部件中的项目。此模式也可用于列表和表格小部件。

------





## 使用项目视图拖放

模型/视图框架完全支持Qt的拖放式基础架构。列表，表和树中的项目可以在视图中拖动，数据可以作为MIME编码数据导入和导出。

标准视图自动支持内部拖放，其中移动项目以更改它们的显示顺序。默认情况下，不会为这些视图启用拖放功能，因为它们是为最简单，最常见的用途配置的。要允许拖动项目，需要启用视图的某些属性，并且项目本身还必须允许拖动。

仅允许从视图导出项目且不允许将数据放入其中的模型的要求少于完全启用的拖放模型的要求。

有关在新模型中启用拖放支持的更多信息，另请参阅“ [模型子类化参考](https://doc.qt.io/qt-5/model-view-programming.html#model-subclassing-reference) ”。



### 使用便利视图

与[QListWidget](https://doc.qt.io/qt-5/qlistwidget.html)，[QTableWidget](https://doc.qt.io/qt-5/qtablewidget.html)和[QTreeWidget](https://doc.qt.io/qt-5/qtreewidget.html)一起使用的每种类型的项目都配置为默认使用不同的标志集。例如，每个[QListWidgetItem](https://doc.qt.io/qt-5/qlistwidgetitem.html)或[QTreeWidgetItem](https://doc.qt.io/qt-5/qtreewidgetitem.html)最初都是启用，可检查，可选择的，并且可以用作拖放操作的源; 每个[QTableWidgetItem](https://doc.qt.io/qt-5/qtablewidgetitem.html)也可以编辑并用作拖放操作的目标。

虽然所有标准项都有一个或两个标记设置为拖放，但您通常需要在视图中设置各种属性，以利用内置的拖放支持：

- 要启用项目拖动，请将视图的[dragEnabled](https://doc.qt.io/qt-5/qabstractitemview.html#dragEnabled-prop)属性设置为`true`。
- 要允许用户删除视图中的内部或外部项，请将视图的[viewport（）](https://doc.qt.io/qt-5/qabstractscrollarea.html#viewport)的[acceptDrops](https://doc.qt.io/qt-5/qwidget.html#acceptDrops-prop)属性设置为`true`。
- 要向用户显示当前正在拖动的项目将被放置的位置，请设置视图的[showDropIndicator](https://doc.qt.io/qt-5/qabstractitemview.html#showDropIndicator-prop)属性。这为用户提供了有关视图中项目放置的持续更新信息。

例如，我们可以使用以下代码行在列表小部件中启用拖放：

```c++
QListWidget *listWidget = new QListWidget(this);
listWidget->setSelectionMode(QAbstractItemView::SingleSelection);
listWidget->setDragEnabled(true);
listWidget->viewport()->setAcceptDrops(true);
listWidget->setDropIndicatorShown(true);  
```

结果是一个列表小部件，它允许在视图中复制项目，甚至允许用户在包含相同类型数据的视图之间拖动项目。在这两种情况下，都会复制项目而不是移动项目。

要使用户能够在视图中移动项目，我们必须设置列表小部件的[dragDropMode](https://doc.qt.io/qt-5/qabstractitemview.html#dragDropMode-prop)：

```c++
listWidget->setDragDropMode(QAbstractItemView::InternalMove);
```



### 使用模型/视图类

设置拖放视图遵循与便捷视图一起使用的相同模式。例如，可以使用与[QListWidget](https://doc.qt.io/qt-5/qlistwidget.html)相同的方式设置[QListView](https://doc.qt.io/qt-5/qlistview.html)：

```c++
QListView *listView = new QListView(this);
listView->setSelectionMode(QAbstractItemView::ExtendedSelection);
listView->setDragEnabled(true);
listView->setAcceptDrops(true);
listView->setDropIndicatorShown(true);
```

由于对视图显示的数据的访问由模型控制，因此使用的模型还必须为拖放操作提供支持。可以通过重新实现[QAbstractItemModel :: supportedDropActions](https://doc.qt.io/qt-5/qabstractitemmodel.html#supportedDropActions)（）函数来指定模型支持的操作。例如，使用以下代码启用复制和移动操作：

```c++
Qt::DropActions DragDropListModel::supportedDropActions() const
{
    return Qt::CopyAction | Qt::MoveAction;
}
```

虽然可以给出[Qt :: DropActions](https://doc.qt.io/qt-5/qt.html#DropAction-enum)的值的任意组合，但是需要编写模型来支持它们。例如，要允许[Qt :: MoveAction](https://doc.qt.io/qt-5/qt.html#DropAction-enum)与列表模型一起使用，模型必须直接或通过从其基类继承实现来提供[QAbstractItemModel :: removeRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#removeRows)的实现。

#### 启用项目的拖放功能

通过重新实现[QAbstractItemModel :: flags](https://doc.qt.io/qt-5/qabstractitemmodel.html#flags)（）函数以提供合适的标志，模型向视图指示可以拖动哪些项目以及哪些项目将接受丢弃。

例如，提供基于[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)的简单列表的模型可以通过确保返回的标志包含[Qt :: ItemIsDragEnabled](https://doc.qt.io/qt-5/qt.html#ItemFlag-enum)和[Qt :: ItemIsDropEnabled](https://doc.qt.io/qt-5/qt.html#ItemFlag-enum)值来为每个项启用拖放：

```c++
Qt::ItemFlags DragDropListModel::flags(const QModelIndex &index) const
{
    Qt::ItemFlags defaultFlags = QStringListModel::flags(index);

    if (index.isValid())
        return Qt::ItemIsDragEnabled | Qt::ItemIsDropEnabled | defaultFlags;
    else
        return Qt::ItemIsDropEnabled | defaultFlags;
}
```

请注意，可以将项目放入模型的顶层，但仅对有效项目启用拖动。

在上面的代码中，由于模型是从[QStringListModel](https://doc.qt.io/qt-5/qstringlistmodel.html)派生的，因此我们通过调用flags（）函数的实现来获取一组默认的标志。

#### 编码导出的数据

当在拖放操作中从模型导出数据项时，它们被编码为对应于一种或多种MIME类型的适当格式。模型通过重新实现[QAbstractItemModel :: mimeTypes](https://doc.qt.io/qt-5/qabstractitemmodel.html#mimeTypes)（）函数来声明它们可用于提供项目的MIME类型，返回标准MIME类型的列表。

例如，仅提供纯文本的模型将提供以下实现：

```c++
QStringList DragDropListModel::mimeTypes() const
{
    QStringList types;
    types << "application/vnd.text.list";
    return types;
}
```

该模型还必须提供以广告格式对数据进行编码的代码。这是通过重新实现[QAbstractItemModel :: mimeData](https://doc.qt.io/qt-5/qabstractitemmodel.html#mimeData)（）函数来提供[QMimeData](https://doc.qt.io/qt-5/qmimedata.html)对象来实现的，就像在任何其他拖放操作中一样。

以下代码显示了与给定索引列表对应的每个数据项如何编码为纯文本并存储在[QMimeData](https://doc.qt.io/qt-5/qmimedata.html)对象中。

```c++
QMimeData *DragDropListModel::mimeData(const QModelIndexList &indexes) const
{
    QMimeData *mimeData = new QMimeData();
    QByteArray encodedData;

    QDataStream stream(&encodedData, QIODevice::WriteOnly);

    foreach (const QModelIndex &index, indexes) {
        if (index.isValid()) {
            QString text = data(index, Qt::DisplayRole).toString();
            stream << text;
        }
    }

    mimeData->setData("application/vnd.text.list", encodedData);
    return mimeData;
}
```

由于模型索引列表被提供给函数，因此该方法通常足以用于分层和非层次模型。

请注意，必须将自定义数据类型声明为[元对象，](https://doc.qt.io/qt-5/qmetaobject.html)并且必须为它们实现流运算符。有关详细信息，请参阅[QMetaObject](https://doc.qt.io/qt-5/qmetaobject.html)类说明。

#### 将拖出的数据插入模型中

任何给定模型处理丢弃数据的方式取决于其类型（列表，表或树）以及其内容可能呈现给用户的方式。通常，用于容纳丢弃数据的方法应该是最适合模型的底层数据存储的方法。

不同类型的模型倾向于以不同方式处理丢弃的数据。列表和表模型仅提供存储数据项的扁平结构。因此，当数据被放置在视图中的现有项目上时，它们可能会插入新的行（和列），或者它们可能会使用提供的某些数据覆盖模型中项目的内容。树模型通常能够将包含新数据的子项添加到其底层数据存储中，因此就用户而言，其行为将更加可预测。

丢弃的数据由模型重新实现[QAbstractItemModel :: dropMimeData](https://doc.qt.io/qt-5/qabstractitemmodel.html#dropMimeData)（）来处理。例如，处理简单字符串列表的模型可以提供一种实现，该实现将分别放到现有项目上的数据处理到放入模型顶层的数据（即，放到无效项目上）。

模型可以通过重新实现[QAbstractItemModel :: canDropMimeData](https://doc.qt.io/qt-5/qabstractitemmodel.html#canDropMimeData)（）来禁止丢弃某些项目，或者根据丢弃的数据。

模型首先必须确保操作应该被操作，提供的数据是可以使用的格式，并且模型中的目标是有效的：

```c++
bool DragDropListModel::canDropMimeData(const QMimeData *data,
    Qt::DropAction action, int row, int column, const QModelIndex &parent)
{
    Q_UNUSED(action);
    Q_UNUSED(row);
    Q_UNUSED(parent);

    if (!data->hasFormat("application/vnd.text.list"))
        return false;

    if (column > 0)
        return false;

    return true;
}
bool DragDropListModel::dropMimeData(const QMimeData *data,
    Qt::DropAction action, int row, int column, const QModelIndex &parent)
{
    if (!canDropMimeData(data, action, row, column, parent))
        return false;

    if (action == Qt::IgnoreAction)
        return true;
```

如果提供的数据不是纯文本，或者为drop提供的列号无效，则简单的一列字符串列表模型可以指示失败。

要插入到模型中的数据的处理方式不同，具体取决于是否将其拖放到现有项目上。在这个简单的示例中，我们希望在列表中的第一个项目之前和最后一个项目之后允许现有项目之间的丢弃。

当发生丢弃时，与父项对应的模型索引将有效，表示项目上发生了丢弃，或者它将无效，表示丢弃发生在视图中某个与模型顶层对应的位置。

```c++
    int beginRow;

    if (row != -1)
        beginRow = row;
```

我们首先检查提供的行号，看看我们是否可以使用它来将项目插入到模型中，无论父索引是否有效。

```c++
    else if (parent.isValid())
        beginRow = parent.row();
```

如果父模型索引有效，则会在项目上发生丢弃。在这个简单的列表模型中，我们找出项目的行号，并使用该值将已删除的项目插入到模型的顶层。

```c++
    else
        beginRow = rowCount(QModelIndex());
```

当视图中的其他位置发生放置，并且行号不可用时，我们会将项目附加到模型的顶层。

在分层模型中，当项目发生丢弃时，最好将新项目作为该项目的子项插入到模型中。在此处显示的简单示例中，模型只有一个级别，因此这种方法不合适。

#### 解码导入的数据

[dropMimeData()的](https://doc.qt.io/qt-5/qabstractitemmodel.html#dropMimeData)每个实现还必须解码数据并将其插入到模型的底层数据结构中。

对于简单的字符串列表模型，可以对编码的项进行解码并将其流式传输到[QStringList](https://doc.qt.io/qt-5/qstringlist.html)：

```c++
    QByteArray encodedData = data->data("application/vnd.text.list");
    QDataStream stream(&encodedData, QIODevice::ReadOnly);
    QStringList newItems;
    int rows = 0;

    while (!stream.atEnd()) {
        QString text;
        stream >> text;
        newItems << text;
        ++rows;
    }
```

然后可以将字符串插入底层数据存储中。为了保持一致性，可以通过模型自己的界面完成：

```c++
    insertRows(beginRow, rows, QModelIndex());
    foreach (const QString &text, newItems) {
        QModelIndex idx = index(beginRow, 0, QModelIndex());
        setData(idx, text);
        beginRow++;
    }

    return true;
}
```

请注意，模型通常需要提供[QAbstractItemModel :: insertRows](https://doc.qt.io/qt-5/qabstractitemmodel.html#insertRows)（）和[QAbstractItemModel :: setData](https://doc.qt.io/qt-5/qabstractitemmodel.html#setData)（）函数的实现。

------





## 代理模型

在模型/视图框架中，单个模型提供的数据项可以由任意数量的视图共享，并且每个视图中的每一个都可以以完全不同的方式表示相同的信息。自定义视图和委托是提供相同数据的完全不同表示的有效方法。但是，应用程序通常需要在相同数据的处理版本上提供传统视图，例如对项目列表的不同排序视图。

尽管将排序和过滤操作作为视图的内部函数执行似乎是合适的，但是这种方法不允许多个视图共享这种潜在代价高昂的操作的结果。涉及在模型本身内进行排序的替代方法导致类似的问题，其中每个视图必须显示根据最近的处理操作组织的数据项。

为解决此问题，模型/视图框架使用代理模型来管理各个模型和视图之间提供的信息。代理模型是从视图的角度看像普通模型一样的组件，并代表该视图从源模型访问数据。模型/视图框架使用的信号和槽确保无论在自身和源模型之间放置多少个代理模型，每个视图都会得到适当更新。



### 使用代理模型

可以在现有模型和任意数量的视图之间插入代理模型。Qt提供了标准代理模型[QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)，它通常被实例化并直接使用，但也可以进行子类化以提供自定义过滤和排序行为。该[QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)类可以通过以下方式使用：

```c++
    QSortFilterProxyModel *filterModel = new QSortFilterProxyModel(parent);
    filterModel->setSourceModel(stringListModel);

    QListView *filteredView = new QListView;
    filteredView->setModel(filterModel);
```

由于代理模型继承自[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)，因此它们可以连接到任何类型的视图，并且可以在视图之间共享。它们还可用于处理从管道布置中的其他代理模型获得的信息。

该[QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)类被设计成被实例化，并直接在应用程序中使用。通过继承此类并实现所需的比较操作，可以创建更专业的代理模型。



### 自定义代理模型

通常，代理模型中使用的处理类型涉及将每个数据项从其在源模型中的原始位置映射到代理模型中的不同位置。在某些模型中，某些项目在代理模型中可能没有相应的位置; 这些模型是*过滤*代理模型。使用代理模型提供的模型索引查看访问项目，这些项目不包含有关源模型或该模型中原始项目位置的信息。

[QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)允许在提供给视图之前过滤来自源模型的数据，并且还允许将源模型的内容作为预排序数据提供给视图。

#### 自定义过滤模型

所述[QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)类提供的滤波模式，是相当通用的，并且可以在各种常见的情况下使用。对于高级用户，可以对[QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)进行子类化，从而提供一种可以实现自定义过滤器的机制。

[QSortFilterProxyModel的](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)子类可以重新实现在请求或使用代理模型的模型索引时调用的两个虚函数：

- [filterAcceptsColumn()](https://doc.qt.io/qt-5/qsortfilterproxymodel.html#filterAcceptsColumn)用于从源模型的一部分中过滤特定列。
- [filterAcceptsRow()](https://doc.qt.io/qt-5/qsortfilterproxymodel.html#filterAcceptsRow)用于过滤部分源模型中的特定行。

[QSortFilterProxyModel中](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)上述函数的默认实现返回true以确保所有项都传递给视图; 重新实现这些函数应返回false以过滤掉各个行和列。

#### 自定义排序模型

[QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html)实例使用Qt的内置qStableSort（）函数来设置源模型中的项与代理模型中的项之间的映射，允许将项的排序层次结构公开给视图，而无需修改源模型的结构。要提供自定义排序行为，请重新实现[lessThan()](https://doc.qt.io/qt-5/qsortfilterproxymodel.html#lessThan)函数以执行自定义比较。

------





## 模型子类参考

模型子类需要提供[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)基类中定义的许多虚函数的实现。需要实现的这些函数的数量取决于模型的类型 - 它是否提供具有简单列表，表或项的复杂层次结构的视图。从[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)和[QAbstractTableModel](https://doc.qt.io/qt-5/qabstracttablemodel.html)继承的模型可以利用这些类提供的函数的默认实现。在树状结构中公开数据项的模型必须为[QAbstractItemModel中的](https://doc.qt.io/qt-5/qabstractitemmodel.html)许多虚函数提供实现。

需要在模型子类中实现的函数可以分为三组：

- **项目数据处理：**所有模型都需要实现函数，以使视图和委托能够查询模型的维度，检查项目和检索数据。
- **导航和索引创建：**分层模型需要提供视图可以调用的函数来导航它们公开的树状结构，并获取项目的模型索引。
- **拖放支持和MIME类型处理：**模型继承控制内部和外部拖放操作执行方式的函数。这些函数允许根据其他组件和应用程序可以理解的MIME类型来描述数据项。



### 物品数据处理

模型可以提供对它们提供的数据的不同级别的访问：它们可以是简单的只读组件，某些模型可以支持调整大小操作，而其他模型可以允许编辑项目。



### 只读访问权限

要提供对模型提供的数据的只读访问，*必须*在模型的子类中实现以下函数：

|                                                              |                                                              |
| :----------------------------------------------------------: | :----------------------------------------------------------- |
| [flags()](https://doc.qt.io/qt-5/qabstractitemmodel.html#flags) | 由其他组件使用以获取有关模型提供的每个项目的信息。在许多模型中，标志的组合应包括[Qt :: ItemIsEnabled](https://doc.qt.io/qt-5/qt.html#ItemFlag-enum)和[Qt :: ItemIsSelectable](https://doc.qt.io/qt-5/qt.html#ItemFlag-enum)。 |
| [data()](https://doc.qt.io/qt-5/qabstractitemmodel.html#data) | 用于向视图和委托提供项目数据。一般情况下，模型只需要为提供数据[的Qt :: DisplayRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)任何应用程序特定的用户角色，但它也是很好的做法，对提供数据[的Qt :: ToolTipRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)，[Qt的:: AccessibleTextRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)和[Qt的:: AccessibleDescriptionRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)。有关与每个角色关联的类型的信息，请参阅[Qt :: ItemDataRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)枚举文档。 |
| [headerData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#headerData) | 提供包含要在其标题中显示的信息的视图。该信息仅由可显示标题信息的视图检索。 |
| [rowCount()](https://doc.qt.io/qt-5/qabstractitemmodel.html#rowCount) | 提供模型公开的数据行数。                                     |

这四个函数必须在所有类型的模型中实现，包括列表模型（[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)子类）和表模型（[QAbstractTableModel](https://doc.qt.io/qt-5/qabstracttablemodel.html)子类）。

此外，*必须*在[QAbstractTableModel](https://doc.qt.io/qt-5/qabstracttablemodel.html)和[QAbstractItemModel的](https://doc.qt.io/qt-5/qabstractitemmodel.html)直接子类中实现以下函数：

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [columnCount()](https://doc.qt.io/qt-5/qabstractitemmodel.html#columnCount) | 提供模型公开的数据列数。列表模型不提供此功能，因为它已在[QAbstractListModel中](https://doc.qt.io/qt-5/qabstractlistmodel.html)实现。 |

#### 可编辑的项目

可编辑模型允许修改数据项，还可以提供允许插入和删除行和列的功能。要启用编辑，必须正确实现以下功能：

|                                                              |                                                              |
| :----------------------------------------------------------: | ------------------------------------------------------------ |
| [flags()](https://doc.qt.io/qt-5/qabstractitemmodel.html#flags) | 必须为每个项目返回适当的标志组合。特别是，除了应用于只读模型中的项的值之外，此函数返回的值还必须包括[Qt :: ItemIsEditable](https://doc.qt.io/qt-5/qt.html#ItemFlag-enum)。 |
| [setData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#setData) | 用于修改与指定模型索引关联的数据项。为了能够接受用户界面元素提供的用户输入，该函数必须处理与[Qt :: EditRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)相关的数据。实现还可以接受与[Qt :: ItemDataRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)指定的许多不同类型的角色相关联的数据。更改数据项后，模型必须发出[dataChanged()](https://doc.qt.io/qt-5/qabstractitemmodel.html#dataChanged)信号以通知其他组件更改。 |
| [setHeaderData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#setHeaderData) | 用于修改水平和垂直标题信息。更改数据项后，模型必须发出[headerDataChanged()](https://doc.qt.io/qt-5/qabstractitemmodel.html#headerDataChanged)信号以通知其他组件更改。 |

#### 可调整大小的模型

所有类型的模型都可以支持行的插入和删除。表模型和分层模型也可以支持列的插入和删除。重要的是在模型发生*之前*和*之后*通知其他组件有关模型尺寸变化的信息。因此，可以实现以下函数以允许调整模型大小，但实现必须确保调用适当的函数以通知附加的视图和委托：

|                                                              |                                                              |
| :----------------------------------------------------------: | ------------------------------------------------------------ |
| [insertRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#insertRows) | 用于向所有类型的模型添加新行和数据项。实现必须调用[beginInsertRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginInsertRows) *之前*插入新行到任何基础数据结构，并调用[endInsertRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#endInsertRows) *之后立即*。 |
| [removeRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#removeRows) | 用于从所有类型的模型中删除行及其包含的数据项。实现必须调用[beginRemoveRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginRemoveRows) *之前*从任何底层数据结构被移除的行，并调用[endRemoveRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#endRemoveRows) *之后立即*。 |
| [insertColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#insertColumns) | 用于向表模型和分层模型添加新列和数据项。实现必须调用[beginInsertColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginInsertColumns) *之前*将新列插入任何底层的数据结构，并调用[endInsertColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#endInsertColumns) *之后立即*。 |
| [removeColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#removeColumns) | 用于从表模型和分层模型中删除列中包含的列及其数据项。实现必须调用[beginRemoveColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginRemoveColumns) *之前*从任何底层数据结构被去除柱，并调用[endRemoveColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#endRemoveColumns)*之后立即*。 |

通常，如果操作成功，这些函数应返回true。但是，可能会出现手术部分成功的情况; 例如，如果可以插入少于指定的行数。在这种情况下，模型应返回false以指示无法启用任何附加组件来处理该情况。

在调整大小API的实现中调用的函数发出的信号使附加组件有机会在任何数据变得不可用之前采取操作。使用begin和end函数封装insert和remove操作还可以使模型正确管理[持久模型索引](https://doc.qt.io/qt-5/qpersistentmodelindex.html)。

通常，开始和结束函数能够通知其他组件有关模型底层结构的更改。对于模型结构的更复杂更改（可能涉及内部重组，数据排序或任何其他结构更改），必须执行以下顺序：

- 发出[layoutAboutToBeChanged()](https://doc.qt.io/qt-5/qabstractitemmodel.html#layoutAboutToBeChanged)信号
- 更新表示模型结构的内部数据。
- 使用[changePersistentIndexList()](https://doc.qt.io/qt-5/qabstractitemmodel.html#changePersistentIndexList)更新持久索引
- 发出[layoutChanged()](https://doc.qt.io/qt-5/qabstractitemmodel.html#layoutChanged)信号。

此序列可用于任何结构更新，以代替更高级别和方便的受保护方法。例如，如果一个200万行的模型需要删除所有奇数行，那就是每个1个元素的100万个不规则范围。可以使用beginRemoveRows和endRemoveRows 100万次，但这显然效率低下。相反，这可以作为单个布局更改发出信号，一次更新所有必需的持久索引。

#### 懒惰的模型数据

懒惰的模型数据有效地允许延迟对模型的信息的请求，直到视图实际需要它为止。

某些模型需要从远程源获取数据，或者必须执行耗时的操作以获取有关数据组织方式的信息。由于视图通常会请求尽可能多的信息以准确显示模型数据，因此限制返回给它们的信息量以减少不必要的数据后续请求会很有用。

在分层模型中，查找给定项的子项数是一项昂贵的操作，确保仅在必要时调用模型的[rowCount（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#rowCount)实现很有用。在这种情况下，可以重新实现[hasChildren（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#hasChildren)函数，以便为视图提供一种廉价的方法来检查是否存在子项，并且在[QTreeView](https://doc.qt.io/qt-5/qtreeview.html)的情况下，为其父项绘制适当的装饰。

是否重新实现[hasChildren（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#hasChildren)返回`true`或者`false`，视图可能没有必要调用[rowCount（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#rowCount)来查找有多少[个子](https://doc.qt.io/qt-5/qabstractitemmodel.html#rowCount)元素。例如，如果父项尚未展开以显示它们，则[QTreeView](https://doc.qt.io/qt-5/qtreeview.html)不需要知道有多少[个子](https://doc.qt.io/qt-5/qtreeview.html)项。

如果已知许多项目将有子项，则重新实现[hasChildren（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#hasChildren)无条件返回`true`有时是一种有用的方法。这确保了以后可以为子项检查每个项目，同时尽可能快地进行模型数据的初始填充。唯一的缺点是，在用户尝试查看不存在的子项之前，某些视图中可能会显示没有子项的项目。



### 导航和模型索引创建

分层模型需要提供视图可以调用的函数来导航它们公开的树状结构，并获取项目的模型索引。

#### 父母和孩子

由于暴露给视图的结构由底层数据结构决定，因此通过提供以下函数的实现，每个模型子类都可以创建自己的模型索引：

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [index()](https://doc.qt.io/qt-5/qabstractitemmodel.html#index) | 给定父项的模型索引，此函数允许视图和委托访问该项的子项。如果找不到有效的子项（对应于指定的行，列和父模型索引），则该函数必须返回QModelIndex()，这是一个无效的模型索引。 |
| [parent()](https://doc.qt.io/qt-5/qabstractitemmodel.html#parent) | 提供与任何给定子项的父项对应的模型索引。如果指定的模型索引对应于模型中的顶级项，或者模型中没有有效的父项，则该函数必须返回使用空QModelIndex()构造函数创建的无效模型索引。 |

上述两个函数都使用[createIndex()](https://doc.qt.io/qt-5/qabstractitemmodel.html#createIndex-1)工厂函数为其他要使用的组件生成索引。模型为此函数提供一些唯一标识符是正常的，以确保模型索引可以在以后与其相应的项重新关联。



### 拖放支持和MIME类型处理

模型/视图类支持拖放操作，提供足以满足许多应用程序的默认行为。但是，也可以在拖放操作期间自定义项目的编码方式，默认情况下是否复制或移动项目，以及如何将它们插入到现有模型中。

此外，便捷视图类实现的专业行为应该严格遵循现有开发人员的预期。“ [便捷视图”](https://doc.qt.io/qt-5/model-view-programming.html#convenience-views)部分提供了此行为的概述。

#### MIME数据

默认情况下，内置模型和视图使用内部MIME类型（`application/x-qabstractitemmodeldatalist`）来传递有关模型索引的信息。这指定了项目列表的数据，包含每个项目的行号和列号，以及有关每个项目支持的角色的信息。

使用此MIME类型编码的数据可以通过使用包含要序列化的项的[QModelIndexList](https://doc.qt.io/qt-5/qmodelindex.html#QModelIndexList-typedef)调用[QAbstractItemModel :: mimeData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#mimeData)来获得。

在自定义模型中实现拖放支持时，可以通过重新实现以下功能以特殊格式导出数据项：

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [mimeData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#mimeData) | 可以重新实现此函数以返回默认`application/x-qabstractitemmodeldatalist`内部MIME类型以外的格式的数据。子类可以从基类获取默认的[QMimeData](https://doc.qt.io/qt-5/qmimedata.html)对象，并以其他格式向其添加数据。 |

对于许多模型，提供由MIME类型（如`text/plain`和）表示的通用格式的项目内容很有用`image/png`。请注意，可以使用[QMimeData :: setImageData()](https://doc.qt.io/qt-5/qmimedata.html#setImageData)，[QMimeData :: setColorData](https://doc.qt.io/qt-5/qmimedata.html#setColorData)()和[QMimeData :: setHtml](https://doc.qt.io/qt-5/qmimedata.html#setHtml)()函数轻松地将图像，颜色和HTML文档添加到[QMimeData](https://doc.qt.io/qt-5/qmimedata.html)对象中。

#### 接受拖出的数据

在视图上执行拖放操作时，将查询基础模型以确定它支持哪种类型的操作以及它可以接受的MIME类型。此信息由[QAbstractItemModel :: supportedDropActions](https://doc.qt.io/qt-5/qabstractitemmodel.html#supportedDropActions)（）和[QAbstractItemModel :: mimeTypes](https://doc.qt.io/qt-5/qabstractitemmodel.html#mimeTypes)（）函数提供。不覆盖[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)提供的实现的模型支持复制操作和项的默认内部MIME类型。

当序列化项目数据被放到视图上时，数据将使用其[QAbstractItemModel :: dropMimeData](https://doc.qt.io/qt-5/qabstractitemmodel.html#dropMimeData)（）的实现插入到当前模型中。此函数的默认实现永远不会覆盖模型中的任何数据; 相反，它会尝试将数据项作为项的兄弟项或作为该项的子项插入。

要利用[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)对内置MIME类型的默认实现，新模型必须提供以下功能的重新实现：

|                                                              |                                                              |
| :----------------------------------------------------------: | ------------------------------------------------------------ |
| [insertRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#insertRows) [insertColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#insertColumns) | 这些函数使模型能够使用[QAbstractItemModel :: dropMimeData](https://doc.qt.io/qt-5/qabstractitemmodel.html#dropMimeData)()提供的现有实现自动插入新数据。 |
| [setData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#setData) | 允许使用项填充新的行和列。                                   |
| [setItemData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#setItemData) | 此功能为填充新项目提供了更有效的支持。                       |

要接受其他形式的数据，必须重新实现这些功能：

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [supportedDropActions()](https://doc.qt.io/qt-5/qabstractitemmodel.html#supportedDropActions) | 用于返回[放置操作](https://doc.qt.io/qt-5/qt.html#DropAction-enum)的组合，指示模型接受的拖放操作的类型。 |
| [mimeTypes()](https://doc.qt.io/qt-5/qabstractitemmodel.html#mimeTypes) | 用于返回可由模型解码和处理的MIME类型列表。通常，支持输入到模型中的MIME类型与编码数据以供外部组件使用时可以使用的MIME类型相同。 |
| [dropMimeData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#dropMimeData) | 执行通过拖放操作传输的数据的实际解码，确定将在模型中设置的位置，并在必要时插入新的行和列。如何在子类中实现此函数取决于每个模型公开的数据的要求。 |

如果[dropMimeData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#dropMimeData)函数的实现通过插入或删除行或列来更改模型的维度，或者如果数据项被修改，则必须注意确保发出所有相关信号。简单地调用子类中其他函数的重新实现（例如[setData()](https://doc.qt.io/qt-5/qabstractitemmodel.html#setData)，[insertRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#insertRows)和[insertColumns()](https://doc.qt.io/qt-5/qabstractitemmodel.html#insertColumns) )可能很有用，以确保模型的行为一致。

为了确保拖动操作正常工作，重新实现从模型中删除数据的以下函数非常重要：

- [removeRows（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#removeRows)
- [removeRow（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#removeRow)
- [removeColumns（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#removeColumns)
- [removeColumn（）](https://doc.qt.io/qt-5/qabstractitemmodel.html#removeColumn)

有关使用项目视图进行拖放的更多信息，请参阅[使用项目视图拖放](https://doc.qt.io/qt-5/model-view-programming.html#using-drag-and-drop-with-item-views)。

#### 便利的意见

便捷视图（[QListWidget](https://doc.qt.io/qt-5/qlistwidget.html)，[QTableWidget](https://doc.qt.io/qt-5/qtablewidget.html)和[QTreeWidget](https://doc.qt.io/qt-5/qtreewidget.html)）覆盖默认的拖放功能，以提供适用于许多应用程序的不太灵活但更自然的行为。例如，由于将数据放入[QTableWidget中的](https://doc.qt.io/qt-5/qtablewidget.html)单元格更为常见，将现有内容替换为正在传输的数据，底层模型将设置目标项目的数据，而不是将新行和列插入模型中。有关便捷视图中拖放的更多信息，您可以看到[使用项目视图拖放](https://doc.qt.io/qt-5/model-view-programming.html#using-drag-and-drop-with-item-views)。



### 针对大量数据的性能优化

所述[canFetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#canFetchMore)函数检查父具有可用更多的数据，并返回`true`或假相应。所述[fetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#fetchMore)函数提取根据指定的父数据。这两个函数可以组合在一起，例如，在涉及增量数据的数据库查询中，以填充[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)。我们重新实现[canFetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#canFetchMore)以指示是否有更多数据要提取，[fetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#fetchMore)根据需要填充模型。

另一个例子是动态填充的树模型，我们在扩展树模型中的分支时重新实现[fetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#fetchMore)。

如果重新实现[fetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#fetchMore)会向模型中添加行，则需要调用[beginInsertRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginInsertRows)和[endInsertRows()](https://doc.qt.io/qt-5/qabstractitemmodel.html#endInsertRows)。此外，必须重新实现[canFetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#canFetchMore)和[fetchMore()](https://doc.qt.io/qt-5/qabstractitemmodel.html#fetchMore)，因为它们的默认实现返回false并且不执行任何操作。

------





## 模型/视图类

这些类使用模型/视图设计模式，其中基础数据（在模型中）与用户（在视图中）呈现和操纵数据的方式分开。

|                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [QAbstractItemDelegate](https://doc.qt.io/qt-5/qabstractitemdelegate.html) | 用于显示和编辑模型中的数据项                                 |
| [化QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html) | 项目模型类的抽象接口                                         |
| [QAbstractItemView中](https://doc.qt.io/qt-5/qabstractitemview.html) | 项视图类的基本功能                                           |
| [QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html) | 抽象模型，可以子类化创建一维列表模型                         |
| [QAbstractProxyModel](https://doc.qt.io/qt-5/qabstractproxymodel.html) | 可以执行排序，过滤或其他数据处理任务的代理项模型的基类       |
| [QAbstractTableModel](https://doc.qt.io/qt-5/qabstracttablemodel.html) | 可以子类化以创建表模型的抽象模型                             |
| [QColumnView](https://doc.qt.io/qt-5/qcolumnview.html)       | 列视图的模型/视图实现                                        |
| [QDataWidgetMapper](https://doc.qt.io/qt-5/qdatawidgetmapper.html) | 将数据模型的一部分映射到窗口小部件                           |
| [QFileSystemModel](https://doc.qt.io/qt-5/qfilesystemmodel.html) | 本地文件系统的数据模型                                       |
| [QHeaderView](https://doc.qt.io/qt-5/qheaderview.html)       | 项目视图的标题行或标题列                                     |
| [QIdentityProxyModel](https://doc.qt.io/qt-5/qidentityproxymodel.html) | 代理其源模型未经修改                                         |
| [QItemDelegate](https://doc.qt.io/qt-5/qitemdelegate.html)   | 显示和编辑模型中数据项的工具                                 |
| [QItemEditorCreator](https://doc.qt.io/qt-5/qitemeditorcreator.html) | 使得可以创建项目编辑器创建者基础而无需继承QItemEditorCreatorBase |
| [QItemEditorCreatorBase](https://doc.qt.io/qt-5/qitemeditorcreatorbase.html) | 在实现新项目编辑器创建者时必须进行子类化的抽象基类           |
| [QItemEditorFactory](https://doc.qt.io/qt-5/qitemeditorfactory.html) | 用于在视图和委托中编辑项目数据的小组件                       |
| [QItemSelection](https://doc.qt.io/qt-5/qitemselection.html) | 管理有关模型中所选项目的信息                                 |
| [QItemSelectionModel](https://doc.qt.io/qt-5/qitemselectionmodel.html) | 跟踪视图的选定项目                                           |
| [QItemSelectionRange](https://doc.qt.io/qt-5/qitemselectionrange.html) | 管理有关模型中一系列选定项目的信息                           |
| [QListView](https://doc.qt.io/qt-5/qlistview.html)           | 列表或图标视图到模型上                                       |
| [QListWidget](https://doc.qt.io/qt-5/qlistwidget.html)       | 基于项目的列表小部件                                         |
| [QListWidgetItem](https://doc.qt.io/qt-5/qlistwidgetitem.html) | 用于QListWidget项视图类的项                                  |
| [QModelIndex](https://doc.qt.io/qt-5/qmodelindex.html)       | 用于在数据模型中定位数据                                     |
| [QPersistentModelIndex](https://doc.qt.io/qt-5/qpersistentmodelindex.html) | 用于在数据模型中定位数据                                     |
| [QSortFilterProxyModel](https://doc.qt.io/qt-5/qsortfilterproxymodel.html) | 支持对另一个模型和视图之间传递的数据进行排序和过滤           |
| [QStandardItem](https://doc.qt.io/qt-5/qstandarditem.html)   | 用于QStandardItemModel类的项目                               |
| [QStandardItemEditorCreator](https://doc.qt.io/qt-5/qstandarditemeditorcreator.html) | 无需子类化QItemEditorCreatorBase就可以注册小部件             |
| [QStandardItemModel](https://doc.qt.io/qt-5/qstandarditemmodel.html) | 用于存储自定义数据的通用模型                                 |
| [QStringListModel](https://doc.qt.io/qt-5/qstringlistmodel.html) | 为视图提供字符串的模型                                       |
| [QStyledItemDelegate](https://doc.qt.io/qt-5/qstyleditemdelegate.html) | 显示和编辑模型中数据项的工具                                 |
| [QTableView中](https://doc.qt.io/qt-5/qtableview.html)       | 表视图的默认模型/视图实现                                    |
| [QTableWidget的](https://doc.qt.io/qt-5/qtablewidget.html)   | 具有默认模型的基于项目的表视图                               |
| [QTableWidgetItem](https://doc.qt.io/qt-5/qtablewidgetitem.html) | 用于QTableWidget类的项目                                     |
| [QTableWidgetSelectionRange](https://doc.qt.io/qt-5/qtablewidgetselectionrange.html) | 在不使用模型索引和选择模型的情况下与模型中的选择进行交互的方法 |
| [QTreeView则](https://doc.qt.io/qt-5/qtreeview.html)         | 树视图的默认模型/视图实现                                    |
| [QTreeWidget](https://doc.qt.io/qt-5/qtreewidget.html)       | 使用预定义树模型的树视图                                     |
| [QTreeWidgetItem](https://doc.qt.io/qt-5/qtreewidgetitem.html) | 用于QTreeWidget便利类的项目                                  |
| [QTreeWidgetItemIterator](https://doc.qt.io/qt-5/qtreewidgetitemiterator.html) | 迭代QTreeWidget实例中的项的方法                              |





## 相关实例

- [Dir View](https://doc.qt.io/qt-5/qtwidgets-itemviews-dirview-example.html)
- [Spin Box Delegate](https://doc.qt.io/qt-5/qtwidgets-itemviews-spinboxdelegate-example.html)
- [Pixelator](https://doc.qt.io/qt-5/qtwidgets-itemviews-pixelator-example.html)
- [Simple Tree Model](https://doc.qt.io/qt-5/qtwidgets-itemviews-simpletreemodel-example.html)
- [Chart](https://doc.qt.io/qt-5/qtwidgets-itemviews-chart-example.html)