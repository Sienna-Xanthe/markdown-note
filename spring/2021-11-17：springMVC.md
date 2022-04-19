

# 学习笔记（11.17）

#### 知识点

###### 一：SpringMVC的组件解析

1. ![image-20211117160757551](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117160757551.png)

2. ![image-20211117160859714](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117160859714.png)

3. ![image-20211117162036962](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117162036962.png)

   ```java
   package com.itheima.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       //请求地址  http://localgost:8080/quick
       @RequestMapping(value = "/quick",method = RequestMethod.GET,params = {"username"})
       public String save(){
           System.out.println("Controller save running...");
           return "/success.jsp";
       }
   }
   ```

   

4. ![image-20211117162120141](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117162120141.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context ="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   
   
   <!--    Controller的组件扫描-->
       <context:component-scan base-package="com.itheima">
           <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>//包括
       </context:component-scan>
   </beans>
   ```

5. 关于forword 和 redirect

   ```java
   package com.itheima.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       //请求地址  http://localgost:8080/quick
       @RequestMapping(value = "/quick",method = RequestMethod.GET,params = {"username"})
       public String save(){
           System.out.println("Controller save running...");
           return "redirect:/success.jsp";//重定向
           return "forward:/success.jsp";//转发
       }
   }
   ```

6. ![image-20211117171238954](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117171238954.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:context ="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   
   
   <!--    Controller的组件扫描-->
       <context:component-scan base-package="com.itheima">
           <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
       </context:component-scan>
   
   <!--    配置内部资源视图解析器-->
       <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
   <!--        /jsp/  -->
           <property name="prefix" value="/jsp/"></property>
           <property name="suffix" value=".jsp"></property>
       </bean>
   </beans>
   ```

   

7. ![image-20211117171617706](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117171617706.png)

   

###### 二：SpringMVC的数据响应

1. SpringMVC的数据响应方式

   ![image-20211117200630230](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117200630230.png)

2. 页面跳转

   ![image-20211117200901298](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117200901298.png)

   

3. 页面跳转代码示例

   ```java
   package com.itheima.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.servlet.ModelAndView;
   
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       //第五种方式（不常用）
       @RequestMapping("/quick5")
       public String save5(HttpServletRequest request){
           //设置模型数据
           request.setAttribute("username","第五种方式（不常用）");
           return "success";
       }
       
       //第四种方法
       @RequestMapping("/quick4")
       public String save4(Model model){
           //设置模型数据
           model.addAttribute("username","第四种方法");
           return "success";
       }
   	//第三种方法
       @RequestMapping("/quick3")
       public ModelAndView save3(ModelAndView modelAndView){
           //设置模型数据
           modelAndView.addObject("username","第三种方法");
           //设置视图名称
           modelAndView.setViewName("success");
           return modelAndView;
       }
   	//第二种方法
       @RequestMapping("/quick2")
       public ModelAndView save2(){
           /*
           Model:模型  作用封装数据
           View: 视图  作用展示数据
            */
           ModelAndView modelAndView = new ModelAndView();
           //设置模型数据
           modelAndView.addObject("username","第二种方法");
           //设置视图名称
           modelAndView.setViewName("success");
           return modelAndView;
       }
   	//第一种方法
       //请求地址  http://localgost:8080/quick
       @RequestMapping(value = "/quick",method = RequestMethod.GET,params = {"username"})
       public String save(){
           System.out.println("Controller save running...");
           return "success";
       }
   }
   ```

4. 回写数据

   ![image-20211117204356069](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211117204356069.png)

   数据回写代码示例

   ```Java
   package com.itheima.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       //回写数据方法2
       @RequestMapping("/quick7")
       @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
       public String save7()  {
           return "回写数据方法2";
       }
   
       //回写数据方法1
       @RequestMapping("/quick6")
       public void save6(HttpServletResponse response) throws IOException {
           response.getWriter().print("回写数据方法1");
       }
   
       //请求地址  http://localgost:8080/quick
       @RequestMapping(value = "/quick",method = RequestMethod.GET,params = {"username"})
       public String save(){
           System.out.println("Controller save running...");
           return "success";
       }
   }
   ```

   

   

5. 数据回写，直接回写json格式字符串

   ```xml
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
   ```

   ```java
   package com.itheima.controller;
   
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.itheima.domain.User;
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
       //回写数据-->json格式返回
       @RequestMapping("/quick8")
       @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
       public String save8() throws JsonProcessingException {
           User user = new User();
           user.setUsername("aha");
           user.setAge(12);
           //使用json的转换工具将对象转换成json格式字符串的返回
           ObjectMapper objectMapper = new ObjectMapper();
           String json = objectMapper.writeValueAsString(user);
   
           return json;
       }
   }
   ```

   

6. ![image-20211118094541717](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118094541717.png)

   ```xml
   <!--    配置处理器映射器-->
       <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
           <property name="messageConverters">
               <list>
                   <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
               </list>
           </property>
       </bean>
   ```

   ```java
   package com.itheima.controller;
   
   import com.fasterxml.jackson.core.JsonProcessingException;
   import com.fasterxml.jackson.databind.ObjectMapper;
   import com.itheima.domain.User;
   import org.springframework.stereotype.Controller;
   import org.springframework.ui.Model;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RequestMethod;
   import org.springframework.web.bind.annotation.ResponseBody;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   import java.io.IOException;
   
   @Controller
   @RequestMapping("/user")
   public class UserController {
   
   
       @RequestMapping("/quick9")
       @ResponseBody //告知SpringMVC框架 不进行视图跳转，直接进行数据响应
       // 期望SpringMVC自动将User转换成json格式的字符串
       public User save9() throws JsonProcessingException {
           User user = new User();
           user.setUsername("kinkrit");
           user.setAge(19);
   
   
           return user;
       }
   }
   
   //输出：{"username":"kinkrit","age":19}
   ```

   

   

7. 回写数据优化：

   ![image-20211118100754302](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118100754302.png)

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context ="http://www.springframework.org/schema/context"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
   
   
   <!--    Controller的组件扫描-->
       <context:component-scan base-package="com.itheima">
           <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
       </context:component-scan>
   
   <!--    配置内部资源视图解析器-->
       <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
   <!--        /jsp/  -->
           <property name="prefix" value="/jsp/"></property>
           <property name="suffix" value=".jsp"></property>
       </bean>
   
   <!--&lt;!&ndash;    配置处理器映射器&ndash;&gt;-->
   <!--    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">-->
   <!--        <property name="messageConverters">-->
   <!--            <list>-->
   <!--                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>-->
   <!--            </list>-->
   <!--        </property>-->
   <!--    </bean>-->
   
   <!--    MVC的注解-->
       <mvc:annotation-driven/>
       
       
   </beans>
   ```

   输出：{"username":"kinkrit","age":19}

   

8. ![image-20211118101341406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211118101341406.png)

#### 问题

###### 问题描述

在导入jackson包之后，报错

```
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'userController': Lookup method resolution failed; nested exception is java.lang.IllegalStateException: Failed to introspect Class [com.itheima.controller.UserController] from ClassLoader [ParallelWebappClassLoader
  context: itheima_spring_mvc11_16_war_exploded
```

###### 问题分析

jar包可能没有导入

###### 问题解决

打开Project Structure–>Artifacts，把原来的删了再新加上一个,就好了

![img](https://img2020.cnblogs.com/blog/1833731/202009/1833731-20200928200546783-583824016.png)

 删掉在加回去就行了，用的IDEA2020.1 不知道为什么反正BUG很多