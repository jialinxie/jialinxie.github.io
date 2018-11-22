---
layout: post
title: debug bluez5 on raspberrypi 3
date: 2018-11-20
tags: raspberry 3B
---

```
pi@scmbot:~ $ sudo bluetoothctl
[NEW] Controller 00:1B:DC:0F:5A:0C scmbot [default]
[bluetooth]# power on
Changing power on succeeded
[bluetooth]# agent on
Agent registered
[bluetooth]# pairable on
Changing pairable on succeeded
[bluetooth]# scan on
Discovery started
[CHG] Controller 00:1B:DC:0F:5A:0C Discovering: yes
[NEW] Device DC:2C:26:41:B8:10 MEDION MD86853
[bluetooth]# scan off
[CHG] Device DC:2C:26:41:B8:10 RSSI is nil
Discovery stopped
[CHG] Controller 00:1B:DC:0F:5A:0C Discovering: no
[bluetooth]# pair DC:2C:26:41:B8:10
Attempting to pair with DC:2C:26:41:B8:10
[CHG] Device DC:2C:26:41:B8:10 Connected: yes
[agent] PIN code: XXXXXX
[CHG] Device DC:2C:26:41:B8:10 Modalias: usb:v04E8p7021d011B
[CHG] Device DC:2C:26:41:B8:10 UUIDs:
00001000-0000-1000-8000-00805f9b34fb
00001124-0000-1000-8000-00805f9b34fb
00001200-0000-1000-8000-00805f9b34fb
[CHG] Device DC:2C:26:41:B8:10 Paired: yes
Pairing successful
[CHG] Device DC:2C:26:41:B8:10 Connected: no
[bluetooth]# trust DC:2C:26:41:B8:10
[CHG] Device DC:2C:26:41:B8:10 Trusted: yes
Changing DC:2C:26:41:B8:10 trust succeeded
[bluetooth]# connect DC:2C:26:41:B8:10 
Attempting to connect to DC:2C:26:41:B8:10
[CHG] Device DC:2C:26:41:B8:10 Connected: yes
Connection successful
[CHG] Device DC:2C:26:41:B8:10 Connected: no
[CHG] Device DC:2C:26:41:B8:10 Class: 0x000540
[CHG] Device DC:2C:26:41:B8:10 Icon: input-keyboard
[CHG] Device DC:2C:26:41:B8:10 Connected: yes
[bluetooth]# quit
Agent unregistered
[DEL] Controller 00:1B:DC:0F:5A:0C scmbot [default]
pi@scmbot:~ $
```







Now here's the trick. Bluez PNAT is not needed and breaks things, so edit main.conf:

Code: [Select all](https://www.raspberrypi.org/forums/viewtopic.php?p=521067#)

```
sudo nano /etc/bluetooth/main.conf
```

and add this line towards the top:

Code: [Select all](https://www.raspberrypi.org/forums/viewtopic.php?p=521067#)

```c
DisablePlugins = pnat
```



### [Receive files on Raspberry Pi 3 with bluetooth](https://www.raspberrypi.org/forums/viewtopic.php?t=145530#p959280)

```
sudo nano /etc/systemd/system/dbus-org.bluez.service
```

and add a ' -C' flag at the end of the ExecStart= line. Reboot, and try to start obexpushd again. Try with sudo if that does not work.





bluetoothctl代替了bluez-simple-agent 

bluetoothctl怎么配对？

Bluez-utils has been removed in the Bluez 5 package

Bluez-simple-agent is renamed, to simple-agent (I think) but bluetoothctl has been added for managing devices. It's a sweet little CLI used for various tasks like pairing and trusting





fix "Failed to pair: org.bluez.Error.AlreadyExists"

##配对成功(iPhone 6s)

```
root@edison:~# rfkill unblock bluetooth   
root@edison:~# bluetoothcutl   
power on
[bluetooth]# agent KeyboardDiasplay   
[bluetooth]# default-agent
[bluetooth]# scan on   
[bluetooth]# scan off   
[bluetooth]# pair <device>
```



```
Failed to connect: org.bluez.Error.Failed
```