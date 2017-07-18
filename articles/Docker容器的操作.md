# Docker容器的操作
----
以Centos 7为例，介绍Docker容器的一些基本的操作

## download Centos 7 image
`docker pull centos`

## create, start, exit and stop container
### create container
`docker run -i -t centos /bin/bash`

&ensp;&ensp;&ensp;&ensp;创建容器完毕之后，会自动进入该容器的shell交互模式。

&ensp;&ensp;&ensp;&ensp;如果想在创建容器的时候给该容器命名，则需要使用如下命令：

`docker run --name [container name] -i -t centos /bin/bash`

### exit container
直接在命令行输入`exit`，即可退出当前容器，并且该容器被关闭。**如果想在不关闭容器的情况下，回到宿主机的命令行，使用如下快捷键：先按*ctrl + p*，接着按*ctrl + q*。**
### start container
`docker start [container name 或者 container ID]`

&ensp;&ensp;&ensp;&ensp;容器的container name 和 container ID可以通过`docker ps -a`命令查看
### `run`和`start`的区别
&ensp;&ensp;&ensp;&ensp;`docker run` 只在第一次运行时使用，将镜像放到容器中，以后再次启动这个容器时，只需要使用命令`docker start` 即可。

&ensp;&ensp;&ensp;&ensp;`docker run`相当于执行了两步操作：将镜像放入容器中（docker create）,然后将容器启动，使之变成运行时容器（`docker start`），如图所示（图来自：[10张图带你深入理解Docker容器和镜像](http://dockone.io/article/783)）。

<div align=center>
<img src="/figures/diff-run-start.jpg"/>
</div>

### 进入container的shell交互界面
`docker attach [container name]`
### stop container
`docker stop [container name 或者container ID]`

**注意**：此命令用于宿主机的shell交互界面

## rename container
`docker rename [old container name] [new container name]`

&ensp;&ensp;&ensp;&ensp;也可以在创建容器的时候为其命名，例如：`docker run --name [container name] -i -t centos /bin/bash`

## 查看容器：ps(Process Status)
### 查看正在运行的容器
`docker ps`

得到如下结果：

> CONTAINER ID &ensp;&ensp;&ensp;&ensp; IMAGE &ensp;&ensp;&ensp;&ensp; COMMAND &ensp;&ensp;&ensp;&ensp; CREATED &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; STATUS &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; PORTS &ensp;&ensp; NAMES
> 
> 28fe24874794 &ensp;&ensp;&ensp;&ensp; centos &ensp;&ensp;&ensp;&ensp; "/bin/bash" &ensp;&ensp; 56 seconds ago &ensp; Exited (0) 50 seconds ago &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; centos7

可以得到container的`name`和`ID`
### 查看所有容器
`docker ps -a`
## 查看容器logs
`docker logs [container name或者container ID]`
## 查看容器内的进程
`docker top [container name或者container ID]`

## 查看容器的运行信息
`docker stats [container name或者container ID]`

&ensp;&ensp;&ensp;&ensp;`stats`后可以有多个容器

## 在容器内运行进程
&ensp;&ensp;&ensp;&ensp;在容器内运行交互式shell

`docker exec -i -t [container name或者container ID] /bin/bash`

&ensp;&ensp;&ensp;&ensp;在桌面创建*test.txt*

`docker exec -d [container name或者container ID] touch test.txt`

## inspect container
### 查看容器的所有信息
`docker inspect [container name或者container ID]`
### 查看指定的参数
`docker inspect --format '{{.Name}} {{.State.Running}}'[container name或者container ID]`

&ensp;&ensp;&ensp;&ensp;参数可以有多个，容器也可以有多个
## delete container
`docker rm [container name或者container ID]`