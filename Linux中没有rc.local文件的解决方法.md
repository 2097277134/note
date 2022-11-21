## 1、设置rc-local.service
```bash
sudo vim /etc/systemd/system/rc-local.service
```
```bash
[Unit]
 Description=/etc/rc.local Compatibility
 ConditionPathExists=/etc/rc.local

[Service]
 Type=forking
 ExecStart=/etc/rc.local start
 TimeoutSec=0
 StandardOutput=tty
 RemainAfterExit=yes
 SysVStartPriority=99

[Install]
 WantedBy=multi-user.target
1
2
3
4
5
6
7
8
9
10
11
12
13
14
```
## 2、激活rc-local.service
```bash
sudo systemctl enable rc-local.service
```
## 3、添加启动服务
手工创建或者拷贝已有的/etc/rc.local，并赋予执行权限
```bash
#!/bin/sh -e
#
# rc.local
#
# This script is executed at the end of each multiuser runlevel.

# Make sure that the script will "exit 0" on success or any other
# value on error.
#
# In order to enable or disable this script just change the execution
# bits.
#
# By default this script does nothing.

# 下面这条是要开机启动的命令
/home/parallels/bin/Python /home/parallels/test.py > /home/parallels/auto.log 

exit 0

```