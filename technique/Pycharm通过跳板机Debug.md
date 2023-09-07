# Pycharm通过跳板机Debug

###  准备工作

- windows电脑下载git bash
- linux无需额外操作

### 步骤一：生成电脑的公钥

windows电脑用git bash，linux用命令行运行下述命令：

```sh
ssh-keygen
```

### 步骤二：将电脑的公钥配置到服务器上

如果电脑可以直接访问服务器，运行以下命令拷贝公匙到服务器上

```sh
ssh-copy-id -i .ssh/id_ed25519.pub -p 端口号 用户名@服务器地址 
```

如果电脑无法直接访问

则将.ssh/id_ed25519.pub中的内容拷贝到服务器上的 用户名/.ssh/authorized_keys文件夹内

### 步骤三：配置config文件

在usr/.ssh/目录下建立config文件

windows的路径为C:\Users\用户名\ .ssh

写入下面的配置规则

```sh
Host jump
  HostName x.x.x.x
  User xxx

Host Server
  HostName x.x.x.x
  User xxx
# ProxyJump jump # 将jump作为跳板机
  ProxyCommand ssh.exe -W %h:%p jump #Windows上Pycharm可能用不了ProxyJump，可以替换为这一行
```

### 步骤四：测试配置结果

在命令行运行下面的命令

```sh
ssh Server
```

如果无需输入密码代表配置成功