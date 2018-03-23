# 项目的容器化部署

---

## 本地运行

本地运行很简单我们使用 `java -jar` 命令即可

## 容器化部署流程

1. 为每个需要部署的项目配置 Dockerfile
2. 将 Dockerfile 和打包好的 Jar 文件上传至打包服务器
3. 基于 Dockerfile 将 Jar 文件打包进镜像
4. 启动容器

## Dockerfile 配置案例

此处以项目 `leeshop-service-redis` 为例，在项目根目录创建 `docker/Dockerfile` 文件

```
FROM openjdk:8-jre

MAINTAINER Lusifer <topsale@vip.qq.com>

ENV APP_VERSION 1.0.0-SNAPSHOT
ENV DOCKERIZE_VERSION v0.6.0
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

RUN mkdir /app
WORKDIR /app
COPY leeshop-service-redis-$APP_VERSION.jar /app/app.jar
ENTRYPOINT ["dockerize", "-timeout", "5m", "-wait", "tcp://192.168.75.130:6379", "java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app/app.jar"]
EXPOSE 20887
```

注：`dockerize` 是一个小工具，可用于在启动容器之前等待其他需要先行启动的服务

## 构建 Docker 镜像

```
docker build -t leeshop-service-redis:latest .
```

## docker-compose.yml

```
version: '3.1'
services:
  leeshop-service-redis:
    image: leeshop-service-redis
    restart: always
    container_name: leeshop-service-redis
    ports:
      - 20887:20887
```