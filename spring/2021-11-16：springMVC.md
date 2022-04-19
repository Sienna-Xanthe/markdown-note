

# 学习笔记（11.16）

#### 知识点

###### 一：SpringMVC快速入门

1. ![image-20211115163612917](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211115163612917.png)

2. 代码示例：

   1. pox.xml文件：导入SpringMVC相关坐标

   ```xml
   <!--    pom.xml  -->
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>org.example</groupId>
       <artifactId>itheima_spring_mvc11_16</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <properties>
           <maven.compiler.source>8</maven.compiler.source>
           <maven.compiler.target>8</maven.compiler.target>
       </properties>
   
       <dependencies>
       <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.21</version>
       </dependency>
       <dependency>
           <groupId>c3p0</groupId>
           <artifactId>c3p0</artifactId>
           <version>0.9.1.2</version>
       </dependency>
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.1.10</version>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-test</artifactId>
           <version>5.0.5.RELEASE</version>
       </dependency>
   
   
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.13.1</version>
           <scope>test</scope>
       </dependency>
   
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.0.5.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>org.junit.jupiter</groupId>
           <artifactId>junit-jupiter</artifactId>
           <version>RELEASE</version>
           <scope>compile</scope>
       </dependency>
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-test</artifactId>
           <version>5.0.5.RELEASE</version>
       </dependency>
       <dependency>
           <groupId>javax.servlet</groupId>
           <artifactId>javax.servlet-api</artifactId>
           <version>3.0.1</version>
           <scope>provided</scope>
       </dependency>
       <dependency>
           <groupId>javax.servlet.jsp</groupId>
           <artifactId>javax.servlet.jsp-api</artifactId>
           <version>2.2.1</version>
           <scope>provided</scope>
       </dependency>
   <!--    <dependency>-->
   <!--        <groupId>org.springframework</groupId>-->
   <!--        <artifactId>spring-web</artifactId>-->
   <!--        <version>5.0.5.RELEASE</version>-->
   <!--    </dependency>-->
           <dependency>
               <groupId>org.springframework</groupId>
               <artifactId>spring-webmvc</artifactId>
               <version>5.0.5.RELEASE</version>
           </dependency>
   
       </dependencies>
   
   
   </project>
   ```

   2.  web.xml ： 配置SpringMVC的前端控制器

      ```xml
      <!--    配置SpringMVC的前端控制器-->
          <servlet>
              <servlet-name>DispatcherServlet</servlet-name>
              <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
              <init-param>
                  <param-name>contextConfigLocation</param-name>
                  <param-value>classpath:spring-mvc.xml</param-value>// 5.加载配置SpringMVC核心文件 spring-mvc.xml
              </init-param>
              <load-on-startup>1</load-on-startup>
          </servlet>
          <servlet-mapping>
              <servlet-name>DispatcherServlet</servlet-name>
              <url-pattern>/</url-pattern>
          </servlet-mapping>
      ```

      

   3. 创建controller类和视图

      ```java
      //UserController
      package com.itheima.controller;
      
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      
      @Controller //使用注解
      @RequestMapping("/user")
      public class UserController {
      
          //请求地址  http://localgost:8080/quick
          @RequestMapping("/quick")
          public String save(){
              System.out.println("Controller save running...");
              return "/success.jsp";
          }
      }
      ```

      ```jsp
      //success.jsp
      
      <%--
        Created by IntelliJ IDEA.
        User: Administrator
        Date: 2021/11/16
        Time: 17:04
        To change this template use File | Settings | File Templates.
      --%>
      <%@ page contentType="text/html;charset=UTF-8" language="java" %>
      <html>
      <head>
          <title>Title</title>
      </head>
      <body>
      
      <h1>Success</h1>
      
      </body>
      </html>
      ```

      

   4. 配置SpringMVC核心文件 spring-mvc.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:context ="http://www.springframework.org/schema/context"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                 http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
      
      
      <!--    Controller的组件扫描-->
          <context:component-scan base-package="com.itheima.controller"/>
      </beans>
      ```

      

3. web流程图

   ![image-20211117153244126](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117153244126.png)

   

4. springmvc流程图

   ![image-20211117153414692](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117153414692.png)

5. ![image-20211117153506118](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117153506118.png)

6. 
