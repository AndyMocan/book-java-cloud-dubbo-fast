# Dubbo Admin

---

## 克隆

```
git clone https://github.com/dubbo/dubbo-ops.git
```

## 打包

```
mvn clean package
```

## 配置

编辑 `dubbo-admin/WEB-INF/dubbo.properties` 配置文件

```
dubbo.registry.address=zookeeper://192.168.75.130:2181
dubbo.admin.root.password=root
dubbo.admin.guest.password=guest
```

## 启动 Tomcat

## 访问 Tomcat

```
http://localhost:8080/dubbo-admin/
```

## 登录账号

* 账号：root，密码：root
* 账号：guest，密码：guest

## 服务页面

![](/assets/Lusifer1517526838.png)