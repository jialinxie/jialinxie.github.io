# ubuntu16.04搭建samba

## 0 install samba

`sudo apt-get install samba samba-common-bin`

## 1 add user

`sudo smbpasswd -a user-name`

## 2 edit smb.conf

[home]

​	comment = Home Directories

​	browseale = yes

​	read only = no

​	create mask = 0755

​	directory mask = 0755

## 3 启动samba

`sudo smbd`

`sudo nmbd`

## 4 window 访问 ubuntu

\\\ip

