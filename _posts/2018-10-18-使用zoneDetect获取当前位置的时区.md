# 使用zoneDetect获取当前位置的时区

获取时区的方案分为在线版和离线版，在嵌入式设备上一般使用离线版本。参考信息如下：

**Time Zone Location Web Services**

- [Google Maps Time Zone API](https://developers.google.com/maps/documentation/timezone/)
- [Microsoft Azure Maps Time Zone API](https://docs.microsoft.com/en-us/rest/api/maps/timezone)
- [GeoNames Time Zone API](http://www.geonames.org/export/web-services.html#timezone)
- [TimeZoneDB API](https://timezonedb.com/api)
- [AskGeo](https://askgeo.com/) - commercial (but [arguably more accurate than GeoNames](https://stackoverflow.com/a/5519523/634824))
- [GeoGarage Time Zone API](https://www.geogarage.com/blog/news-1/post/geogarage-time-zone-api-31) - commercial, focusing on Nautical time zones.

**Raw Time Zone Boundary Data**

- [Timezone Boundary Builder](https://github.com/evansiroky/timezone-boundary-builder) - builds time zone shapefiles from OpenStreetMaps map data. Includes territorial waters near coastlines.

The following projects have previously been sources of time zone boundary data, but are no longer actively maintained.

- [tz_world](http://efele.net/maps/tz/world/) - the original shapefile data from Eric Muller
- [whereonearth-timezone](https://github.com/straup/whereonearth-timezone) - GeoJSON version with WOEDB data merged in

**Time Zone Geolocation Offline Implementations**

Implementations that use the Timezone Boundary Builder data

- [node-geo-tz](https://github.com/evansiroky/node-geo-tz) - JavaScript library
- [tz-lookup](https://github.com/darkskyapp/tz-lookup/) - JavaScript library
- [GeoTimeZone](https://github.com/mj1856/GeoTimeZone) - .NET library
- [timezonefinder](https://github.com/MrMinimal64/timezonefinder) - Python library
- [ZoneDetect](https://github.com/BertoldVdb/ZoneDetect) - C library
- [Timeshape](https://github.com/RomanIakovlev/timeshape) - Java library

Implementations that use the older tz_world data

- [latlong](https://github.com/bradfitz/latlong) - Go library (Read [this post](https://plus.google.com/u/0/+BradFitzpatrick/posts/XVyy1bAzkZd) also.)
- [TimeZoneMapper](https://sites.google.com/a/edval.biz/www/mapping-lat-lng-s-to-timezones) - Java library
- [tzwhere](https://github.com/mattbornski/tzwhere) - JavaScript/Node library
- [pytzwhere](https://github.com/pegler/pytzwhere) - Python library
- [timezone_finder](https://github.com/gunyarakun/timezone_finder) - Ruby library
- [LatLongToTimeZone](https://github.com/drtimcooper/LatLongToTimezone) - Java and Swift libraries
- [What Time is it here?](https://derickrethans.nl/what-time-is-it.html) - Blog post describing PHP and MongoDB

**Libraries that call one of the web services**

- [timezone](https://rubygems.org/gems/timezone) - Ruby gem that calls GeoNames
- [AskGeo](https://askgeo.com/#libraries) has its own libraries for calling from Java or .Net
- [GeoNames](http://www.geonames.org/export/client-libraries.html) has client libraries for just about everything

**Other Ideas**

- Find the nearest city [with an R-Tree](https://stackoverflow.com/a/5584826/634824)
- Find the nearest city [with MySQL](https://stackoverflow.com/a/13849653/634824)

*Please update this list if you know of any others*

Also, note that the nearest-city approach may not yield the "correct" result, just an approximation.

**Conversion To Windows Zones**

Most of the methods listed will return an IANA time zone id. If you need to convert to a Windows time zone for use with the `TimeZoneInfo` class in .NET, use the [TimeZoneConverter](https://github.com/mj1856/TimeZoneConverter) library.

**Don't use zone.tab**

The [tz database](https://en.wikipedia.org/wiki/Tz_database) includes a file called [`zone.tab`](https://en.wikipedia.org/wiki/Zone.tab). This file is primarily used to present a list of time zones for a user to pick from. It includes the latitude and longitude coordinates for the point of reference for each time zone. This allows a map to be created highlighting these points. For example, see the interactive map shown on [the moment-timezone home page](https://momentjs.com/timezone/).

While it may be tempting to use this data to resolve the time zone from a latitude and longitude coordinates, consider that these are points - not boundaries. The best one could do would be to determine the *closest* point, which in many cases will not be the correct point.

Consider the following example:

​                            ![Time Zone Example Art](https://i.stack.imgur.com/tjd5u.png)

The two squares represent different time zones, where the black dot in each square is the reference location, such as what can be found in zone.tab. The blue dot represents the location we are attempting to find a time zone for. Clearly, this location is within the orange zone on the left, but if we just look at closest distance to the reference point, it will resolve to the greenish zone on the right.

[share](https://stackoverflow.com/a/16086964)[improve this answer](https://stackoverflow.com/posts/16086964/edit)

[edited May 11 at 3:58](https://stackoverflow.com/posts/16086964/revisions)



通过查阅上述的开源库，js版，python版和c版都已经测试成功，java和.net由于开发环境限制暂时不考虑测试。

**js版测试步骤**

只需要npm install [库名称]，然后编写.js文件，使用node [.js文件]即可运行并检查结果。

**C版-ZoneDetect-测试步骤**

需要先编译出library中的动态库

```c
make
```



生成libzoneDetect.so, 然后编译demo.，

```c
gcc -o demo dmeo.c -L. -lzoneDetect
```



然后执行测试

```c
./demo ../library/*.bin lat lon
```



*c版-ZoneDetect-交叉编译测试*

使用arm-linux-gnueabihf-gcc交叉编译工具链编译

make之前，修改Makefile中的CC

```c
CC=arm-linux-gnueabihf-gcc
```

然后make生成so库

编译测试文件时，注意需要带上-lm(math库)

```c
gcc -o demo demo.c -L. -lzoneDetect -lm
```

在设备上可以执行demo,还需要添加时区名->utc差值的接口，可参考python版本的实现思路