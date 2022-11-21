#### 安装 Samba 和 Samba Commons Binary

```bash
sudo apt-get install samba samba-common-bin
```

## 修改配置文件

使用 nano 或 vim 编辑器编辑 /etc/samba/smb.conf，例如 sudo nano /etc/samba/smb.conf，并直接在配置文件最后加上以下这几段语句。

> [pi]
>
>   path = /home/pi/
>
>   valid users = pi
>
>   browseable = Yes
>
>   writeable = Yes
>
>   writelist = pi
>
>   create mask = 0777
>
>   directory mask = 0777

其中 Path 是 Samba 的默认目录，也是根目录。设置为 /home/pi 后，用户可以访问 /home/pi/Downloads，但是不可以访问 /home。Valid Users 如果没有修改用户名保持为 pi 即可，Writelist 即为可写用户列表，同 Valid Users。

#### 保存之后，重新运行 Samba 服务。

```bash
sudo /etc/init.d/samba restart
```

#### 添加 pi 用户为 Samba 用户，设置密码时密码不会显示在窗口中。

```bash
sudo smbpasswd -a pi
```

利用网络驱动器映射到电脑

![此电脑 2022-06-30 16_18_27](D:\Users\lenovo\Videos\Captures\此电脑 2022-06-30 16_18_27.png)![此电脑 2022-06-30 16_17_47](D:\Users\lenovo\Videos\Captures\此电脑 2022-06-30 16_17_47.png)