

# 学习笔记（11.19）

#### 知识点

###### 一：Spring JdbcTemplate基本使用

1. ![image-20211119090425053](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119090425053.png)

   

2. jdbcTemplate开发步骤

   ![image-20211119090702619](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119090702619.png)

   1. ```xml
      //pom.xml
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
      ```

   2. ![image-20211119092723509](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119092723509.png)

   3. ![image-20211119095334075](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119095334075.png)

      

3. Spring产生JdbcTemplate对象

   ![image-20211119095715638](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119095715638.png)

   ![image-20211119101926200](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119101926200.png)

   ```java
   @Test
   //测试Spring产生jdbcTemplate对象
   public void test2() throws PropertyVetoException {
       ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext.xml");
       JdbcTemplate jdbcTemplate = app.getBean(JdbcTemplate.class);
       int row = jdbcTemplate.update("insert into account values(3,?,?)", "alkali1", 8000);
       System.out.println(row);
   }
   ```

   优化：

   ![image-20211119102450641](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119102450641.png)

   ![image-20211119102540123](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119102540123.png)

   

   

4. JdbcTemplate基本操作

   ![image-20211119103136756](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119103136756.png)

   ```java
   //JdbcTemplateCRUDTest.java
   package com.itheima.test;
   
   
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.jdbc.core.JdbcTemplate;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration("classpath:applicationContext.xml")
   public class JdbcTemplateCRUDTest {
   
       @Autowired
       private JdbcTemplate jdbcTemplate;
   
       @Test
       public void testUpdate(){
           jdbcTemplate.update("update account set money = ? where name = ?",10000,"tom");
       }
   
       @Test
       public void testDelete(){
           jdbcTemplate.update("delete from account where name = ?","tom");
       }
   
   }
   ```

   

5. 查询操作

   1. 查询全部

      ```java
      @Test
      public void testQueryAll(){
          List<Account> accountList = jdbcTemplate.query("select * from account", new BeanPropertyRowMapper<Account>(Account.class));
          System.out.println(accountList);
      }
      
      //输出：[Account{name='alkali', money=8000.0}, Account{name='alkali1', money=8000.0}, Account{name='assd', money=8000.0}, Account{name='assd', money=8000.0}]
      ```

   2. 查询单个

      ```java
      @Test
      public void testQueryOne(){
          Account account = jdbcTemplate.queryForObject("select * from account where name = ?", new BeanPropertyRowMapper<Account>(Account.class), "assd");
          System.out.println(account);
      }
      //输出：Account{name='assd', money=8000.0}
      ```

      

   3. 查询数量

      ![image-20211119105144755](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119105144755.png)

      ```java
      @Test
      public void testQueryCount(){
          Long count = jdbcTemplate.queryForObject("select count(*) from account", Long.class);
          System.out.println(count);
      }
      //输出：4
      ```

   4. ![image-20211119105754202](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119105754202.png)

      

###### 二：Spring练习

1. 环境搭建

   ![image-20211119105926399](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211119105926399.png)

   

