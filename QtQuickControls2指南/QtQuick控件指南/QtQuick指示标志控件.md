# Indicator Controls



Qt Quick Controls 2提供一系列类似指示器的控件。

|                                                              |                                  |
| ------------------------------------------------------------ | -------------------------------- |
| [BusyIndicator](https://doc.qt.io/qt-5/qml-qtquick-controls2-busyindicator.html) | 表示后台活动，例如，在加载内容时 |
| [PageIndicator](https://doc.qt.io/qt-5/qml-qtquick-controls2-pageindicator.html) | 表示当前活动的页面               |
| [ProgressBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-progressbar.html) | 表示操作的进度                   |
| [ScrollBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-scrollbar.html) | 垂直或水平交互式滚动条           |
| [ScrollIndicator](https://doc.qt.io/qt-5/qml-qtquick-controls2-scrollindicator.html) | 垂直或水平非交互式滚动指示器     |

每种类型的指标都有自己特定的目标用例。以下部分提供了根据用例选择适当类型的指标的指南。



## BusyIndicator Control

![img](QtQuick指示标志控件.assets/qtquickcontrols2-busyindicator.png)

[BusyIndicator](https://doc.qt.io/qt-5/qml-qtquick-controls2-busyindicator.html) 可用于显示正在进行的操作，以及UI必须等待操作完成。



## PageIndicator Control

![img](QtQuick指示标志控件.assets/qtquickcontrols2-pageindicator.png)

[PageIndicator](https://doc.qt.io/qt-5/qml-qtquick-controls2-pageindicator.html) 用于指示多页容器中当前活动的页面。



## ProgressBar Control



![img](QtQuick指示标志控件.assets/qtquickcontrols2-progressbar.gif)



[ProgressBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-progressbar.html) 表示操作的进度。该值应定期更新。



## ScrollBar Control



![img](QtQuick指示标志控件.assets/qtquickcontrols2-scrollbar.gif)



[ScrollBar](https://doc.qt.io/qt-5/qml-qtquick-controls2-scrollbar.html) 是一个交互式栏，指示当前滚动位置，可用于滚动到[Flickable中](https://doc.qt.io/qt-5/qml-qtquick-flickable.html)的特定位置。



## ScrollIndicator Control



![img](QtQuick指示标志控件.assets/qtquickcontrols2-scrollindicator.gif)

[ScrollIndicator](https://doc.qt.io/qt-5/qml-qtquick-controls2-scrollindicator.html) 是一个非交互式指示器，指示当前滚动位置，可用于滚动到[Flickable中](https://doc.qt.io/qt-5/qml-qtquick-flickable.html)的特定位置。