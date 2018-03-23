# leeshop-web-api

---

## POM

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.lusifer</groupId>
    <artifactId>leeshop-web-api</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>leeshop-web-api</name>
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
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <!-- Spring Boot End -->

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

        <!-- Swagger2 Begin -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
        </dependency>
        <!-- Swagger2 End -->

        <!-- Project Begin -->
        <dependency>
            <groupId>com.lusifer</groupId>
            <artifactId>leeshop-service-portal-api</artifactId>
            <version>1.0.0-SNAPSHOT</version>
        </dependency>

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
                    <mainClass>com.lusifer.leeshop.web.api.LeeShopWebApiApplication</mainClass>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

## application.yml

```
server:
  port: 8500

spring:
  application:
    name: leeshop-web-api

dubbo:
  scan:
    base-packages: com.lusifer.leeshop.web.api.controller
  application:
    id: leeshop-web-api
    name: leeshop-web-api
  registry:
    id: zookeeper
    address: zookeeper://192.168.75.130:2181?backup=192.168.75.130:2182,192.168.75.130:2183

rest:
  path:
    api:
      v1: api/v1
      v2: api/v2
```

## 跨域配置

```
package com.lusifer.leeshop.web.api.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;

/**
 * 解决跨域问题
 * <p>Title: CORSConfiguration</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/9 9:50
 */
@Configuration
public class CORSConfiguration extends WebMvcConfigurerAdapter {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**").allowedOrigins("*")
                .allowedMethods("GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS")
                .allowCredentials(false).maxAge(3600);
    }
}
```

## Swagger2 配置

```
package com.lusifer.leeshop.web.api.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;

import java.util.Date;

/**
 * Swagger2 配置
 * <p>Title: Swagger2Configuration</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/7 10:53
 */
@Configuration
public class Swagger2Configuration {
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.lusifer.leeshop.web.api.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("我的商城 API 文档")
                .description("我的头条 API 网关接口，http://")
                .termsOfServiceUrl("http://")
                .version("1.0.0")
                .build();
    }
}
```

## 通用响应结构

```
package com.lusifer.leeshop.web.api.dto;

import lombok.Data;

import java.io.Serializable;

/**
 * 自定义数据返回结构体
 * <p>Title: BaseResult</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2018/3/8 14:33
 */
@Data
public class BaseResult implements Serializable {
    // 状态码
    private int status;

    // 自定义消息
    private String msg;

    // 请求的数据
    private Object data;

    /**
     * 成功的请求
     *
     * @param data
     * @return
     */
    public static BaseResult success(Object data) {
        BaseResult baseResult = new BaseResult();
        baseResult.setStatus(200);
        baseResult.setMsg("请求成功");
        baseResult.setData(data);
        return baseResult;
    }

    /**
     * 失败的请求
     * @param status 状态码
     * @param msg 消息
     * @return
     */
    public static BaseResult fail(int status, String msg) {
        BaseResult baseResult = new BaseResult();
        baseResult.setStatus(status);
        baseResult.setMsg(msg);
        return baseResult;
    }
}
```