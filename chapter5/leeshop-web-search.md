# leeshop-web-search

---

## POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>leeshop-web-search</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>leeshop-web-search</name>
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
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
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

        <!-- Nekohtml Begin -->
        <dependency>
            <groupId>net.sourceforge.nekohtml</groupId>
            <artifactId>nekohtml</artifactId>
        </dependency>
        <!-- Nekohtml End -->

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
                    <mainClass>com.lusifer.leeshop.web.search.LeeShopWebSearchApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

## application.yml

```
server:
  port: 8102

spring:
  application:
    name: leeshop-web-search
  thymeleaf:
    cache: false
    mode: LEGACYHTML5
    encoding: UTF-8
    content-type: text/html

dubbo:
  scan:
    base-packages: com.lusifer.leeshop.web.search.controller
  application:
    id: leeshop-web-search
    name: leeshop-web-search
  registry:
    id: zookeeper
    address: zookeeper://192.168.75.130:2181?backup=192.168.75.130:2182,192.168.75.130:2183
```