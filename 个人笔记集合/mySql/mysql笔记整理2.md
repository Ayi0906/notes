[Toc]

## 2.6 连接查询

- 即多表查询, 当要查询的字段涉及到多个表时使用连接查询

### 2.6.1 连接查询分类

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

### 2.6.2 sql92语法连接查询

#### 2.6.2.1 sql92_等值连接

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

##### 2.6.2.1.1 sql92_等值连接的练习

- from后面的表名是否可以更换顺序? 能. 等值连接不区分引入顺序

- 等值连接的筛选

  - 案例1: 查询有奖金的员工名,部门名

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

- 等值连接的分组

  - 案例1: 查询每个城市的部门个数

    - ```sql
      SELECT COUNT(*) AS 部门个数,l.`city` AS 城市
      FROM locations AS l, departments AS d
      WHERE d.`location_id`=l.`location_id`
      GROUP BY l.`city`
      ```

  - 案例2: 查询出有奖金的每个部门的部门名和部门的领导编号 和该部门的最低工资

    - ```sql
      SELECT d.`department_name` AS 部门名,e.`manager_id` AS 领导编号, MIN(e.`salary`) AS 该部门最低工资
      FROM departments AS d,employees AS e
      WHERE e.`department_id`=d.`department_id`
      AND e.`commission_pct` IS NOT NULL
      GROUP BY d.`department_name`
      ```

- 等值连接的排序

  - 案例1: 查询出每个工种的工种名和员工个数, 并按员工个数降序

    - ```sql
      SELECT j.`job_title` AS 工种名,COUNT(*) AS 员工个数
      FROM employees AS e,jobs AS j
      WHERE e.`job_id`=j.`job_id`
      GROUP BY j.`job_title`
      ORDER BY 员工个数 DESC
      ```

- 等值连接实现多表连接

  - 查询员工名, 部门名与所在城市

    - ```sql
      SELECT e.`last_name` AS 员工名,d.`department_name` AS 部门名,l.`city` AS 所在城市
      FROM employees AS e,departments AS d,locations AS l
      WHERE e.`department_id`=d.`department_id`
      AND d.`location_id`=l.`location_id`
      ```

#### 2.6.2.2 sql92_非等值连接

- 案例1: 查询员工的工资和工资级别

  - 在工资级别.txt中复制sql语句执行, 注意去掉注释符号

  - ```sql
    SELECT e.`salary` AS 工资,jg.`grade_level` AS 工资级别  
    FROM employees AS e,job_grades AS jg
    WHERE e.`salary` BETWEEN jg.`lowest_sal` AND jg.`highest_sal`
    # 当salary在[jg.`lowest_sal`, jg.`highest_sal`]之间时, 获得对应行的level字段的值
    ORDER BY 工资 ASC
    ```

#### 2.6.2.3 sql92_自连接

- 案例1: 在员工表里查询 员工名和他上级的名称

  - 员工表里 有员工的employee_id字段和manager_id字段,相当于把一张表当成两张表来用

  - ```sql
    SELECT e1.`last_name` AS 员工名,e2.`last_name` AS 上级名
    FROM employees AS e1,employees AS e2
    WHERE e1.`manager_id`=e2.`employee_id`
    ORDER BY e1.`last_name` ASC
    ```

  - 在from 子句里将同一张表引入两次并设置不同的别名

#### 2.6.2.4 sql92语法, 等值连接, 非等值连接, 自连接练习题

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

### 2.6.3 sql99语法连接查询

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

#### 2.6.3.1 sql99_等值内连接

- ```
  select 查询列表
  from 表1 别名
  inner join 表2 别名
  on 连接条件
  
  分类: 等值 非等值 自连接
  特点:可以添加排序分组筛选, inner可以省略, 筛选条件放在where后面, 连接条件放在on后面, 提高分离性, 便于阅读; inner join 连接与sql92语法中的等值连接效果是一样的, 都是查询多表的交集部分
  ```

- 案例1: 查询员工名, 部门名(调换位置)

  - ```sql
    SELECT last_name,department_name
    FROM employees e
    INNER JOIN departments d
    ON e.`department_id`=d.`department_id`
    ```

- 案例2: 查询名字中包含e的员工名和他的工种名

  - ```sql
    SELECT last_name,job_title
    FROM employees AS e
    INNER JOIN jobs AS j
    ON e.`job_id`=j.`job_id`
    WHERE e.`last_name` LIKE '%e%'
    ```

- 案例3: 查询部门个数>3的城市名和部门个数

  - ```sql
    SELECT COUNT(*) AS 部门个数, city AS 城市名
    FROM departments AS d
    INNER JOIN locations l
    ON d.`location_id`=l.`location_id`
    GROUP BY 城市名
    HAVING 部门个数>3
    ```

- 案例4: 查询员工名\部门名\工种名, 并按部门名降序

  - ```sql
    SELECT last_name AS 员工名,department_name AS 部门名,job_title AS 工种名
    FROM employees AS e
    INNER JOIN departments AS d
    ON e.`department_id`=d.`department_id`
    INNER JOIN jobs AS j
    ON e.`job_id`=j.`job_id`
    ORDER BY 部门名 DESC
    ```

#### 2.6.3.2 sql99_非等值内连接

- 案例1: 查询员工的工资级别

  - ```sql
    SELECT salary,grade_level
    FROM employees AS e
    JOIN job_grades AS jg
    ON e.`salary` BETWEEN jg.`lowest_sal` AND jg.`highest_sal`
    ORDER BY salary
    ```

- 案例2: 查询工资级别人数的个数>2的,并且按工资级别进行降序排序

  - ```sql
    SELECT COUNT(*) AS 工资级别人数,grade_level AS 工资级别
    FROM employees AS e
    INNER JOIN job_grades AS jg
    ON e.`salary` BETWEEN jg.`lowest_sal` AND jg.`highest_sal`
    GROUP BY 工资级别
    HAVING 工资级别人数>2
    ORDER BY 工资级别 DESC
    ```

#### 2.6.3.3 sql99_自连接

- 案例1: 查询员工的名字和他的上级的名字

  - ```sql
    SELECT e1.last_name AS 员工名字,e2.last_name AS 上级名字
    FROM employees AS e1
    INNER JOIN employees AS e2
    ON e1.`manager_id`=e2.`employee_id`
    ```

- 案例2: 查询姓名中包含字符k的员工的名字, 和他上级的名字

  - ```sql
    SELECT e1.`last_name` AS 员工名字, e2.`last_name` AS 上级名字
    FROM employees AS e1
    INNER JOIN employees AS e2
    ON e1.`manager_id`=e2.`employee_id`
    WHERE e1.`last_name` LIKE  '%k%'
    ```

#### 2.6.3.4 sql99_左/右外连接

- 内连接是通过给定的两表连接条件将两表合成一个表(笛卡尔乘积), 并给出两表的交集部分
- 外连接首先设定一个主表, 主表的数据不会丢失, 无法与另一个表进行内连接的部分也会被保留下来

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
  
  1、左外连接：是A和B的交集再并上A的所有数据。
  2、右外连接：是A和B的交集再并上B的所有数据。
  ```

- 案例1: 查询男友没在数据表中的女性名

  - ```sql
    SELECT girls.name,boys.*
    FROM beauty AS girls
    LEFT OUTER JOIN boys
    ON girls.`boyfriend_id`=boys.`id`
    WHERE boys.`id` IS NULL
    ```

- 案例2: 使用右外连接实现查询男友没在数据表中的女性名

  - ```sql
    SELECT girls.name,boys.*
    FROM boys
    RIGHT OUTER JOIN beauty AS girls
    ON girls.`boyfriend_id`=boys.`id`
    WHERE boys.`id` IS NULL
    ```

- 案例3: 查询哪个部门没有员工

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
    # 对查询结果去重SELECT DISTINCT 
    d.department_name,e.`employee_id`FROM departments AS dLEFT OUTER JOIN	employees AS eON e.`department_id`=d.`department_id`WHERE employee_id IS NULL
    ```

#### 2.6.3.5 sql99_全外连接(了解)

- mysql语法不支持全外连接`full outer join`语句

- mysql可以使用union实现全外连接

- 案例1: 将 beauty表与boys表完全结合起来

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

- 案例2: 求 beauty 表与 boys 表去除可以完全匹配的部分( 即求所有单身女性和单身男性 )

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

#### 2.6.3.6 sql99_交叉连接

- ```sql
  SELECT girls.*,boys.*
  FROM beauty AS girls
  CROSS JOIN boys
  ```

- 交叉连接不需要连接规则, 只需要将A表的每一行与B表的每一行强行进行连接即可. 

  - 如果A表有5行, B表有4行, 那么最终结果有20行

- 即笛卡尔乘积

#### 2.6.3.7 sql99内连接,左/右外连接,自连接练习题

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

## 2.7 子查询

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

### 2.7.1 where 后面的标量子查询使用

- where或having后面
  - 标量子查询(单行子查询), 即子查询结果只有一行一列
  - 列子查询(多行子查询), 即子查询结果是一列多行
  - 行子查询(多行多列), 即子查询结果是一张完整的表
- 子查询都会放在小括号内
- 子查询一般放在条件的右侧
- 标量子查询, 一般搭配的单行操作符使用 > < <= = >= <>
- 列子查询, 一般搭配者多行操作符使用 IN , ANY/SOME, ALL
- 子查询优先于主查询执行, 主查询的条件用到了子查询的结果

- 案例1: 查询Abel的工资

  - ```sql
    SELECT salary
    FROM employees
    WHERE employees.`last_name`='Abel'
    ```

- 案例2: 查询员工的信息, 满足 salary> Abel的工资

  - ```sql
    SELECT salary
    FROM employees
    WHERE salary>(
    	SELECT salary
    	FROM employees
    	WHERE last_name='abel'
    )
    ```

- 案例3: 返回job_id与141号员工相同, salary比143号员工多的员工的姓名, job_id 和工资

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

- 案例4: 返回公司工资最少的员工的last_name,job_id和salary

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

- 案例5: 查询最低工资大于50号部门的最低工资的部门id和其最低工资

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

### 2.7.2 列子查询(一列多行)

- ```
  操作符             含义
  IN/NOT IN         等于列表中的任意一个; IN 与 =ANY 效果相同; not in 与!=All 效果相同
  ANY|SOME		  和子查询返回的某一个值比较; <Any : 小于最大值; >ANy: 大于最小值
  ALL				  和子查询返回的所有值比较; >ALL与大于最大值效果相同
  ```

- 案例1: 返回location_id是1400或1700的部门中的所有员工姓名

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

- 案例2: 返回其它部门中比job_id为`IT_PROG`工种任一工资低的员工的员工号, 姓名, job_id以及salary

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

- 案例3: 返回其它部门中比job_id为`IT_PROG`部门所有工资都低的员工的员工号, 姓名, job_id以及salary

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

### 2.7.3 行子查询

- 结果集一行多列或多行多列

- 案例1: 查询员工编号最小并且工资最高的员工信息

  - (这个题目似乎有问题)

  - ```sql
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

### 2.7.4 select后面的子查询

- 仅支持标量查询

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

### 2.7.5 from后面的子查询使用

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

### 2.7.6 exists后面的子查询使用(相关子查询)

- 它就是一个布尔值, 只关心后面的子查询有没有结果, 有结果返回1,没有返回0

- ```
  语法
  exists(完整的查询语句)
  ```

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

- 案例1: 查询有员工的部门名

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

- 案例2: 查询没有女朋友的男生信息

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

### 2.7.7 子查询练习题

- 案例1: 查询和zlotkey相同部门的员工姓名和工资

  - ```sql
    SELECT last_name AS 姓名,salary AS 工资, department_id AS 部门编号
    FROM employees
    WHERE department_id=(
    	SELECT department_id 
    	FROM employees
    	WHERE last_name='zlotkey'
    )
    ```

- 案例2: 查询工资比公司平均工资高的员工的员工号,姓名和工资

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

- 案例3: 查询各部门中工资比本部门平均工资高的员工的员工号,姓名和工资

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

- 案例4: 查询和姓名中包含字母u的员工在相同部门的员工的员工号和姓名

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

- 案例5: 查询在部门的location_id为1700的部门工作的员工的员工号

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

- 案例6: 查询管理者是king的员工姓名和工资

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

- 案例7: 查询工资最高的员工的姓名, 要求first_name和last_name显示为一列,列名为姓,名

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

## 2.8 分页查询

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

- 案例1: 查询前五条员工信息

  - ```sql
    SELECT *
    FROM employees
    LIMIT 0,5 # 从0开始, 显示5条, 当从第一条开始时, 0可以省略
    ```

- 案例2: 查询第11条到第25条

  - ```sql
    SELECT *
    FROM employees
    LIMIT 10,15
    ```

- 案例3: 查询有奖金的员工信息, 并且工资较高的前十名

  - ```sql
    SELECT *
    FROM employees
    WHERE commission_pct IS NOT NULL
    ORDER BY salary DESC
    LIMIT 10
    ```

## 2.9 几道练习题

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

## 2.10 子查询练习题

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

## 2.11 一些练习题

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

## 2.12 联合查询

```sql
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

- 特点

```sql
/*
	1.要求多条查询语句的查询列数是一致的
	2.要求多条查询语句的查询每一列的类型和顺序最好一致
	3.union关键字默认去重, 如果使用union all 可以包含重复项
*/
```