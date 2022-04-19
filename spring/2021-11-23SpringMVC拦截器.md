

# 学习笔记（11.23）SpringMVC拦截器

#### 知识点

###### 一：拦截器（interceptor）的作用

1. ![image-20211123153043919](https://gitee.com/kinkrit/homework-drawing-bed/raw/master//img/image-20211123153043919.png)

2. 总结：

   在访问目标资源时做相应的干预

3. 拦截器和过滤器的区别

   ![image-20211123153712150](https://gitee.com/kinkrit/homework-drawing-bed/raw/master//img/image-20211123153712150.png)

   

4. 拦截器的快速入门

   ![image-20211123154727985](https://gitee.com/kinkrit/homework-drawing-bed/raw/master//img/image-20211123154727985.png)

   1. pom.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <project xmlns="http://maven.apache.org/POM/4.0.0"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
          <modelVersion>4.0.0</modelVersion>
      
          <groupId>org.example</groupId>
          <artifactId>itheima_spring_interceptor</artifactId>
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
      
              <dependency>
                  <groupId>com.fasterxml.jackson.core</groupId>
                  <artifactId>jackson-core</artifactId>
                  <version>2.11.0</version>
              </dependency>
              <dependency>
                  <groupId>com.fasterxml.jackson.core</groupId>
                  <artifactId>jackson-databind</artifactId>
                  <version>2.11.0</version>
              </dependency>
              <dependency>
                  <groupId>com.fasterxml.jackson.core</groupId>
                  <artifactId>jackson-annotations</artifactId>
                  <version>2.11.0</version>
              </dependency>
      
              <dependency>
                  <groupId>commons-fileupload</groupId>
                  <artifactId>commons-fileupload</artifactId>
                  <version>1.3.1</version>
              </dependency>
              <dependency>
                  <groupId>commons-io</groupId>
                  <artifactId>commons-io</artifactId>
                  <version>2.3</version>
              </dependency>
      
              <!--        ///////////////////////////-->
              <dependency>
                  <groupId>commons-logging</groupId>
                  <artifactId>commons-logging</artifactId>
                  <version>1.2</version>
              </dependency>
              <dependency>
                  <groupId>org.slf4j</groupId>
                  <artifactId>slf4j-log4j12</artifactId>
                  <version>1.7.7</version>
              </dependency>
              <dependency>
                  <groupId>log4j</groupId>
                  <artifactId>log4j</artifactId>
                  <version>1.2.17</version>
              </dependency>
              <dependency>
                  <groupId>org.springframework</groupId>
                  <artifactId>spring-jdbc</artifactId>
                  <version>5.0.5.RELEASE</version>
              </dependency>
              <dependency>
                  <groupId>org.springframework</groupId>
                  <artifactId>spring-tx</artifactId>
                  <version>5.0.5.RELEASE</version>
              </dependency>
              <dependency>
                  <groupId>jstl</groupId>
                  <artifactId>jstl</artifactId>
                  <version>1.2</version>
              </dependency>
      
          </dependencies>
      
      </project>
      ```

      

   2. spring-mvc.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:mvc="http://www.springframework.org/schema/mvc"
             xmlns:context="http://www.springframework.org/schema/context"
             xsi:schemaLocation="
             http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
             http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
             http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      
      ">
          <!--    1.mvc注解驱动-->
          <mvc:annotation-driven/>
      
          <!--    2.配置视图解析器-->
          <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
              <property name="prefix" value="/"/>
              <property name="suffix" value=".jsp"/>
          </bean>
      
          <!--    3.静态资源权限开放-->
          <mvc:default-servlet-handler/>
      
          <!--    4.组件扫描 扫描Controller-->
          <context:component-scan base-package="com.itheima.controller"/>
      
      <!--    配置拦截器-->
          <mvc:interceptors>
              <mvc:interceptor>
      <!--            对哪些资源执行拦截操作-->
                  <mvc:mapping path="/**"/>
                  <bean class="com.itheima.interceptor.MyInterceptor1"/>
              </mvc:interceptor>
          </mvc:interceptors>
      
      </beans>
      ```

      

   3. TargetController

      ```java
      package com.itheima.controller;
      
      import org.springframework.stereotype.Controller;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.servlet.ModelAndView;
      
      @Controller
      public class TargetController {
      
          @RequestMapping("/target")
          public ModelAndView show(){
              System.out.println("目标资源执行.....");
              ModelAndView modelAndView = new ModelAndView();
              modelAndView.addObject("name","itcast");
              modelAndView.setViewName("index");
              return modelAndView;
          }
      }
      ```

      

   4. interceptor/MyInterceptor1

      ```java
      package com.itheima.interceptor;
      
      import org.springframework.web.servlet.HandlerInterceptor;
      import org.springframework.web.servlet.ModelAndView;
      
      import javax.servlet.http.HttpServletRequest;
      import javax.servlet.http.HttpServletResponse;
      
      public class MyInterceptor1 implements HandlerInterceptor {
      
          @Override
          //在目标方法执行之前执行
          public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
              System.out.println("preHandle....");
              return true;//这里返回的值决定着下面的程序是否运行
          }
      
          @Override
          //在目标方法执行之后 视图对象返回之前执行
          public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
              System.out.println("postHandle......");
          }
      
          @Override
          //在流程都执行完毕之后 执行
          public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
              System.out.println("afterCompletion.....");
          }
      }
      ```

      ![image-20211123192312589](https://gitee.com/kinkrit/homework-drawing-bed/raw/master//img/image-20211123192312589.png)

5. ![image-20211123192452932](https://gitee.com/kinkrit/homework-drawing-bed/raw/master//img/image-20211123192452932.png)

6. 

7. 

8. 

###### 二：

内容

###### 三：

内容

###### 四：

1. 
2. 

###### 五：

内容

###### 六：

内容

###### 七：

1. 
2. 

###### 八：

内容

###### 九：

内容

...

#### 问题

###### 问题描述

内容

###### 问题分析

内容

###### 问题解决

内容