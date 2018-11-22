---
layout: post
title: test bluez4 on arm-linux
date: 2018-11-20
tags: bluez BT bluez4
---

初始化蓝牙

```c
sh /etc/enable_bt.sh
```

查看蓝牙,获取设备编号 hci0

```                                                                                                      c
hciconfig -a
hci0:   Type: BR/EDR  Bus: UART
        BD Address: 20:70:02:A0:00:00  ACL MTU: 1021:8  SCO MTU: 64:1
        DOWN 
        RX bytes:604 acl:0 sco:0 events:30 errors:0
        TX bytes:398 acl:0 sco:0 commands:30 errors:0
        Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
        Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3 
        Link policy: RSWITCH SNIFF 
        Link mode: SLAVE ACCEPT
```

开启蓝牙

```c
hciconfig hci0 up
```

扫描蓝牙

```c
hcitool scan                                                                                                       

Scanning ...
        E0:19:1D:A4:62:64       HUAWEI MT7-TL00

```

连接手机

```
agent -a hci0 0000 E0:19:1D:A4:62:64                                                                               

Can't find adapter hci0
Launch helper exited with unknown return code 1

```

使设备可见可连接

```
hciconfig hci0 piscan
```

*BlueZ bug*

0:设备蓝牙无法连接手机/电脑
```
hcitool scan                                                                                                       

Scanning ...
        E0:19:1D:A4:62:64       HUAWEI MT7-TL00

agent -a hci0 0000 20:16:D8:FE:58:72                                                                            

Can't find adapter hci0
Launch helper exited with unknown return code 1

agent -a hci0 0000 E0:19:1D:A4:62:64                                                                               

Can't find adapter hci0
Launch helper exited with unknown return code 1

hciconfig -a

hci0:   Type: BR/EDR  Bus: UART
        BD Address: 20:70:02:A0:00:00  ACL MTU: 1021:8  SCO MTU: 64:1
        UP RUNNING 
        RX bytes:9488 acl:0 sco:0 events:163 errors:0
        TX bytes:1475 acl:0 sco:0 commands:114 errors:0
        Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
        Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3 
        Link policy: RSWITCH SNIFF 
        Link mode: SLAVE ACCEPT 
        Name: 'BCM20702A1 Generic UART Class 1 @ 26 MHz'
        Class: 0x000000
        Service Classes: Unspecified
        Device Class: Miscellaneous, 
        HCI Version: 4.0 (0x6)  Revision: 0x1000
        LMP Version: 4.0 (0x6)  Subversion: 0x220e
        Manufacturer: Broadcom Corporation (15)
```

1.手机不能连接设备蓝牙
输入配对密钥0000或1234，连接不上

```
agent 0000

Can't get default adapter
Launch helper exited with unknown return code 1
```

#############################################################
pair with phone and BT, test OK

```
source /etc/enable_bt.sh
bluetoothd --udev
hciconfig hci0 piscan
agent 0000 &
sdptool add OPUSH
```

#############################################################
phone->device			test FAIL!
添加本机 OBEX Object Push 服务

```
sdptool add OPUSH
```

查询开发板上的 OBEX Object 服务 channel 号
```
sdptool browse local
```

等待接收文件
```
obex_test -b local <chanel 号>
```

#############################################################
device->phone			//test OK

查看发送chanel

```
sdptool browse E0:19:1D:A4:62:64

Service Name: OBEX Object Push
Service RecHandle: 0x1000c
Service Class ID List:
  "OBEX Object Push" (0x1105)
Protocol Descriptor List:
  "L2CAP" (0x0100)
  "RFCOMM" (0x0003)
    Channel: 12
  "OBEX" (0x0008)
Profile Descriptor List:
  "OBEX Object Push" (0x1105)
    Version: 0x0100
```

	获取OBEX Object Push chanel : 12

发送文件

```
obex_test -b E0:19:1D:A4:62:64 12

Using Bluetooth RFCOMM transport
OBEX Interactive test client/server.

c 			//选择连接
Connect OK!
Version: 0x10. Flags: 0x00
x				//选择发送
PUSH filename> /opt/test1.txt
name=/opt/test1.txt, size=12
Going to send /opt/test1.txt(test1.txt), 12 bytes
Filling stream!
Filling stream!
PUT successful	//发送成功
```