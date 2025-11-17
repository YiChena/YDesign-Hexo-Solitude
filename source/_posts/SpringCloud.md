---
title: SpringCloud
date: 2020-11-11 00:00:00
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/development.png
tags:  
    - 技术
    - Java
    - Spring
---

# SpringCloud



## Nacos

在bin目录下打开命令提示符 startup.cmd -m standalone

### Nacos配置管理

配置管理-配置列表



#### 新建配置

DataID ：服务名称-开发环境.yaml   例如：userservice-dev.yaml



#### 配置内容

有变化的配置

```yaml
pattern：
	dateformat：yyyy-MM-dd HH:mm:ss
```

点击发布-发布成功



#### 获取配置

##### 将配置交给Nacos管理的步骤

在Nacos中添加配置文件
在微服务中引入nacos的config依赖
在微服务中添加bootstrap.yml，配置nacos地址、
当前环境、服务名称、文件后缀名。这些决定了程序
启动时去nacos读取哪个文件



项目启动-读取本地配置文件application.yml-创建spring容器-加载bean

Nacos会先bootstrap.yml文件拿到nacos地址后读取nacos中配置文件再去读取本地配置文件

bootstrap.yml：与nocos地址和配置文件有关的所有信息

引入Nacos的配置管理客户端依赖

```xml
<!--nacos配置管理依赖-->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

新建bootstrap.yml文件

```yaml
spring:
  application:
    name: userservice
  profiles:
    active: dev #环境
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos地址
      config:
        file-extension: yaml #文件后缀名
```

usercontroller

```java
	@Value("${pattern.dateformat}")
    private String dateformat;

    @GetMapping("now")
    public String now(){
        return LocalDateTime.now().format(DateTimeFormatter.ofPattern(dateformat));
    }
```

测试 localhost:8086/user/now

#### 配置自动刷新

Nacos中的配置文件变更后，微服务无需重启就可以感知。不过需要通过下面两种配置实现

方式一：在@Value注入的变量所在类上添加注解@RefreshScope

方式二：使用 @ConfigurationProperties 注解

#### 多环境配置共享

微服务会从nacos读取的配置文件:
[服务名]-[spring.profile.active].yaml，环境配置
[服务名].yaml，默认配置，多环境共享
优先级:
[服务名]-[环境].yaml >[服务名].yaml >本地配置



微服务启动时会从nacos读取多个配置文件

- 服务名称-开发环境.yaml userservice-dev.yaml
- 服务名称.yaml userservice.yaml

无论profile如何变化userservice.yaml这个文件一定会加载，因此多环境共享配置可以写入这个文件
