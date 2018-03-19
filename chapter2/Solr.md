# Solr

---

## 创建工作目录

在 Linux 服务器上创建 `/usr/local/docker/solr/ikanalyzer` 目录

说明：

* `/usr/local/docker/solr`：用于存放 `docker-compose.yml` 配置文件
* `/usr/local/docker/solr/ikanalyzer`：用于存放 `Dockerfile` 镜像配置文件

## docker-compose.yml

```
version: '3.1'
services:
  solr:
    build: ikanalyzer
    restart: always
    container_name: solr
    ports:
      - 8983:8983
    volumes:
      - /usr/local/docker/solr/solrdata:/opt/solrdata
```

## Dockerfile

```
FROM solr

MAINTAINER Lusifer <topsale@vip.qq.com>

# 创建 Core
WORKDIR /opt/solr/server/solr
RUN mkdir ik_core
WORKDIR /opt/solr/server/solr/ik_core
RUN echo 'name=ik_core' > core.properties
RUN mkdir data
RUN cp -r ../configsets/sample_techproducts_configs/conf/ .

# 安装中文分词
WORKDIR /opt/solr/server/solr-webapp/webapp/WEB-INF/lib
ADD ik-analyzer-solr5-5.x.jar .
ADD solr-analyzer-ik-5.1.0.jar .
WORKDIR /opt/solr/server/solr-webapp/webapp/WEB-INF
ADD ext.dic .
ADD stopword.dic .
ADD IKAnalyzer.cfg.xml .

# 增加分词配置
COPY managed-schema /opt/solr/server/solr/ik_core/conf

WORKDIR /opt/solr
```

## 部署成功效果图

访问地址：http://192.168.75.135:8983/

![](/assets/Lusifer1520779234.png)