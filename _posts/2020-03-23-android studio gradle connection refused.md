这个问题困扰我很久了，最近在维护一个安卓app，经常改着改着就sync失败了，查看原因就是connection refused, 虽然明知道是网络问题，但明明setting-proxy没有问题，所以耽误了许多时间。

今天在知乎上找到了答案，直接修改~/.gradle/gradle.properties文件即可实现gradle全局proxy

## Step0-configure shadowsock

![image-20200323211555353](https://tva1.sinaimg.cn/large/00831rSTgy1gd683vl949j30qw0b60ve.jpg)

##Step1-configure gradle.properties

```shell
vim $HOME/.gradle/gradle.properties
```

```shell
systemProp.http.proxyHost=127.0.0.1
systemProp.https.proxyPort=1087
systemProp.https.proxyHost=127.0.0.1
systemProp.http.proxyPort=1087
```

enjoy!



if not OK, try following, use aliyun for gradle instead of official source

modify project's root build.gradle

```shell
maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
```

