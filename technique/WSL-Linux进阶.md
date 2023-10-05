# WSL-Linux进阶

## 1.安装wsl

 在管理员模式下打开 PowerShell 或 Windows 命令提示符，方法是右键单击并选择“以管理员身份运行”，输入 wsl --install 命令，然后重启计算机。

```bash
wsl --install
```

## 2.更改wsl安装位置

在 Windows 命令提示符中查看ubuntu版本号

```bash
wsl -l -v 
```

关闭Ubuntu

```bash
wsl --shutdown Ubuntu
```

文件备份，将其导出到E盘的WSL2UBUNTUBACKUP

```bash
wsl --export Ubuntu E:\WSL2UBUNTUBACKUP\WSL2Ubuntu.bak
```

导入虚拟机到D盘的WSL2UbuntuLTS文件夹下

```bash
wsl --import Ubuntu D:\WSL2UbuntuLTS E:\WSL2UBUNTUBACKUP\WSL2Ubuntu.bak --version 2
```

设置默认用户名 username

```bash
ubuntu config --default-user username
```

## 3.用户操作

设置密码

```bash
passwd
```

创建子用户 -m 用户名 -d 用户根目录 -s shell目录

```bash
useradd -m username -d /mnt/disk/user -s /bin/bash
```

指定shell为bash

```bash
chsh -s /bin/bash
```

切换用户

```bash
su - username
```

指定默认用户, 切换到Windows 命令提示符中

```bash
ubuntu.exe config --default-user username
```

[linux之用户和权限管理（干货） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/467200809)

## 4.SSH配置

检查系统是否安装SSH，如果缺少''openssh-server"则需要进行安装

```bash
dpkg -l | grep ssh
```

安装SSH

```bash
sudo apt-get install openssh-server
```

启动SSH服务

```bash
sudo service ssh start
```

SSH进阶设置

[Win11将WSL做SSH服务器，实现通过局域网SSH远程连接到WSL上，并且开机自动启动，手把手教学_wsl ssh-CSDN博客](https://blog.csdn.net/q4616756/article/details/131842814)

[Windows怎么让防火墙开放端口_电脑防火墙怎么开放端口-CSDN博客](https://blog.csdn.net/qq754772661/article/details/110876957)