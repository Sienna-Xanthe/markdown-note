

# 学习笔记（11.18）

#### 知识点

###### 一：SpringMVC获得请求数据

1. ![image-20211118101704854](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118101704854.png)

   

2. 获得基本数据类型

   ![image-20211118101844651](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118101844651.png)

   ```java
   @RequestMapping(value = "/quick10",params = {"username","age"})
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save10(String username,int age) throws JsonProcessingException {
       System.out.println(username);
       System.out.println(age);
   }
   
   //控制台输出具体数据
   ```

   

3. 获得POJO类型参数

   ![image-20211118103135027](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118103135027.png)

   ```java
   @RequestMapping(value = "/quick11",params = {"username","age"})
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save11(User user) throws JsonProcessingException {
       System.out.println(user);
   }
   ```

   输出：

   User{username='askjd', age=45}

   //只要有user这个对象与其输入的属性名字一致，且该类中有toString方法，则可自动封装为user对象

   

4. 获得数组类型参数

   ![image-20211118103806098](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118103806098.png)

   ```java
   @RequestMapping(value = "/quick12")
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save12(String[] strs) throws JsonProcessingException {
       System.out.println(Arrays.asList(strs));
   }
   ```

   输出：[aad, asdg, 534sdf]

   

5. 获得集合类型参数

   一：

   ```java
   //UserController
   @RequestMapping(value = "/quick13")
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save13(VO vo) throws JsonProcessingException {
       System.out.println(vo);
   }
   ```

   ```java
   //VO
   package com.itheima.domain;
   
   import java.util.List;
   
   public class VO {
       private List<User> userList;
   
       public List<User> getUserList() {
           return userList;
       }
   
       public void setUserList(List<User> userList) {
           this.userList = userList;
       }
   
       @Override
       public String toString() {
           return "VO{" +
                   "userList=" + userList +
                   '}';
       }
   }
   ```

   ```jsp
   //form.jsp
   <%--
     Created by IntelliJ IDEA.
     User: Administrator
     Date: 2021/11/18
     Time: 10:59
     To change this template use File | Settings | File Templates.
   --%>
   <%@ page contentType="text/html;charset=UTF-8" language="java" %>
   <html>
   <head>
       <title>Title</title>
   </head>
   <body>
   
       <form action="${pageContext.request.contextPath}/user/quick13" method="post">
   <%--      表明是第几个User对象的username--%>
         <input type="text" name="userList[0].username"><br/>
         <input type="text" name="userList[0].age"><br/>
         <input type="text" name="userList[1].username"><br/>
         <input type="text" name="userList[1].age"><br/>
           <input type="submit" value="提交">
   
       </form>
   </body>
   </html>
   ```

   输出： VO{userList=[User{username='billkin', age=22}, User{username='ppkrit', age=22}]}

   

   二：

   ![image-20211118143641976](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118143641976.png)

   ![image-20211118171927701](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118171927701.png)

   ```java
    @RequestMapping(value = "/quick15")
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save15(@RequestBody List<User> userList) throws JsonProcessingException {
       System.out.println(userList);
   }
   ```

   ![image-20211118172019850](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118172019850.png)

   ```xml
       
      //方式1：
   <!--    开放资源的访问-->
       <mvc:resources mapping="/js/**" location="/js/"/>
       //方式2：
       <mvc:default-servlet-handler/>
   ```

   

6. 请求数据的乱码问题

   ![image-20211118172627134](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118172627134.png)

   ```xml
   //web.xml
   <!--    配置全局过滤的filter-->
       <filter>
           <filter-name>CharacterEncodingFilter</filter-name>
           <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
           <init-param>
               <param-name>encoding</param-name>
               <param-value>UTF-8</param-value>
           </init-param>
       </filter>
       <filter-mapping>
           <filter-name>CharacterEncodingFilter</filter-name>
           <url-pattern>/*</url-pattern>
       </filter-mapping>
   ```

   

   

7. 参数绑定注解@requestParam

   ![image-20211118173925324](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118173925324.png)

   ![image-20211118173937281](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118173937281.png)

   ```java
   @RequestMapping(value = "/quick16")
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save16(@RequestParam(value = "name",required = false,defaultValue = "pxy") String username)  {
       System.out.println(username);
   }
   ```

   

8. 获取Restful风格的参数

   ![image-20211118174936219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118174936219.png)

   ![image-20211118175005302](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118175005302.png)

   ```java
   //localhost:8080/user/quick17/zhangsan
   @RequestMapping(value = "/quick17/{username}")
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save17(@PathVariable(value = "username") String username)  {
       System.out.println(username);
   }
   ```

   

9. 自定义类型转换器

   ![image-20211118180606030](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118180606030.png)

   ![image-20211118181140050](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118181140050.png)

   ```xml
   //spring-mvc.xml
   
   <!--    MVC的注解驱动-->
       <mvc:annotation-driven conversion-service="conversionService"/>
       
   <!--    开放资源的访问-->
   <!--    <mvc:resources mapping="/js/**" location="/js/"/>-->
   
       <mvc:default-servlet-handler/>
   
   <!--    声明转换器-->
       <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
           <property name="converters">
               <list>
                   <bean class="com.itheima.converter.DataConverter"></bean>
               </list>
           </property>
       </bean>
   ```

   输出：Sat Dec 11 00:00:00 CST 2021

10. ![image-20211118202240127](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118202240127.png)

    ```java
    @RequestMapping(value = "/quick19")
    @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
    public void save19(HttpServletRequest request, HttpServletResponse response, HttpSession session)  {
        System.out.println(request);
        System.out.println(response);
        System.out.println(session);
    }
    ```

    输出：

    org.apache.catalina.connector.RequestFacade@289afbdb
    org.apache.catalina.connector.ResponseFacade@7f5a69cd
    org.apache.catalina.session.StandardSessionFacade@47a55922

    

11. 获取请求头

    ![image-20211118203459562](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118203459562.png)

    ```java
    @RequestMapping(value = "/quick20")
    @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
    public void save20(@RequestHeader(value = "User-Agent",required = false) String user_agent)  {
        System.out.println(user_agent);
    }
    //输出:Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/95.0.4638.69 Safari/537.36
    ```

    ![image-20211118203543066](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118203543066.png)

    ```java
    @RequestMapping(value = "/quick21")
    @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
    public void save21(@CookieValue(value = "JSESSIONID") String jsessionId)  {
        System.out.println(jsessionId);
    }
    //输出：E2B9AA511A0622525F0F488945DDDC8A
    ```

    

###### 二：SpringMVC获得请求数据文件上传

1. ![image-20211118204154302](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118204154302.png)

   ![image-20211118210649965](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118210649965.png)

   ![image-20211118205305717](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118205305717.png)

   ![image-20211118205624004](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118205624004.png)

   ![image-20211118205655448](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118205655448.png)

   ```xml
   //pom.xml
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
   ```

   ![image-20211118210038191](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118210038191.png)

   ```xml
   //spring-mvc.xml
   <!--    配置文件上传解析器-->
       <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
           <property name="defaultEncoding" value="UTF-8"/>
           <property name="maxUploadSize" value="500000"/>
       </bean>
   ```

   ![image-20211118210430356](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118210430356.png)

   ```java
   @RequestMapping(value = "/quick22")
       @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
       public void save22(String username, MultipartFile uploadFile) throws IOException {
           System.out.println(username);
           System.out.println(uploadFile);
           //获得上传文件的名称
           String originalFilename = uploadFile.getOriginalFilename();//获取文件名
           uploadFile.transferTo(new File("E:\\upload\\" + originalFilename));//存储文件的地址以及名字
       }
   ```

   输出：

   dada
   org.springframework.web.multipart.commons.CommonsMultipartFile@c287261

   

2. 多文件上传

   ![image-20211118213438675](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118213438675.png)

   ![image-20211118213449055](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118213449055.png)

   ```java
   @RequestMapping(value = "/quick23")
   @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
   public void save23(String username, MultipartFile[] uploadFile) throws IOException {
       System.out.println(username);
       for (MultipartFile multipartFile : uploadFile) {
           //获得上传文件的名称
           String originalFilename = multipartFile.getOriginalFilename();
           multipartFile.transferTo(new File("E:\\upload\\" + originalFilename));
       }
   
   }
   ```

3. ![image-20211118215128897](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118215128897.png)

   