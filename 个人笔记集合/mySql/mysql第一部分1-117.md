[Toc]

# mysql基础

## 安装

- ```
  https://dev.mysql.com/downloads/windows/installer/8.0.html
  ```

- 通过安装器下载, 只需要下载mysql server

  - 选择custom
  - 选择mysql server 8.0.28 x64  , next
  - 点击execute, 然后next
  - 设置根密码:123456
  - 修改serviceName: MySQL0815
  - 点击execute

## 配置环境变量

- 进入此电脑->配置->服务 可以看到Mysql0815 正在运行
- 进入此电脑->属性->高级->环境变量没有找到mysql的环境变量

  - 将`C:\Program Files\MySQL\MySQL Server 8.0\bin`保存到环境变量中
- 进入`C:\Program Files\MySQL\MySQL Server 8.0`没有找到my.ini与data文件夹
  - 在`C:\ProgramData\MySQL\MySQL Server 8.0`下找到了my.ini文件

## my.ini配置文件

- 在安装文件夹`C:\Program Files\MySQL\MySQL Server 8.0`下找到my.ini文件
  - 在`C:\ProgramData\MySQL\MySQL Server 8.0`下找到了my.ini文件

- 没有, 重新建一个
  - [mysqld]----> 服务端配置
  - port: 端口号
  - basedir: 安装目录
  - datadir: 数据库的保存目录 // C:\ProgramData\MySQL\MySQL Server 8.0
  - character-set-server=utf8 : 字符集
  - default-storage-engine=INNODB : 数据库存储引擎
- 有些可以改, 但是改完需要重新启动服务器

### 部分my.ini配置项

- 这些项不要求自己配置, 至少我安装后到目前为止还没有必要修改my.ini文件

- 数据来源于菜鸟教程

- ```
  [client]
  # 设置mysql客户端默认字符集
  default-character-set=utf8
   
  [mysqld]
  # 设置3306端口
  port = 3306
  # 设置mysql的安装目录
  basedir=C:\\web\\mysql-8.0.11
  # 设置 mysql数据库的数据的存放目录，MySQL 8+ 不需要以下配置，系统自己生成即可，否则有可能报错
  # datadir=C:\\web\\sqldata
  # 允许最大连接数
  max_connections=20
  # 服务端使用的字符集默认为8比特编码的latin1字符集
  character-set-server=utf8
  # 创建新表时将使用的默认存储引擎
  default-storage-engine=INNODB
  ```

## mysql服务的启动和停止

- 计算机->管理->服务->mysql0815->自动
- 或者通过命令行的方式开启服务
  - 管理员身份打开命令行
  - `net stop mysql0815 ` 停止服务
  - `net start mysql0815`启动服务

## 服务端的登录与退出

- 找到command line client, 这是mysql自带的客户端
- 输入密码登录
- `exit`命令退出

## 命令行方式

- `mysql -hlocalhost -P3306 -uroot -p123456`
  - h:host
  - P(大写): 端口号
  - u: 用户名
  - p(小写): 密码, 不建议跟着写, 写完 `-uroot  -p`就可以回车, 然后要求输入密码再输入
  - 前三项有没有空格都可以, 第四项密码不可以有空格
  - 报错记录
    - `ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)`
      - 不能写完-uroot就回车, 还要加上 -p 才能回车
- 简写方式: `mysql -uroot -p123456`
  - 默认连接 localhost:3306

## mysql常见命令

- `show databases;`

  - 显示所有数据库

  - 注意要求分号

    - ```
      information_schema // 保存元数据, 不能动
      mysql // 保存用户信息, 不能动
      performance_schema // 收集性能参数, 不能动
      sys
      ```

    - 这四个是默认自带的库

- `use [指定的库名];`

  - 注意要求分号
  - 进入指定的数据库

- `show tables;`

  - 显示当前数据库中的所有表

- `show tables from mysql;`

  - 和上面的代码一样, 显示当前数据库中的表

- `select database();`

  - 显示当前正在操作的数据库

- ```sql
  create table stuinfo(   // Enter
  id int, //Enter
  name varchar(20))
  ```
  
  - 新建一个stuinfo表, 该表中有id字段, 其类型是整数(int), name字段, 其类型是
  - `show tables;` 可以看到这个表已在当前数据库建立
  
- `desc stuinfo;`

  - 显示表单的描述, 包括该表单的字段, 类型, 默认值等信息

-  `select * from userinfo`

  - 查看该表单的所有内容

- `insert into stuinfo (id,name) values(1,'john')`

  - 给stuinfo表单添加内容 {id:1,name:'john'}

- `update stuinfo set name='lilei' where id=1`

  - 更改stuinfo表单中id=1的行的name字段值为lilei

- `delete from stuinfo where id=1`

  - 删除stuinfo表单的id=1的行

## 查看mysql服务器版本

- `mysql --version`或者`mysql -V`

## 总结mysql常见命令

- 查看当前的所有数据

  - `show database;`

- 打开指定的库

  - `use 库名`

- 查看当前库的所有表

  - `show tables;`

- 查看其它库的所有表

  - `show tables from 库名`

- 创建表

  - ```sql
    create table 表名(
    	字段 字段类型,
        字段 字段类型
        ...
    )
    ```

- 查看表结构

  - `desc 表名`

- 查看服务器版本

  - 登录到mysql服务端
    - `select version();`
  - 没有登录到mysql服务端
    - `mysql --version`或者`mysql --V`

## mysql语法规范

- 不区分大小写
  - `show databases;`与`SHOW DATABASES;`	没有区别
  - 但建议关键字大写, 表名, 字段名小写
- 每条命令用分号结尾
  - 备注: 老师这里使用了一个\g来代替分号
  - `show databases\g`和`show databases;`作用一致
- 每条命令根据需要, 可以进行缩进或换行
- 注释
  - 单行注释: `#注释文字`
  - 单行注释: `-- 注释文字`
  - 多行注释: `/* 注释文字 */`

## 图形化用户界面客户端

### 安装与注册

- 下载SQLyog

  - 下载链接 `https://www.jb51.net/database/645936.html`

- 2058错误码解决方法

  - cmd登录mysql
  
  - 将下列代码的`root`修改为用户名,`password`修改为密码
  
  - ```
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
    ```
  

### 连接数据库

- 新建
- 新连接名随便, 可以与mysql服务器名相同
- 我的sql主机地址, 用户名, 端口号不用写, 自带
- 填写密码
- 数据库不写默认显示所用数据库
- 点击连接

## myemployees库的四张表介绍

- SQL语言的细分
  - DQL :DATA QUERY LANGUAGE  ( 针对查询 )
  - DML: DATA MANIPULATION LANGUAGE ( 增删改 )
- 在老师给的sql文件中找到`myemployees.sql`文件
  - SQLyog左侧 `root@localhost` 右键 `执行SQL脚本`, 选择这个文件载入
  - 选择`刷新对象浏览器`, 可以在左侧发现 myemployees 数据库
- 打开表,有departments, employees, jobs, locations 四张表

#### employees表

- 字段
  - employee_id :  员工编号
  - first_name : 名
  - last_name : 姓
  - email: 邮箱
  - phone_number: 电话号码
  - job_id : 工作编号
  - salary: 薪水
  - commission_pct: 奖金率
  - manager_id: 上级领导的员工编号
  - department_id: 部门编号
  - hiredate: 入职时间

#### department表

- 字段
  - department_id: 部门id
  - department_name: 部门名
  - manager_id: 部门管理的员工编号
  - location_id: 位置编号

#### locations表

- 字段
  - location_id: 位置id
  - street_address: 所属街道
  - postal_code: 邮编
  - city: 城市
  - state_province: 州/省
  - country_id: 国家编号

#### jobs表

- 字段
  - job_id: 工种编号
  - job_title: 工种名称
  - min_salary: 最低工资
  - max_salary: 最高工资

## 基础查询

### 基础查询介绍

- `SELECT 查询列表 from 表名`

  - 查询列表可以是以下任意一种
    - 表中的字段
    - 常量
    - 表达式
    - 函数

  - 查询结果是一个虚拟表格

- 查询表中的单个字段
  - `SELECT last_name FROM employees;`
- 查询表中的多个字段
  - `SELECT last_name,salary,email FROM employees;`

- 查询表中的所有字段
  - `SELECT * FROM employees;`

### 查询细节补充

- 查询前需要添加`USE 库名`
- 查询字段时可以添加`号, 用来标识其是一个字段

### 查询常量,表达式,函数

- `SELECT 100`
- `SELECT 100*98`
- `SELECT VERSION()`

- 没什么用...

### 为字段起别名

方式一: 

- select last_name as 姓,first_name as 名 from employees;
  - 这样的话查询结果的字段名分别被替换为姓,名
  - 如果查询的字段有重名的情况, 可以通过别名来区别

方式二:

- select last_name 姓,first_name 名 from employees; 
- 使用空格代替AS

案例: 查询salary, 显示结果为 out put

- SELECT salary "out put" FROM employees;
  - 需要给别名添加引号

### 去重

- 查询员工表中涉及到的所有部门编号
  - `SELECT department_id FROM employees;`查询所有的员工部门编号, 大量重复
  - `SELECT DISTINCT department_id FROM employees;` 只看到涉及到的部门编号, 每个值只出现一次

### +号的作用, concat拼接字段值

- 查询员工名和姓连接成一个字段, 并显示为姓名

  - msql中的+号仅仅用作运算符

    - `select 100+90` 两个操作数都是数值型, 则做加法预算
    - `select '123'+90` 其中一方为字符型, 试图将字符型转换为数值型
      - 转换成功, 继续做加法运算
      - `select 'john'+90` 转换失败, 字符型转换为数值0 // 90
    - `select null+80`  只要其中一方为null, 结果必然为null

  - ```sql
    SELECT CONCAT(last_name,'-',first_name) AS 姓名 FROM employees;
    ```

    - 将 last_name字段, - 符号, first_name字段的结果拼接在一起作为姓名字段输出

### 基础查询的案例讲解

- 显示表department的结构, 并查询其中的全部数据

  - ```sql
    DESC departments;
    SELECT * FROM departments;
    ```

- 显示表employees中的全部job_id, 不能重复

  - ```sql
    SELECT DISTINCT job_id FROM employees;
    ```

- 显示出表employees的全部列的值, 各个列的值之间用逗号连接, 列头显示out_put

  - IFNULL的用法

    - ```sql
      SELECT IFNULL(commission_pct,0) AS 奖金率,commission_pct FROM employees;
      ```

    - 如果commission_pct值为null, 那么输出0

  - ```sql
    select concat(first_name,',',last_name,',',job_id,',',iFNULL(commission_pct,0)) as "out put" from employees; 
    ```

## 条件查询介绍

- `select 查询列表 from 表名 where 筛选条件`

- 按条件表达式筛选
  - 大于(>), 小于(<), 等于(=),不等于(!=,<>)
- 按逻辑表达式筛选
  - &&, ||, !
- 模糊查询
  - like, between, and , in, is null

### 按条件表达式查询

- 查询员工工资大于12000的员工信息

  - ```sql
    SELECT * FROM employees WHERE salary>12000;
    ```

- 查询部门编号不等于90的员工名和部门编号

  - ```sql
    SELECT last_name,department_id FROM employees WHERE department_id!=90;
    ```

  - ```sql
    SELECT last_name,department_id FROM employees WHERE department_id<>90;
    ```

### 按逻辑表达式查询

- &&: 全true才true
- ||: 全false才false
- !: 

- 查询工资在10000-20000之间的员工名, 工资以及奖金

  - ```sql
    SELECT last_name,salary,IFNULL(commission_pct,0) FROM employees WHERE salary>10000&&salary<20000;
    ```

- 查询部门编号不是在90-110之间, 或者工资高于15000的员工的全部信息

  - ```sql
    SELECT * FROM employees WHERE !(department_id>=90&&department_id<=110)||salary>15000;
    ```

### 模糊查询

- like, between and , in, is null|is not null

- 查询员工名中包含字符a的员工信息

  - ```sql
    select * from employees where last_name like '%a%';
    ```

  - like 一般和通配符搭配使用

    - % 代表任意多个字符, 包含0个字符
    - _ 代表任意单个字符

- 查询员工名中第三个字符为n. 第五个字符为l的员工名和工资

  - ```sql
    SELECT last_name,salary FROM employees WHERE last_name LIKE '__n_l%';
    ```

- 查询出员工名中第二个字符为下划线的员工名

  - ```sql
    SELECT last_name FROM employees WHERE last_name LIKE '_\_%'
    ```

  - ```sql
    SELECT last_name FROM employees WHERE last_name LIKE '_$_%' ESCAPE '$';
    ```

  - _和%作为文本匹配项需要转义, 可以使用EACAPE自定义转义符号

- 查询员工编号在100到120之间的所有员工信息

  - ```sql
    SELECT * FROM employees WHERE employee_id BETWEEN 100 AND 120;
    ```

  - between and 执行的是闭区间 [] 

  - 100与120数值不可交换位置

- 查询员工的工种编号是 IP_PROG , AD_VP, AD_PRES 中的一个, 获取员工名和工种编号

  - ```SQL
    SELECT last_name,job_id FROM employees WHERE job_id IN ('IT_PORT','AD_VP','AD_PRES');
    ```

  - 判断某个值是否属于指定中的其中一个

  - in列表中的值必须类型一致或兼容(即'123'可以被转换为数值123)

  - in列表中的值不允许使用通配符, 因为in列表执行的是=逻辑, 而不是like

- 查询没有奖金的员工名和奖金率

  - ```SQL
    select last_name,commission_pct from employees where commission_pct is null;
    ```

  - ```sql
    select last_name,commission_pct from employees where commission_pct=null;
    // 错误, = 号不能判断null值
    ```

  - 查询有奖金的

    - ```sql
      select last_name,commission_pct from employees where commission_pct is NOT null;
      ```

  - = 号与 <> 号不能判断null值

### 安全等于<=>

- <=> 可以判断null值

  - ```sql
    select last_name,commission_pct from employees where commission_pct<=>null;
    ```

- is NULL : 仅仅可以判断NULL值

- <=> : 既可以判断NULL值, 也可以判断其它数值, 但可读性较低

## 条件查询

- 查询员工号为176的员工的姓名和部门号和年薪

  - ```sql
    select employee_id,last_name,department_id,salary*12*(1+IFNULL(commission_pct,0)) AS 年薪 from employees where employee_id=176;
    ```

    - commission_pct 可能为null, 如果为null则替换为0

### 练习题

- 查询没有奖金, 且工资小于18000的salary, last_name

  - ```sql
    SELECT last_name,salary FROM employees WHERE salary<18000&&(`commission_pct`<=>NULL);
    ```

- 查询employee表中, job_id不为'IT'开头或者工资为12000的员工信息

  - ```sql
    SELECT * FROM employees WHERE !(job_id LIKE 'IT%');
    ```

- 查看部门department表的结构

  - ```sql
    desc departments;
    ```

- 查看部门department表中涉及到了哪些位置编号

  - ```sql
    SELECT DISTINCT location_id FROM departments;
    ```

- 面试题: 请问`select * from employees` 和`select * from employees where commission_pct like '%%' and last_name like '%%'`的结果是否一样

  - 不一样, 如果判断的字段存在null值, 该字段不会返回.

## 排序查询

- `select 查询列表 from 表 where 筛选条件 order by 排序列表 [asc|desc]`

  - order by 可以支持单个字段, 多个字段, 表达式, 函数, 别名
  - order  by 子句一般是放在查询语句的最后面, 只有limit 子句可以放在它后面

- 查询员工信息, 要求工资从高到低排序

  - ```sql
    SELECT * FROM employees ORDER BY salary DESC
    ```

  - 从高到低就是把 desc 换成 asc; desc 是降序, asc是升序

  - asc可以不加, 因为默认就是按升序排列

- 查询部门编号大于等于90的员工信息, 并且按入职时间先后进行排序

  - ```sql
    SELECT * FROM employees WHERE department_id>=90 ORDER BY hiredate ASC
    ```

- 按年薪的高低显示员工的信息和年薪( 按表达式排序 )

  - ```sql
    SELECT *,salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
    FROM employees
    ORDER BY salary*12*(1+IFNULL(commission_pct,0)) DESC
    ```

  - 按别名排序

    - ```sql
      SELECT *,salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
      FROM employees
      ORDER BY 年薪 DESC
      ```

- 按姓名的长度显示员工的姓名和工资( 按函数排序 )

  - ```sql
    SELECT last_name,salary,LENGTH(last_name) AS 字节长度
    FROM employees
    ORDER BY LENGTH(last_name) ASC
    ```

- 查询员工信息, 要求先按工资升序, 再按员工编号降序 (按多个字段排序)

  - ```sql
    select salary,employee_id
    from employees
    order by salary asc, employee_id desc
    ```

  - 首先将工资按升序排列, 在不违背工资升序排列的前提下, 对相同工资的行按employee_id进行降序排列.

- 练习题: 

  - 查询员工的姓名和部门号和年薪, 按年薪降序, 按姓名升序

    - ```sql
      SELECT last_name,employee_id,salary*12*(1+IFNULL(commission_pct,0)) AS 年薪
      FROM employees
      ORDER BY 年薪 DESC, LENGTH(last_name) ASC
      ```

  - 选择工资不在8000到17000的员工的姓名和工资, 按工资降序

    - ```sql
      select last_name, salary
      from employees
      where salary<8000||salary>17000
      order by salary desc
      ```

  - 查询邮箱中包含e的员工信息, 并按邮箱的字节数降序, 再按部门号升序

    - ```sql
      SELECT *,LENGTH(email) AS 邮箱字节数
      FROM employees
      WHERE email LIKE '%e%'
      ORDER BY LENGTH(email) DESC, department_id ASC
      ```

## 常见函数介绍

- 分类
  - 单行函数( concat, length, ifNull )
  - 分组函数()

### 单行函数

- 字符函数 

  - length(str)  获取参数值的直接个数

    - 可以放字段名, 也可以放字符串常量

      - ```sql
        select length('john') // 4
        ```

      - ````sql
        SELECT LENGTH('哈哈') // 6
        ````

    - 查看客户端字符集

      - ```sql
        SHOW VARIABLES LIKE '%char%' // 在client字段里找到值utf8
        ```

      - utf8里一个中文占三个字节

  - concat(str1,str2)

    - ```sql
      select concat(last_name,'_',first_name) as 姓名 from employees 
      ```

  - upper/lower

    - ```sql
      SELECT UPPER(last_name) FROM employees
      ```

    - 将姓变大写, 名变小写, 然后拼接

      - ```sql
        SELECT CONCAT(LOWER(first_name),'_',UPPER(last_name)) AS 姓名
        FROM employees
        ```

  - substr / substring

    - 截取字符串

      - ```sql
        select substr('好好学习,天天向上',6) as out_put // 天天向上
        ```

        - sql语句里substr的所有序号都是从1开始, 天 字的序号是6, 截取了从第6位到最后一位的字符

      - ```sql
        SELECT SUBSTR('好好学习,天天向上',2,5) AS out_put // 好学习,天
        ```

        - 第一个参数是截取初始序号, 第二个字符是截取字符长度

      - substring与substr的功能一样

    - 案例: 姓名中首字符大写, 然后用 _ 拼接起来

      - ```sql
        SELECT 
        CONCAT(
        	CONCAT(
        		UPPER(SUBSTR(first_name,1,1)),
        		SUBSTR(last_name,2)
        	),
        	'_',
        	CONCAT(
        		UPPER(SUBSTR(last_name,1,1)),
        		SUBSTR(last_name,2)
        	)
        )
        AS full_name
        FROM employees
        ```

  - instr 返回第二个参数在第一个参数第一次出现的索引, 如果找不到就返回0

    - ```sql
      SELECT INSTR('明月出天山','天山') AS out_put // 4
      ```

    - 找不到就返回0

  - trim 去掉前后空格/或去掉指定的字符

    - ```sql
      select trim('    明月出天山    ') as out_put
      ```

    - ```sql
      SELECT TRIM('a' FROM 'aaa明月aaa出天山aaaa') AS out_put // 明月aaa出天山
      ```

    - ```sql
      SELECT TRIM('aa' FROM 'aaa明月aaa出天山aaaa') AS out_put // a明月aaa出天山
      ```

  - lpad (类似于js的padStart) 如果字符串长度不够设定的长度, 那么使用第三个参数在原字符前面进行填充

    - ```sql
      select lpad('hello',10,'=') as out_put // =====hello
      ```

  - replace 替换

    - ```sql
      select replace('hello world','l','b') as out_put // hebbo worbd
      ```

- 数学函数

  - round 四舍五入

    - ```sql
      SELECT ROUND(1.65) AS out_put // 2
      ```

    - ```sql
      SELECT ROUND(-1.65) AS out_put // -2
      ```

    - ```sql
      SELECT ROUND(1.6558965,2) AS out_put // 1.66 小数点后保留两位
      ```

  - ceil 向上取整

    - ```sql
      SELECT CEIL(1.27) // 2
      ```

    - ```sql
      SELECT CEIL(-1.27) // -1
      ```

  - floor 向下取整

    - ```sql
      SELECT FLOOR(1.68) AS out_put // 1
      ```

    - ```sql
      SELECT FLOOR(-1.68) AS out_put // -2
      ```

  - truncate 截断 

    - ```sql
      select truncate(1.6999,1) as out_put // 1.6
      ```

    - ```sql
      SELECT TRUNCATE(5,1) AS out_put // 5
      ```

  - mod 取余

    - ```sql
      select mod(10,3) as out_put // 1
      ```

- 日期函数

  - now 返回当前系统日期+时间

    - ```sql
      SELECT NOW() AS out_put // 2022-5-13 16:05:42
      ```

  - curdate 返回当前系统该日期, 不包含时间

    - ```sql
      SELECT CURDATE() AS out_put // 2022-05-13
      ```

  - curTime 返回当前时间, 不包含日期

    - ```sql
      select curtime() as out_put // 16:08:13
      ```

  - 获取指定的部分: 年 , 月, 日, 小时, 分钟 , 秒

    - ```sql
      SELECT YEAR(NOW()) AS out_put // 2022
      ```

    - ```sql
      SELECT YEAR('2025-06-08') AS out_put // 2025
      ```

    - ```sql
      SELECT YEAR(hiredate) AS 年 
      FROM employees
      ```

    - MONTH/MONTHNAME , DAY , HOUR等等

  - str_to_date  将日期格式的字符转换成日期

    - | 序号 | 格式符 | 功能                               |
      | ---- | ------ | ---------------------------------- |
      | 1    | %Y     | 四位的年份                         |
      | 2    | %y     | 2位的年份                          |
      | 3    | %m     | 月份(01,02,03...)                  |
      | 4    | %c     | 月份(1,2,3...), 似乎与%m功能合并了 |
      | 5    | %d     | 日(01,02...)                       |
      | 6    | %H     | 小时(24小时制)                     |
      | 7    | %h     | 小时(12小时制)                     |
      | 8    | %i     | 分钟(00,01,02...)                  |
      | 9    | %s     | 秒(00,01...)                       |

    - ```sql
      SELECT STR_TO_DATE('1998/3/2','%Y/%c/%d') AS out_put
      // 1998-03-02
      ```

    - 查询入职日期为1992-4-3的员工数量

      - ```sql
        select * from employees
        where hiredate ='1992-4-3'
        ```

      - ```sql
         SELECT * FROM employees WHERE hiredate=STR_TO_DATE('4-3 1992','%c-%d %Y')
        ```

        - 考虑到传入的查询数据可能不符合数据库的格式, 那么需要将它转换为专用格式

  - date_formate: 将日期转换成字符

    - ```sql
      select date_format('2018/6/6','%Y年%m月%d日') as out_put
      // 2018年6月6日
      ```

    - ```sql
      select date_format(now(),'%y年%m月%d日') as out_put
      // 22年05月13日
      ```

- 其它函数

  - ```
    SELECT VERSION();
    SELECT DATEBASE();
    SELECT USER()
    ```

- 流程控制函数

  - if函数

    - ```sql
      select if(10>5,'大','小') as out_put // 大
      ```

    - ```sql
      SELECT last_name ,IF(commission_pct IS NULL,'没奖金,呵呵','有奖金,嘻嘻') AS 备注
      FROM employees
      ```

  - case函数 : switch case的效果

    - ```
      case 要判断的字段或表达式
      when 常量1 then 要显示的值1或语句1
      wehn 常量2 then 要显示的值2或语句2
      ...
      else 要显示的值n或语句n
      end
      ```

    - ```sql
      select salary as 原始工资,department_id,
      case department_id
      when 30 then salary*1.1
      when 40 then salary*1.2
      when 50 then salary*1.3
      else salary
      end
      as 新工资
      from employees
      order by department_id asc
      ```

    - case的函数第二种使用方法  类似于 多重if

      - ```
        case
        when 条件1 then 要显示的值1或语句1
        when 条件2 then 要显示的值2或语句2
        ...
        when 条件n then 要显示的值n或语句n
        else 要显示的default值或语句
        end
        ```

      - 第一种方法用于判断等值, 第二种方法用于判断区间

      - 查询员工的工资情况, 如果工资>20000, 显示A级别; 如果工资>15000, 显示B级别, 如果工资>10000, 显示C级别 , 否则显示D级别

        - ```sql
          select salary,
          case
          when salary>=20000 then  'A'
          when salary>=15000&&salary<20000 then 'B'
          when salary>=10000&&salary<15000 then 'C'
          else 'other'
          end
          as '工资等级'
          from employees
          order by salary asc
          ```

- 单行函数练习题

  - 显示系统时间: 日期+时间

    - ```sql
      select now()
      ```

  - 查询员工号, 姓名, 工资以及工资提高百分之二十后的结果( new salary )

    - ```sql
      select employee_id,last_name,salary,salary*1.2 as 'new salary'
      from employees
      ```

  - 将员工的姓名按首字母排序, 并写出姓名的长度

    - ```sql
      SELECT last_name, SUBSTR(last_name,1,1) AS 首字母 ,LENGTH(last_name) AS 姓名长度
      FROM employees
      ORDER BY SUBSTR(last_name,1,1) ASC
      ```

  - 做一个查询, 产生以下结果

    - `<last_name> earns  <salary> monthly but wants <salary*3> Dreams Salary`

    - `King earns 24000 monthly but wants 72000`

    -  ```sql
       select concat(last_name,' earns ',salary,' monthly but wants ',salary*3,' Dreams Salary')
       from employees
       ```

  - 使用case-when, 按照下面的条件

    - ```
      job  grade
      AD_PRES  A
      ST_MAN  B
      IT_PROG  C
      SA_REP D
      ST_CLERK E
      ```

    - ```sql
      SELECT last_name,job_id AS job,
      CASE 
      WHEN job_id LIKE 'AD%' THEN 'A'
      WHEN job_id LIKE 'ST_M%' THEN 'B'
      WHEN job_id LIKE 'IT%' THEN 'C'
      WHEN job_id LIKE 'SA%' THEN 'D'
      WHEN job_id LIKE 'ST_C%' THEN 'E'
      END
      AS 'grade'
      FROM employees
      WHERE job_id='AD_PRES'
      ```

### 分组函数

- 分类: sum 求和, avg 平均值, max 最大值, min 最小值, count 计算个数

- ```sql
  select sum(salary) as 'sum' from employees
  // 691400.00 直接求出salary字段整列的值的和
  ```

- ```sql
  SELECT AVG(salary) AS 'avg' FROM employees
  // 求出整个字段的平均值
  ```

- ```sql
  SELECT MIN(salary) AS 'min' FROM employees
  // 求出整个字段的最小值
  ```

- ```sql
  select max(salary) as 'max' from employees
  // 求出整个字段的最大值
  ```

- ```sql
  SELECT COUNT(salary) AS 'count' FROM employees
  // 求出整个字段的值有多少
  ```

- ```sql
  SELECT SUM(salary) AS 和,
  ROUND(AVG(salary),2) AS 平均值,
  MIN(salary) AS 最小值,
  MAX(salary) AS 最大值,
  COUNT(salary) AS 数据量
  FROM employees
  ```

#### 参数支持哪些类型

- sum, avg一般用于处理数值型. 
- max,min,count用于处理任何类型
- 所有的分组函数都忽略null值

#### 可以和distinct搭配实现去重运算

- ```sql
  select sum(distinct salary),sum(salary) from employees
  ```

- count使用distinct挺多的

#### count函数的详细介绍

- ```sql
  SELECT COUNT(*) FROM employees
  // 经常使用这个来统计表单的总行数
  // 因为除非整行的值都是null, 否则都会被统计上
  ```

- ```sql
  select count(1) from employees
  // 这个也可以用来统计表单的总行数
  // count()内部的值可以是任意数值或字符串, 都可以用来统计总行数
  ```

- 效率: 

  - 在INNODB之前用的是MYISAM引擎
  - MYISAM引擎下, count(*)的效率高
  - INNODB引擎下, count(*)和count(1)的效率差不多, 比count(字段)要高一些
  - 因此, 现在一般使用count(*)统计总行数

#### 分组函数的使用注意事项

- 和分组函数一同查询的字段要求是group by后的字段

#### 练习题

- 查询公司员工工资的最大值,最小值, 平均值, 总和

  - ```sql
    SELECT 
    MAX(salary) AS 最大值,
    MIN(salary) AS 最小值,
    ROUND(AVG(salary)) AS 平均值,
    SUM(salary) AS 总和
    FROM employees
    ```

- 查询员工表中的最大入职时间和最小入职时间的相错天数

  - ```sql
    SELECT
    DATEDIFF(MAX(hiredate),MIN(hiredate))
    AS 'diff'
    FROM employees
    // 8735
    // 不可以直接使用max()-min()来得到
    ```

- 查询部门编号为90的员工个数

  - ```sql
    SELECT
    COUNT(*)
    AS 个数
    FROM employees
    WHERE department_id=90
    ```

## 分组查询

- 查询每个部门的平均工资

### group by 子句语法

- group by 子句将表中的数据分成若干组

  - ```
    select 分组函数,列(要求出现在分组函数的后面)
    from 表
    [where 筛选条件]
    [group by 分组的列表]
    [order by 子句]
    ```

  - 案例: 查询每个工种的最高工资

    - ```sql
      SELECT MAX(salary),job_id
      FROM employees
      GROUP BY job_id
      ```

  - 案例2: 查询每个位置上的部门个数

    - ```sql
      SELECT COUNT(*),location_id
      FROM departments
      GROUP BY location_id
      ```

### 添加分组前筛选

- 查询邮箱中包含a字符的, 每个部门的平均工资

- ```sql
  select avg(salary),department_id,email
  from employees
  where email like '%a%'
  group by department_id
  ```

- 查询有奖金的每个领导手下员工的最高工资

  - ```sql
    SELECT MAX(salary),manager_id
    FROM employees
    WHERE commission_pct IS NOT NULL
    GROUP BY manager_id
    ```

### 添加复杂的筛选条件__添加分组后的筛选

- 查询那个部门的员工个数>2

  - 首先查询每个部门的员工个数

    - ```sql
      select count(*),department_id
      from employees
      group by department_id
      ```

  - 根据上面的结果进行筛选, 查询哪个部门的员工数>2

    - ```sql
      select count(*) as 个数,department_id
      from employees
      group by department_id
      having 个数>2
      ```

- 查询每个工种有奖金的员工的最高工资>12000的工种编号和最高工资

  - 查询每个工种有奖金的员工的最高工资

    - ```sql
      SELECT MAX(salary),job_id
      FROM employees
      WHERE commission_pct IS NOT NULL
      GROUP BY job_id
      ```

  - 根据上面的结果进行查询后筛选

    - ```sql
      SELECT MAX(salary) AS 最高工资,job_id
      FROM employees
      WHERE commission_pct IS NOT NULL
      GROUP BY job_id
      HAVING 最高工资>12000
      ```

- 查询领导编号>102的, 且领导手下的最低工资>5000的编号是哪些

  - 查询领导编号>102的

    - ```sql
      SELECT manager_id
      FROM employees
      WHERE manager_id>102
      ```

  - 根据manager_id字段进行分组查询同行的salary字段的最小值

    - ```sql
      SELECT MIN(salary),manager_id
      FROM employees
      WHERE manager_id>102
      GROUP BY manager_id
      ```

  - 根据上面的结果查询最小值>5000的

    - ```sql
      SELECT MIN(salary),manager_id
      FROM employees
      WHERE manager_id>102
      GROUP BY manager_id
      HAVING MIN(salary)>5000
      ```

### 分组筛选的总结

- 分组查询的筛选条件分为两类

  - |            | 数据源         | 位置                | 关键字 |
    | ---------- | -------------- | ------------------- | ------ |
    | 分组前筛选 | 原始表         | group by 子句的前面 | where  |
    | 分组后筛选 | 分组后的结果集 | group by 子句的后面 | having |

  - 分组函数做条件肯定是放在having子句中

  - 能用分组前筛选的, 尽量使用分组前筛选(考虑到性能问题)

### 按函数分组

- 按员工姓名的长度分组, 查询每一组的员工个数, 筛选员工个数>5的有哪些

  - ```sql
    select count(*),length(last_name) as 姓名长度
    from employees
    group by 姓名长度
    having count(*)>5
    ```

  - where不支持别名, group by 支持别名

### 按多个字段分组

- 查询每个部门每个工种的员工的平均工资

  - ```sql
    SELECT AVG(salary),department_id,job_id
    FROM employees
    GROUP BY job_id,department_id
    ```

### 添加排序

- 查询每个部门每个工种的员工的平均工资, 并且按照平均工资的高低显示

  - ```sql
    SELECT AVG(salary),department_id,job_id
    FROM employees
    WHERE department_id IS NOT NULL
    GROUP BY job_id,department_id
    ORDER BY department_id DESC
    ```

  - 显示平均工资高于10000的

    - ```sql
      SELECT AVG(salary),department_id,job_id
      FROM employees
      WHERE department_id IS NOT NULL
      GROUP BY job_id,department_id
      HAVING AVG(salary)>10000
      ORDER BY department_id DESC
      ```

### 分组查询总结

- group by 子句支持单个字段分组, 多个字段分组(多个字段之间使用逗号隔开且没有顺序要求), 表达式 或者函数
- 也可以添加排序(排序放在整个分组查询的最后)

### 练习题

- 查询各job_id的员工工资的最大值,最小值,平均值,总和,并按job_id升序

  - ```sql
    SELECT MAX(salary),MIN(salary),AVG(salary),SUM(salary),job_id
    FROM employees
    GROUP BY job_id
    ORDER BY job_id ASC
    ```

- 查询员工最高工资和最低工资的差距(difference)

  - ```sql
    SELECT MAX(salary),MIN(salary),(MAX(salary)-MIN(salary)) AS differ
    FROM employees
    ```

- 查询各个管理者手下员工的最低工资, 其中最低工资不能低于6000, 没有管理者的员工不计算在内

  - ```sql
    SELECT MIN(salary) AS 最低工资,manager_id
    FROM employees\
    WHERE manager_id IS NOT NULL
    GROUP BY manager_id
    HAVING 最低工资>=6000
    ```

- 查询所有部门的编号, 员工数量和工资平均值, 并按平均工资降序

  - ```sql
    SELECT COUNT(*) AS 员工数量,ROUND(AVG(salary),2) AS 平均工资,department_id
    FROM employees
    GROUP BY department_id
    ORDER BY 平均工资 DESC
    ```

- 选择具有各个job_id的员工人数

  - ```sql
    select count(*),job_id
    from employees
    where job_id is not null
    group by job_id
    ```

## 连接查询

-  多表查询, 当要查询的字段涉及到多个表时使用连接查询

- 笛卡尔乘积现象: 

  - 存在一个girls表有5行数据, 一个boys表有4行数据, 现在要根据girls表的bf字段匹配boys表id字段
  - `select bf,id from boys,girls` // 这样的话会有4X5=20条数据

  - 官方解释: 

    - 表1有m行, 表2有n行, 结果=m*n行

    - 发生原因: 没有有效的连接条件

    - 连接条件的设置

      - ```sql
        select [表1字段],[表2字段]
        from [表1],[表2]
        where 表1[字段]=表2[字段]
        ```

      - ```sql
        SELECT NAME,boyName
        FROM beauty,boys
        WHERE beauty.`boyfriend_id`=boys.`id`
        ```

### 连接查询的分类

- 按年代分类
  - sql92分类
    - 仅仅支持内连接 
  - sql99分类[推荐]
    - 支持所有内连接, 外连接(左外,右外),交叉连接
- 按功能分类
  - 内连接
    - 等值连接
    - 非等值连接
    - 自连接
  - 外连接
    - 左外连接
    - 右外连接
    - 全外连接
  - 交叉连接

### sql92标准

#### 71 等值连接的介绍

- 查询员工名和对应的部门名

  - ```sql
    SELECT last_name,department_name
    FROM employees,departments
    WHERE employees.`department_id`=departments.`department_id`
    ```

- 查询员工名, 工种号, 工种名,

  - ```sql
    SELECT last_name,employees.job_id,job_title 
    #employees表和jobs表都有job_id字段, 因此必须注明表名
    FROM employees,jobs
    WHERE employees.`job_id`=jobs.`job_id`
    ```

  - 使用别名省略冗长的表名, 区分重名的字段

    - ```sql
      SELECT last_name,e.`job_id`,j.`job_title`
      FROM employees AS e,jobs AS j
      WHERE e.`job_id`=j.`job_id`
      ```

    - 如果为表起了别名,则查询的字段就必须使用别名限定

      - 因为SQL语句的第一条是从FROM开始读取, 然后才是SELECT. 表名一旦设定别名, 那么接下来不能使用原表名

#### 72 等值连接的示例

- FROM 语句后面的表名能否更换顺序? 能

- 可以加筛选

  - 案例: 查询有奖金的员工名,部门名

    - ```sql
      select e.`last_name`,d.`department_name`
      from employees as e,departments as d
      where e.`department_id`=d.`department_id`&&e.`commission_pct` is not null
      ```

    - ```sql
      SELECT e.`last_name`,d.`department_name`
      FROM employees AS e,departments AS d
      WHERE e.`department_id`=d.`department_id`
      AND e.`commission_pct` IS NOT NULL
      ```

      - where 后面的条件语句不限顺序

  - 案例2: 查询城市名中第二个字符为o的部门名和城市名

    - ```sql
      SELECT d.`department_name`,l.`city`
      FROM locations AS l, departments AS d
      WHERE SUBSTR(l.`city`,2,1)='o' # 这一句也可以写 l.city like '_o%'
      AND d.`location_id`=l.`location_id`
      ```

- 可以加分组

  - 查询每个城市的部门个数

    - ```sql
      SELECT COUNT(*) AS 部门个数,l.`city` AS 城市
      FROM locations AS l, departments AS d
      WHERE d.`location_id`=l.`location_id`
      GROUP BY l.`city`
      ```

  - 案例二: 查询出有奖金的每个部门的部门名和部门的领导编号 和该部门的最低工资

    - ```sql
      SELECT d.`department_name` AS 部门名,e.`manager_id` AS 领导编号, MIN(e.`salary`) AS 该部门最低工资
      FROM departments AS d,employees AS e
      WHERE e.`department_id`=d.`department_id`
      AND e.`commission_pct` IS NOT NULL
      GROUP BY d.`department_name`
      ```

- 可以加排序

  - 查询出每个工种的工种名和员工个数, 并按员工个数降序

    - ```sql
      SELECT j.`job_title` AS 工种名,COUNT(*) AS 员工个数
      FROM employees AS e,jobs AS j
      WHERE e.`job_id`=j.`job_id`
      GROUP BY j.`job_title`
      ORDER BY 员工个数 DESC
      ```

- 可以实现三表连接

  - 查询员工名, 部门名与所在城市

    - ```sql
      SELECT e.`last_name` AS 员工名,d.`department_name` AS 部门名,l.`city` AS 所在城市
      FROM employees AS e,departments AS d,locations AS l
      WHERE e.`department_id`=d.`department_id`
      AND d.`location_id`=l.`location_id`
      ```

- 总结

  - 多表等值连接的结果为多表的交集部分
  - n表连接, 至少需要n-1个连接条件
  - 多表的顺序没有要求
  - 一般需要为表起别名
  - 可以搭配所有子句使用, 比如排序, 分组, 筛选

#### 73 非等值连接

- 案例1: 查询员工的工资和工资级别

  - 在工资级别.txt中复制sql语句执行, 注意去掉注释符号

  - ```sql
    SELECT e.`salary` AS 工资,jg.`grade_level` AS 工资级别  
    FROM employees AS e,job_grades AS jg
    WHERE e.`salary` BETWEEN jg.`lowest_sal` AND jg.`highest_sal`
    # 当salary在[jg.`lowest_sal`, jg.`highest_sal`]之间时, 获得对应行的level字段的值
    ORDER BY 工资 ASC
    ```

#### 74 自连接

- 在员工表里查询 员工名和他上级的名称

  - 员工表里 有员工的employee_id字段和manager_id字段,相当于把一张表当成两张表来用

  - ```sql
    SELECT e1.`last_name` AS 员工名,e2.`last_name` AS 上级名
    FROM employees AS e1,employees AS e2
    WHERE e1.`manager_id`=e2.`employee_id`
    ORDER BY e1.`last_name` ASC
    ```

  - 在from 子句里将同一张表引入两次并设置不同的别名

#### 75 测试题2讲解

- 显示员工表的最大工资,和平均工资

  - ```sql
    SELECT MAX(salary), ROUND(AVG(salary),2)
    FROM employees
    ```

- 查询员工表的employee_id,job_id,last_name, 按deparment_id降序,salary升序

  - ```slq
    select employee_id,job_id,last_name
    from employees
    order by department_id desc,salary asc
    ```

- 查询员工表的job_id中包含 a和e的, 并且a在e前面

  - ```sql
    SELECT job_id 
    FROM employees
    WHERE job_id LIKE '%a%e%'
    ```

- 显示当前日期, 以及去前后空格, 截取值字符串的函数

  - ```sql
    SELECT NOW() AS 当前日期
    ```

  - now(), trim(), substr()



#### 77作业讲解

- 显示所有员工的姓名,部门号和部门名称

  - ```sql
    SELECT last_name,e.department_id,department_name
    FROM employees AS e,departments AS d
    ```

- 查询90号部门员工的job_id和90号部门的location_id

  - ```sql
    SELECT e.department_id,job_id,location_id
    FROM employees AS e,departments AS d
    WHERE e.department_id=90
    AND e.`department_id`=d.`department_id`
    ```

- 选择所有有奖金的员工的last_name, deaprtment_name, location_id,city

  - ```sql
    SELECT e.last_name,d.department_name,d.location_id,l.city,e.`commission_pct`
    FROM employees AS e,departments AS d,locations AS l
    WHERE e.`department_id`=d.`department_id`
    AND d.`location_id`=l.`location_id`
    AND e.`commission_pct` IS NOT NULL
    ```

- 选择city在toronto工作的员工的last_name,job_id,department_id,department_name

  - ```sql
    SELECT last_name,job_id,e.department_id,department_name,l.`city`
    FROM employees AS e,departments AS d,locations AS l
    WHERE l.city='toronto'
    AND e.`department_id`=d.`department_id`
    AND l.`location_id`=d.`location_id`
    ```

- 查询每个工种,每个部门的部门名,工种名和最低工资

  - ```sql
    SELECT e.`job_id` AS 工种id,d.`department_name` AS 所属部门名,j.`job_title` AS 工种名,MIN(salary) AS 最低工资
    FROM jobs AS j,departments AS d,employees AS e
    WHERE e.`job_id`=j.`job_id`
    AND d.`department_id`=e.`department_id`
    GROUP BY e.`job_id`
    ORDER BY e.`job_id`
    ```

- 查询每个国家下的部门个数大于1的国家编号

  - ```sql
    SELECT COUNT(*) AS 部门个数,l.`country_id` AS 国家编号
    FROM locations AS l,departments AS d
    WHERE d.`location_id`=l.`location_id`
    GROUP BY 国家编号
    HAVING 部门个数>2
    ```

- 选择指定员工的姓名, 员工号, 以及它的管理者的姓名和员工号, 结果类似于下面的格式

  - ```
    employees  emp#  manager mgr#
    kochhar 101 king 100
    ```

  - ```sql
    SELECT e1.`employee_id` AS 员工号,e1.`last_name` AS 员工名,e2.`employee_id` AS 上级编号,e2.`last_name` AS 上级名
    FROM employees AS e1,employees AS e2
    WHERE e1.`manager_id`=e2.`employee_id`
    ORDER BY 员工号
    ```



#### 78 sql99语法

- 支持内连接, 外连接(左外,右外),交叉连接

- ```
  select 查询列表
  from 表1 as 别名 [连接类型]
  join 表2 as 别名 
  on 连接条件
  [where 筛选条件]
  [group by 分组]
  [having 筛选条件]
  [order by 排序列表]
  ```

- ```
  内连接: inner
  外连接:
  	左外: left [outer]
  	右外: right [outer]
  	全外: full [outer]
  交连接诶: cross
  ```



#### 79 sql99 等值连接

- 内连接

  - ```
    select 查询列表
    from 表1 别名
    inner join 表2 别名
    on 连接条件
    
    分类: 等值 非等值 自连接
    特点:可以添加排序分组筛选, inner可以省略, 筛选条件放在where后面, 连接条件放在on后面, 提高分离性, 便于阅读; inner join 连接与sql92语法中的等值连接效果是一样的, 都是查询多表的交集部分
    ```
    
  - 查询员工名, 部门名(调换位置)
  
    - ```sql
      SELECT last_name,department_name
      FROM employees e
      INNER JOIN departments d
      ON e.`department_id`=d.`department_id`
      ```
  
  - 查询名字中包含e的员工名和他的工种名
  
    - ```sql
      SELECT last_name,job_title
      FROM employees AS e
      INNER JOIN jobs AS j
      ON e.`job_id`=j.`job_id`
      WHERE e.`last_name` LIKE '%e%'
      ```
  
  - 查询部门个数>3的城市名和部门个数
  
    - ```sql
      SELECT COUNT(*) AS 部门个数, city AS 城市名
      FROM departments AS d
      INNER JOIN locations l
      ON d.`location_id`=l.`location_id`
      GROUP BY 城市名
      HAVING 部门个数>3
      ```
  
  - 查询每个部门的员工个数>3的部门名和员工个数, 并按个数降序
  
    - ```sql
      SELECT COUNT(*) AS 员工个数,department_name
      FROM employees AS e
      INNER JOIN departments AS d
      ON e.`department_id`=d.`department_id`
      GROUP BY department_name
      HAVING 员工个数>3
      ORDER BY 员工个数 DESC
      ```
    
  - 查询员工名\部门名\工种名, 并按部门名降序
  
    - ```sql
      SELECT last_name AS 员工名,department_name AS 部门名,job_title AS 工种名
      FROM employees AS e
      INNER JOIN departments AS d
      ON e.`department_id`=d.`department_id`
      INNER JOIN jobs AS j
      ON e.`job_id`=j.`job_id`
      ORDER BY 部门名 DESC
      ```



#### 80 sql99的非等值连接

- 查询员工的工资级别

  - ```sql
    SELECT salary,grade_level
    FROM employees AS e
    JOIN job_grades AS jg
    ON e.`salary` BETWEEN jg.`lowest_sal` AND jg.`highest_sal`
    ORDER BY salary
    ```

- 查询工资级别人数的个数>2的,并且按工资级别进行降序排序

  - ```sql
    SELECT COUNT(*) AS 工资级别人数,grade_level AS 工资级别
    FROM employees AS e
    INNER JOIN job_grades AS jg
    ON e.`salary` BETWEEN jg.`lowest_sal` AND jg.`highest_sal`
    GROUP BY 工资级别
    HAVING 工资级别人数>2
    ORDER BY 工资级别 DESC
    ```

#### 81 sql99_自连接

- 查询员工的名字和他的上级的名字

  - ```sql
    SELECT e1.last_name AS 员工名字,e2.last_name AS 上级名字
    FROM employees AS e1
    INNER JOIN employees AS e2
    ON e1.`manager_id`=e2.`employee_id`
    ```

- 查询姓名中包含字符k的员工的名字, 和他上级的名字

  - ```sql
    SELECT e1.`last_name` AS 员工名字, e2.`last_name` AS 上级名字
    FROM employees AS e1
    INNER JOIN employees AS e2
    ON e1.`manager_id`=e2.`employee_id`
    WHERE e1.`last_name` LIKE  '%k%'
    ```

#### 82 sql99_左/右外连接

- ```
  应用场景
  查询一个表中有, 但另一个表中没有的记录
  外连接的查询结果为主表中的所有记录
  	如果从表中有和它匹配的, 则显示匹配的值
  	如果从表中没有和它匹配的, 则显示null
  	外连接的查询结果=内连接结果+主表中有而从表中没有的记录
  左外连接: left join左边的是主表
  右外连接: right join右边的是主表
  左外与右外交换顺序可以实现同样的效果
  ```

- ```
  1、左外连接：是A和B的交集再并上A的所有数据。
  
  2、右外连接：是A和B的交集再并上B的所有数据。
  ```

- 

- 

- 查询男友没在数据表中的女性名

  - ```sql
    SELECT girls.name,boys.*
    FROM beauty AS girls
    LEFT OUTER JOIN boys
    ON girls.`boyfriend_id`=boys.`id`
    WHERE boys.`id` IS NULL
    ```

  - 使用右外连接

    - ```sql
      SELECT girls.name,boys.*
      FROM boys
      RIGHT OUTER JOIN beauty AS girls
      ON girls.`boyfriend_id`=boys.`id`
      WHERE boys.`id` IS NULL
      ```

  - girls.`boyfriend_id`=boys.`id`  这条语句求两个表的交集, 

    - FROM beauty AS girls LEFT OUTER JOIN boys, 即将左边的表的id设为全集合U, 求右边表中子集相对于全集合U的补集

- 查询哪个部门没有员工

  - ```sql
    # 首先确定主表: 获取没有员工的部门, 那么就存在 员工表-部门表, 全部部门 对应 全部员工, 一些部门对应值是null, 即主表是部门表
    # 接下来确定 所有的部门与员工的对应关系
    SELECT d.department_id,e.`employee_id`
    FROM departments AS d
    LEFT OUTER JOIN	employees AS e
    ON e.`department_id`=d.`department_id`
    ```

  - ```sql
    # 接下来获取所有部门与有部门的员工对应关系中, 员工id为null的部门
    SELECT d.department_name,e.`employee_id`
    FROM departments AS d
    LEFT OUTER JOIN	employees AS e
    ON e.`department_id`=d.`department_id`
    WHERE employee_id IS NULL
    ```

  - ```sql
    # 对查询结果去重
    SELECT DISTINCT d.department_name,e.`employee_id`
    FROM departments AS d
    LEFT OUTER JOIN	employees AS e
    ON e.`department_id`=d.`department_id`
    WHERE employee_id IS NULL
    ```

#### 83 spl99_全外连接

- mysql 不支持 full outer join 语句, 如果想在mysql中使用全外连接, 按照如下使用
- 将 beauty表与boys表完全结合起来

- ```sql
  SELECT * 
  FROM beauty AS girls
  LEFT JOIN boys
  ON girls.`boyfriend_id`=boys.`id`
  UNION
  SELECT *
  FROM beauty AS girls
  RIGHT JOIN boys
  ON girls.`boyfriend_id`=boys.`id` 
  ```

- 求 beauty 表与 boys 表去除可以完全匹配的部分

  - ```sql
    SELECT * 
    FROM beauty AS girls
    LEFT JOIN boys
    ON girls.`boyfriend_id`=boys.`id`
    WHERE boys.`id` IS NULL # 以girls表为主表, 得到boys表与之不匹配的部分
    UNION
    SELECT *
    FROM beauty AS girls
    RIGHT JOIN boys
    ON girls.`boyfriend_id`=boys.`id` 
    WHERE girls.`id` IS NULL # 以boys表为主表, 得到girls表与之不匹配的部分
    ```

- 全外连接即三部分组成: 交集部分, 左表独有的无法匹配的数据, 右表独有的无法匹配的数据

#### 84 交叉连接

- ```sql
  SELECT girls.*,boys.*
  FROM beauty AS girls
  CROSS JOIN boys
  ```

- 交叉连接不需要连接规则, 只需要将A表的每一行与B表的每一行强行进行连接即可. 

  - 如果A表有5行, B表有4行, 那么最终结果有20行

#### 85 总结连接查询

- sql99 支持更多的语法

- sql99 实现连接条件与筛选条件的分离

- 交集,并集,补集,相对补集与绝对补集

  - 交集:  *A*∩*B*= {*x*|*x*∈*A*∧*x*∈*B*}

    - 集合 {1,2,3} 和 {2,3,4} 的交集为 {2,3}。即{1,2,3}∩{2,3,4}={2,3}。

  - 并集: A∪B={x|x∈A,或x∈B}

    - 集合{1, 2, 3} 和 {2, 3, 4} 的并集是 {1, 2, 3, 4}

  - 补集 

    - 若给定全集U，有A⊆U，则A在U中的相对补集称为A的绝对补集（或简称补集），写作∁UA。

    - ```
      注意：学习补集的概念，首先要理解全集的相对性，补集符号∁UA有三层含义：
      1、A是U的一个子集，即A⊆U；
      2、∁UA表示一个集合，且∁UA⊆U；
      3、∁UA是由U中所有不属于A的元素组成的集合，∁UA与A没有公共元素，U中的元素分布在这两个集合中。
      ```

    - 

  - 相对补集 *B \ A=B - A* = { *x* | *x*∈B，x ∉ A}

    - 即B - A = B - A∩B

    - {1,2,3} - {2,3,4}={1}.

      {2,3,4} - {1,2,3}={4}.

  - 绝对补集

    - **A在B中的相对补集**其实就是**A∩B在B中的绝对补集**

- 查询表A与表B的交集, 使用内连接

- 查询表A左外连接表B, 即查询A + A∩B

- 查询表A右外连接表B, 即查询B + A∩B

- 查询A- A∩B, 即查询表A左外连接表B, 并限定B.id is Null

  - 即查询表A与表B不配的所有表A的项

- 查询 B-A∩B , 即查询表B左外连接表A, 并限定A.id is Null

  - 即查询表B与表A不匹配的所有的B的项



#### 86 案例_多表连接

- 查询编号>3的女神的男朋友信息, 如果有则列出详细信息, 没有则用null填充

  - ```sql
    # 首先获取所有女生的boys表匹配信息, 再设定查询条件
    SELECT girls.`name`,boys.*
    FROM beauty AS girls
    LEFT OUTER JOIN boys
    ON girls.`boyfriend_id`=boys.`id`
    WHERE girls.`id`>3
    ```

- 查询哪个城市没有部门

  - ```sql
    # 城市表L, 部门表D, 求D-A∩D, 城市表为主表
    # 先查询所有城市对应的部门
    SELECT DISTINCT l.`city` AS 城市名,d.`department_name` AS 部门名
    FROM locations AS l
    LEFT OUTER JOIN departments AS d
    ON l.`location_id`=d.`location_id`
    WHERE d.`location_id` IS NULL
    ```

- 查询部门名为SAL或IT的员工信息

  - ```sql
    # 部门表D 员工表E, 先查询所有的部门(主表)与员工信息的对照
    SELECT d.`department_name` AS 部门名,e.`last_name` AS 员工名
    FROM departments AS d
    LEFT OUTER JOIN employees AS e
    ON d.`department_id`=e.`department_id`
    WHERE d.`department_name` IN ('sal','it') AND e.`department_id` IS NOT NULL
    ```

#### 87 子查询介绍

- ```
  出现在其它语句中的select 语句, 称为子查询或内查询
  外部的查询语句, 称为主查询或外查询
  分类
  	按子查询出现的位置
  		select 后面
  			仅仅支持标量子查询
  		from 后面
  			支持表子查询
  		where或having后面
  			标量子查询(单行)
  			列子查询(多行)
  			行子查询
  		exists后面(相关子查询)
  			表子查询
  	按结果集的行列数不同
  		标量子查询(结果集只有一行一列)
  		列子查询(结果集只有一列多行)
  		行子查询(结果集有一行多列)
  		表子查询(结果集一般为多行多列)
  ```

#### 88 where后面的标量子查询使用

- ```
  - where或having后面
    - 标量子查询(单行子查询)
    - 列子查询(多行子查询)
    - 行子查询(多行多列)
  - 子查询都会放在小括号内
  - 子查询一般放在条件的右侧
  - 标量子查询, 一般搭配的单行操作符使用 > < <= = >= <>
  - 列子查询, 一般搭配者多行操作符使用 IN , ANY/SOME, ALL
  ```

- ```
  子查询优先于主查询执行, 主查询的条件用到了子查询的结果
  ```

- 

- 案例: 查询Abel的工资

  - ```sql
    SELECT salary
    FROM employees
    WHERE employees.`last_name`='Abel'
    ```

- 查询员工的信息, 满足 salary> Abel的工资

  - ```sql
    SELECT salary
    FROM employees
    WHERE salary>(
    	SELECT salary
    	FROM employees
    	WHERE last_name='abel'
    )
    ```

- 返回job_id与141号员工相同, salary比143号员工多的员工的姓名, job_id 和工资

  - ```sql
    # 子查询查找141的员工的job_id
    # 子查询查找143号员工的salary
    
    SELECT last_name,job_id, salary
    FROM employees
    WHERE salary>(
    	SELECT salary 
        FROM employees 
        WHERE employee_id=143
    )
    AND
    job_id=(
    	SELECT job_id 
        FROM employees 
        WHERE employee_id=141
    )
    ```

- 返回公司功能工资最少的员工的last_name,job_id和salary

  - ```sql
    # 先查找最低工资
    # 查询字段, 要求salary=最低工资
    SELECT last_name,job_id,salary
    FROM employees
    WHERE salary=(
    	SELECT MIN(salary)
    	FROM employees
    )
    ```

- 查询最低工资大于50号部门的最低工资的部门id和其最低工资

  - ```sql
    # 查询50部门的最低工资
    SELECT MIN(salary)
    FROM employees
    WHERE department_id=50
    # 查询 以departmnet_id为分组的最低工资
    SELECT department_id, MIN(salary) AS 最低工资
    FROM employees
    GROUP BY department_id
    # 查询
    
    SELECT department_id, MIN(salary) AS 最低工资
    FROM employees
    GROUP BY department_id
    HAVING 最低工资>(
    	SELECT MIN(salary)
    	FROM employees
    	WHERE department_id=50
    )
    ```

#### 89 列子查询

- 返回多行, 使用多行比较操作符

- ```
  操作符             含义
  IN/NOT IN         等于列表中的任意一个; IN 与 =ANY 效果相同; not in 与!=All 效果相同
  ANY|SOME		  和子查询返回的某一个值比较; <Any : 小于最大值; >ANy: 大于最小值
  ALL				  和子查询返回的所有值比较; >ALL与大于最大值效果相同
  ```

- 返回location_id是1400或1700的部门中的所有员工姓名

  - ```sql
    # 查询location_id在[1400,1700]中的部门
    SELECT DISTINCT department_id
    FROM departments
    WHERE location_id IN (1400,1700)
    # 根据上面的查询结果查询对应部门id的员工名
    SELECT last_name
    FROM employees
    WHERE department_id IN (
    	SELECT DISTINCT department_id
    	FROM departments
    	WHERE location_id IN (1400,1700)
    )
    ```

- 返回其它部门中比job_id为`IT_PROG`工种任一工资低的员工的员工号, 姓名, job_id以及salary

  - ```sql
    # 首先查询job_id为IT PROG部门的员工工资
    SELECT DISTINCT salary
    FROM employees
    WHERE job_id IN ('IT_PROG')
    # 查询其它部门的所有员工工资
    SELECT employee_id AS 员工号,last_name AS 姓名,job_id AS 部门编号,salary AS 薪水
    FROM employees
    WHERE job_id != 'IT_PROG'
    # 结合上面两个查询
    SELECT employee_id AS 员工号,last_name AS 姓名,job_id AS 部门编号,salary AS 薪水
    FROM employees
    WHERE job_id != 'IT_PROG'
    AND salary < ANY( # 这里可以直接写成 < 最大值
    	SELECT DISTINCT salary
    	FROM employees
    	WHERE job_id IN ('IT_PROG')
    )
    ```

- 返回其它部门中比job_id为`IT_PROG`部门所有工资都低的员工的员工号, 姓名, job_id以及salary

  - ```sql
    SELECT employee_id AS 员工号,last_name AS 姓名,job_id AS 部门编号,salary AS 薪水
    FROM employees
    WHERE job_id != 'IT_PROG'
    AND salary < ALL( # 这里可以直接写成 < 最小值
    	SELECT DISTINCT salary
    	FROM employees
    	WHERE job_id IN ('IT_PROG')
    )
    ```



#### 90 行子查询

- 结果集一行多列或多行多列

- 案例: 查询员工编号最小并且工资最高的员工信息

  - ```SQL
    # 标量子查询
    # 查询最小的员工编号
    SELECT MIN(employee_id)
    FROM employees
    # 查询最高的工资
    SELECT MAX(salary)
    FROM employees
    # 查询员工信息
    SELECT *
    FROM employees
    WHERE employee_id=(
    	SELECT MIN(employee_id)
    	FROM employees
    )
    AND salary=(
    	SELECT MAX(salary)
    	FROM employees
    )
    ```

  - ```sql
    # 行子查询
    SELECT *
    FROM employees
    WHERE (employee_id,salary)=(
    	SELECT MIN(employee_id),MAX(salary)
    	FROM employees
    )
    ```

### 91 select后面的子查询

- 查询每个部门的员工个数

  - ```sql
    # 常规方法
    SELECT department_name,COUNT(*)
    FROM departments AS d
    INNER JOIN employees AS e
    ON d.`department_id`=e.`department_id`
    GROUP BY d.department_id
    ```

  - ```sql
    SELECT d.`department_name`,(
    	SELECT COUNT(*)
    	FROM employees AS e
    	WHERE e.department_id=d.`department_id`
    ) AS 个数
    FROM departments AS d
    ```

- 查询员工号=102的部门名

  - ```sql
    SELECT (
    	SELECT department_name
    	FROM departments AS d
    	INNER JOIN employees e
    	ON e.`department_id`=d.department_id
    	WHERE e.`employee_id`=102
    ) AS 部门名
    ```

- 仅支持标量查询

#### 92 from 后面的子查询使用

- 将子查询结果充当一张表, 必须起别名

- 查询每个部门的平均工资的工资等级

  - ```sql
    # 先求出所有的部门的平均工资
    SELECT AVG(salary),department_id
    FROM employees
    GROUP BY department_id
    # 连接上面的结果集和job_grade ,筛选条件平均工资 between lowest_sal and highest_sal
    select avg_dep.部门名,avg_dep.平均工资, jg.grade_level
    from (
    	SELECT department_id AS 部门名,AVG(salary) AS 平均工资
    	FROM employees
    	GROUP BY 部门名	
    ) as avg_dep
    inner join job_grades as jg
    on avg_dep.平均工资 between jg.lowest_sal and highest_sal
    
    ```



#### 93 exists后面的子查询使用(相关子查询)

- 它就是一个布尔值, 只关心后面的子查询有没有结果, 有结果返回1,没有返回0

- ```
  语法
  exists(完整的查询语句)
  ```

- 

- ```sql
  select exists(
  	select employee_id from employees
  )as 是否存在结果
  # 是否存在结果
  # 1
  ```

- ```sql
  select exists(
  	select employee_id from employees where employee_id=1100
  )as 是否存在结果
  # 是否存在结果
  # 0
  ```

- 查询有员工的部门名

  - ```sql
    # 常规写法
    SELECT DISTINCT department_name
    FROM departments AS d
    INNER JOIN employees AS e
    ON e.`department_id`=d.`department_id`
    WHERE e.`department_id` IS NOT NULL
    ```

  - ```sql
    SELECT department_name
    FROM departments AS d
    WHERE EXISTS(
    	SELECT * 
    	FROM employees AS e 
    	WHERE d.`department_id`=e.`department_id`
    )
    ```

  - ```sql
    #能用exists写的一定能用IN写
    SELECT department_name
    FROM departments d
    WHERE d.`department_id` IN(
    	SELECT department_id FROM employees
    )
    ```

- 查询没有女朋友的男生信息

  - ```sql
    SELECT * 
    FROM boys
    WHERE NOT EXISTS(
    	SELECT *
    	FROM beauty AS girls
    	WHERE boys.`id`=girls.`boyfriend_id`
    )
    ```

  - ```sql
    # in 写
    SELECT *
    FROM boys
    WHERE boys.`id` NOT IN (
    	SELECT girls.`boyfriend_id`
    	FROM beauty AS girls
    )
    ```

### 94 子查询案例

- 查询和zlotkey相同部门的员工姓名和工资

  - ```sql
    SELECT last_name AS 姓名,salary AS 工资, department_id AS 部门编号
    FROM employees
    WHERE department_id=(
    	SELECT department_id 
    	FROM employees
    	WHERE last_name='zlotkey'
    )
    ```

- 查询工资比公司平均工资高的员工的员工号,姓名和工资

  - ```sql
    # 查询公司平均工资
    SELECT AVG(salary)
    FROM employees
    # 
    SELECT employee_id AS 员工号,last_name AS 姓名,salary AS 工资
    FROM employees
    WHERE salary>(
    	SELECT AVG(salary)
    	FROM employees
    )
    ```

- 查询各部门中工资比本部门平均工资高的员工的员工号,姓名和工资

  - ```sql
    # 查询各部门平均工资
    SELECT AVG(salary) AS 部门平均工资, department_id AS 部门编号
    FROM employees
    GROUP BY 部门编号
    HAVING 部门编号 IS NOT NULL
    # 连接 empolyees表与上表
    SELECT 
    employee_id AS 员工编号,
    last_name AS 员工姓名,
    salary AS 工资,
    department_id AS 部门编号,
    avg_sal.部门平均工资
    FROM employees AS e
    INNER JOIN (
    	SELECT AVG(salary) AS 部门平均工资, department_id AS 部门编号
    	FROM employees
    	GROUP BY 部门编号
    	HAVING 部门编号 IS NOT NULL
    ) AS avg_sal
    ON e.`department_id`=avg_sal.部门编号
    WHERE e.salary>avg_sal.部门平均工资
    ```

- 查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名

  - ```sql
    # 查询姓名中包含字母u的员工的部门编号
    SELECT last_name,employee_id,department_id
    FROM employees
    WHERE last_name LIKE '%u%'
    # 将employees表与上表结合,查询
    SELECT employee_id,last_name, e.department_id
    FROM employees AS e
    INNER JOIN (
    	SELECT DISTINCT department_id
    	FROM employees
    	WHERE last_name LIKE '%u%'
    ) AS u_dep
    ON u_dep.department_id=e.`department_id`
    ```

- 查询在部门的location_id为1700的部门工作的员工的员工号

  - ```sql
    # 查询location_id为1700的部门有哪些
    SELECT DISTINCT department_id
    FROM departments AS d
    INNER JOIN locations AS l
    ON d.`location_id`=l.`location_id`
    WHERE l.`location_id`=1700
    # 连接员工表与上表
    select employee_id,last_name
    from employees as e
    inner join (
    	SELECT DISTINCT department_id
    	FROM departments AS d
    	INNER JOIN locations AS l
    	ON d.`location_id`=l.`location_id`
    	WHERE l.`location_id`=1700
    ) as l_1700
    on l_1700.department_id=e.`department_id`
    ```

- 查询管理者是king的员工姓名和工资

  - ```sql
    # 查询king的员工号
    select employee_id
    from employees
    where last_name='K_ing' # 两个
    # 连接员工表与上表
    select last_name,salary,manager_id
    from employees as e
    where e.`manager_id` in (
    	SELECT employee_id
    	FROM employees
    	WHERE last_name='K_ing'
    )
    ```

- 查询工资最高的员工的姓名, 要求first_name和last_name显示为一列,列名为姓,名

  - ```sql
    # 查询最高工资
    select max(salary)
    from employees
    # 查询工资等于上面的结果的员工的姓名
    SELECT first_name, last_name
    FROM employees
    WHERE salary=(
    	SELECT MAX(salary)
    	FROM employees
    )
    ```




#### 95 分页查询

- ```
  应用场景
  数据特别多的时候需要分批次显示
  语法: 
  	select 查询列表
  	from 表
  	[join type] join 表
  	on 连接条件
  	where 筛选条件
  	group by 分组字段
  	having 分组后的筛选
  	order by 排序的字段
  	limit offset,size 
  
  	// offset: 要显示的条目的起始索引, (起始索引从0开始)
  	// size: 要显示的条目个数
  	// limit语句必须在查询语句的最后
  	// 公式:
  		要显示的页数page,每页的条目数size
  		
  		select 查询列表
  		from 表
  		limit ((page-1)*size),size
  ```

- 案例一: 查询前五条员工信息

  - ```sql
    SELECT *
    FROM employees
    LIMIT 0,5 # 从0开始, 显示5条, 当从第一条开始时, 0可以省略
    ```

- 查询第11条到第25条

  - ```sql
    SELECT *
    FROM employees
    LIMIT 10,15
    ```

- 查询有奖金的员工信息, 并且工资较高的前十名

  - ```sql
    SELECT *
    FROM employees
    WHERE commission_pct IS NOT NULL
    ORDER BY salary DESC
    LIMIT 10
    ```



#### 96 测试题3讲解

- 已知表 stuinfo, id学号,name姓名,email邮箱,gradeId年级编号,sex性别(男,女),age年龄

- 已知表 grade, id年级编号, gradeName年级名称

- 查询所有学员的邮箱的用户名, (注: 邮箱中@前面的字符)

  - ```sql
    # 查询所有学院的邮箱名
    select substr(email,1,instr(email,'@')-1) from stuinfo
    ```

    - instr: `SELECT INSTR('hello@163.com','@')` 6

- 查询男生和女生的个数

  - ```sql
    select sex,count(*)
    from stuinfo
    group by sex
    ```

- 查询年龄大于18岁的所有学生的姓名和年级名称

  - ```sql
    select name,gradeName
    from stuinfo
    inner join grade
    on stuinfo.gradeId=grade.id
    where age>18
    ```

- 查询哪个年级的学生的最小年龄>20岁

  - ```sql
    # 查询每个年级的学生的最小年龄
    select min(age),gradeId
    from stuinfo
    group by gradeId
    having min(age)>20
    ```

- 试说出查询语句中涉及到的所有关键字, 以及执行先后顺序

  - ```sql
    select 查询列表
    from 表
    连接类型 join 表2
    on 连接条件
    group by 分组列表
    having 分组后的筛选
    order by 排序列表
    limit 偏移, 条目数
    ```

  - ```
    执行顺序
    1. 先执行 from, 引入数据源
    2. 再执行join, 引入第二个数据源,两个数据源生成一个笛卡尔乘积表(大数据源)
    3. 执行on, 根据连接的条件生成新的表
    4. 执行where , 再筛选生成新的表
    5. group by, 生成分组后的新表
    6. having, 对新表再次筛选
    7. select, 根据选择的字段生成新的表
    8. order by 对上面的表排序
    9. limit 对上面的表进行限制条目输出
    ```

#### 97 复习

#### 98 子查询经典案例

- 查询工资最低的员工信息: last_name,salary

  - ```sql
    # 先查询最低工资
    SELECT MIN(salary)
    FROM employees
    # 
    SELECT last_name,salary
    FROM employees
    WHERE salary=(
    	SELECT MIN(salary)
    	FROM employees
    )
    ```

- 查询平均工资最低的部门信息

  - ```sql
    # 查询各部门平均工资
    SELECT AVG(salary),department_id
    FROM employees
    GROUP BY department_id
    ```

  - ```sql
    #查询上面表的最低工资
    SELECT MIN(avg_sal)
    FROM (
    	SELECT AVG(salary) AS avg_sal,department_id
    	FROM employees
    	GROUP BY department_id
    ) AS avgTab
    ```

  - ```sql
    #查询上面结果对应的部门id
    SELECT department_id
    FROM employees
    GROUP BY department_id
    HAVING AVG(salary)=(
    	SELECT MIN(avg_sal)
    	FROM (
    		SELECT AVG(salary) AS avg_sal,department_id
    		FROM employees
    		GROUP BY department_id
    	) AS avgTab
    )
    ```

  - ```sql
    # 查询部门的信息
    SELECT * 
    FROM departments
    WHERE department_id=(
    	SELECT department_id
    	FROM employees
    	GROUP BY department_id
    	HAVING AVG(salary)=(
    		SELECT MIN(avg_sal)
    		FROM (
    			SELECT AVG(salary) AS avg_sal,department_id
    			FROM employees
    			GROUP BY department_id
    		) AS avgTab
    	)
    )
    ```

  - 方式二

    - ```sql
      # 1.求出各部门的平均工资
      SELECT AVG(salary),department_id
      FROM employees
      GROUP BY department_id
      
      # 2.求出最低平均工资的部门编号
      SELECT department_id
      FROM employees
      GROUP BY department_id
      ORDER BY AVG(salary)
      LIMIT 0,1
      
      # 3.查询部门信息
      SELECT * 
      FROM departments
      WHERE department_id=(
      	SELECT department_id
      	FROM employees
      	GROUP BY department_id
      	ORDER BY AVG(salary)
      	LIMIT 1,1
      )
      ```

- 查询平均工资最低的部门信息和该部门的平均工资

  - ```sql
    # 查询所有部门的平均工资
    SELECT AVG(salary) AS 平均工资,department_id
    FROM employees
    GROUP BY department_id
    ORDER BY AVG(salary)
    LIMIT 0,1
    
    # 根据上表联合departments表查询部门信息和平均工资
    SELECT d.*,avg_dep.平均工资
    FROM departments AS d
    INNER JOIN (
    	SELECT AVG(salary) AS 平均工资,department_id
    	FROM employees
    	GROUP BY department_id
    	ORDER BY AVG(salary)
    	LIMIT 0,1
    ) AS avg_dep
    ON d.department_id=avg_dep.department_id
    ```

- 查询平均工资最高的job信息

  - ```sql
    # 查询最高的平均工资的job_id
    SELECT job_id
    FROM employees
    GROUP BY job_id
    ORDER BY AVG(salary) DESC
    LIMIT 0,1
    
    # 根据上面的结果查询到job信息
    SELECT jobs.* 
    FROM jobs
    WHERE job_id=(
    	SELECT job_id
    	FROM employees
    	GROUP BY job_id
    	ORDER BY AVG(salary) DESC
    	LIMIT 0,1
    )
    ```

- 查询平均工资高于公司平均工资的部门有哪些

  - ```sql
    SELECT d.*
    FROM departments AS d
    WHERE d.`department_id` IN (
    	SELECT department_id
    	FROM employees
    	GROUP BY department_id
    	HAVING AVG(salary)>(
    		SELECT AVG(salary) AS 公司平均工资
    		FROM employees
    	)
    	AND department_id IS NOT NULL
    )
    ```

- 查询出公司中所有manager的详细信息

  - ```sql
    # 查询所有manager的id
    SELECT DISTINCT manager_id
    FROM employees
    WHERE manager_id IS NOT NULL
    
    # 查询所有manager的信息
    SELECT * 
    FROM employees
    WHERE employee_id IN (
    	SELECT DISTINCT manager_id
    	FROM employees
    	WHERE manager_id IS NOT NULL
    )
    ```

- 各个部门中最高工资中最低的那个部门的最低工资是多少

  - ```sql
    # 查询最低部门工资的部门id
    SELECT department_id
    FROM employees
    GROUP BY department_id
    ORDER BY AVG(salary)
    LIMIT 0,1
    
    # 查询这个部门中的最低员工工资
    SELECT MIN(salary)
    FROM employees
    WHERE department_id=(
    	SELECT department_id
    	FROM employees
    	GROUP BY department_id
    	ORDER BY AVG(salary)
    	LIMIT 0,1
    )
    ```

  - ```sql
    # 查询各部门中的单个员工工资最高的部门id
    SELECT department_id
    FROM employees
    GROUP BY department_id
    ORDER BY MAX(salary) DESC
    LIMIT 0,1
    
    # 根据这个id, 查找这个部门下的员工的最低工资
    SELECT MIN(salary)
    FROM employees
    WHERE department_id=(
    	SELECT department_id
    	FROM employees
    	GROUP BY department_id
    	ORDER BY MAX(salary) DESC
    	LIMIT 0,1
    )
    ```

  - 

- 查平均工资最高的部门的manager的详细信息

  - ```sql
    # 查询部门平均工资最高的部门id
    SELECT department_id
    FROM employees
    GROUP BY department_id
    ORDER BY AVG(salary) DESC
    LIMIT 0,1
    
    # 根据部门id查找部门管理者的id
    SELECT employee_id
    FROM employees
    WHERE department_id=(
    	SELECT department_id
    	FROM employees
    	GROUP BY department_id
    	ORDER BY AVG(salary) DESC
    	LIMIT 0,1
    )
    AND
    manager_id IS NULL
    
    # 根据id得到全部信息
    SELECT * 
    FROM employees
    WHERE employee_id=(
    	SELECT employee_id
    	FROM employees
    	WHERE department_id=(
    		SELECT department_id
    		FROM employees
    		GROUP BY department_id
    		ORDER BY AVG(salary) DESC
    		LIMIT 0,1
    	)
    	AND
    	manager_id IS NULL
    )
    ```

#### 99 作业讲解

- 查询每个专业的学生人数

  - ```sql
    SELECT m.`majorname` AS 专业,COUNT(*) AS 专业人数
    FROM student AS s
    INNER JOIN major AS m
    ON s.`majorid`=m.`majorid`
    GROUP BY s.majorid
    ```

- 查询参加考试的学生中, 每个学生的平均分, 最高分

  - ```sql
    SELECT AVG(score) AS 平均分, MAX(score) AS 最高分,studentno
    FROM result
    GROUP BY studentno
    # 有些学生有多次考试成绩
    ```

- 查询姓张的每个学生的最低分大于60的学号\姓名

  - ```sql
    # 查询姓张的学生id有哪些
    SELECT studentno
    FROM student
    WHERE studentname LIKE "张%"
    
    # 查询姓张的,且最低分大于60的学生的学号, 并连接studnent表获得姓名
    SELECT r.studentno,s.`studentname`,MIN(score)
    FROM result AS r
    INNER JOIN (
    	SELECT studentno
    	FROM student
    	WHERE studentname LIKE "张%"
    ) AS z
    ON r.`studentno`=z.`studentno`
    INNER JOIN student AS s
    ON r.`studentno`=s.`studentno`
    GROUP BY studentno
    HAVING MIN(score)>60
    ```

- 查询每个专业生日在"1988-1-1"后的学生姓名\专业名称

  - ```sql
    SELECT studentname,majorname,borndate
    FROM student AS s
    INNER JOIN major AS m
    ON s.`majorid`=m.`majorid`
    WHERE `borndate`<'1988-1-1'
    ```

- 查询每个专业的男性人数和女性人数分别是多少

  - ```sql
    # 查找男性人数
    SELECT COUNT(*) AS 男性人数
    FROM student
    WHERE sex='男'
    
    # 查找女性人数
    SELECT COUNT(*) AS 女性人数
    FROM student
    WHERE s1.sex='女'
    
    # 结合两表
    SELECT t1.num AS 男性人数, t2.num AS 女性人数
    FROM (
    	SELECT COUNT(*) AS num
    	FROM student
    	WHERE sex='男'
    ) AS t1
    INNER JOIN (
    	SELECT COUNT(*) AS num
    	FROM student
    	WHERE sex='女'
    ) AS t2
    ```

- 查询专业和张翠山一样的学生的最低分

  - ```sql
    # 查询张翠山的专业id
    SELECT majorid
    FROM student
    WHERE studentname='张翠山'
    
    # 查询专业和张翠山一样的学生id
    SELECT studentno
    FROM student
    WHERE majorid=(
    	SELECT majorid
    	FROM student
    	WHERE studentname='张翠山'
    )
    
    # 查询这些学生的最低分
    SELECT MIN(score)
    FROM result
    WHERE studentno IN (
    	SELECT studentno
    	FROM student
    	WHERE majorid=(
    		SELECT majorid
    		FROM student
    		WHERE studentname='张翠山'
    	)
    )
    ```

- 查询大于60分的学生的姓名, 密码, 专业名

  - ```sql
    SELECT studentname,loginpwd,majorname
    FROM student
    INNER JOIN major
    ON student.`majorid`=major.`majorid`
    WHERE student.`studentno` IN (
    	SELECT DISTINCT studentno
    	FROM result
    	WHERE score>60
    )
    ```

- 按邮箱位数分组, 查询每组的学生个数

  - ```sql
    SELECT COUNT(*),LENGTH(email) AS el
    FROM student
    GROUP BY LENGTH(email)
    HAVING el IS NOT NULL
    ```
  
- 查询学生名 专业名 分数

  - ```sql
    SELECT studentname,majorname,score
    FROM student
    JOIN major
    ON student.`majorid`=major.`majorid`
    JOIN result
    ON student.`studentno`=result.`studentno`
    ```

- 查询哪个专业没有学生,分别用左连接和右连接实现

  - ```sql
    SELECT m.*,s.*
    FROM major AS m
    LEFT OUTER JOIN student AS s
    ON m.`majorid`=s.`majorid`
    WHERE s.`studentno` IS NULL
    # 全部专业都有学生
    ```

  - ```sql
    SELECT m.*,s.*
    FROM student AS s
    RIGHT OUTER JOIN major AS m
    ON m.`majorid`=s.`majorid`
    WHERE s.`studentno` IS NULL
    ```

- 查询没有成绩的学生人数

  - ```sql
    select count(*)
    from student as s
    left join result as r
    on s.`studentno`=r.`studentno`
    where r.`score` is null
    ```




#### 100 联合查询

- ```sql
  # union 联合: 将多条查询语句的结果合并成一个结果
  
  # 案例: 查询部门编号大于90或邮箱中包含a的员工信息
  SELECT *
  FROM employees
  WHERE email LIKE '%a%'
  OR department_id>90
  
  # 使用union重写
  SELECT * FROM employees
  WHERE email LIKE '%a%'
  UNION
  SELECT * FROM employees
  WHERE department_id>90
  
  # 语法
  查询语句1
  union
  查询语句2
  union
  ...
  # 应用场景
  /*
  要查询的结果来自于多个表, 但多个表没有直接的连接关系,但是每个表的字段含义(字段名不一定一致)是一致
  */
  ```



#### 101 联合查询的特点

- ```sql
  /*
  	1.要求多条查询语句的查询列数是一致的
  	2.要求多条查询语句的查询每一列的类型和顺序最好一致
  	3.union关键字默认去重, 如果使用union all 可以包含重复项
  */
  ```

## 102 插入语句的方式1

- DML语言(数据操作语言)

  - 插入(insert)
  - 修改(update)
  - 删除(delete)

- 语法

  - ```sql
    insert into 表名 (列名,...)
    values (值1,...)
    ```

- 案例1: 插入到beauty表名

  - 插入的值的类型要与列的类型一致或兼容

    - ```sql
      insert into beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
      values(14,'xxx','女','1990-4-23','18877776666',NULL,2)
      ```

    - int是整数类型; varchar(10)是字符型,要求用单引号引起来; char(1)要求用单引号引起; datetime类型要求用单引号引起, 并且是合法的日期; photo是Nullable, 允许使用null

- 可以为null的列是如何插入值的?

  - 不可以为null的列必须插入值, 

  - 可以为null的列可以插入Null,也可以什么都不写.

    - 没有插入的值的字段(如果是nullable字段)会被自动插入NULL( 前提是insert into 中没有这个字段名 )

  - ```sql
    insert into beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
    values(14,'xxx','女','1990-4-23','18877776666',NULL,2)
    ```

  - ```sql
    insert into beauty(id,name,sex,borndate,phone,boyfriend_id)
    values(14,'xxx','女','1990-4-23','18877776666',2)
    ```

- 插入数据的顺序是可以调整的, 但必须保证字段与值一一对应

- insert into写了字段名, 但是values不给予值会报错. 字段与值必须一一对应

- 可以省略列名, 默认所有列, 而且列的顺序和表中的列的顺序是一致的

  - ```sql
    insert into beauty
    values(18,'zzz','男',Null,'119',Null,null)
    # 这种情况下不可以省略null
    ```

### 增删改

#### 103 插入语句的方式2

- ```sql
  insert into 表名
  set 列名=值,列名=值...
  ```

  - ```sql
    insert into beauty
    set id=99,name='ff',phone='5456'
    ```

#### 104 两种方式的比较

- 方式1支持插入多行

  - ```sql
    insert into 表名
    values(...)
    values(...)
    values(...)
    ```

- 方式2不支持插入多行

- 方式1支持子查询, 方式2不支持

  - ```sql
    insert into table1(id,name,phone)
    select id,name,phone # 必须与插入的列字段一致
    from table2
    # 将从table2中查询到的结果添加到table1中
    ```

  - 

#### 105 修改单表的记录

- ```
  修改单表的记录
  语法
  	update 表名
  	set 字段=值,字段=值,字段=值...
  	where 筛选条件
  ```

- 案例一: 修改beauty表中姓唐的女生的电话为13366667777

  - ```sql
    update beauty set phone='13366667777'
    where name like '唐%'
    ```

  - 记得要刷新才能看到结果

- 案例二: 修改boys表中id为2的名称为张飞, 魅力值为10

  - ```sql
    update boys 
    set boyname='张飞',
    	userrcp=10
    where id=2
    ```

#### 106 修改多表的记录

- sql92语法

  - ```sql
    update 表1 别名,表2 别名
    set 字段=值,...
    where 连接条件
    and 筛选条件
    ```

- sql99语法

  - ```sql
    update 表1 别名
    inner|left|right join 表2 别名
    on 连接条件
    set 字段=值,...
    where 筛选条件
    ```

- 案例1: 修改张无忌的女友的手机号为114

  - ```sql
    update boys bo
    inner join beauty b
    on bo.id=b.boyfriend_id # 先把两个表连接上
    set b.phone=114
    where bo.name='张无忌'
    ```

  - ```sql
    UPDATE beauty AS girls
    INNER JOIN boys
    ON girls.`boyfriend_id`=boys.`id` # 连接两表没有任何顺序要求
    SET girls.`phone`=114 # 只有这里需要指定表单名和字段名和值, 这里才是更新表单的位置
    WHERE boys.`boyName`='张无忌'
    ```

- 案例2: 修改没有男朋友的女生的男朋友编号为2

  - ```sql
    # 首先确定主表为girls表, girls表的信息必须全部存在, boys表与之不匹配的丢弃, 没有匹配的以null值填充
    UPDATE beauty AS girls
    LEFT JOIN boys
    ON girls.`boyfriend_id`=boys.`id`
    SET girls.`boyfriend_id`=2
    WHERE boys.`id` IS NULL
    ```

#### 107 删除语句的介绍

- ```sql
  /*
  	方式一:delete
  	语法: 
  		1. 单表的删除
  		delete from 表名 where 筛选条件
  		需要注意的是删除一定是删除整行.
  		
  		2.多表的删除
  		sql92语法
  			delete 表1别名 # 代表删除表1的记录
  			from 表1 别名,表2 别名
  			where 连接条件
  			and 筛选条件
  			
  		sql99语法
  			delete 表1别名,表2别名
  			from 表1 别名
  			inner|left|right join 表2 别名
  			on 连接条件
  			where 筛选条件
  	方式二: truncate
  	语法: truncate table 表名
  */
  ```

- 方式一单表删除案例:

  - 删除beauty表手机号以9结尾的女生信息

    - ```sql
      delete from beauty where phone like '%9'
      ```

- 方式一多表删除案例:

  - 删除张无忌的女朋友的信息

    - ```sql
      DELETE girls # 删除的是girls表的数据
      FROM beauty AS girls
      INNER JOIN boys # 这两行用于进行内连接, 不分顺序
      ON boys.`id`=girls.`boyfriend_id` # 连接条件
      WHERE boys.`boyName`='张无忌' # 删除的行的筛选条件
      ```

  - 删除男生id为3的信息, 以及他女友的信息

    - ```sql
      DELETE boys,girls
      FROM boys
      INNER JOIN beauty AS girls
      ON boys.`id`=girls.`boyfriend_id`
      WHERE boys.`id`=3
      ```

#### 109 删除方式二

- truncate语句

- 案例: 将魅力值>100的男生信息删除

  - ```sql
    truncate table boys where usercp>100
    # 执行报错, truncate语句里不允许加where
    ```

- truncate语句被称作"清空"

  - ```
    语法:
    truncate table 表名
    // 用于直接清空表的所有数据
    ```

- delete 可以加where条件, truncate不可以加条件; 

- 用于删除表单的情况下, truncate的效率比delete高. 

- 假如要删除的表中有自增长列, 如果用delete删除后, 再插入数据, 自增长列的值从断点开始, 而truncate删除后, 再插入数据, 自增长列的值从1开始

  - 什么是自增长列: 一般id会被设置为自增长列, 如果id字段被设置为自增长列, 那么插入数据的时候, 如果没有指定id值, 会自动在原先已存在的id值上+1; 如果插入id值则以插入的id值为准
  - 执行`DELETE FROM boys`删除全部boys表数据, 然后再插入一条数据, 那么id值会在原来的最后一行的id值加1

- truncate删除没有返回值, delete删除有返回值

- truncate删除不能回滚, delete删除可以回滚

#### 110 [案例讲解]数据的增删改

- 

#### 111 DDL语言的介绍

- 库的管理
  - 创建,修改,删除
- 表的管理
  - 创建,修改,删除
- 创建 create
- 修改 alter
- 删除 drop

#### 112 库的管理

- ```
  语法
  	create database [if not exists] 库名
  ```

- 案例: 创建book库

  - ```sql
    CREATE DATABASE if not exists books
    ```

  - 在`C:\ProgramData\MySQL\MySQL Server 8.0\Data`中查看数据库的文件夹

- 库的修改

  - 库基本不做修改

  - 可以更改的只有库的字符集

    - ```sql
      ALTER DATABASE books CHARACTER SET gbk
      ```

- 库的删除

  - ```sql
    DROP DATABASE IF EXISTS books
    ```



#### 113 表的创建

- ```sql
  语法:
  	create table 表名(
      	字段名 字段类型[(长度),约束],
      	字段名 字段类型[(长度),约束],
      	字段名 字段类型[(长度),约束],
          ...
      	字段名 字段类型[(长度),约束]
      )
  ```

- 案例: 创建表book

  - ```sql
    create table if not exists book(
    	id INT, # 编号
        bName VARCHAR(20), #图书名
        authorId INT,# 作者编号
        publishDate DATETIME #出版日期
    )
    ```

  - 如果没有加任何约束, 那么每个字段都可以为空, 默认值也为null

- 案例: 创建表author

  - ```sql
    CREATE TABLE author(
    	id INT,
    	au_name VARCHAR(20),
    	nation VARCHAR(10)
    )
    ```

#### 114 表的修改

- ```
  修改列名
  修改列的类型或约束
  添加列
  删除列
  修改表名
  ```

- 修改列名: 将book表的publishdate字段名修改为pubDate

  - ```sql
    ALTER TABLE book CHANGE COLUMN publishdate pubDate DATETIME
    ```

- 修改列的类型或约束: 将pubdate的类型修改为timestamp

  - ```sql
    ALTER TABLE book MODIFY COLUMN pubdate TIMESTAMP
    ```

- 添加新列

  - ```sql
    ALTER TABLE author ADD COLUMN annual DOUBLE
    ```

- 删除新列

  - ```sql
    ALTER TABLE author DROP COLUMN annual
    ```

- 修改表名

  - ```sql
    ALTER TABLE author RENAME TO book_author
    ```

- 语法总结

  - ```sql
    alter table 表名 add|drop|modify|cahnge column 列名 [类型 约束]
    ```

#### 115 表的删除

- ```sql
  drop table if exists book_author
  ```

- 通用写法

  - ```sql
    drop database if exists 旧库名
    create database 新库名
    ```

  - ```sql
    drop table if exists 旧表名
    create database 新表名
    ```

#### 116 表的复制

- 仅仅复制表的结构

  - ```sql
    create table copy like author # 创建copy表, 复制author表的结构
    ```

- 复制表的结构和数据

  - ```sql
    create table copy2
    select * from author # 不仅复制author表的结构, 也同样复制author表的数据
    ```

- 复制表的部分数据

  - ```sql
    create table copy3
    select id,au_name
    from author 
    where nation='中国' # 就是将子查询结果的数据和结构进行复制
    ```

- 复制表的部分结构

  - ```sql
    create table copy4
    select id,au_name
    from author
    where 0 # 即数据全部不满足条件, 只复制结构
    ```

#### 117 库和表的管理案例讲解

- 创建表deptl

  - ```
    NAME  NULL?  TYPE
    id           INT(7)
    NAME         VARCHAR(25)
    ```

  - ```sql
    use test
    create table deptl(
    	id INT(7),
        NAME VARCHAR(25)
    )
    ```

- 将表departnemnts中的数据插入到新表dept2中

  - ```sql
    create table dept2
    select department_id,department_name
    from myemployees.deaprtments
    # 这是一个跨库的表复制, 在test库里创建表dept2, 然后从myemployees库的departments表复制数据到dept2表
    ```

- 创建一个表emp5

  - ```
    NAME         NULL?   TYPE
    id                   INT(7)
    First_name           VARCHAR(25)
    Last_name            VARCHAR(25)
    Dept_id              INT(7)
    ```

  - ```sql
    create table emp5(
    	id INT(7),
        first_name VARCHAR(25),
        last_name VARCHAR(25),
        dept_id INT(7)
    )
    ```

- 将last_name的长度增加到50

  - ```sql
    ALTER TABLE emp5 MODIFY last_name VARCHAR(50)
    ```

- 根据employees创建employees2表

  - ```sql
    ALTER TABLE employees2 LIKE myemployees.`employees`
    ```

- 删除表emp5

  - ```sql
    drop table if exists emp5
    ```

- 将表employees2重命名为emp5

  - ```sql
    alter table employees2 rename to emp5
    ```

- 在表dept和emp5中添加新列test_column, 并检查所做的操作

  - ```sql
    alter table emp5 add column test_column int
    desc emp5
    ```

- 直接删除表emp5中的列dept_id

  - ```sql
    alter table emp5 drop column dept_id
    ```



#### 118,119 数据类型介绍

- ```
  数值型: 
  	整型
  	小数: 
  		定点数:
  		浮点数
  字符型
  	较短的文本: char, varchar
  	较长的文本: text, blob(较长的二进制数据)
  日期型
  ```



#### 119 整型

- ```
  整型
  	tinyint:1字节. 有符号:-128~127; 无符号: 0~255;
  	smallint:2字节
  	mediumint: 3字节
  	int\integer:4字节
  	bigint:8字节
  ```

- 如何设置无符号和有符号

  - ```sql
    drop table if exists tab_int
    create table tab_int(
    	t1 int, # 有符号数据
        t2 int unsigned # 无符号数据
    )
    # 然后插入一个值-123456
    insert into tab_int values(-123456) # 不报错, 插入成功
    insert into tab_int values(-123456,-123456) # 报错
    select * from tab_int # 第二个值插入失败,无符号的最小值是0, -123456超过了下限, 显示临界值0
    ```

  - 如果不设置无符号还是有符号, 默认有符号; 如果想设置无符号, 需要设置unsigned

  - 如果插入值超出了整型的范围, 会报`out of range`异常, 并将值设置为临界值

  - 如果不设置长度, 会有默认的长度(desc tab_int 查看)

    - 无符号默认长度10
    - 有符号默认长度11
    - 有时会看到 `INT(7)`这种, 它不代表限制输入的整型数值长度, 它的作用是如果设置`INT(7) zerofill`时, 如果输入数据长度不满足7位, 则用0填充至7位, 如果长度超过7位, 则无视. 真正决定可输入字符长度的是`INT`,`Tinyint`等
      - 添加zerofill 默认该字段的值会是无符号整数

  
