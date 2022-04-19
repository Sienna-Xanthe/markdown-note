# **MYSQL**

1. 基础查询（6.21）

```sql
#基础查询
/*
语法：
select 查询列表；
from 表明；
*/

USE myemployees;

#1.查询表中的单个字段
 SELECT last_name  FROM employees;

#2.查询表中多个字段
SELECT last_name,salary,email
FROM employees;

#3.查询表中的所有字段
SELECT *
FROM employees;

#4.查询常量值
SELECT 100;
SELECT 'john';

#5.查询表达式
SELECT 100%98;

#6.查询函数
SELECT VERSION();

#7.起别名
/*
1）便于理解
2）如果要查询的字段有重名的情况，使用别名可以区分
*/
#方式一： AS
SELECT 100%98 AS 结果;
SELECT last_name AS 姓, first_name AS 名 from employees;

#方式二：空格
SELECT last_name 姓,first_name 名 FROM employees;

#案例：查询salsry，显示结果为 out put
SELECT salary AS 'out put' FROM employees;

#8.去重

#案例：查询员工表中涉及到的所有的部门编号
SELECT DISTINCT department_id FROM employees;

#9.+号的作用
/*
只做运算符
SELECT 110+90; 两个操作数都为数值型，则做加法运算
SELECT '123'+90; 其中一方为字符型，视图将字符型数值转换成数值型
								     如果转换成功，则继续做加法运算
SELECT	'john'+90;	 如果转换失败，则将字符型转换为0

SELECT null+10;    只要其中一方为null，则结果肯定为null

*/


#案例：查询员工名和姓连接成一个字段，并显示为姓名

SELECT CONCAT(last_name,' ',first_name) AS 姓名；

FROM employees;

#10.departments的结构，并查询其中的全部数据

DESC departments;  #单独执行
SELECT * 
FROM departments;

#11.显示出表employees的全部列，各个列之间用逗号连接，列头显示为 OUT_PUT

#IFNULL(expr1,expr2)函数表示判断expr1是否为null，如果是就显示为expr2，反之就是原来的显示
SELECT
        IFNULL(commission_pct,0) AS 奖金率,
				commission_pct
FROM 
			  employees;
#--------------------------------------------

SELECT 
       CONCAT(first_name,',',last_name,',',IFNULL(commission_pct,0)) AS "OUT_PUT"
FROM 
			 employees;
```

2. 

