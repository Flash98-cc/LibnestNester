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

main.cpp:程序运行的启动函数；

MainWindow.hpp和MainWindow.cpp：

