# 通过Docker networking实现容器间的链接

通过Docker networking实现sinatra容器与redis容器的链接，将sinatra容器产生的数据存入redis容器，并从redis容器中取出数据。

## 1.创建Docker networking

```
docker network create app
```

此外，docker network的命令还有：

```
docker network connect app [container name or ID]
docker network disconnect app [container name or ID]
docker network inspect app
docker network ls
docker network prune
docker network rm app
```

## 2.创建sinatra镜像

创建sinatra目录,创建Dockerfile：

```
mkdir -p sinatra
cd sinatra
touch Dockerfile
```

编写Dockerfile

```
FROM ubuntu:latest
MAINTAINER Imer "example@email.com"
ENV REFRESHED_AT 2017-7-27

RUN apt-get update -yqq && apt-get -yqq install ruby ruby-dev build-essential 		redis-tools
RUN gem install --no-rdoc --no-ri sinatra json redis

RUN mkdir -p /opt/webapp
EXPOSE 4567

CMD [ "/opt/webapp/bin/webapp" ]
```

创建sinatra镜像

```
docker build -t komonron/sinatra .
```

## 2.创建redis镜像

创建redis目录，创建Dockerfile

```
mkdir -p sinatra/redis && cd sinatra/redis
touch Dockerfile
```

编写Dockerfile

```
FROM ubuntu:latest
MAINTAINER Imer "example@email.com"
ENV REFRESHED_AT 2017-7-27

RUN apt-get -yyq update && apt-get -yqq install redis-server redis-tools
EXPOSE 6379
ENTRYPOINT [ "/usr/bin/redis-server" ]
CMD []
```

创建redis镜像

```
docker build -t komonron/redis .
```

## 3.创建redis容器

```
docker run -d --net=app --name db komonron/redis
```

## 4.创建sinatra容器

下载sinatra配置文件，修改配置文件的权限

```
cd sinatra
wget --cut-dirs=3 -nH -r --reject Dockerfile,index.html --no-parent http://dockerbook.com/code/5/sinatra/webapp_redis/
chmod +x webapp_redis/bin/webapp
```

运行sinatra容器

```
docker run -p 4567 --net=app --name webapp -t -i -v $PWD/webapp_redis:/opt/webapp komonron/sinatra /bin/bash
```

## 5.测试

按`Ctrl + p`，再按`Ctrl + q`回到宿主机，得到webapp的端口

```
docker port webapp 4567
```

得到的结果为：

> 0.0.0.0:32768

测试将数据传递到redis容器

```
curl -i -H 'Accept: application/json' -d 'name=Foo&status=Bar' http://localhost:32768/json
```

测试从redis容器取回数据

```
curl -i http://localhost:32768/json
```