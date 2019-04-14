------

[TOC]

------



# 使用带有Qt快速视图的C ++模型



## 自定义C ++模型中提供的数据

模型可以用C ++定义，然后可用于QML。这对于将现有C ++数据模型或其他复杂数据集公开给QML非常有用。

C ++模型类可以定义为[QStringList](https://doc.qt.io/qt-5/qstringlist.html)，[QVariantList](https://doc.qt.io/qt-5/qvariant.html#QVariantList-typedef)，[QObjectList](https://doc.qt.io/qt-5/qobject.html#QObjectList-typedef)或[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)。前三个对于公开更简单的数据集很有用，而[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)为更复杂的模型提供了更灵活的解决方案。

这是一个视频教程，带您了解将C ++模型公开给QML的整个过程：



<iframe allowfullscreen="" frameborder="0" src="https://www.youtube.com/embed/9BcAYDlpuT8" style="margin: 0px; padding: 0px; border: 0px; font-size: 16px; vertical-align: baseline; width: 536px; height: 322px;"></iframe>



### 基于QStringList的模型

模型可以是简单的[QStringList](https://doc.qt.io/qt-5/qstringlist.html)，它通过*modelData*角色提供列表的内容。

这是一个带有委托的[ListView](https://doc.qt.io/qt-5/qml-qtquick-listview.html)，它使用`modelData`角色引用其模型项的值：

```c++
ListView {
    width: 100; height: 100

    model: myModel
    delegate: Rectangle {
        height: 25
        width: 100
        Text { text: modelData }
    }
}
```

Qt应用程序可以加载此QML文档并将值设置`myModel`为[QStringList](https://doc.qt.io/qt-5/qstringlist.html)：

```c++
    QStringList dataList;
    dataList.append("Item 1");
    dataList.append("Item 2");
    dataList.append("Item 3");
    dataList.append("Item 4");

    QQuickView view;
    QQmlContext *ctxt = view.rootContext();
    ctxt->setContextProperty("myModel", QVariant::fromValue(dataList));
```

Qt安装目录中的[examples / quick / models / stringlistmodel中](https://doc.qt.io/qt-5/qtquick-models-stringlistmodel-example.html)提供了此示例的完整源代码。

**注意：**视图无法知道[QStringList](https://doc.qt.io/qt-5/qstringlist.html)的内容已更改。如果[QStringList](https://doc.qt.io/qt-5/qstringlist.html)发生更改，则需要通过再次调用[QQmlContext :: setContextProperty](https://doc.qt.io/qt-5/qqmlcontext.html#setContextProperty)()来重置模型。



### 基于QVariantList的模型

模型可以是单个[QVariantList](https://doc.qt.io/qt-5/qvariant.html#QVariantList-typedef)，它通过*modelData*角色提供列表的内容。

API的工作方式与[QStringList](https://doc.qt.io/qt-5/qstringlist.html)类似，如上一节所示。

**注意：**视图无法知道[QVariantList](https://doc.qt.io/qt-5/qvariant.html#QVariantList-typedef)的内容已更改。如果[QVariantList](https://doc.qt.io/qt-5/qvariant.html#QVariantList-typedef)发生更改，则需要重置模型。



### 基于QObjectList的模型

[QObject](https://doc.qt.io/qt-5/qobject.html) *值列表也可以用作模型。[QList](https://doc.qt.io/qt-5/qlist.html) < [QObject的](https://doc.qt.io/qt-5/qobject.html) *>提供在列表中作为角色对象的属性。

以下应用程序创建一个`DataObject`具有[Q_PROPERTY](https://doc.qt.io/qt-5/qobject.html#Q_PROPERTY)值的类，当[QList](https://doc.qt.io/qt-5/qlist.html) <DataObject *>向QML公开时，这些值可作为命名角色访问：

```c++
class DataObject : public QObject
{
    Q_OBJECT

    Q_PROPERTY(QString name READ name WRITE setName NOTIFY nameChanged)
    Q_PROPERTY(QString color READ color WRITE setColor NOTIFY colorChanged)
    ...
};

int main(int argc, char ** argv)
{
    QGuiApplication app(argc, argv);

    QList<QObject*> dataList;
    dataList.append(new DataObject("Item 1", "red"));
    dataList.append(new DataObject("Item 2", "green"));
    dataList.append(new DataObject("Item 3", "blue"));
    dataList.append(new DataObject("Item 4", "yellow"));

    QQuickView view;
    view.setResizeMode(QQuickView::SizeRootObjectToView);
    QQmlContext *ctxt = view.rootContext();
    ctxt->setContextProperty("myModel", QVariant::fromValue(dataList));
    ...
```

该[QObject的](https://doc.qt.io/qt-5/qobject.html) *可作为`modelData`属性。为方便起见，对象的属性也可以直接在委托的上下文中使用。这里，`view.qml`引用[ListView](https://doc.qt.io/qt-5/qml-qtquick-listview.html)委托中的`DataModel`属性：

```c++
ListView {
    width: 100; height: 100

    model: myModel
    delegate: Rectangle {
        height: 25
        width: 100
        color: model.modelData.color
        Text { text: name }
    }
}
```

注意使用`color`带限定符的属性。对象的属性不会在对象中复制`model`，因为它们可以通过`modelData`对象轻松获得。

Qt安装目录中的[examples / quick / models / objectlistmodel中](https://doc.qt.io/qt-5/qtquick-models-objectlistmodel-example.html)提供了此示例的完整源代码。

注意：视图无法知道[QList](https://doc.qt.io/qt-5/qlist.html)的内容已更改。如果[QList](https://doc.qt.io/qt-5/qlist.html)发生变化，则需要通过再次调用[QQmlContext :: setContextProperty](https://doc.qt.io/qt-5/qqmlcontext.html#setContextProperty)()来重置模型。



### QAbstractItemModel子类

可以通过[继承QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)来定义模型。如果您拥有其他方法无法支持的更复杂模型，这是最佳方法。甲[化QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)还可以自动通知QML视图当模型数据改变。

通过重新实现[QAbstractItemModel :: roleNames](https://doc.qt.io/qt-5/qabstractitemmodel.html#roleNames)()，可以将[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)子类的角色公开给QML 。

这是一个名为[QAbstractListModel](https://doc.qt.io/qt-5/qabstractlistmodel.html)子类的应用程序`AnimalModel`，它公开了*类型*和*大小的*角色。它重新实现[QAbstractItemModel :: roleNames](https://doc.qt.io/qt-5/qabstractitemmodel.html#roleNames)（）以公开角色名称，以便可以通过QML访问它们：

```c++
class Animal
{
public:
    Animal(const QString &type, const QString &size);
    ...
};

class AnimalModel : public QAbstractListModel
{
    Q_OBJECT
public:
    enum AnimalRoles {
        TypeRole = Qt::UserRole + 1,
        SizeRole
    };

    AnimalModel(QObject *parent = 0);
    ...
};

QHash<int, QByteArray> AnimalModel::roleNames() const {
    QHash<int, QByteArray> roles;
    roles[TypeRole] = "type";
    roles[SizeRole] = "size";
    return roles;
}

int main(int argc, char ** argv)
{
    QGuiApplication app(argc, argv);

    AnimalModel model;
    model.addAnimal(Animal("Wolf", "Medium"));
    model.addAnimal(Animal("Polar bear", "Large"));
    model.addAnimal(Animal("Quoll", "Small"));

    QQuickView view;
    view.setResizeMode(QQuickView::SizeRootObjectToView);
    QQmlContext *ctxt = view.rootContext();
    ctxt->setContextProperty("myModel", &model);
    ...
```

此模型由访问*类型*和*大小*角色的[ListView](https://doc.qt.io/qt-5/qml-qtquick-listview.html)委托显示：

```c++
ListView {
    width: 200; height: 250

    model: myModel
    delegate: Text { text: "Animal: " + type + ", " + size }
}         
```

模型更改时，QML视图会自动更新。请记住，模型必须遵循模型更改的标准规则，并在模型更改时通过使用[QAbstractItemModel :: dataChanged](https://doc.qt.io/qt-5/qabstractitemmodel.html#dataChanged)()，[QAbstractItemModel :: beginInsertRows](https://doc.qt.io/qt-5/qabstractitemmodel.html#beginInsertRows)()等通知视图。有关更多信息，请参阅[Model子类化参考](https://doc.qt.io/qt-5/model-view-programming.html#model-subclassing-reference)。

Qt安装目录中的[examples / quick / models / abstractitemmodel中](https://doc.qt.io/qt-5/qtquick-models-abstractitemmodel-example.html)提供了此示例的完整源代码。

[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)呈现表的层次结构，但QML当前提供的视图只能显示列表数据。要显示分层模型的子列表，请使用[DelegateModel](https://doc.qt.io/qt-5/qml-qtqml-models-delegatemodel.html) QML类型，该类型提供以下属性和函数以与[QAbstractItemModel](https://doc.qt.io/qt-5/qabstractitemmodel.html)类型的列表模型一起使用：

- *hasModelChildren*角色属性，用于确定节点是否具有子节点。
- [DelegateModel :: rootIndex](https://doc.qt.io/qt-5/qml-qtqml-models-delegatemodel.html#rootIndex-prop)允许指定根节点
- [DelegateModel :: modelIndex](https://doc.qt.io/qt-5/qml-qtqml-models-delegatemodel.html#modelIndex-method)()返回一个[QModelIndex](https://doc.qt.io/qt-5/qmodelindex.html)，可以分配给[DelegateModel :: rootIndex](https://doc.qt.io/qt-5/qml-qtqml-models-delegatemodel.html#rootIndex-prop)
- [DelegateModel :: parentModelIndex](https://doc.qt.io/qt-5/qml-qtqml-models-delegatemodel.html#parentModelIndex-method)()返回一个[QModelIndex](https://doc.qt.io/qt-5/qmodelindex.html)，可以分配给[DelegateModel :: rootIndex](https://doc.qt.io/qt-5/qml-qtqml-models-delegatemodel.html#rootIndex-prop)



### SQL模型

Qt提供了支持SQL数据模型的C ++类。这些类在底层SQL数据上透明地工作，减少了对基本SQL操作（如创建，插入或更新）运行SQL查询的需要。有关这些类的更多详细信息，请参阅[使用SQL模型类](https://doc.qt.io/qt-5/sql-model.html)。

尽管C ++类提供了对SQL数据进行操作的完整功能集，但它们不提供对QML的数据访问。因此，您必须将C ++自定义数据模型实现为其中一个类的子类，并将其作为类型或上下文属性公开给QML。



#### 只读数据模型

自定义模型必须重新实现以下方法才能对QML中的数据进行只读访问：

- roleName()将角色名称公开给QML前端。例如，以下版本将选定表的字段名称作为角色名称返回：

  ```c++
   QHash<int, QByteArray> SqlQueryModel::roleNames() const
   {
      QHash<int, QByteArray> roles;
      // record() returns an empty QSqlRecord
      for (int i = 0; i < this->record().count(); i ++) {
          roles.insert(Qt::UserRole + i + 1, record().fieldName(i).toUtf8());
      }
      return roles;
  }
  ```

- data()将SQL数据公开给QML前端。例如，以下实现返回给定模型索引的数据：

  ```
  QVariant SqlQueryModel::data(const QModelIndex &index, int role) const
  {
      QVariant value;
  
      if (index.isValid()) {
          if (role < Qt::UserRole) {
              value = QSqlQueryModel::data(index, role);
          } else {
              int columnIdx = role - Qt::UserRole - 1;
              QModelIndex modelIndex = this->index(index.row(), columnIdx);
              value = QSqlQueryModel::data(modelIndex, Qt::DisplayRole);
          }
      }
      return value;
  }
  ```

该[QSqlQueryModel](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#qsqlquerymodel)类是不够好，实现自定义的只读模式，代表了一个SQL数据库中的数据。例如该[聊天教程](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html)，通过实现自定义模式从获取联系方式说明了这一点很好[的SQLite](https://doc.qt.io/qt-5/qtsql-attribution-sqlite.html#sqlite)数据库。



#### 可编辑的数据模型

[QSqlTableModel](https://doc.qt.io/qt-5/qtquickcontrols-chattutorial-example.html#qsqltablemodel)实现了setData()[如下所述](https://doc.qt.io/qt-5/qtquick-modelviewsdata-cppmodels.html#changing-model-data)。

根据模型使用的[EditStrategy](https://doc.qt.io/qt-5/qsqltablemodel.html#EditStrategy-enum)，更改要么排队等待以后提交，要么立即提交。

您还可以通过调用[QSqlTableModel :: insertRecord](https://doc.qt.io/qt-5/qsqltablemodel.html#insertRecord)()将新数据插入到模型中。在以下示例代码段中，将使用书籍详细信息填充[QSqlRecord](https://doc.qt.io/qt-5/qsqlrecord.html)并将其附加到模型中：

```c++
...
QSqlRecord newRecord = record();
newRecord.setValue("author", "John Grisham");
newRecord.setValue("booktitle", "The Litigators");
insertRecord(rowCount(), newRecord);
...
```



### 将C ++数据模型公开给QML

上面的示例使用[QQmlContext :: setContextProperty](https://doc.qt.io/qt-5/qqmlcontext.html#setContextProperty)()直接在QML组件中设置模型值。另一种方法是将C ++模型类注册为QML类型（[直接](https://doc.qt.io/qt-5/qtqml-cppintegration-overview.html#defining-qml-types-from-c)从C ++入口点或在[QML C ++插件](https://doc.qt.io/qt-5/qtqml-modules-cppplugins.html)的初始化函数中注册，如下所示）。这将允许模型类直接作为QML中的类型创建：

C++

```c++
class MyModelPlugin : public QQmlExtensionPlugin
{
    Q_OBJECT
    Q_PLUGIN_METADATA(IID "org.qt-project.QmlExtension.MyModel" FILE "mymodel.json")
public:
    void registerTypes(const char *uri)
    {
        qmlRegisterType<MyModel>(uri, 1, 0, "MyModel");
    }
}
```

QML

```c++
MyModel {
    id: myModel
    ListElement { someProperty: "some value" }
}

ListView {
    width: 200; height: 250
    model: myModel
    delegate: Text { text: someProperty }
}
```



有关[编写QML](https://doc.qt.io/qt-5/qtqml-tutorials-extending-qml-example.html) C ++插件的详细信息，请参阅[使用C ++编写QML扩展](https://doc.qt.io/qt-5/qtqml-tutorials-extending-qml-example.html)。



### 更改模型数据

除了`roleNames()`和之外`data()`，可编辑模型必须重新实现[setData](https://doc.qt.io/qt-5/qabstractitemmodel.html#setData)方法以保存对现有模型数据的更改。以下版本的方法检查给定的模型索引是否有效且是否`role`等于[Qt :: EditRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)：

```c++
bool EditableModel::setData(const QModelIndex &index, const QVariant &value, int role)
{
    if (index.isValid() && role == Qt::EditRole) {
        // Set data in model here. It can also be a good idea to check whether
        // the new value actually differs from the current value
        if (m_entries[index.row()] != value.toString()) {
            m_entries[index.row()] = value.toString();
            emit dataChanged(index, index, { Qt::EditRole, Qt::DisplayRole });
            return true;
        }
    }
    return false;
}
```

**注意：**保存更改后发出[dataChanged](https://doc.qt.io/qt-5/qabstractitemmodel.html#dataChanged)（）信号很重要。

与C ++项目视图（如[QListView](https://doc.qt.io/qt-5/qlistview.html)或[QTableView](https://doc.qt.io/qt-5/qtableview.html)）不同，`setData()`必须在适当的时候从QML委托中显式调用该方法。只需将新值分配给相应的模型属性即可完成此操作。

```c++
ListView {
    anchors.fill: parent
    model: EditableModel {}
    delegate: TextField {
        width: ListView.view.width
        text: model.edit
        onAccepted: model.edit = text
    }
}
```

**注意：**该`edit`角色等于[Qt :: EditRole](https://doc.qt.io/qt-5/qt.html#ItemDataRole-enum)。有关内置角色名称，请参阅[roleNames](https://doc.qt.io/qt-5/qabstractitemmodel.html#roleNames)()。但是，现实生活模型通常会注册自定义角色。