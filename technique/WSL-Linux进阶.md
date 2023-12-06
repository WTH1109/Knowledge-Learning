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

注销原来的系统

```bash
wsl --unregister ubuntu
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

## 5.使用pyenv进行python的环境管理

python虚拟环境讨论：

- 直接在系统python上的都是狠人，直接抬走。
- conda环境与pip环境会产生很多冲突，在生成requirements.txt以及打包docker时会出现严重的问题，不利于项目复现，优势在于简单方便。
- Virtualenv以及其扩展VirtualenvWrapper的优势在于可以方便的建立虚拟环境，并且都是以pip的形式存在，但缺点是只能安装系统版本的python，如果需要其他版本的python的虚拟环境将会很麻烦。
- docker的优势是可以打包环境为镜像，在新的机器上可以很快的配置好环境，但是缺点是在docker安装包需要将镜像运行为容器，之后再打包回镜像，并且打包后的镜像体积过大。采用Dockerfile的方式可以解决这一问题，但也需要提前准备好requirements.txt，因此docker适合项目的部署，并不适合项目在开发时候使用。

### Linux安装python的依赖项(Ubuntu / Debian)

如果之前系统中没有安装过python，需要首先安装下述依赖项

```bash
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev libbz2-dev python3-tk tk-dev liblzma-dev
```

### Linux系统下安装pyenv

使用Git克隆pyenv仓库

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

将pyenv添加至环境变量

- 在命令行中添加环境变量

  ```bash
  echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
  echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
  echo 'eval "$(pyenv init --path)"' >> ~/.bashrc
  ```

- 重新加载配置文件

  ```bash
  source ~/.bashrc
  ```

- 运行`pyenv --version`查看是否安装成功

pyenv用于python版本的管理，与virtualenv配合使用可以实现不同版本的虚拟环境的目的

### pyenv的使用

安装python版本

```bash
# 查看所有版本
pyenv versions
# 查看所有可安装的版本
pyenv install --list
# 安装指定版本
pyenv install 3.6.5
# 安装新版本后rehash一下
pyenv rehash
# 删除指定版本
pyenv uninstall 3.5.2
# 指定全局版本
pyenv global 3.6.5
# 指定多个全局版本, 3版本优先
pyenv global 3.6.5 2.7.14
```

如果网络速度慢可以用下面的方式先下载进缓存后进行安装

```bash
wget https://registry.npmmirror.com/-/binary/python/3.9.0/Python-3.9.0.tar.xz -P ~/.pyenv/cache/

pyenv install 3.9.0
```

### pyenv与pyenv-virtualenv包管理插件联合使用

- pyenv-virtualenv下载

  ```bash
  git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
  ```

- 在命令行中添加环境变量

  ```bash
  echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
  echo 'eval "$(pyenv init -)"' >> ~/.bashrc
  ```

- 重新加载配置文件

  ```bash
  source ~/.bashrc
  ```

最终bashrc里面的文件需要变为如下所示（如果不添加最后一行在激活环境时会报错）

```bash
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv virtualenv-init -)"
eval "$(pyenv init -)"
```

基本操作

```bash
# 创建虚拟环境
pyenv virtualenv 3.6.9 project 
# 激活虚拟环境
pyenv activate name 
# 退出虚拟环境
pyenv deactivate  
# 删除name环境
pyenv virtualenv-delete name  
```

## 注：自动加载bashrc文件

注意，如果希望linux自动加载bashrc的配置文件，需要在`.bash_profile`文件里重新引用一次`.bashrc`

```bash
vim ~/.bash_profile
```

```bash
if test -f .bashrc ; then
source .bashrc
fi
```

