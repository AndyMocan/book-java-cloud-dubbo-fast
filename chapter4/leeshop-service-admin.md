# leeshop-service-admin

---

## 创建接口项目

### 项目名称

leeshop-service-admin-api

### POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>leeshop-service-admin-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>leeshop-service-admin-api</name>
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

leeshop-service-admin

### POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>leeshop-service-admin</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>leeshop-service-admin</name>
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
            <artifactId>leeshop-service-admin-api</artifactId>
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
                    <mainClass>com.lusifer.leeshop.service.admin.LeeShopServiceAdminApplication</mainClass>
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
    name: leeshop-service-admin
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

mybatis:
    type-aliases-package: com.lusifer.leeshop.domain
    mapper-locations: classpath:mapper/*.xml

dubbo:
  scan:
    base-packages: com.lusifer.leeshop.service.admin.api
  application:
    id: leeshop-service-admin
    name: leeshop-service-admin
  protocol:
    id: dubbo
    name: dubbo
  registry:
    id: zookeeper
    address: zookeeper://192.168.75.130:2181?backup=192.168.75.130:2182,192.168.75.130:2183
```

## application.properties

Dubbo 配置中有一个 BUG（2.6.0），无法识别 application.yml 配置中的 dubbo.protocol.port 配置，但可以识别 application.properties 中的配置，所以我们需要新建一个 application.properties 配置文件，并配置 Dubbo 的端口

```
dubbo.protocol.port=20880
```