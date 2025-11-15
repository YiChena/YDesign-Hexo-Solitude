---
title: Mybatis
date: 2020-11-08 00:00:00
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/Mybatis.png
tags:  
    - 技术
    - Java
    - Spring
---

# Mybatis

> Mybatis框架是为了解决操作数据库得问题

```java
@Test
public void userInfoFinAll() throws IOException {
  //读取配置文件
  InputStream inputStream = Resources.getResourceAsStream("sqlMapConfig.xml");
  //根据配置文件构建SQLSessionFactory
  SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
  //通过SqlSessionFactory创建SqlSession
  SqlSession sqlSession = sqlSessionFactory.openSession();
  //qlSession 执行映射文件中的定义为SQL,并返回映射结果
  List<UserInfo> userInfoList = sqlSession.selectList("UserInfoMapper.userInfoFindAll");
  //打印数据
  System.out.println(userInfoList);
  //关闭SqlSession
  sqlSession.close();
}
```



Mapper 接口开发需要遵循以下规范:

- Mapper.xml 文件中的 namespace 与 mapper 接口的全限定名相同
- Mapper 接口方法名和Mapper.xml中定义的每个statement的id相同
- Mapper 接q方法的输入参数类型和mapper.xml中定义的每个sql的parameterType的类型相同
- Mapper 接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同



### Myabtis环境搭建

#### 创建 Maven 的 web 项目

#### 导入 mybatis 和 mysql 的 jar 包

```xml
<dependencies>
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.9</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.44</version>
    </dependency>
</dependencies>
```

#### 创建mybatis的全局配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- environments：mybatis的数据库得环境 -->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/testdb"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <!-- mappers:mybatis操作数据库的方式-->
    <mappers>
        <!--<mapper resource="org/mybatis/example/BlogMapper.xml"/>-->
        <!--
            Mybatis操作数据库得方式：
            XML映射配置文件
            注解
        -->
        <package name="com.hopu.dao"/>
    </mappers>
</configuration>
```

- 创建包：com.hopu.dao

### Mybatis使用

#### 创建Dao层接口

操作testdb的user表

```java
package com.hopu.dao;

import com.hopu.pojo.User;
import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Update;

public interface UserDao {
    //增删改查
    //添加
    @Insert("insert into user values(null, #{name}, #{pass})")
    public void insertUser(User user);

    //修改
    @Update("update user set name = #{name}, pass=#{pass} where id = #{id}")
    public void updateUser(User user);

    //删除
    @Delete("delete from user where id = #{id}")
    public void deleteUser(int id);


}
```

#### Demo测试类

```java
package com.hopu.com.hopu.test;

import com.hopu.dao.UserDao;
import com.hopu.pojo.User;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class Demo {
    public static void main(String[] args) throws IOException {
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        SqlSession session = sqlSessionFactory.openSession();
        UserDao dao = session.getMapper(UserDao.class);
        //1、添加
        //dao.insertUser(new User(0, "lisi", "123"));

        //2、修改
        //dao.updateUser(new User(3, "wangwu", "456"));

        //3、删除
        dao.deleteUser(4);
        //mybatis增删改会对数据库进行改动，需要确认提交
        session.commit();
    }
}
```



### 动态SQL

```java
<select id="userInfoFind" resultType="com.yichen.entity.UserInfo">
    select * from userinfo
    <where>
        <if test="id!=0">
            and id=#{id}
        </if>
        <if test="user_name!=null">
            and user_name=#{user_name}
        </if>
        <if test="user_password!=null">
            and user_name=#{user_password}
        </if>
    </where>
</select>
```



```java
<select id="userInfoByIds" parameterType="list" resultType="com.yichen.entity.UserInfo">
    select * from userinfo
    <where>
        <foreach collection="list" open="id in(" close=")" item="id" separator=","">
            #{id}
        </foeach>
    </where>
</select>
```

