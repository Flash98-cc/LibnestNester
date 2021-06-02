# Libnest_Nester
请仔细阅读README.md,并一步一步照此修改，不然程序运行不通

Part1：项目配置
  在自己电脑上对Libnester项目配置：

  一、在vs中打开Libnester项目，点击最上方功能栏，依次点击：项目(P)——youtu属性

  二、按照下面顺序依次改为自己的目录

  ​	1.在VC++目录——引用目录：D:\Libnester\include2d

  ​	2.C/C++——附加包含目录：D:\Libnester\include2d；E:\nlopt

  ​	3.链接器——常规——附加库目录：D:\Libnester\include2d；E:\nlopt

  ​	4.链接器——输入——附加依赖项：

  ​		D:\Libnester\thirdparty\debug\libdxfrw.lib

  ​		D:\Libnester\thirdparty\debug\tinyxml2.lib

  ​		E:\nlopt\libnlopt-0.lib

  说明：如果你把项目fork下来之后放在D:\Project目录下，引用目录需要改成：D:\Project\Libnester\include2d;   其他目录照此修改为自己目录。

  E:\nlopt是安装的nlopt的安装位置，按照自己安装nlopt的目录修改。

part2：GUI界面文件说明
  main.cpp:程序运行的启动函数，不需要管；

![Image text](https://raw.githubusercontent.com/Flash98-cc/Libnest_Nester/main/%E4%B8%BB%E9%A1%B5.png)
  MainWindow.hpp和MainWindow.cpp：定义了GUI的主窗口，包括打开nest项目、导入dxf文件、将已有项目保存为nest后缀项目等操作,见上图；

  qimportdlg.hpp和qimportdlg.cpp：定义了导入dxf文件的窗口和一系列操作函数，如图；
![Image text](https://raw.githubusercontent.com/Flash98-cc/Libnest_Nester/main/import.png)

  qprjwidget.hpp和qprjwidget.cpp:主要包含导入dxf文件之后的主页面通过设置参数之后调用进行排样，如图；
![Image text](https://raw.githubusercontent.com/Flash98-cc/Libnest_Nester/main/MainWindows.png)
QPrjwidget:QPrjwidget()：主要用代码的方法绘制界面，如果想了解主界面每一个按钮在构造函数里面找到对应函数即可，比如说runBtn为例
![Image text](https://raw.githubusercontent.com/Flash98-cc/Libnest_Nester/main/run.png)
需要了解connect函数和singal/slot信号槽机制，当runBtn点击会发出click()信号，然后由当前对象接受并执行run函数；
在QPrjwidget.cpp能找到主页面的所有功能对应的函数，可以用断点调试的方法快速熟悉项目。
QPrjWidget::run()是进行排样的入口函数，在run函数里面主要的工作包括：首先将从GUI导入的dxf文件通过libdxfrw解析之后放在了uiPrj_,遍历uiPrj_的shapes()返回图形，用std::vector<Item> input
来存储每一个图形的顶点信息。如下图  
![Image text](https://raw.githubusercontent.com/Flash98-cc/Libnest_Nester/main/uiprj_.png)
接下来通过调用libnest2d::nest(input, Box(mm(wh.height()), mm(wh.width())), 0, config,ProgressFunction{ [](unsigned cnt) {std::cout << "parts left: " << cnt << std::endl;)进行排样，图形数据放在std::vector<Item> Input，Box是原材料的尺寸(height是水平方向一般是无限长，width是竖直方向一般是确定值，0是裁片之间的间距，config是不同排样策略组合)然后就调用libnest2d.hpp:123行的模板nest函数，之后调用 libnest2d:hpp:89行模板nest函数，虽然函数名字一样，但是传过来的参数不一样，在初期看的时候最好用断点调试一步一步的执行看如何调用。
此外在libnest2D项目里面设计模板类、模板函数、模板结构体、typedef重命名等c++知识最好重点学习，不然看起来很费劲。
经过libnest2D排样之后的结果放在pgrp中，在run函数剩下部分主要是构造数据结构将libnest2D的排样结果返回在GUI上，需要构造出一个ui_nestsolution_s对象，以及ui_placement_s子对象，
之后调用createQSolution(uslt)函数将排样结果画在GUI上面，此处需要学习QT用Qpaint、Qpath、Qgraphicsview等组合绘图。
  
part3：libnest2D文件说明
  首先对libnest2D项目说明下，libnest2D是github上(https://github.com/tamasmeszaros/libnest2d) 的开源项目，由于该项目是用cmake管理，但由于vs无法在github下载相关文件编译不通，最后选择新建解决方案，通过添加现有文件把libnest2D的文件添加进去。(只包含libnest2D无GUI的版本也会发给你)。
  libnest2D有三种选择策略（DJD、FIRSTFIT、FILLER）和两种放置策略（NFPPlacer和BottomLeft），Filler函数有问题未解决，将选择和放置策略进行组合共有四种可能，libnest2D_UML.vsdx文件是我用visio画的libnest2D的流程图，流程图只画了NFP+FirstFit组合的运行过程，其他组合一样，仅供参考。
