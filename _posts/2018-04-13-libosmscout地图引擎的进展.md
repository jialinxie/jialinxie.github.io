# libosmscout地图引擎验证过程

libosmscout是非常强大的跨平台、离线矢量地图引擎渲染库，支持OpenStreetMap的原生地图格式，.osm或.osm.pbf，具有路径规划，并且支持多种交通工具的规划。

## 1.开发环境验证

无论在osx还是ubuntu上验证，都需要参考官网给的building dependence, 注意使用CMAKE最新版，V3.11，环境安装完之后，在osx/ubuntu均可以完整编译。

​	1.OSX 10.13.3 , build OK,install OK

​	2.Ubuntu 16.04, build OK

## 2.demo验证

​	1.OSMScout2

​	启动demo时需要输入 map-dictionary, style-sheet, translator file..etc,实际在osx测试时，出现	Can't load translator for locale QLocale,然后打开在线地图，然而并没有正常显示，可能是网络地址问题。

​	已经把这个问题发到libosmscout的issue上，等待回复。

​	1.其它没有窗口显示的工具都可以正常运行。



####已经得到回复

原因是缺少marisa库，在Import地图文件时没有生成text索引文件，所以未加载地图

在github上clone了marisa之后，编译，安装，即可顺利运行OSMScout2 Demo.

Great!