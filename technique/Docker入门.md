# Docker入门

参考博客：[Docker Desktop 安装使用教程_dockerdesktop安装-CSDN博客](https://blog.csdn.net/GoodburghCottage/article/details/131413312)

### Docker基本操作

```bash
# 启动docker
sudo service docker start
# 重启docker
sudo service docker restart
# 停止docker
sudo service docker stop
```

### 镜像操作

#### 基本操作

```bash
# 查看所有镜像
docker images
# 拉取镜像
docker pull 镜像名称
docker pull 仓库名称/镜像名称
# 删除镜像
docker image rm 镜像名称
```

#### 运行镜像

```bash
# 例
docker run -i -d -t --name=kali-test kalilinux/kali-rolling
```

<img src="./image/Docker%E5%85%A5%E9%97%A8/image-20231008153621128.png" alt="image-20231008153621128" style="zoom:25%;" />

注意，如果希望容器能够联网，需要将8080端口进行暴露给主机

```
eg：docker run -i -d -t -p 8081:8080 --name=kali-test kalilinux/kali-rolling
```

### 容器操作

#### 基本操作

```
# 查看容器
docker ps
# 停止容器
docker stop 容器名或容器id
# 启动容器
docker start 容器名或容器id
# 删除容器
docker rm 容器名或容器id
```

#### 操作容器

用-t可以通过命令行的方式操作容器

```bash
# 例
docker exec -it kali-test /bin/bash
```

<img src="./image/Docker%E5%85%A5%E9%97%A8/image-20231008154146768.png" alt="image-20231008154146768" style="zoom:25%;" />

#### 容器制作为镜像

```bash
# 将容器制作成镜像
docker commit 容器名 镜像名
# 镜像打包备份(打包备份的文件会自动存放在当前命令行的路径下,如果想让保存的文件可以打开,可以加.tar后缀)
docker save -o 保存的文件名 镜像名
# 镜像解压
docker load -i 文件路径/备份文件
```

