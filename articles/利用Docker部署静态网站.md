# 利用Docker部署nginx静态网站

## 1.编写Dockerfile

### 创建Dockerfile

```
mkdir sample && cd sample
touch Dockerfile
```

### 编写Dockerfile

```
FROM ubuntu:16.04
MAINTAINER Imer "example@email.com"
ENV REFRESHED_AT 2017-7-25
RUN apt-get -yqq update && apt-get -yqq install nginx # -qq：不输出信息，错误除外
RUN mkdir -p /var/www/html/website
ADD nginx/global.conf /etc/nginx/conf.d/
ADD nginx/nginx.conf /etc/nginx/nginx.conf
EXPOSE 80
```

## 2.下载nginx配置文件

```
mkdir nginx && cd nginx
wget https://raw.githubusercontent.com/jamtur01/dockerbook-code/master/code/5/sample/nginx/global.conf
wget https://raw.githubusercontent.com/jamtur01/dockerbook-code/master/code/5/sample/nginx/nginx.conf
cd ..
```

## 3.创建镜像

```
docker build -t komonron/nginx .
```

执行完毕之后，可以查看执行过程：`docker history komonron/nginx`

## 4.下载静态网页资源

```
mkdir website && cd website
wget https://raw.githubusercontent.com/jamtur01/dockerbook-code/master/code/5/sample/website/index.html
cd ..
```

## 5.启动容器

```
docker run -d -p 80 --name website \
-v $PWD/website:/var/www/html/website \
komonron/nginx nginx
```

## 6.访问网站

通过`docker ps`命令得到宿主机的端口，利用该端口访问容器。在浏览器中输入：`localhost:[host port]`即可访问网页。