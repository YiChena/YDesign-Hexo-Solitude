---
title: JavaWeb
date: 2022-10-01 16:17:28
cover: https://ydesign.oss-cn-beijing.aliyuncs.com/Java%20Web.png
tags:  
    - 技术
    - Java
---

# Java Web

- C/S 程序、B/S 程序:浏览器-服务器的程序（网站）

- 请求响应模型

  - 浏览器发送一个请求，请求某个服务器中的资源（静态资源和动态资源）
  - 服务器接收到请求响应结果给浏览器

- 浏览器发送请求的方式

  - 超链接：a 
  - form 表单
  - js：window.location.href
  - ajax：jquery 的 $.ajax()，post()，get()

- 发送请求时，传递数据的方式

  - get
  - post

- 请求交给 Servlet 类处理，Servlet 必须直接或间接实现 Servlet 接口，一般用继承 HttpServlet 类的方式

  - 写一个类继承 HttpServlet

    - 导入jar包

      ```xml
          <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
          </dependency>
      ```

    - 创建一个类，继承HttpServlet类，添加@WebServlet注解，指定处理的请求

      ```java
      package com.hopu.servlet;
      
      import javax.servlet.ServletException;
      import javax.servlet.annotation.WebServlet;
      import javax.servlet.http.HttpServlet;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import java.io.IOException;
      
      @WebServlet("/login")
      public class LoginServlet extends HttpServlet {
      
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              System.out.println("Get");
          }
      
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              System.out.println("Post");
          }
      }
      
      ```

  - 重写doGet和doPost方法

- 在doGet或doPost方法中处理请求的步骤

  - 获取请求参数

    - request.getParameter("请求参数的名字")

      ```java
      package com.hopu.servlet;
      
      import javax.servlet.ServletException;
      import javax.servlet.annotation.WebServlet;
      import javax.servlet.http.HttpServlet;
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      import java.io.IOException;
      
      @WebServlet("/login")
      public class TestServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              doPost(req, resp);
          }
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
              System.out.println("处理请求的地方post");
              //处理请求的位置
              //1、获取请求参数
              //获取登录时用户输入的账号和名
              String name = req.getParameter("name");
              String pass = req.getParameter("pass");
              System.out.println(name+"\t"+pass);
      
              //2、调用service、dao层操作数据库
      
              //3、共享数据
      
              //4、响应（把结果给浏览器）
      
          }
      }
      ```

    - 在浏览器发送请求

      http://localhost/web/login?name=zs&pass=123

    - 控制台打印结果

  - 调用service类，操作数据库

    - 准备数据库

      创建testdb数据库，创建用户表user，创建列：id,name,pass，插入一条数据

      ```sql
      create table user(
      	id int primary key auto_increment,
      	name varchar(50),
      	pass varchar(50)
      );
      
      insert into user values(null, 'zs', '123');
      ```

    - 在pom.xml中导入数据库驱动jar包

      ```xml
          <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.44</version>
          </dependency>
      ```

    - 创建实体类

      导入lombok的jar包

      ```
          <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.18</version>
          </dependency>
      ```

      编写实体类

      ```java
      package com.hopu.pojo;
      
      import lombok.AllArgsConstructor;
      import lombok.Data;
      import lombok.NoArgsConstructor;
      import lombok.ToString;
      
      @Data
      @NoArgsConstructor
      @AllArgsConstructor
      @ToString
      public class User {
          private int id;
          private String name;
          private String pass;
      }
      ```

      

    - 创建Service类

      ```java
      package com.hopu.service;
      
      import com.hopu.dao.UserDao;
      import com.hopu.pojo.User;
      
      public class UserService {
      
          public User login(String name, String pass){
              return new UserDao().login(name, pass);
          }
      }
      ```

      

    - 创建Dao层类

      ```java
      package com.hopu.dao;
      
      import com.hopu.pojo.User;
      
      import java.sql.Connection;
      import java.sql.DriverManager;
      import java.sql.ResultSet;
      import java.sql.Statement;
      
      public class UserDao {
          public User login(String name, String pass) {
              //操作数据库
              try {
                  Class.forName("com.mysql.jdbc.Driver");//加载驱动
                  Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/testdb", "root", "123456");
                  Statement st = conn.createStatement();
                  ResultSet rs = st.executeQuery("select * from user where name = '" + name + "' and pass='" + pass + "'");
                  if(rs.next()){
                      return new User(rs.getInt(1), rs.getString(2), rs.getString(3));
                  }
              } catch (Exception e) {
                  e.printStackTrace();
              }
              return null;
          }
      }
      ```

      

    - 在Servlet类调用Service

  - 共享数据

    - request：共享的数据只在一次请求响应内有效
    - session：在一个会话内有效
    - application：整个项目发布期间有效

  - 响应

    - 重定向：浏览器一个会发送两次请求
    - 请求转发：浏览器只会发送一次请求

- 使用EL表达式获取共享的数据，使用JSTL把数据显示在页面指定的位置