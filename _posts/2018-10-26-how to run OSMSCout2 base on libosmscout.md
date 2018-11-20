# libosmscout-OSMSCout2 demo test

first, pull down from libosmscout

###check environment variable

```
export QTDIR=/opt/Qt5.9.3/5.9.3/gcc_64
export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PATH
export QT_QPA_PLATFORM_PLUGIN_PATH=$QTDIR/plugins
export PATH=$PATH:$QTDIR/bin
export PATH=$PATH:/home/toolchain/arm-2014.09/bin
export PATH=$PATH:/opt/qt5.9.3-arm-confirm-license/bin
alias qtcreator=/opt/Qt5.9.3/Tools/QtCreator/bin/qtcreator
export CMAKE_PREFIX_PATH=$QTDIR:$CMAKE_PREFIX_PATH
export CMAKE_PREFIX_PATH=$QTDIR/lib/cmake/Qt5Widgets:$CMAKE_PREFIX_PATH
```

installed third-part libs to support Import and OSMScout2 demo.

e.g.

libxml2, libglfw3, libprotobuf, libmarisa, and so on. You can get explicit list when run cmake

### build

```
mkdir ./libosmscout/build
cmake ..
make -j8
sudo make install
```

*libosmscout should be installed to /usr/local/*

###run

open ./libosmscout/OSMScout2/*.pro or CMakeLists.txt in QtCreator

####case .pro

*suggest to comment line of PKGCONFIG, instead of INCLUDE and LIBS. i will try to use PKGCONFIG later.*

```c
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets
INCLUDEPATH += /usr/local/include

LIBS += -L/usr/local/lib -losmscout -losmscout_map -losmscout_map_qt -losmscout_client_qt -lxml2 -lmarisa -losmscout_gpx -losmscout_import
#PKGCONFIG += libosmscout-map-qt libosmscout-client-qt
```

then you should be OK with compile and run

####case CMakeLists.txt

check CMAKE_PREFIX_PATH,

```
find_library(Qml Qwidgets Quick and so on)
```

