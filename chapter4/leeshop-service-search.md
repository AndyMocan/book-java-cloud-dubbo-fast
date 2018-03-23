# leeshop-service-search

---

## 修改 managed-schema 配置业务系统字段

需要用到的业务字段如下：

* 商品 ID
* 商品标题
* 商品卖点
* 商品价格

由于 Solr 中自带 id 字段所以无需添加，其它字段需要手动添加 Solr 字段

```
# 字段域
<field name="item_title" type="text_ik" indexed="true" stored="true"/>
<field name="item_sell_point"  type="text_ik" indexed="true" stored="true"/>
<field name="item_price" type="plong" indexed="false" stored="true" />

# 复制域（Solr 的搜索优化功能，将多个字段域复制到一个域里，提高查询效率）
<field name="item_keywords" type="text_ik" indexed="true" stored="false" multiValued="true"/>
<copyField source="item_title" dest="item_keywords"/>
<copyField source="item_sell_point" dest="item_keywords"/>
```

## 复制配置到容器并重启

```
# 复制到容器
docker cp managed-schema solr:/opt/solr/server/solr/ik_core/conf

# 重启容器
docker-compose restart
```

## 创建接口项目

### 项目名称

leeshop-service-search-api

### POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>leeshop-service-search-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>leeshop-service-search-api</name>
    <description></description>

    <parent>
        <groupId>com.lusifer</groupId>
        <artifactId>leeshop-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <dependency>
            <groupId>com.lusifer</groupId>
            <artifactId>leeshop-domain</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
        </dependency>
    </dependencies>

</project>
```

## 创建服务项目

### 项目名称

leeshop-service-search

### POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>leeshop-service-search</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>leeshop-service-search</name>
    <description></description>

    <parent>
        <groupId>com.lusifer</groupId>
        <artifactId>leeshop-dependencies</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <!-- Spring Boot Begin -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-solr</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Spring Boot End -->

        <!-- Database Begin -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!-- Database End -->

        <!-- Alibaba Begin -->
        <dependency>
            <groupId>com.alibaba.boot</groupId>
            <artifactId>dubbo-spring-boot-starter</artifactId>
        </dependency>
        <!-- Alibaba End -->

        <!-- Zookeeper Begin -->
        <dependency>
            <groupId>com.101tec</groupId>
            <artifactId>zkclient</artifactId>
        </dependency>
        <!-- Zookeeper End -->

        <!-- Project Begin -->
        <dependency>
            <groupId>com.lusifer</groupId>
            <artifactId>leeshop-service-search-api</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>

        <dependency>
            <groupId>com.lusifer</groupId>
            <artifactId>leeshop-commons</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>
        <!-- Project End -->
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <mainClass>com.lusifer.leeshop.service.search.LeeShopServiceSearchApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

## application.yml

```
spring:
  application:
    name: leeshop-service-search
  datasource:
    druid:
      url: jdbc:mysql://192.168.75.130:3306/leeshop?useUnicode=true&characterEncoding=utf-8&useSSL=false
      username: root
      password: 123456
      initial-size: 1
      min-idle: 1
      max-active: 20
      test-on-borrow: true
      driver-class-name: com.mysql.jdbc.Driver
  data:
    solr:
      host: http://192.168.75.130:8983/solr

mybatis:
    type-aliases-package: com.lusifer.leeshop.domain
    mapper-locations: classpath:mapper/*.xml

dubbo:
  scan:
    base-packages: com.lusifer.leeshop.service.search.api
  application:
    id: leeshop-service-search
    name: leeshop-service-search
  protocol:
    id: dubbo
    name: dubbo
  registry:
    id: zookeeper
    address: zookeeper://192.168.75.130:2181?backup=192.168.75.130:2182,192.168.75.130:2183
```

## application.properties

```
dubbo.protocol.port=20882
```