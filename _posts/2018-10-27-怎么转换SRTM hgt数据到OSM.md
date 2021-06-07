# 怎么转换SRTM hgt数据到OSM

​																	-jacky

推荐使用工具	[phyghtmap](https://wiki.openstreetmap.org/wiki/Phyghtmap) [主页](http://katze.tfiu.de/projects/phyghtmap/) [使用手册](https://www.mankier.com/1/phyghtmap)

参考环境

Ubuntu16.04 python3.5



## 第一步 搭建使用环境

#### 搭建python3.5开发环境

####验证

首先验证当前python版本，验证方法如下：

打开终端(control + alt + T),输入

这是python2.7的版本，phyghtmap不依赖2.7的版本，只需要3.x的版本

```
jacklin@ubuntu:~$ python --version
Python 2.7.12
```

这是python3.5.2的版本, phyghtmap依赖3.x的版本，所以此版本必须安装，建议>=3.0即可

```
jacklin@ubuntu:~$ python3 --version
Python 3.5.2
```

如果验证python版本号是3.x，则参考如下Case 0

如果验证python3 没有版本号，则参考如下Case1

##### case 0

可能当前只安装了python3.x，而且没有修改环境变量，所以只需要给python3换个别名即可

```
vim ~/.bash_rc
alias python3=你的python3完整路径
```

##### case1

可能没有安装python3，所以安装即可

```
sudo apt-get install python3
```

*安装完后继续回去验证*

安装好了继续第二步

## 第二步 下载phyghtmap

- Source distibution: [phyghtmap_2.20.orig.tar.gz](http://katze.tfiu.de/projects/phyghtmap/phyghtmap_2.20.orig.tar.gz)

下载phyghtmap_2.20.orig.tar.gz,默认放在~/Downlaod目录下

```
jacklin@ubuntu:~/Downloads$ ls *.tar.gz
phyghtmap_2.20.orig.tar.gz
jacklin@ubuntu:~/Downloads$ tar zxvf phyghtmap_2.20.orig.tar.gz
phyghtmap-2.20/
phyghtmap-2.20/MANIFEST.in
phyghtmap-2.20/docs/
phyghtmap-2.20/docs/Makefile
phyghtmap-2.20/docs/phyghtmap.1.html
phyghtmap-2.20/docs/phyghtmap.1
phyghtmap-2.20/docs/phyghtmap.help2man
phyghtmap-2.20/phyghtmap/
phyghtmap-2.20/phyghtmap/configUtil.py
phyghtmap-2.20/phyghtmap/hgt.py
phyghtmap-2.20/phyghtmap/pbfUtil.py
phyghtmap-2.20/phyghtmap/o5mUtil.py
phyghtmap-2.20/phyghtmap/varint.py
phyghtmap-2.20/phyghtmap/__init__.py
phyghtmap-2.20/phyghtmap/NASASRTMUtil.py
phyghtmap-2.20/phyghtmap/osmUtil.py
phyghtmap-2.20/phyghtmap/main.py
phyghtmap-2.20/PKG-INFO
phyghtmap-2.20/INSTALL
phyghtmap-2.20/setup.py
phyghtmap-2.20/Changelog
phyghtmap-2.20/README
phyghtmap-2.20/COPYRIGHT
phyghtmap-2.20/LICENSE_GPL-2
解压完毕，进入解压的目录
jacklin@ubuntu:~/Downloads$ cd phyghtmap-2.20/
查看解压的文件
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ ls
build      dist     LICENSE_GPL-2  phyghtmap.egg-info  setup.py
Changelog  docs     MANIFEST.in    PKG-INFO
COPYRIGHT  INSTALL  phyghtmap      README
```



## 第三步 安装phtghtmap

```
查看安装说明
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ cat INSTALL 
The installation requires a working python setuptools installation.
To install it on a debian-like system say apt-get install python3-setuptools.
The program itself should run with python >= 3.0.

Some additional requirements have to be installed on your system in order to run
phyghtmap:

* python3-pkg-resources (on Debian-like systems, say apt-get install
  python3-pkg-resources)
* python3-matplotlib (on Debian-like systems, say apt-get install
	python3-matplotlib)
* python3-bs4 (on Debian-like systems, say apt-get install
	python-bs4)
* python3-numpy (on Debian-like systems, say apt-get install python-numpy)

To install phyghtmap, finally say

sudo python setup.py install

which will install the stuff on your system.
安装上述依赖库
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ sudo apt-get install python3-setuptools
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ sudo apt-get install python3-pkg-resources
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ sudo apt-get install python3-matplotlib
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ sudo apt-get install python3-bs4
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ sudo apt-get install python3-numpy
安装完依赖库，安装phyghtmap
jacklin@ubuntu:~/Downloads/phyghtmap-2.20$ sudo python setup.py install
```



#### 验证phyghtmap版本

```
jacklin@ubuntu:~/Downloads$ phyghtmap -v
phyghtmap 2.20
```

出现版本号即可使用

## 第四步 转换SRTM hgt数据

自行下载hgt文件到一个指定文件夹，此处假设hgt文件夹路径在~/Download

```
jacklin@ubuntu:~/Documents/srtm$ cd ~/Downloads/
jacklin@ubuntu:~/Downloads$ ls
hgt2Osm
jacklin@ubuntu:~/Downloads$ cd hgt2Osm/
jacklin@ubuntu:~/Downloads/hgt2Osm$ ls
N22E114.hgt  N22E115.hgt  N22E116.hgt
可见文件夹内有3个hgt文件
现在转换这3个hgt文件为osm文件，保存在当前目录
如下命令中，5是等高线最小单位，数字越小，产生的osm文件越大，可自行调整20 100 400 等数据
jacklin@ubuntu:~/Downloads/hgt2Osm$ phyghtmap -s 5 --start-node-id=$((1<<33)) --start-way-id=$((1<<33)) --max-nodes-per-tile=0 ./*.hgt
hgt file ./N22E114.hgt: 1201 x 1201 points, bbox: (114.00000, 22.00000, 115.00000, 23.00000)
hgt file ./N22E115.hgt: 1201 x 1201 points, bbox: (115.00000, 22.00000, 116.00000, 23.00000)
hgt file ./N22E116.hgt: 1201 x 1201 points, bbox: (116.00000, 22.00000, 117.00000, 23.00000)
jacklin@ubuntu:~/Downloads/hgt2Osm$ ls -l
total 357612
-rw-rw-r-- 1 jacklin jacklin 357525637 10月 27 17:46 lon114.00_117.00lat22.00_23.00_local-source.osm
-rw-rw-r-- 1 jacklin jacklin   2884802 10月 27 17:42 N22E114.hgt
-rw-rw-r-- 1 jacklin jacklin   2884802 10月 27 17:42 N22E115.hgt
-rw-rw-r-- 1 jacklin jacklin   2884802 10月 27 17:42 N22E116.hgt

```

#### 验证

上述文件lon114.00_117.00lat22.00_23.00_local-source.osm即转换完成的osm文件，需要使用FYIII的地图导入工具-Import，和详图一起导入，才能在FYIII中加载，导入教程的中文版下次再出



