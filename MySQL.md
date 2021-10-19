











# MySQL



## 开启服务和登录

开启服务要以管理员身份运行

![image-20210818083046068](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210818083046068.png)



- 登录MySQL

![image-20210921144121341](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921144121341.png)

## DQL查询语言

## 基本查询和条件查询

```mysql
SHOW DATABASES;
-- 进阶一：基本查询
#语法：select 查询列表（可以是字段、常量值等） from 表名；
#查询结果是一个虚拟的表格，表格数据的顺序等不会改变真实表格的


#查询之前要先打开对应的库
USE myemployees;
#查询单个字段(要执行的语句选中执行)
SELECT last_name FROM employees;

#查询多个字段
SELECT last_name,salary,email FROM employees;
/*
查询表中所有字段 
方法一：双击表中栏位所有字段(优点：字段顺序可以灵活呈现)
	``这是着重号，作用：有时关键字会与字段字相等，加在字段上区分
*/
SELECT 
  `first_name` 
  `last_name` 
  `email`
  `phone_number`
  `job_id`
  `department_id`
  `hiredate`
  `salary`
  `job_id` 
FROM
  employees ;
#方法二 缺点：字段只能按照原始表格顺序呈现
SELECT * FROM employees;

#查询常量（包含数值常量和字符常量等）
SELECT 
	100;
#字符都用单引号包裹
SELECT
	'小明';
#查询表达式
SELECT
	100+20;#返回的是计算结果
	
#查询函数
SELECT
	version();#输出的是返回值



/*起别名
作用：便于理解，当字段有重复时便于区分	*/
#方法一：用as（可读性强）
SELECT
	last_name AS 姓,first_name AS 名
FROM 	employees;
#方法二：用空格
SELECT
	phone_number 电话号码,
	email 邮箱
FROM employees;

#当别名中有特殊符号时别名要用双引号阔起来
SELECT
	salary AS "in com"
FROM 	employees;


#去重 案例：找出所有工种编号用到的编号
-- DISTINCT后面有多个列的话是作用与后面的所有列而不是单个列,且个数会跟最多的那个一样多
SELECT
	DISTINCT job_id,
	last_name
FROM 	employees;


/*
+号的作用   #last_name+first_name（错的）
只能用于运算，在java中+号可以连接字符串，数据库不行

SELECT 100+20; 是做加法运算
SELECT '123'+20; 当一方为字符型的时候，会尝试将字符型转化成数值型
	  若转化成功就得到做运算
SELECT 'lai'+10;	  若转化失败则字符型转为0
SELECT 10+null+200;	若其中一方为null，则结果为null

有一个连接字符的函数concat（）
还有一个ifnull（）函数，判断值是否为null，如果为null让它等于指定的值
*/
#案例：把姓和名连接起来并起别名为姓名
SELECT
	concat(last_name,first_name) AS 姓名
FROM 	employees;
#测试ifnull（）；
SELECT
	concat(last_name,ifnull(commission_pct,0))
FROM 	employees;

#显示表的结构
DESC
	departments;

#只查询前几行数据OFFSET 3是跳过第三行从第四行开始
SELECT
	manager_id
FROM 	departments
LIMIT	5
OFFSET	3;
-- OFFSET可省略
SELECT
	manager_id
FROM 	departments
LIMIT	3,5;





/*进阶二：条件查询
语法：
select 
	查询列表
from	表名
where	查询条件

分类：
一： 按条件表达式
条件运算符：>,<,=,>=,<=,<>(!=)
二： 按逻辑表达式
逻辑运算符：&&，||,! (java)
	and,or,not (sql)
三：模糊查询
	like
	between and
	is null
	in
	
*/

-- 按条件表达式
-- 案例一：查询工资大于12000的员工信息（的后面就是要查的信息）
SELECT
	*
FROM 	employees
WHERE	salary>12000;

-- 案例二：查询部门编号不等于90的员工名和部门编号
SELECT
	last_name,department_id
FROM 	employees
WHERE 	department_id<>90;

-- 按逻辑表达式	
-- 查询工资在10000到20000之间的员工名、工资及奖金
SELECT
	last_name,salary,commission_pct
FROM 	employees
WHERE 	salary>10000 AND salary<20000;

-- 查询部门编号不是在90到110之间，或者工资高于15000的员工信息
SELECT
	*
FROM 	employees
WHERE 	NOT(department_id>90 AND department_id<110) OR salary>15000;

/*
模糊查询
1.like	通配符
	%表示若干个字符，包含0个字符
	_下划线代表一个字符
2.between and
	1.包含临界值
	2.提高代码的简洁度
	3.and前后两个值不可调换位置（因为默认表示的是大于等于和小于等于）
	
3.is null  ,is not null
	1.只能判断null值
	2.可读性较高

4.in 	判断某字段是否在in列表中的某一项
	相当于用or连接，但是用in代码更简洁
	in里面值的类型要一直或者兼容
	in里面的值不能用通配符

5. <=>(安全等于)
	1.可以判断null值
	2.也可判断其他值
	3.可读性较低
*/
-- like 案例1:查询名字里面包含字母a的员工信息
SELECT
	*
FROM 	employees
WHERE	last_name LIKE '%a%';
-- 查询名字里面第四个是m第六个是n的员工信息
SELECT
	*
FROM 	employees
WHERE	last_name LIKE'___M_N%';


#between and 查询工资在10000到20000之间的员工信息
SELECT
	*
FROM 	employees
WHERE	salary BETWEEN 10000 AND 20000;

-- IS NULL 查询没有奖金的员工信息
SELECT
	*
FROM 	employees
WHERE	commission_pct IS NULL;

-- IS NOT NULL 查询有奖金的员工信息
SELECT 
	*
FROM 	employees
WHERE 	commission_pct IS NOT NULL;

# <=>查询工资是9500的员工信息
SELECT
	*
FROM 	employees
WHERE 	salary <=>9500;

-- 查询工种编号是IT_PROG或者FI_MGR或者PU_CLERK的员工信息
SELECT 
	*
FROM 	employees
WHERE	job_id IN('IT_PROG','FI_MGR','PU_CLERK');

#转义字符 案例：查询员工名中第二个字符为_的员工名
       -- 可以用\，也可以指定一个字符作转义字符 ESCAPE '$'
SELECT
	last_name
FROM  
	employees
WHERE
	last_name LIKE '_\_%';
	
	
SELECT
	last_name
FROM  
	employees
WHERE
	last_name LIKE '_$_%' ESCAPE '$';
		




#练习	
1.查询工资大于12000的员工姓名和工资
SELECT
	last_name,
	first_name,
	salary
FROM 	employees
WHERE	salary>12000;
#2.查询员工号为176的员工姓名和部门号和年薪
SELECT
	last_name,
	first_name,
	department_id,
	salary*12*(1+commission_pct)
FROM 	employees
WHERE	employee_id IN 176;
#3选择在20或50号部门工作的员工姓名和部门号
SELECT
	last_name,
	first_name,
	department_id
FROM 	employees
WHERE	department_id IN('20','50');
#4选择工资不在5000到12000的员工的姓名和工资
SELECT
	first_name,
	salary
FROM 	employees
WHERE 	NOT(salary BETWEEN 5000 AND 12000);
#5选择公司中没有管理者的员工姓名及job_id
SELECT 
	first_name,
	job_id
FROM 	employees
WHERE	manager_id IS NULL;
#6选择公司中有奖金的员工姓名，工资和奖金级别
SELECT
	first_name,
	salary,
	commission_pct
FROM 	employees
WHERE 	commission_pct IS NOT NULL;
#7选择员工姓名的第三个字母是a的员工姓名
SELECT
	first_name
FROM 	employees
WHERE 	first_name LIKE '__a%';
#8选择员工姓名中有字母a和c的员工姓名
SELECT
	first_name
FROM 	employees
WHERE 	first_name LIKE'%a%' AND first_name LIKE'%e%';
#9显示出表有first_name以‘e‘结尾的员工信息
SELECT 
	*
FROM 	employees
WHERE 	first_name LIKE '%e';
```

## 排序查询

```MySQL
USE myemployees;
SELECT
	salary,
	last_name
FROM
	employees
WHERE
	salary<18000;
	
	
SELECT
	*
FROM 	employees
WHERE
	job_id<>'IT' OR salary=12000;
	

DESC
	DEPARTMENTS;

SELECT
	*
FROM 	employees;

SELECT
	*
FROM 	employees
WHERE
	job_id LIKE '%%' AND last_name LIKE '%%';
	
/*
排序查询
语法：select 查询列表 
      from 表名
      （where 查询条件）
      order by  查询列表 desc(降序)
      order by 查询列表  asc(升序) 
      不写的话默认是升序
      	可以用于 单个字段 多个字段 表达式 函数 别名
      一般用于字句的最后一句，除了limit也在的情况
*/
#查询员工信息按工资从多到少排序
SELECT
	*
FROM 	employees
ORDER BY salary DESC;
#查询部门标号>=90的员工信息，按入职先后顺序排序[添加筛选条件]
SELECT
	*
FROM 	employees
WHERE 	department_id>=90
ORDER BY hiredate ASC;

#按年薪的高低显示员工信息和年薪[按表达式和别名]
SELECT *,salary*12*(1+ifnull(commission_pct,0)) 年薪
FROM employees
ORDER BY salary*12*(1+ifnull(commission_pct,0)) DESC;
#ORDER BY 年薪 DESC;   


#按名字的长度显示员工的名字 [按函数]
SELECT
	last_name,
	length(last_name) 名字长度
FROM 	employees
ORDER BY length(last_name) DESC;

#查询员工信息 先按工资降序，再按部门标号升序[按多个字段]
SELECT
	*
FROM 	employees
ORDER BY 	salary DESC,department_id ASC;
#-----------
SELECT
	last_name,
	department_id,
	salary*12*(1+ifnull(commission_pct,0)) 年薪
FROM 	employees
ORDER BY 	年薪 DESC,length(last_name);
#-----------
SELECT 
	last_name,
	salary
FROM 	employees
WHERE 	NOT(salary BETWEEN 8000 AND 17000)
ORDER BY salary DESC;

#------------
SELECT
	*
FROM 	employees
WHERE 	email LIKE '%c%'
ORDER BY 	length(email) DESC,department_id ASC;
```



## 经典面试题

![image-20210817212020822](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210817212020822.png)

以上用or 语句的只需要满足一个条件，是null的就不执行

like是不会执行有null的值的，所以下面用or语句那个是和第一个相等的



## 常用函数

```MySQL
USE myemployees;
/* 函数语法
select 函数（）；*/
#字符函数
-- concat拼接字符串
SELECT 
       concat('a','j','是一种鞋') put_out;
-- lengrh查询字符的字节长度
SELECT
	length('abc'); # 字母是一个字节
SELECT
	length('张无忌');#utf-8下一个中文是3个字节，JDK下是4个

-- upper lower 变大小写
# 
SELECT 
	upper('john');
SELECT 
	lower('johN');

#将姓变大写，名变小写然后拼接
SELECT
	concat(upper(last_name),'_',lower(first_name)) 姓名
FROM 	employees;

-- substr,substring 截取字符
SELECT
	substr('张无忌爱赵敏',4);#从指定位置截取到最后
SELECT
	substr('张无忌爱赵敏',2,4);#从指定位置截取指定长度的字符
#姓名首字母大写，其他字母小写，用_连接
SELECT
	concat(upper(substr(last_name,1,1)),'_',substr(last_name,2)) 姓名
FROM 	employees;

-- instr 返回字串在字符串中第一次出现的索引，找不到的话就返回0
SELECT
	instr('爱张无忌爱赵敏','不爱');

-- trim 去掉字符串首尾空格,和去掉首尾指定字符

SELECT 
	length(trim('   aaa   '));
	
select
	trim('ff' from 'ffffffffffffffbbbffbbfffffffffffffff');

-- lpad 实现向左边填充指定字符到指定长度
select 
	lpad('张无忌爱赵敏',10,'爱');

-- rpad 实现向右边填充指定字符到指定长度
SELECT 
	rpad('张无忌爱赵敏',10,'爱');
	
-- replace 替换
select
	replace('张无忌爱周芷若','周芷若','赵敏');
	
#数学函数
-- round 四舍五入
select 
	round(1.55);#2
SELECT 
	round(1.557,2);#保留两位小数1.56
-- ceil 向上取整
select 
	ceil(1.1);#2
SELECT 
	ceil(-1.1);#-1
	
-- floor 向下取整
select
	floor(1.3);#1
SELECT
	floor(-1.4);#-2
	
-- truncate 截断
select truncate(1.32326,2);#保留2位小数，其他的全部截断

-- mod取模 （内部由一个公式实现的  a-a/b*b）
select
	mod(10,3);-- 10%3
SELECT
	mod(-10,-3);-- -1

#日期函数
-- now 返回当前日期加时间
select 
	now();
-- curdate 返回当前日期没有时间
select
	curdate();
-- curtime 返回当前时间没有日期
select
	curtime();
-- 可以获取指定部分 年 月 日 分 秒 小时 
select year(now());
select month(hiredate) FROM employees;
SELECT monthname(hiredate) FROM employees;-- 用英文显示月份


-- str_to_date 字符串转化成日期

select
	str_to_date('10/1/2009','%m/%d/%y');
#查询入职日期是1992-4-3的员工信息

select
	*
from 	employees
where 	hiredate=str_to_date('1992-4-3','%Y-%m-%d');
	
-- date_formate 日期转化为字符串
select
	date_format(hiredate,'%d-%m-%Y')
from 	employees;
#其他函数
select
	version(); -- 查看版本
select
	user();  -- 查看用户
select
	database(); -- 查看当前库
	
#流程控制函数
-- if函数 三元运算符类似
select
	if(10>5,'大','小');-- 10>5是对的话输出中间的值，错的话输出最后个值

#case
/* 查询员工的工资 要求：
部门号=30，工资显示为工资的1.1倍
部门号=40，工资显示为工资的1.2倍
部门号=50，工资显示为工资的1.3倍

否则返回原本工资
*/
select
 department_id,salary,
 CASE department_id
 when 30 then salary*1.1 
 WHEN 40 THEN salary*1.2 
 WHEN 50 THEN salary*1.3 
else salary 
end as 工资
from employees;

-- 另一种case 案例：查看员工工资信息
select salary,
case
when salary>20000 then 'A'
WHEN salary>15000 THEN 'B'
WHEN salary>12000 THEN 'C'
ELSE 'D'
eND AS 工资级别
FROM employees;

#--------------
select
	now();
#--------------
select
	job_id,
	last_name,
	salary,
	salary*1.2 "new salary"
from 	employees;

#--------------
select
	last_name,
	length(last_name) 长度
from 	employees
order by	substr(last_name,1,1);

#--------------
select
	last_name,
	concat(salary,' monthy') earns,
	salary*3 "but wants"
from 	employees;
#--------------

select job_id,
case job_id
when 'AD_PRES' then 'A'
WHEN 'ST_MAN' THEN 'B'
WHEN 'IT_PROG' THEN 'C'
END grade
from employees;

















```

## 分组函数

```MySQL
USE myemployees;
/* 分组函数
输入多个值最后返回一个值

*/
-- sum 求和
SELECT sum(salary) FROM employees;
-- avg 平均值
SELECT avg(salary) FROM employees;
-- max 最大值
SELECT max(salary) FROM employees;
-- min 最小值
SELECT min(salary) FROM employees;
-- count 统计个数
SELECT count(salary) FROM employees;



#分组函数使用参数限制
-- avg sum 只作用于数值型
SELECT avg(last_name) FROM employees;
SELECT sum(last_name) FROM employees;
-- max min count可以用于任何类型
SELECT max(last_name) FROM employees;
SELECT min(last_name) FROM employees;
SELECT count(last_name) FROM employees;



#分组函数与distinct关键字搭配使用,去重之后再统计
SELECT sum(salary),sum(DISTINCT salary) FROM employees;
SELECT avg(salary),avg(DISTINCT salary) FROM employees;

#分组函数都是忽略null值的
SELECT count(commission_pct) FROM employees;

#count函数的详细介绍
SELECT count(salary) FROM employees;
SELECT count(*) FROM employees;-- 可以检查所有函数
SELECT count(1) FROM employees;-- 可以检查所有函数 参数也可以是2 3等常量

#日期函数 datediff，输出两个日期的差值
SELECT
	datediff(max(hiredate),min(hiredate))
FROM 	employees;

#效率
myisam存储引擎下 count(*)效率比较高
innodb存储引擎下 count(*)、count(1)效率差不多，但是都比参数是字段效率高，检索字段要判断是不是null

#------------
SELECT
	max(hiredate)-min(hiredate) diffrence
FROM 	employees;
#------------
SELECT
	count(*)
FROM employees
WHERE department_id=90;
```



## 分组查询

```MySQL


USE myemployees;
#分组查询
/*
SELECT
	分组函数,根据什么分组
FROM
	employees
（where 筛选条件）
GROUP BY 	
	job_id;
（order by 排序）


能用分组前筛选的就用分组前筛选（效率比分组后筛选高）
*/
-- 案例一：调查每个工种的最高工资
SELECT
	max(salary),job_id
FROM
	employees
GROUP BY 	
	job_id;
-- 案例二：调查每个位置上的部门个数
SELECT
	count(*),location_id
FROM 
	departments
GROUP BY 	
	location_id;

#添加分组前增加的筛选条件 WHERE
-- 案例一：查询邮箱中含a字符，每个部门的平均工资
SELECT
	avg(salary),department_id
FROM 	
	employees
WHERE 	
	email LIKE '%a%'
GROUP BY
	department_id;
-- 案例二：查询有奖金的每个领导手下员工的最高工资
SELECT
	max(salary),manager_id
FROM 
	employees
WHERE
	commission_pct IS NOT NULL
GROUP BY
	manager_id;

#添加分组后的增加筛选条件 HAVING
-- 案例一：查询哪个部门的员工个数>2
SELECT
	count(*),department_id
FROM
	employees
GROUP BY
	department_id
HAVING
	count(*)>2;
-- 案例二：查询每个工种有奖金员工最高工资>12000的工种编号和最高工资
SELECT
	max(salary),job_id
FROM 	
	employees
GROUP BY
	job_id
HAVING
	max(salary)>12000;
-- 查询领导编号>102的每个领导手下最低工资>5000的领导编号是哪个，以及最低工资
SELECT
	min(salary),manager_id
FROM
	employees
WHERE
	manager_id>102
GROUP BY
	manager_id
HAVING
	min(salary)>5000;
#按函数分组
-- 按员工姓名长度分组，查询每一组员工个数，筛选员工个数>5的有哪些
SELECT
	count(*),length(last_name)
FROM
	employees
GROUP BY
	length(last_name)
HAVING
	count(*)>5;
-- GROUP BY HAVING 都可以用别名，where不可以用别名
SELECT
	count(*) 员工数,length(last_name) 名长
FROM
	employees
GROUP BY
	名长
HAVING
	员工数>5;
-- 按多个字段分组
-- 每个部门每个工种的员工平均工资
SELECT
	avg(salary),department_id,job_id
FROM
	employees
GROUP BY
	department_id,job_id;
#添加排序	
-- 每个部门每个工种的员工平均工资,并按工资高低排序
SELECT
	avg(salary),department_id,job_id
FROM
	employees
GROUP BY
	department_id,job_id
ORDER BY 	avg(salary) DESC;
```

![image-20210821172918319](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210821172918319.png)

## 连接查询

### sql92标准

- 内连接

- ```mysql
  -- sql92标准
  -- 等值连接
  -- 案例：找出女神对应的男朋友名字
  USE girls;
  SELECT
  	NAME,boyName
  FROM
  	beauty,boys;
  WHERE
  	beauty.`id`=boys.`id`;
  
  
  USE myemployees;
  
  -- 查询员工名对应的部门名
  -- 为表取别名 1.提高语句简介度，区分多个重名字段
  SELECT
  	last_name,department_name
  FROM
  	employees e,departments d  -- 为表取别名
  WHERE	
  	e.`department_id`=d.`department_id`;
  	
  SELECT * FROM employees;
  
  -- 两个表的位置可以调换
  -- 案例：查询员工名，工种号，工种名
  SELECT
  	last_name,e.job_id,job_title -- 起了别名的话，这里也要起别名，否则歧义
  FROM
  	employees e,jobs j
  WHERE
  	e.`job_id`=j.`job_id`;
  -- 调换表的位置
  SELECT
  	last_name,e.job_id,job_title 
  FROM
  	jobs j,employees e
  WHERE
  	e.`job_id`=j.`job_id`;
  
  #添加筛选
  -- 案例：查询有奖金的员工名和部门名
  SELECT
  	last_name,department_name,commission_pct
  FROM
  	employees e,departments d
  WHERE
  	e.`department_id`=d.`department_id`
  AND
  	commission_pct IS NOT NULL;
  	
  
  -- 查询城市名第二个字符为o的部门名和城市名
  
  SELECT
  	department_name,city
  FROM
  	departments d,locations l
  WHERE
  	d.`location_id`=l.`location_id`
  AND
  	city LIKE '_o%';
  
  #添加分组
  
  -- 案例：查询每个城市的部门个数
  SELECT
  	city,count(*)
  FROM
  	locations l,departments d
  WHERE 
  	l.`location_id`=d.`location_id`
  GROUP BY
  	city;
  	
  -- 查询有奖金的每个部门的部门名和部门的领导编号和该部门的最低工资
  SELECT
  	e.manager_id,min(salary),department_name
  FROM 
  	departments d,employees e
  WHERE
  	d.`department_id`=e.`department_id`
  AND
  	commission_pct IS NOT NULL
  GROUP BY
  	department_name,manager_id;
  	
  
  #添加排序
  -- 查询每个工种的工种名和员工个数，并按员工个数降序
  SELECT
  	job_title,count(*)
  FROM
  	jobs j,employees e
  WHERE
  	j.`job_id`=e.`job_id`
  GROUP BY
  	job_title
  ORDER BY 
  	count(*) DESC;
  
  #实现三表连接
  -- 查询员工名，部门名和所在城市
  SELECT
  	last_name,department_name,city
  FROM 
  	employees e,departments d,locations l
  WHERE
  	e.`department_id`=d.`department_id`
  AND
  	d.`location_id`=l.`location_id`;
  
  #非等值连接
  -- 查询员工的工资和工资级别·
  SELECT
  	salary,`grade_level`
  FROM
  	employees,job_grades
  WHERE
  	salary BETWEEN lowest_sal AND highest_sal
  AND
  	grade_level='A';
  	
  SELECT * FROM job_grades;
  
  
  #自连接
  -- 一张表中查询的时候会当成多张表在连接查询
  -- 查询员工名和上级名称
  SELECT
  	e.last_name,e.`manager_id`,m.`last_name`,m.`employee_id`
  FROM 
  	employees e,employees m
  WHERE
  	e.`manager_id`=m.`employee_id`;
  
  #练习
  #------------- 
  -- 显示员工表的最大工资和工资平均值
  SELECT
  	max(salary),avg(salary)
  FROM
  	employees;
  
  -- 查询员工表的employee_id，job_id,last_name,按department_id降序，按salary升序
  SELECT
  	employee_id,e.job_id,last_name
  FROM
  	employees e,jobs j,departments d
  WHERE
  	
  	e.`department_id`=d.`department_id`
  AND
  	e.`job_id`=j.`job_id`
  ORDER BY
  	e.department_id DESC,salary;
  -- 查询员工表中job_id中含a，e的，并且a在e前
  SELECT
  	job_id
  FROM
  	employees
  WHERE
  	job_id LIKE '%A%'
  AND
  	job_id LIKE '%E%';-- '%a%e%'
  
  
  
  SELECT
  	now();
  
  SELECT
  	trim('');
  	
  
  #---------------
  SELECT
  	last_name,e.department_id,department_name
  FROM
  	employees e,departments d
  WHERE
  	e.`department_id`=d.`department_id`;
  
  #---------------
  SELECT
  	last_name,e.job_id,d.location_id,e.`department_id`
  FROM
  	employees e,departments d
  WHERE
  	e.`department_id`=d.`department_id`
  AND
  	e.`department_id`=90;
  
  
  #---------------
  SELECT
  	last_name,department_name,d.location_id,city,commission_pct
  FROM
  	employees e,departments d,locations l
  WHERE
  	e.`department_id`=d.`department_id`
  AND	d.`location_id`=l.`location_id`
  AND	commission_pct IS NOT NULL;
  #---------------
  SELECT
  	last_name,job_id,d.department_id,department_name,city
  FROM
  	employees e,departments d,locations l
  WHERE
  	city='toronto'
  AND
  	e.`department_id`=d.`department_id`
  AND
  	d.`location_id`=l.`location_id`;
  #---------------
  SELECT
  	job_title,department_name,min(salary)
  FROM
  	employees e,jobs j,departments d
  WHERE
  	e.`department_id`=d.`department_id`
  AND	
  	e.`job_id`=j.`job_id`
  GROUP BY
  	job_title,department_name;
  #---------------
  SELECT
  	country_id,count(*)
  FROM
  	departments d,locations l
  WHERE
  	d.`location_id`=l.`location_id`
  GROUP BY
  	country_id
  HAVING
  	count(department_id)>2;
  SELECT
  	e.last_name,e.employee_id,m.last_name,m.employee_id
  FROM
  	employees e,employees m
  WHERE
  	e.`manager_id`=m.`employee_id`;
  
  ```

  

  

  

  ```mysql
  
  ```
  
  

### sql99

#### 内连接

![](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210824225204467.png)

![image-20210827115049871](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210827115049871.png)

```mysql
USE myemployees;
-- 1.等值连接
-- 案例一：查询员工名，部门名
SELECT
	last_name,department_name
FROM
	employees e
INNER JOIN
	departments d
ON
	e.`department_id`=d.`department_id`;
-- 添加筛选：查询名字中包含a的员工名和工种名
SELECT
	last_name,job_title
FROM
	employees e
INNER JOIN
	jobs j
ON
	e.`job_id`=j.`job_id`
WHERE
	e.`last_name` LIKE '%a%';

-- 筛选加分组：查询部门个数>3的城市名和部门个数
SELECT
	city,count(*)
FROM
	locations l
JOIN
	departments d
ON
	l.`location_id`=d.`location_id`
GROUP BY
	city
HAVING
	count(*)>3;
-- 添加排序：查询哪个部门员工个数>3的部门名和员工个数，并按个数降序
SELECT
	department_name,count(*)
FROM
	departments d
JOIN
	employees e
ON
	d.`department_id`=e.`department_id`
GROUP BY
	d.`department_name`
HAVING
	count(*)>3
ORDER BY
	count(*) DESC;
-- 三表连接：查询员工名，部门名，工种名，并按部门名降序
SELECT
	last_name,department_name,job_title
FROM
	employees e
JOIN
	departments d
ON
	e.`department_id`=d.`department_id`
JOIN
	jobs j
ON
	e.`job_id`=j.`job_id`
ORDER BY
	d.`department_name` DESC;
	
	
#非等值连接
-- 查询工资级别的个数>20的个数，并且按工资级别降序
SELECT
	count(*),j.`grade_level`
FROM
	employees e
JOIN
	job_grades j
ON
	e.`salary` BETWEEN j.`lowest_sal` AND j.`highest_sal`
GROUP BY
	j.`grade_level`
HAVING
	count(*)>20
ORDER BY
	j.`grade_level` DESC;
#自连接
-- 查询姓名中包含字符k的员工的名字，和上级的姓名
SELECT
	e.last_name,e.employee_id,m.last_name,m.employee_id
FROM
	employees e
JOIN
	employees m
ON
	e.`manager_id`=m.`employee_id`
WHERE
	e.`last_name` LIKE '%k%';
```

#### 	外连接和交叉连接

![image-20210825093224450](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210825093224450.png)

```mysql
USE myemployees;
-- 1.等值连接
-- 案例一：查询员工名，部门名
SELECT
	last_name,department_name
FROM
	employees e
INNER JOIN
	departments d
ON
	e.`department_id`=d.`department_id`;
-- 添加筛选：查询名字中包含a的员工名和工种名
SELECT
	last_name,job_title
FROM
	employees e
INNER JOIN
	jobs j
ON
	e.`job_id`=j.`job_id`
WHERE
	e.`last_name` LIKE '%a%';

-- 筛选加分组：查询部门个数>3的城市名和部门个数
SELECT
	city,count(*)
FROM
	locations l
JOIN
	departments d
ON
	l.`location_id`=d.`location_id`
GROUP BY
	city
HAVING
	count(*)>3;
-- 添加排序：查询哪个部门员工个数>3的部门名和员工个数，并按个数降序
SELECT
	department_name,count(*)
FROM
	departments d
JOIN
	employees e
ON
	d.`department_id`=e.`department_id`
GROUP BY
	d.`department_name`
HAVING
	count(*)>3
ORDER BY
	count(*) DESC;
-- 三表连接：查询员工名，部门名，工种名，并按部门名降序
SELECT
	last_name,department_name,job_title
FROM
	employees e
JOIN
	departments d
ON
	e.`department_id`=d.`department_id`
JOIN
	jobs j
ON
	e.`job_id`=j.`job_id`
ORDER BY
	d.`department_name` DESC;
	
	
#非等值连接
-- 查询工资级别的个数>20的个数，并且按工资级别降序
SELECT
	count(*),j.`grade_level`
FROM
	employees e
JOIN
	job_grades j
ON
	e.`salary` BETWEEN j.`lowest_sal` AND j.`highest_sal`
GROUP BY
	j.`grade_level`
HAVING
	count(*)>20
ORDER BY
	j.`grade_level` DESC;
#自连接
-- 查询姓名中包含字符k的员工的名字，和上级的姓名
SELECT
	e.last_name,e.employee_id,m.last_name,m.employee_id
FROM
	employees e
JOIN
	employees m
ON
	e.`manager_id`=m.`employee_id`
WHERE
	e.`last_name` LIKE '%k%';
	
#2.外连接
-- 查询哪个部门没有员工
#左外:在左边的是主表
SELECT
	d.*,last_name
FROM
	departments d
LEFT OUTER JOIN
	employees e
ON
	d.`department_id`=e.`department_id`
WHERE
	e.`employee_id` IS NULL;-- 此处最好用主键（有个钥匙标的，此值在原表中不可能为null）
#右外
SELECT
	d.*,last_name
FROM
	employees e
RIGHT OUTER JOIN
	departments d
ON
	d.`department_id`=e.`department_id`
WHERE
	e.`employee_id` IS NULL;-- 此处最好用主键（有个钥匙标的，此值在原表中不可能为null）
	
USE girls;
	
#全外
-- 结果是获得表1和表2的交集+表1中表2没有的+表2中表1没有的

#交叉连接:使用sql99标准实现笛卡尔乘积
SELECT
	b.*,bo.*
FROM
	beauty b
CROSS JOIN
	boys bo;
	
-- 练习
SELECT
	b.id,bo.*,b.`name`
FROM
	beauty b
LEFT JOIN
	boys bo
ON
	b.`boyfriend_id`=bo.`id`
WHERE
	b.`id`>3;
	
	
-- --------
SELECT
	city,department_name
FROM
	locations l
LEFT JOIN
	departments d
ON
	l.`location_id`=d.`location_id`
WHERE
	department_id IS NULL;
#--------------
SELECT
	department_name,e.*
FROM
	departments d
LEFT JOIN
	employees e
ON
	D.`department_id`=E.`department_id`
WHERE
	department_name ='SAL' OR department_name = 'IT'; 

```



## 子查询



![image-20210825105357581](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210825105357581.png)

![image-20210825114030289](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210825114030289.png)

### 标量子查询

```mysql

-- 一：放在where后面


#标量子查询

-- 案例一：查询谁的工资比Abel高
-- 先查询Abel的工资
SELECT
	salary
FROM
	employees
WHERE
	last_name='Abel';
-- 比Abel工资高的
SELECT
	last_name
FROM
	employees
WHERE
	salary>(
	SELECT
	salary
FROM
	employees
WHERE
	last_name='Abel'
);

-- 案例二：返回job_id与141号员工相同，salary比143号员工多的员工姓名，job_id和工资
-- 141号的Job_id
SELECT
	job_id
FROM
	employees
WHERE
	employee_id=141
-- 143号的salary
SELECT
	salary
FROM
	employees
WHERE
	employee_id=143

-- 最终
SELECT
	last_name,job_id,salary
FROM
	employees
WHERE
	job_id=(SELECT
	job_id
FROM
	employees
WHERE
	employee_id=141)
AND
	salary>(SELECT
	salary
FROM
	employees
WHERE
	employee_id=143);
#案例三：返回公司工资最少的员工的last_name,job_id,salary
-- 公司的最低工资
SELECT
	min(salary)
FROM
	employees
-- 最终
SELECT
	last_name,job_id,salary
FROM
	employees
WHERE
	salary=(
SELECT
	min(salary)
FROM
	employees);
-- 非法使用标量子查询
SELECT
	last_name,job_id,salary
FROM
	employees
WHERE
	salary=(
SELECT
	salary -- 此处变成了多行
FROM
	employees);
-- 案例四：查询最低工资大于50号部门最低工资部门的id和其最低工资
-- 50号部门的最低工资
SELECT
	min(salary)
FROM
	employees
WHERE
	department_id=50;

-- 每个部门的最低工资
SELECT
	department_id,min(salary)
FROM
	employees
GROUP BY
	department_id
-- 最低工资大于50号部门
SELECT
	department_id,min(salary)
FROM
	employees
GROUP BY
	department_id
HAVING
	min(salary)>(SELECT
	min(salary)
FROM
	employees
WHERE
	department_id=50);



```



### 多行子查询

```mysql

#多行子查询（使用多行操作符）

#列子查询（多行子查询）
-- 案例一：返回location_id是1400或1700的部门中的所有员工姓名

-- location_id是1400或1700的部门编号
SELECT
	department_id
FROM 
	departments
WHERE
	location_id IN(1400,1700);
	
#---
SELECT
	last_name
FROM
	employees
WHERE
	department_id IN(SELECT
	department_id
FROM 
	departments
WHERE
	location_id IN(1400,1700)
	)
#或者
SELECT
	last_name
FROM
	employees
WHERE
	department_id =ANY(SELECT
	department_id
FROM 
	departments
WHERE
	location_id IN(1400,1700)
	)
-- 案例二：返回其他工种中比job_id为‘IT_PROG’任一工资低的员工号、姓名、job_id以及salary
-- job_id为‘IT_PROG’任一工资
SELECT
	salary
FROM
	employees
WHERE
	job_id='IT_PROG';
--  
SELECT
	job_id,last_name,employee_id,salary
FROM
	employees
WHERE
	salary<ANY(SELECT
	salary
FROM
	employees
WHERE
	job_id='IT_PROG');
	
-- 或者
SELECT
	job_id,last_name,employee_id,salary
FROM
	employees
WHERE
	salary<(SELECT
	max(salary)
FROM
	employees
WHERE
	job_id='IT_PROG');
-- 案例三：返回其他工种中比job_id为‘IT_PROG’所有工资低的员工号、姓名、job_id以及salary

SELECT
	job_id,last_name,employee_id,salary
FROM
	employees
WHERE
	salary<ALL(SELECT
	salary
FROM
	employees
WHERE
	job_id='IT_PROG');
	
#或者
SELECT
	job_id,last_name,employee_id,salary
FROM
	employees
WHERE
	salary<(SELECT
	min(salary)
FROM
	employees
WHERE
	job_id='IT_PROG');


# 行子查询（一行多列或者多行多列）
#案例：查询员工编号最小并且工资最高的员工信息
SELECT
	min(employee_id)
FROM
	employees;
-- 最高工资
SELECT
	max(salary)
FROM
	employees;
-- 员工信息
SELECT
	*
FROM
	employees
WHERE
	employee_id=(SELECT
	min(employee_id)
FROM
	employees)
AND
	salary=(SELECT
	max(salary)
FROM
	employees);
#-- 行子查询做法
SELECT
	*
FROM
	employees
WHERE
	(employee_id,salary)=(SELECT
	min(employee_id),max(salary)
FROM
	employees)


-- 二：放在select后面
-- 案例：查询每个部门的员工个数
SELECT
	d.*,(
	SELECT count(*)
	FROM employees e
	WHERE e.`department_id`=d.department_id
	) 个数
FROM
	departments d;
-- 查询员工号=102的部门名
SELECT
	(
	SELECT department_name
	FROM departments d
	JOIN employees e
	WHERE e.department_id=d.department_id
	AND employee_id=102
	) n;

-- 三：放from后面（将子查询充当一张表且必须起别名）
-- 案例:查询没个部门的平均工资的工资登级
-- 每个部门的平均工资
SELECT
	avg(salary),department_id
FROM
	employees
GROUP BY
	department_id
-- ---
SELECT  	avg_g.*,g.`grade_level`
	
FROM (SELECT
	avg(salary) ag ,department_id
FROM
	employees
GROUP BY
	department_id) avg_g
JOIN
	job_grades g
ON
	avg_g.ag BETWEEN g.lowest_sal AND g.highest_sal

#四、exists后面（相关子查询）
/*
语法：
exists（完整的查询语句）
结果：1或0（true，false）
*/
SELECT EXISTS(SELECT employee_id FROM employees WHERE salary=999999)

-- 案例一：查询有员工的部门名
SELECT
	department_name
FROM
	departments d
WHERE EXISTS(
	SELECT *
	FROM
	employees e
	WHERE
	e.`department_id`=d.`department_id`
)
#in
SELECT
	department_name
FROM
	departments d
WHERE
	d.department_id IN(
	SELECT department_id
	FROM employees
	)
#案例二：查询没有女朋友的男神信息
-- in
SELECT	
	*
FROM
	boys bo
WHERE 
	bo.id NOT IN(
	SELECT
	boyfriend_id
	FROM beauty)
	
-- exists
SELECT *
FROM boys bo
WHERE NOT EXISTS(
 SELECT boyfriend_id
 FROM beauty b
 WHERE b.boyfriend_id=bo.id	)
```

### 分页查询

![image-20210827120239083](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210827120239083.png)

```MySQL
/*
limit 放在所有查询语句的最后
语法：limit offset，size （页面的起始索引，页数），从0开始的话起始索引可省略

真实应用时起始索引不能写死：第一页（page）：offset 0
		第二页（page）：offset 10
		第三页（page）：offset 20
		所以语法：limit （page-1）*size，size */
#案例一：查询前五条员工信息
SELECT * FROM employees LIMIT 0,5;
SELECT * FROM employees LIMIT 5;

#案例二：查询第十一条到第二十五条
SELECT * FROM employees LIMIT 10,15;

#案例三：查询有奖金的员工信息，工资较高的前十名
SELECT 
  * 
FROM
  employees 
WHERE commission_pct IS NOT NULL 
ORDER BY salary DESC 
LIMIT 10;


```

![image-20210827114458131](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210827114458131.png)

![image-20210827114525009](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210827114525009.png)

## 联合查询



```mysql
/*
联合查询：多条查询语句的结果整合成一个结果
语法： 	  
	   查询语句一
	union
	   查询语句二
	 union
	    查询语句三
	    .....
应用场景：要查询的结果来自多个表，多个表之间没有之间联系（关系），但查询信息相同时	    
特点：1.查询的字段个数(列数)要相同
       2.每一列的字段的类型和顺序最好一致
       3.使用union默认是去重的（多个表中一样的结果只显示一个）
       4.使用union all 可以存在重复项
*/
#案例一：查询部门编号>90或邮箱包含a的员工信息
SELECT * FROM employees WHERE department_id>90
UNION
SELECT * FROM employees WHERE email LIKE '%a%';
#案例二：查询中国用户中男性信息以及外国用户中男性信息
SELECT id,cname,csex FROM t_ca WHERE csex='男'
UNION
SELECT t_id,tname,tGender FROM t_ua WHERE tGender='male';
-- 查询的字段个数要相同
SELECT id,cname,csex FROM t_ca WHERE csex='男'
UNION
SELECT t_id,tnameFROM t_ua WHERE tGender='male';
-- 每一列的字段的类型和顺序最好一致
SELECT id,cname,csex FROM t_ca WHERE csex='男'
UNION
SELECT tGender,t_id,tname FROM t_ua WHERE tGender='male';
```



## DML语言

### 增删改

```mysql
/*
DML语言
1.插入
2.修改
3.删除
*/

/*
一、插入语句：方式一：insert into 表名（列名1，列名2...） value(新值1，新值2...)
*/
#1.插入的值类型要与列的类型一致或兼容
INSERT INTO beauty(id,NAME,sex,borndate,phone,photo,boyfriend_id)
VALUES(14,'唐艺昕','女','1990-9-9','13233333',NULL,10);
SELECT * FROM beauty;
#省略列名
INSERT INTO beauty
VALUES(15,'唐艺昕3','女','1990-9-9','13233333',NULL,15);
SELECT * FROM beauty;
#列名顺序在原表基础上调换顺序（值的顺序也要随之调换）
INSERT INTO beauty(NAME,id,sex,borndate,phone,photo,boyfriend_id)
VALUES('唐艺昕4',18,'女','1990-9-9','13233333',NULL,10);
SELECT * FROM beauty;
#不可以为null的列必须插入值，可以为null的列怎么插入值呢
-- 方式一：写列名，值为null
INSERT INTO beauty(NAME,id,sex,borndate,phone,photo,boyfriend_id)
VALUES('杨幂',19,NULL,NULL,'1212212',NULL,NULL);
SELECT * FROM beauty;
-- 方式二：列名和值都不写
INSERT INTO beauty(NAME,id,phone)
VALUES('杨幂2',29,'12122122');


#插入语句方式二：
# 语法：	insert into 表名
#	set 列名=新值，列名2=新值...
INSERT INTO beauty
SET NAME='赵丽颖',id=22,phone='1100';


#方式一和方式二大PK
-- 方式一可以插入多行，方式二不可以
INSERT INTO boys
VALUES(5,'立下',12),(6,'王凯',22),(7,'苑子文',10)
SELECT  * FROM boys ;
-- 方式一后面可以子接查询，方式二不可以
INSERT INTO beauty(id,NAME,phone)
SELECT 24,'赵露思2','1192';
SELECT * FROM beauty;

INSERT INTO beauty(id,NAME,phone)
SELECT id,boyname,'112122'
FROM  boys WHERE id=1;


/*
修改单表记录
语法： update 表名
        set 列名=新值，列名2=新值...
        where 筛选条件（不加where的话所有行的列都会被改变）
        
修改多表数据{补充}
sql92语法：
update 表1 别名，表2 别名
set 值=....
where 连接条件
and 筛选条件

sql99语法：
update 表1 别名
inner left right join 表2 别名
on 连接条件
set 值=...
where 筛选条件
*/
#修改beauty表中姓唐的女神电话为‘12000’
UPDATE beauty
SET phone='12000'
WHERE NAME LIKE '唐%';
#修改boys表中id为2的名字为张飞，魅力值为10
UPDATE boys
SET boyname='张飞',usercp=10
WHERE id=2;

#修改多表记录
-- 案例1：修改张飞女朋友号码为114
UPDATE beauty b
INNER JOIN boys bo
ON b.`boyfriend_id`=bo.`id`
SET b.`phone`='114'
WHERE bo.`boyName`='张飞';
-- 案例二：修改没有男朋友的女神的男朋友的编号都为2
UPDATE beauty b
LEFT JOIN boys bo
ON b.`boyfriend_id`=bo.`id`
SET b.`boyfriend_id`=2
WHERE bo.`id` IS NULL;
-- 查询检验
SELECT b.*
FROM beauty b
LEFT JOIN boys bo
ON b.`boyfriend_id`=bo.`id`

/*
三：删除语句方式一语法：
单表的删除：
delete from 表名 where（一次删除一行或多行）

多表的删除{补充}

方式二语法：
truncate table 表名（清空：删除整个表）
*/
#单表删除案例：删除手机号码以2结尾的女神信息
DELETE FROM beauty WHERE phone LIKE '%2';
SELECT * FROM beauty;
SELECT * FROM boys;
#多表删除案例
-- 案例一：删除张无忌女朋友信息
DELETE b
FROM beauty b
INNER JOIN boys bo
ON b.boyfriend_id=bo.id
WHERE boyname='张无忌';
-- 案例二：删除黄晓明以及他女朋友的信息
DELETE b,bo
FROM beauty b
INNER JOIN boys bo
ON b.`boyfriend_id`=bo.`id`
WHERE bo.`boyName`='黄晓明';

#方式二：删除整个表
TRUNCATE TABLE boys;

INSERT INTO boys
VALUE(1,'黄晓明',30),(2,'段誉',14),(3,'张飞',13),(4,'李易峰',50)

/*
delete和truncate PK
1.truncate性能会高一丢丢
2.delete可以加where，truncate不可以
3.要是要删除的有自增长列的
用delete删除是从断点处开始增长
用truncate删除是从1开始
4.truncate删除后没有返回值，delete有返回值（提示几行受到影响）
5.用truncate删除不能回滚，用delete删除可以回滚
*/
```



## DDL语言

- 数据定义语言：改变表和库的结构的

![image-20210908222029965](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210908222029965.png)![image-20210908222102398](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210908222102398.png)

- ```mysql
  /*
  DDL 数据定义语言
  1.库的管理
  创建 create
  修改alter
  删除 drop
  2.表的管理
  创建
  修改
  删除
  */
  
  /*
  1.库的创建：
  语法：create database 『if not exit（防止数据库已经存在而报错）』库名
  
  */
  #案例：创建books库
  CREATE DATABASE IF NOT EXISTS books;
  
  #2.库的修改
  RENAME DATABASE TO 新库名 （此方法不安全已被淘汰，改名字直接改文件名字）
  
  #更改库的字符集（utf-8那些）
  ALTER DATABASE books CHARACTER SET gbk;#右键改变数据库可以看字符集类型
  
  #3.库的删除
  DROP DATABASE IF EXISTS books;
  
  /*
  二，表的管理
  1.表的创建
  语法：create table 表名（
  	列名 列的类型 『（长度）约束条件』
  	列名 列的类型 『（长度）约束条件』
  	列名 列的类型 『（长度）约束条件』
  	.....）
  */
  
  #案例：创建表book
  CREATE TABLE book(
  	bname VARCHAR(20),
  	authorid INT,
  	id INT,
  	price DOUBLE,
  	publishDate DATETIME);
  DESC book;
  #创建表author
  CREATE TABLE author(
  	id INT,
  	NAME VARCHAR(20),
  	borndate DATETIME
  	
  );
  DESC author;
  
  #2.表的修改
  # alter table 表名 add\drop\change\modify column
  
  #1.修改列名
  ALTER TABLE book CHANGE COLUMN publishDate pubdate DATETIME;
  
  #修改列的类型或约束
  ALTER TABLE book MODIFY COLUMN pubdate TIMESTAMP;
  
  #添加新列
  ALTER TABLE book ADD COLUMN annual DOUBLE;
  #删除列
  ALTER TABLE book DROP COLUMN annual;
  SELECT * FROM author
  #修改表名
  ALTER TABLE author RENAME TO book_author;
  
  #3.表的删除
  DROP TABLE IF EXISTS book_author;
  
  #通用的写法
  DROP DATABASE IF EXISTS 库名
  CREATE DATABASE 新库名
  
  DROP TABLE IF EXISTS 表名
  CREATE TABLE 新表名
  
  #4.表的复制
  INSERT INTO author VALUES(1,'村上春树','1900-9-9');
  
  #1.仅仅复制表的结构
  CREATE TABLE copy LIKE author;
  
  #2.复制表的部分结构
  CREATE TABLE copy2 
  SELECT id,NAME
  FROM author
  WHERE 0;#0表示false
  DESC copy2
  
  #3,复制表的结构加数据
  CREATE TABLE copy3 
  SELECT * FROM author
  
  SELECT *FROM copy3
  SELECT *FROM copy4
  #4，只复制部分数据
  CREATE TABLE copy4
  SELECT id NAME
  FROM author;
  
  #---------练习
  -- 创建表dept1
  CREATE TABLE IF NOT EXISTS dept1(
  	NAME VARCHAR(25),
  	id INT(7)
  );
  -- 将表department中的数据插入新表dept2中
  CREATE TABLE IF NOT EXISTS dept2
  SELECT * FROM departments;
  
  -- 创建表emp5
  CREATE TABLE IF NOT EXISTS emp5(
  	id INT(7),
  	first_name VARCHAR(25),
  	last_name VARCHAR(25),
  	dept_id INT(7)
  
  );
  DESC emp5
  
  -- 将列last_name的长度增加到50
  ALTER TABLE emp5 MODIFY COLUMN last_name VARCHAR(50)
  
  -- 根据表employees创建employees2
  CREATE TABLE employees2 LIKE employees;
  
  -- 删除表emp5
  DROP TABLE emp5
  
  -- 将表employees2重命名为emp5
  ALTER TABLE employees2 RENAME TO emp5
  
  -- 在表dept和emp5中添加新列test_column,并检查所作的操作
  ALTER TABLE dept1 ADD COLUMN test_column DOUBLE
  DESC dept1
  ALTER TABLE emp5 ADD COLUMN test_column DOUBLE
  DESC emp5
  
  -- 直接删除表emp5中的列dept_id
  ALTER TABLE emp5 DROP COLUMN email;
  ```

  ## 常见的数据类型

  ```MySQL
  /*
  常见的数据类型：
  数值型：
  	整型
  	小数：定点数
  	       浮点数
  	  
  
  字符型：
  	较短的文本：char，varchar
  	较长的文本：text、blob（较长的二进制数据）
  
  日期型：
  
  */
  #1.整型：可存数范围从小到大：tinyint，smallint,mediumint,int(integer),bigint
  -- 有符号和无符号	  1	2	3  4	8
  -- 特点：1如果不设置有符号和无符号默认是有符号，若要设置无符号加关键字UNSIGNED
  -- 2.插入数值超出范围报错 Out of range value
  -- 3.如果不设置长度会有默认的长度
  -- 4,自己设置的长度是最后能显示出来的长度，能插入的范围是int等类型决定的，不够设置的长度会用0填充 搭配ZEROFILL（默认变为无符号）
  #案例1:如何设置无符号和有符号
  DROP TABLE IF EXISTS tab_int ;
  CREATE TABLE IF NOT EXISTS tab_int(
  	t1 INT(7) ZEROFILL,
  	t2 INT(7) ZEROFILL UNSIGNED
  );
  INSERT INTO tab_int VALUE(12345,11)
  INSERT INTO tab_int VALUE(-12345,-22222)
  INSERT INTO tab_int VALUE(88888888888,77777777777)
  
  DESC tab_int
  SELECT * FROM tab_int 
  
  /*
  二、小数
  1.浮点数：float（m,d），double(m,d)
  2.定点数：dec(M,D),decimal(m,d)(精确度更高)
  
  特点：
  1，M:整数部位和小数部位的总长度和D：小数部分的长度 md都可省略。
  如果是decimal，m默认是10，d默认是0
  2，float和double取决于给他设置的精度
  3，货币运算等高精度的用decimal，其他可以用浮点数（节省空间）
  
  4，原则：所选择的类型越简单越好，能保存的数值类型越小越好
  
  */
  #测试M,D
  CREATE TABLE tab_float(
  	f1 FLOAT(5,2),
  	f2 FLOAT(5,2),
  	f3 DECIMAL(5,2)
  );
  INSERT INTO tab_float VALUES(123.22,123.22,123.22)
  INSERT INTO tab_float VALUES(123.2,123.2,123.2)
  INSERT INTO tab_float VALUES(1232.2,1232.2,1232.2)
  SELECT * FROM tab_float;
  
  
  /*
  字符型：
  较短的文本：
  char
  varchar
  
  其他：binary和varbinary用于保存较短的二进制
  enum用于保存枚举
  set用于保存集合
  
  较长的文本：
  text
  blob（较大的二进制）
  
  特点：		M的意思		特点			空间的耗费	效率
  char  char(M)		最大字符数，可省略，默认为1	固定长度字符（开拓字符空间一定）	比较耗费	高
  varchar  varchar(M)	最大字符数，不可省略	可变长度字符			比较节省	低
  
  ###如果字段字符的长度不变可用char性能较高
  */
  
  #枚举:一次只能选择插入一个  例如只能插入男女就用枚举
  CREATE TABLE tab_char (
  	t1 ENUM('a','b','c')
  );
    INSERT INTO tab_char VALUES('a');
    INSERT INTO tab_char VALUES('b');
    INSERT INTO tab_char VALUES('c');
    INSERT INTO tab_char VALUES('A');
    INSERT INTO tab_char VALUES('m');
      SELECT * FROM tab_char;
      
  #set 跟枚举差不多（不过一次能选择插入多个）
  CREATE TABLE tab_set(
  	s1 SET('a','b','c','d')
  );
  INSERT INTO tab_set VALUES('a');
  INSERT INTO tab_set VALUES('a,b,c');
  SELECT * FROM tab_set;
  
  /*
  四、日期型
  分类：
  date只保存日期
  time只保存时间
  year只保存年
  
  datetime保存日期加时间
  timestamp保存日期加时间
  特点：
  	字节	范围	时区等影响
  datetime 	8	1000-9999	不受
  timestamp	4	1970-2038	受
  
  */
  CREATE TABLE tab_date(
  	 t1 DATETIME,
  	 t2 TIMESTAMP
  );
  INSERT INTO tab_date VALUE(now(),now());
  SELECT * FROM tab_date;
  
  SHOW VARIABLES LIKE 'time_zone';
  SET time_zone='+9:00';常见约束约束
  含义：一种限制，用于限制表中的数据，为了保证表中的数据的准确和可靠性
  ```

  



### 常见约束

分类：六大约束

not null：非空，用于保证该字段的值不能为空。比如姓名、学号等。
default：默认，用于保证该字段有默认值。比如性别。
primary key：主键，用于保证该字段的值具有唯一性，并且非空。比如学号、员工编号等。
unique：唯一，用于保证该字段的值具有唯一性，可以为空。比如座位号。
check：检查约束【mysql中不支持】。不日年龄、性别。
foreign key：外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值。在从表添加外键约束，用于应用主表中某列的值。比如学生表的专业编号，员工表的部门编号，员工表的工种编号。
添加约束的时机：

创建表时
修改表时
约束添加的分类：

列级约束：六大约束语法上都支持，但外键约束没有效果

表级约束：除了非空、默认，其他的都支持CREATE DATABASE students;



#### 添加删除约束

```sql
USE students;

CREATE TABLE stuinfo (
  id INT PRIMARY KEY,
  stuname VARCHAR (20) NOT NULL,	# 非空
  gender CHAR(1) CHECK (gender = '男' 
    OR gender = '女'),
  seat INT UNIQUE,	# 唯一
  age INT DEFAULT 18,	# 默认
  majorID INT REFERENCES major (id)
) ;

CREATE TABLE major (
  id INT PRIMARY KEY,
  majorName VARCHAR (20)
) ;

DESC stuinfo;
SHOW INDEX FROM stuinfo;
-- 修改表时删除约束

#1.删除非空约束
ALTER TABLE stuinfo MODIFY COLUMN stuname VARCHAR(20) NULL;
#2.删除默认约束
ALTER TABLE stuinfo MODIFY COLUMN age INT;
#3.删除主键
ALTER TABLE stuinfo DROP PRIMARY KEY;
#4.删除唯一(不会删除索引)
ALTER TABLE stuinfo DROP UNIQUE;
#5.删除外键(不会删除外键的索引)
ALTER TABLE stuinfo DROP FOREIGN KEY fk_stuinfo_major;
#6.删除索引
ALTER TABLE stuinfo DROP INDEX fk_stuinfo_major;
ALTER TABLE stuinfo DROP INDEX seat;

SHOW INDEX FROM stuinfo;
DESC stuinfo;

-- 练习
#1.向表emp2的id列添加primary key约束（my_dept_id_pk）
-- 创建表时
CREATE TABLE emp2(
	id INT(10) PRIMARY KEY -- 列级
	
)
CREATE TABLE emp2(
	id INT(10),
	CONSTRAINT id PRIMARY KEY(id)-- 表级
	
)
-- 修改表时
ALTER TABLE emp2 DROP PRIMARY KEY
ALTER TABLE emp2 MODIFY COLUMN id INT PRIMARY KEY
DROP TABLE IF EXISTS emp2;
DESC emp2;
SHOW INDEX FROM emp2;

#2.向表dept2的id列中添加主键
ALTER TABLE dept2 MODIFY COLUMN `department_id` INT PRIMARY KEY
DESC dept2;

#3.向表emp2中添加列dept_id,并在其中定义外键约束，与之关联的是dept2表中的id列
ALTER TABLE emp2 ADD dept_id INT;
ALTER TABLE emp2 ADD CONSTRAINT fk_emp2_dept2 FOREIGN KEY(dept_id) REFERENCES dept2(`department_id`)
```



#### 标识列

****标识列（自增长列）不用手动插入值，系统自动提供默认地序列值**
**-- 特点：     1.标识列要是一个key** 
	**2.一个表至多一个自增长列**
	**3.标识列的类型只能是数值型**
	4.标识列设置步长（一次增长多少）****

```sql
#标识列（自增长列）不用受冻插入值，系统自动提供默认地序列值
-- 特点：     1.标识列要是一个key 
	2.一个表至多一个自增长列
	3.标识列的类型只能是数值型
	4.标识列设置步长（一次增长多少）
-- 创建表时设置标识列
CREATE TABLE tab_identity(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20)
);
INSERT INTO tab_identity VALUES(NULL,'john');
INSERT INTO  tab_identity (NAME) VALUES('lucy');-- 直接省略null值
SELECT * FROM tab_identity;

-- 查看自增长列的起始值和步长
SHOW VARIABLES LIKE '%auto_increment%';


TRUNCATE tab_identity;
#改变步长
SET auto_increment_increment=3;
SET auto_increment_offset=5; #mysql改不了初始值

-- 修改表时添加标识列
ALTER TABLE tab_identity MODIFY COLUMN id INT PRIMARY KEY AUTO_INCREMENT;

-- 修改表时删除标识列
ALTER TABLE tab_identity MODIFY COLUMN id INT PRIMARY KEY;

```



## 事务

**简介：事务由单独单元的一个或者多个sql语句构成，在这个单元中，每个MySQL语句是相互依赖的**



### 特点

1. 原子性：不可分割，一个事务中的所有sql语句要么全部执行要么都不执行
2. 一致性：由一种一致性转变到另一中一致性
3. 隔离性：多个事务同时进行时，不受其他事务干扰
4. 持久性：一个事务一旦提交，对数据库的影响是永久的



### 创建事务

```sql
/*
事务的创建

隐式事务：事务没有明显的开始和结束的标志
	比如insert 、select、update等语句
	
显式事务：有明显的开始和结束的标记

开启显式事务的前提是设置自动提交语句功能为禁用
set autocommit=0


语法：1、开启事务
	set autocommit=0
	start transaction(可不写)
      2、编写事务中的sql语句（select、update、insert等）
	语句1，
	语句2，
	。。。
      3、结束事务 
	rollback;(回滚事务)
	commit；（提交事务）
	
*/
```



### 事务并发问题

![image-20210920194238409](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210920194238409.png)

​			

**![image-20210921144404857](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921144404857.png)**

**![image-20210921145755887](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921145755887.png)**



![image-20210921155526200](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921155526200.png)

![image-20210921163748070](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921163748070.png)

![image-20210921164148268](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921164148268.png)

![image-20210921165002954](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921165002954.png)

![image-20210921165455149](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210921165455149.png)

### 保存点（回滚点）

```sql
-- savepoint（保存点的使用）
SET autocommit=0;
START TRANSACTION;
DELETE FROM major WHERE id=1;
SAVEPOINT a;
DELETE FROM major WHERE id=3;
ROLLBACK TO a;
```

### delete与truncate

```sql
#delete和truncate在事务中的使用区别 
-- delete可回滚truncate不可以
SET autocommit=0;
START TRANSACTION;
DELETE FROM tab_int;
ROLLBACK;
SELECT * FROM tab_int;

SET autocommit=0;
START TRANSACTION;
TRUNCATE tab_int;
ROLLBACK;
```



## 视图

- 动态生成了保存sql逻辑没有保存结果的表

- 好处：重用sql语句

- 简化复杂的sql操作不用知道代码的具体实现细节

- 保护数据，提高了安全性


### 创建视图

- ```sql
  -- 创建视图
  CREATE VIEW my1
  AS
  SELECT `id` FROM `author`;
  DESC my1;
  
  -- 练习
  -- 1.查询姓名中包含a字符的员工名，部门名和工种信息
  CREATE VIEW myv2
  AS
  SELECT last_name,department_name,j.*
  FROM employees e
  INNER JOIN departments d
  ON e.department_id=d.department_id
  INNER JOIN jobs j
  ON e.job_id=j.job_id;
  DESC myv2;
  SHOW CREATE VIEW myv2;
  SELECT * FROM myv2 WHERE last_name LIKE '%a%';
  SELECT * FROM myv2;
  
  -- 2.查询各部门的平均工资级别
  CREATE VIEW myv3
  AS
  SELECT avg(salary)
  FROM employees
  GROUP BY department_id;
  
  SELECT m.`avg(salary)`,`grade_level` FROM `job_grades`
  INNER JOIN myv3 m
  ON m.`avg(salary)` BETWEEN `lowest_sal` AND `highest_sal`;
  
  -- 3.查询平均工资最低的部门信息
  
  CREATE OR REPLACE VIEW myv4
  AS
  SELECT avg(salary),department_id FROM employees
  ORDER BY avg(salary)
  LIMIT 2;
  
  SELECT * FROM myv5;
  SELECT d.*,m.`avg(salary)`
  FROM departments d
  INNER JOIN myv4 m
  ON d.`department_id`=m.`department_id`;
  
  -- 4.查询平均工资最低的部门名和工资
  CREATE OR REPLACE VIEW myv5
  AS
  SELECT d.*,m.`avg(salary)`
  FROM departments d
  INNER JOIN myv4 m
  ON d.`department_id`=m.`department_id`;
  
  SELECT m.department_name,e.salary
  FROM myv5 m
  INNER JOIN employees e
  ON m.`department_id`=e.`department_id`;
  
  /*
  修改视图
  
  方式一：create or replace view 视图名
           as
           查询语句；
           
  方式二：ater view 视图名
           as
           查询语句；
  */
  
  
  /*
  删除视图
  
  drop view 视图名，视图名，..;
  */
  
  
  /*
  查看视图
  一。desc 视图名；
  
  二。show create view 视图名；
  
  */
  ```

  \G是格式化

  ![image-20210923161536223](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210923161536223.png)

### 更新视图

- **六种情况不能更新视图**
- ![image-20210924220533781](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210924220533781.png)

![image-20210924220639078](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210924220639078.png)

```sql

/*
更新视图（update,insert,delete）
*/

CREATE OR REPLACE VIEW my1
AS
SELECT last_name,salary
FROM employees;
SELECT * FROM my1;
SELECT * FROM employees;
-- 修改
UPDATE my1 SET last_name="nn" WHERE salary=7700.00;

-- 插入(同时数据会插入原表)
INSERT INTO my1 VALUES('mm',1000000);

-- 删除
DELETE FROM my1 WHERE last_name='mm';
```



### 视图与表的对比



![image-20210924220611614](C:\Users\11751\AppData\Roaming\Typora\typora-user-images\image-20210924220611614.png)





## 变量

- 系统变量（系统（服务器）自给的变量）
  - 全局变量 （在所有连接中对变量的修改都有效，但是连接重启之后就无效了）
  - 会话变量（只在当前连接中有效）
- 自定义变量（客户端自己定义的变量）
  - 用户变量（在当前连接的任何地方都可以用）
  - 局部变量（只在定义他它的begin and 中有效，且只用在begin and中的第一句话）



变量

### 系统变量

说明：变量由系统提供，不是用户定义，属于服务器层面

注意：如果是全局级别，则需要加global；如果是会话级别，则需要加session；如果不写，则默认session

使用的语法：

查看所有的系统变量

SHOW GLOBAL|【SESSION】 VARIABLES;
1
查看满足条件的部分系统变量

SHOW GLOBAL|【SESSION】 VARIABLES LIKE '%char%';
1
查看指定的某个系统变量的值

SELECT @@GLOBAL|【SESSION】.系统变量名;
1
为某个系统变量赋值

方式一

set GLOBAL|【SESSION】 系统变量名 = 值;
1
方式二

set @@GLOBAL|【SESSION】.系统变量名 = 值;
1
分类：

#### 全局变量

服务器层面上的，必须拥有super权限才能为系统变量赋值。

作用域：服务器每次启动将为所有的全局变量赋初始值，针对于所有的会话（连接）有效，但不能跨重启。

查看所有的全局变量

SHOW GLOBAL VARIABLES;


- 查看部分的全局变量


SHOW GLOBAL VARIABLES LIKE ‘%char%’;


- 查看指定的全局变量的值


SELECT @@global.autocommit;
SELECT @@global.tx_isolation;


- 为某个指定的全局变量赋值

- 方式1：

  ```
set global autocommit=0;
  ```

- 方式2：

  ```
  SET @@global.autocommit=0;
  ```

#### 会话变量

服务器为每一个连接的客户端都提供了系统变量。

作用域：仅仅针对于当前会话（连接）有效。

查看所有的会话变量

SHOW 【SESSION】 VARIABLES;
```

查看部分的会话变量

SHOW 【SESSION】 VARIABLES LIKE ‘%char%’;
```

查看指定的某个会话变量

SELECT @@【SESSION.】autocommit;
```

为某个会话变量赋值

方式1：

set session autocommit=0;
```

- 方式2：

  ```
  SET @@【session.】autocommit=0;
  

```

### 自定义变量

变量是用户自定义的，不是由系统定义的

使用步骤：声明 赋值 使用（查看、比较、运算等）

分类

#### 用户变量

作用域：针对于当前会话（连接）有效，等同于会话变量的作用域
应用在任何地方，也就是begin end里面或begin end的外面
声明并初始化（三种方式）

set @用户变量名=值；
set @用户变量名:=值；（推荐）
select @用户变量名:=值；

赋值（更新用户变量的值）

方式1：通过set或select（同上）

set @用户变量名=值；
set @用户变量名:=值；（推荐）
select @用户变量名:=值；

案例1：

SET @name='John';
SET @name=100;

方式2：通过select into

select 字段 into 变量名
from 表；

案例1：

SELECT 
  COUNT(*) INTO @count 
FROM
  employees ;

使用（查看用户变量的值）

select @用户变量名；

#### 局部变量

作用域：仅仅在定义它的begin end中有效
应用在begin end中的第一句话
声明

declare 变量名 类型；

declare 变量名 类型 default 值；

赋值

方式1：通过set或select（同上）

set 局部变量名=值；
set 局部变量名:=值；（推荐）
select @局部变量名:=值；

方式2：通过select into

select 字段 into 局部变量名
from 表；

使用

select 局部变量名；

对比用户变量和局部变量：

作用域	定义和使用的位置	语法
用户变量	当前会话	会话中的任何地方	必须加@符号，不用限定类型
局部变量	begin end中	只能在begin end中，且为第一句话	一般不用加@符号，需要限定类型
案例1：声明两个变量并赋初始值，求和，并打印

用户变量

SET @m=1;
SET @n=2;
SET @sum=@m+@n;
SELECT @sum;


```



## 存储过程

- 类似于Java中的方法（一组合法的代码被包装起来）

好处：

1. 提高了代码的重用性
2. 简化操作
3. 减少了编译次数和与数据库服务器的连接次数，提高了效率



### 语法和参数

```sql
-- 创建语法
CREATE PROCEDURE 存储过程名（参数列表）
BEGIN
	存储过程体（一组合法的sql语句）
END

-- 参数列表
模式 参数名 参数类型

-- 模式
IN :传入值，调用方需要填写的值
OUT ：返回值
INOUT ：既需要传入值又可以返回值

-- 注意点
1.当存储过程体只有一条语句时，begin end可以省略
2.存储过程体每句话结尾都要加分号
3.存储过程的结尾要用 DELIMITER 重新设置
例如： DELIMITER $
即连接中的存储过程体都需要用$结尾
```



### 案例

```sql
-- 调用语法
CALL 存储过程名 （参数列表）

SELECT * FROM beauty;
案例一：创建存储过程实现 根据女神名查询对应的男神信息
DELIMITER $
CREATE PROCEDURE myp1 (IN beautyname VARCHAR(20))
BEGIN 
	SELECT bo.*
	FROM boys bo
	RIGHT JOIN beauty b
	ON b.`boyfriend_id`=bo.id
	WHERE b.name=beautyname;
END $
CALL myp1 ('苍老师')

-- 创建存储过程实现 用户是否登录成功
DELIMITER  $  -- (每次都要重新声明)
CREATE PROCEDURE myp2(IN username VARCHAR(20),IN PASSWORD VARCHAR(20))
BEGIN
	DECLARE result INT DEFAULT 0;
	
	SELECT count(*) INTO result
	FROM admin
	WHERE admin.`username`=username
	AND admin.`password`=PASSWORD;
	
	SELECT if(result>0,'成功','失败');
END $

SELECT * FROM admin
CALL myp2('john','8888');


-- 根据女神名，返回对应的男神名
DELIMITER $
CREATE PROCEDURE myp4(IN beautyname VARCHAR(20),OUT boyname VARCHAR(20))
BEGIN
	SELECT bo.boyname INTO boyname
	FROM boys bo
	INNER JOIN beauty b
	ON bo.id=b.boyfriend_id
	WHERE b.name=beautyname;
	
END $
CALL myp4 ('苍老师',@boyname)
SELECT @boyname $

-- 根据女神名，返回对应的男神名和男神魅力值
DELIMITER $
CREATE PROCEDURE myp5(IN beautyname VARCHAR(20),OUT boyname VARCHAR(20),OUT usercp INT)
BEGIN
	SELECT bo.boyname,bo.usercp INTO boyname,usercp
	FROM boys bo
	INNER JOIN beauty b
	ON bo.id=b.boyfriend_id
	WHERE b.name=beautyname;
	
END $
CALL myp5 ('苍老师',@boyname,@usercp)
SELECT @boyname,@usercp 


-- 空参列表
DELIMITER $
CREATE PROCEDURE myp6()
BEGIN
	INSERT INTO admin(username,`password`)
	VALUES('m','2222'),('n','2222'),('z','4444');
END $
CALL myp6()
SELECT * FROM admin
```





```sql
-- 存储过程的查看和删除

SHOW CREATE PROCEDURE myp1;

-- 删除
DROP PROCEDURE myp1;

-- 创建存储过程或函数实现传入一个日期，格式化成**年**月**日并返回
	SELECT date_format(TIME,'%y年%m月%d日') INTO result;
END $
CALL task3(now(),@result)
SELECT @result;

 -- 传入女神名，返回女神名 and 男神名
DELIMITER $
CREATE PROCEDURE task16(IN bname VARCHAR(20),OUT str VARCHAR(50))
BEGIN
	SELECT concat(bname,'and',ifnull(bo.boyname,'null')) INTO str
	FROM boys bo
	RIGHT JOIN beauty b
	ON b.`boyfriend_id`=bo.id
	WHERE b.name=bname;
END $

CALL task16('苍老师',@s)
SELECT @s
CREATE PROCEDURE task3(IN TIME DATETIME,OUT result VARCHAR(50))
BEGIN


-- 根据传入的条目数和起始索引，查询beauty表的记录
DELIMITER $
CREATE PROCEDURE task11(IN OFFSET INT,IN size INT)
BEGIN
	SELECT * FROM beauty
	LIMIT OFFSET,size;
END $
CALL task11(1,2)-- 从第一条开始，查询两条记录
```

