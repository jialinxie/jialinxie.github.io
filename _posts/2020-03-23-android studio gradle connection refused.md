# android studio compile errors

## 查看详细的日志信息

C:\Users\jacky\AppData\Local\Google\AndroidStudio4.2\log



## bug0: gradle connection refused

这个问题困扰我很久了，最近在维护一个安卓app，经常改着改着就sync失败了，查看原因就是connection refused, 虽然明知道是网络问题，但明明setting-proxy没有问题，所以耽误了许多时间。

今天在知乎上找到了答案，直接修改~/.gradle/gradle.properties文件即可实现gradle全局proxy

### Step0-configure shadowsock

![image-20200323211555353](https://tva1.sinaimg.cn/large/00831rSTgy1gd683vl949j30qw0b60ve.jpg)

### Step1-configure gradle.properties

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





## bug1: Could not run build action using Gradle distribution

Hi all,
I got the same issues for gradle-5.3.1, but during the importing project I was choose to overwrite the configuration by pointing the gradle-5.3.1 Installation Directory to the directory of gradle-5.3.1 in my local and pointing the gradle-5.3.1 User Home to the gradle-5.3.1\bin then continued the importing project then it was successful, it solved my problem.

[![gradle](https://user-images.githubusercontent.com/29445606/66884424-5b5a8700-effb-11e9-9bab-de3f180d87aa.PNG)](https://user-images.githubusercontent.com/29445606/66884424-5b5a8700-effb-11e9-9bab-de3f180d87aa.PNG)

from [https://github.com/eclipse/buildship/issues/755]

参考**[nguyentuanviet77](https://github.com/nguyentuanviet77)** 的回答，设置本地的gradle路径即可解决