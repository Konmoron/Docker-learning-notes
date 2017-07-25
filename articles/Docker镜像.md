# Docker镜像

****

[Docker hub官方网址](https://hub.docker.com/)

在命令行登录docker hub的命令是：`docker login`，退出登录的命令是：`docker logout`

## 拉取镜像

`docker pull [image name]:[tag]`

其中，`[tag]`是镜像的版本号，使用指定版本的镜像的命令是：

`docker run --name [container name] [image name]:[tag]`

## 构建镜像

### 1.编写Dockerfile

#### 创建目录以及Dockerfile文件

```
mkdir obj_dir
cd obj_dir
touch Dockerfile
```

#### 编写Dockerfile

通过Dockerfile的方式，构建一个基于centos7的nignx服务器，代码为：

```
# “#”是注释
# v0.0.1
FROM centos:latest # 第一行必须是某个镜像
MAINTAINER Author "example@email.com" # 作者信息
RUN yum install -y epel-release # 拉取centos之后，运行的命令，安装EPEL repositiry
RUN yum install -y nginx # install nginx
RUN echo 'Hi, i am in your container!' \
	/usr/share/nginx/html/index.html
EXPOSE 80 # 暴露容器的80端口
```

### 2.构建镜像

利用Dockerfile文件构建镜像

`docker build -t="komonron/static_web:v2" .`

### 3.利用static_web创建容器

```
docker run -d -P --name static_web komonron/static_web:v2 \
	nginx -g "daemon off;"
```

`-P`的意思是容器暴露的端口，可以使用-p指定端口，包括宿主机的映射端口。

查看容器端口：`docker port [container name]`，得到如下结果：

> 80/tcp -> 0.0.0.0:32771

其中，32771为宿主机的端口

访问容器：`curl localhost:[host port]`，也即`curl localhost:32771`，得到如下结果：

> Hi, i am in your container!

### 4.Dockerfile的其他命令

Dockerfile的其他命令有：

> CMD：指定容器启动时要执行的命令

> ENTRYPOINT：与CMD类似，但不会被覆盖

> ADD：将构建环境下的目录和文件复制到镜像中，具有解压缩功能。

> COPY：将构建环境下的目录和文件复制到镜像中，不具有解压缩功能。

> VOLUME：为基于镜像的容器创建卷。

> WORKDIR：启动容器时，创建工作目录

> USER：指定镜像的用户。

> ONBUILD：为镜像添加触发器，仅对子镜像，对孙子镜像无效。

> LABEL：为镜像添加元数据，键值对格式。

> STOPSIGNAL：停止容器时，发送给容器的停止信号。

> ARG：在docker build命令运行时传递给构建运行时的变量。

> ENV：在镜像构建过程中设置环境变量

## 上传镜像到Docker hub

```
docker push komonron/static_web
```

可以指定上传的版本：

```
docker push komonron/static_web:v2
```

## 删除镜像

```
docker rmi [image name]
```