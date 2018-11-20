# how to import contour line to libosmscout

[referencePage](http://libosmscout.sourceforge.net/tutorials/importing/)

flag:[elevationContour](https://github.com/Framstag/libosmscout/issues/647)

when we get .osm file from hgt, then we can import osm with *.osm.pbf to libosmscout.



```c
./Import --typefile ../../stylesheets/map.ost --destinationDirectory \ ../../maps/demChina1/ ../../maps/china-latest.osm.pbf \
../../maps/lon114.00_117.00lat22.00_23.00_local-source.osm
```



*ubuntu16.04 Import后的database不能在项目中加载地图，需要在mac上测试*



after Import contour line database with detail map, must open elevationContour in standard.ost to render contour line.