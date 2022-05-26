[Toc]

# 1 基本命令与语法

## 1.1 基本命令

- 显示所有数据库 `show databases;`
- 进入指定数据库 `use [指定的库名];`
- 显示当前库中的所有表 `show tables`
- 显示指定数据库中的所有表单 `SHOW TABLES FROM [指定的数据库名] `
- 显示当前正在操作的数据库 `SELECT DATABASE();`
- 显示指定表单的描述 `desc [表名];`
- 查看当前mysql服务器的版本 `select version();`

## 1.2 SQL语言细分介绍

- DQL :DATA QUERY LANGUAGE  ( 针对查询 )
- DML: DATA MANIPULATION LANGUAGE ( 增删改 )

# 2 DQL语法(数据查询语言)

- 基本查询语法

  - ```sql
    select 字段名1,字段名2 from 表名;
    ```

- 别名

  - ```sql
    select last_name from employees as '姓氏';
    ```

- 去重

  - ```sql
    SELECT DISTINCT department_id FROM employees;
    ```

- +号的作用

  - ```sql
    select 100+90  		# 190
    select '100'+90 	# 190
    select 'a'+90 		# 90
    SELECT '100a'+90	# 190
    SELECT 'a100'+90	# 90
    select null+90 		# null
    ```

  - ```
    数值+数值=数值
    数值+数值开头的字符串=数值+开头的数值
    数值+非数值开头的字符串=数值+0
    数值+null=null
    ```

## 2.2 条件查询

### 2.2.1 基本查询

- ```sql
  select 查询列表 from 表名 where 筛选条件
  ```

### 2.2.2 按条件表达式查询

- 条件表达式符号
  - 大于(>), 小于(<), 等于(=或<=>),不等于(!=或<>)

#### 2.2.2.1 案例

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

### 2.2.3 按逻辑表达式查询

- 逻辑表达式符号
  - 与(&&), 或(||), 非(!)

#### 2.2.3.1 案例

- 查询工资在10000-20000之间的员工名, 工资以及奖金

  - ```sql
    SELECT last_name,salary,IFNULL(commission_pct,0) FROM employees WHERE salary>10000&&salary<20000;
    ```

- 查询部门编号不是在90-110之间, 或者工资高于15000的员工的全部信息

  - ```sql
    SELECT * FROM employees WHERE !(department_id>=90&&department_id<=110)||salary>15000;
    ```

### 2.2.4 模糊查询

- 模糊查询关键字
  - like , between , and , in , is null , is not null

#### 2.2.4.1 模糊查询案例

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

#### 2.2.4.2 安全等于符号<=>

- <=> 可以判断null值

  - ```sql
    select last_name,commission_pct from employees where commission_pct<=>null;
    ```

- is NULL : 仅仅可以判断NULL值

- <=> : 既可以判断NULL值, 也可以判断其它数值, 但可读性较低

## 2.3 排序查询

### 2.3.1 基本语法

- ```sql
  select 查询列表 from 表 where 筛选条件 order by 排序列表 [asc|desc]
  ```

- order by 可以支持单个字段, 多个字段, 表达式, 函数, 别名

- order  by 子句一般是放在查询语句的最后面, 只有limit 子句可以放在它后面

### 2.3.2 排序查询案例

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

## 2.4 常见函数

### 2.4.1 函数分类

- 单行函数( concat, length, ifNull )
- 分组函数

### 2.4.2 单行函数_字符函数

#### 2.4.2.1 length()

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

#### 2.4.2.2 concat()

- 用于连接字符

- concat(str1,str2)

  - ```sql
    select concat(last_name,'_',first_name) as 姓名 from employees 
    ```

#### 2.4.2.3 upper()/lower()

- 用于将字符修改为大小写

- upper/lower

  - ```sql
    SELECT UPPER(last_name) FROM employees
    ```

  - 将姓变大写, 名变小写, 然后拼接

    - ```sql
      SELECT CONCAT(LOWER(first_name),'_',UPPER(last_name)) AS 姓名
      FROM employees
      ```

#### 2.4.2.4 substr()/substring()

- 用于截取字符串

- ```sql
  select substr('好好学习,天天向上',6) as out_put // 天天向上
  ```

  - sql语句里substr的所有序号都是从1开始, 天 字的序号是6, 截取了从第6位到最后一位的字符

- ```sql
  SELECT SUBSTR('好好学习,天天向上',2,5) AS out_put // 好学习,天
  ```

  - 第一个参数是截取初始序号, 第二个字符是截取字符长度

- substring与substr的功能一样

##### 2.4.2.4.1 案例

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

#### 2.4.2.5 instr()

- instr() 返回第二个参数在第一个参数第一次出现的索引, 如果找不到就返回0

  - ```sql
    SELECT INSTR('明月出天山','天山') AS out_put // 4
    ```

#### 2.4.2.6 trim()

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

#### 2.4.2.7 lpad()

- lpad (类似于js的padStart) 如果字符串长度不够设定的长度, 那么使用第三个参数在原字符前面进行填充

  - ```sql
    SELECT LPAD('hello',10,'=') AS out_put # =====hello
    ```

#### 2.4.2.8 replace

- ```sql
  select replace('hello world','l','b') as out_put // hebbo worbd
  ```

### 2.4.3 单行函数_数学函数

#### 2.4.3.1 round()

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

#### 2.4.3.2 ceil()

- ceil 向上取整

  - ```sql
    SELECT CEIL(1.27) // 2
    ```

  - ```sql
    SELECT CEIL(-1.27) // -1
    ```

#### 2.4.3.3 floor()

- floor 向下取整

  - ```sql
    SELECT FLOOR(1.68) AS out_put // 1
    ```

  - ```sql
    SELECT FLOOR(-1.68) AS out_put // -2
    ```



#### 2.4.3.4 truncate()

- truncate 截断 

  - ```sql
    select truncate(1.6999,1) as out_put // 1.6
    ```

  - ```sql
    SELECT TRUNCATE(5,1) AS out_put // 5
    ```



#### 2.4.3.5 mod

- - mod 取余

    - ```sql
      select mod(10,3) as out_put // 1
      ```



### 2.4.4 单行函数_日期函数

#### 2.4.4.1 now()

- now 返回当前系统日期+时间

  - ```sql
    SELECT NOW() AS out_put // 2022-5-13 16:05:42
    ```

#### 2.4.4.2 curdate()

- curdate 返回当前系统该日期, 不包含时间

  - ```sql
    SELECT CURDATE() AS out_put // 2022-05-13
    ```

#### 2.4.4.3 curTime()

- curTime 返回当前时间, 不包含日期

  - ```sql
    select curtime() as out_put // 16:08:13
    ```

#### 2.4.4.4 year()/month()/day()/hour()/minute()/second()

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

#### 2.4.4.5 str_to_date()

- 将日期格式的字符转换成日期

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
    - 用处不大, 几乎不会使用SQL语言来修改查询顺序

#### 2.4.4.6 date_formate()

- 将日期转换成字符

- ```sql
  select date_format('2018/6/6','%Y年%m月%d日') as out_put
  // 2018年6月6日
  ```

- ```sql
  select date_format(now(),'%y年%m月%d日') as out_put
  // 22年05月13日
  ```

### 2.4.5 其它函数

- ```sql
  SELECT VERSION();
  SELECT DATEBASE();
  SELECT USER()
  ```

### 2.4.6 流程控制函数 

#### 2.4.6.1 if函数

- if函数

  - ```sql
    select if(10>5,'大','小') as out_put // 大
    ```

  - ```sql
    SELECT last_name ,IF(commission_pct IS NULL,'没奖金,呵呵','有奖金,嘻嘻') AS 备注
    FROM employees
    ```

#### 2.4.6.2 case函数

- switch case的效果

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

### 2.4.7 单行函数练习题

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

  - ```sql
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

### 2.4.8 分组函数

- sum, avg一般用于处理数值型. 
- max,min,count用于处理任何类型
- 所有的分组函数都忽略null值
- 分组函数使用注意事项
  - 和分组函数一同查询的字段要求是group by后的字段

#### 2.4.8.1 分组函数_sum()

- ```sql
  select sum(salary) as 'sum' from employees
  // 691400.00 直接求出salary字段整列的值的和
  ```

- 可以和distinct搭配实现去重运算

  - ```sql
    select sum(distinct salary),sum(salary) from employees
    ```

#### 2.4.8.2 分组函数_avg()

- ```sql
  SELECT AVG(salary) AS 'avg' FROM employees
  // 求出整个字段的平均值
  ```

#### 2.4.8.3 分组函数_max()/min()

- ```sql
  SELECT MIN(salary) AS 'min' FROM employees
  // 求出整个字段的最小值
  ```

- ```sql
  select max(salary) as 'max' from employees
  // 求出整个字段的最大值
  ```

#### 2.4.8.4 分组函数_count()

- ```sql
  SELECT COUNT(salary) AS 'count' FROM employees
  // 求出整个字段的值有多少
  ```

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

#### 2.4.8.5 分组函数练习题

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

## 2.5 分组查询

### 2.5.1 group by 子句语法

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

### 2.5.2 添加分组前筛选

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

### 2.5.3 添加分组后的筛选_having

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

### 2.5.4 分组筛选的总结

分组查询的筛选条件分为两类

- |            | 数据源         | 位置                | 关键字 |
  | ---------- | -------------- | ------------------- | ------ |
  | 分组前筛选 | 原始表         | group by 子句的前面 | where  |
  | 分组后筛选 | 分组后的结果集 | group by 子句的后面 | having |

- 分组函数做条件肯定是放在having子句中

- 能用分组前筛选的, 尽量使用分组前筛选(考虑到性能问题)

### 2.5.5 添加排序

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

### 2.5.6 分组排序练习题

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
