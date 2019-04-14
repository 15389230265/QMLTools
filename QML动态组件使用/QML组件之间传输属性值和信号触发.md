

# 各组件之间属性传递

​	在Loader加载组件时使用 property设置属性，该属性即可在加载的动态组件中使用，同样该属性可以在当前组件中使用。

```c++
  	// main.qml
	Loader {
        id: myloader
        anchors.top: parent.top
        width: parent.width
        height: parent.height/4

        source: "qrc:/MyItem.qml"
        property string imageFile: ""
    }
```

```c++
// MyItem.qml
Item {
    id: myItem

    signal imageChanged(string imgfile)
    signal message(string msg)

    Button {
        anchors.fill: parent
        text: "open"
        onClicked: {
            imageDialog.open()
            console.log(imageFile)
            myItem.message("clicked!")
        }
    }

    FileDialog {
        id: imageDialog
        fileMode: FileDialog.OpenFile
        nameFilters: ["图像文件 (*.jpg *.png *.gif)", "全部文件 (*.*)"]
        onFileChanged: {
            imageFile = imageDialog.file
            console.log(imageFile)
            myItem.imageChanged(imageFile)
        }
    }
}
```



# 各组件之间信号触发

使用Connections链接组件。

- 使用Loader动态加载组件时，要连接Loader的item属性
- 对应普通组件，连接对应id
- 对应从C++注册来的单例组件，连接其注册名

```c++
   	//main.qml
	Connections {
        target: myloader.item
        onMessage: {
            console.log(msg)
        }
        onImageChanged: {
            console.log(imgfile)
            image.source = imgfile
        }
    }

    Image {
        id: image
        anchors.bottom: parent.bottom
        width: parent.width
        height: parent.height/4*3
    }
```

