[Toc]

# 3 DML语法(数据操作语言)

- DML语言(数据操作语言)

  - 插入(insert)
  - 修改(update)
  - 删除(delete)

## 3.1 插入数据(insert into)的语法1

- 语法

```sql
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

```sql
insert into beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
values(14,'xxx','女','1990-4-23','18877776666',NULL,2)
```

```sql
insert into beauty(id,name,sex,borndate,phone,boyfriend_id)
values(14,'xxx','女','1990-4-23','18877776666',2)
```

- 插入数据的顺序是可以调整的, 但必须保证字段与值一一对应
- insert into写了字段名, 但是values不给予值会报错. 字段与值必须一一对应
- 可以省略列名, 默认所有列, 而且列的顺序和表中的列的顺序是一致的

```sql
insert into beauty
values(18,'zzz','男',Null,'119',Null,null)
# 这种情况下不可以省略null
```

## 3.2 插入数据(insert into)的方式2

```sql
insert into 表名
set 列名=值,列名=值...
```

```sql
insert into beauty
set id=99,name='ff',phone='5456'
```

## 3.3 两种插入数据方式的比较

- 方式1支持插入多行


```sql
insert into 表名
values(...)
values(...)
values(...)
```

- 方式2不支持插入多行
- 方式1支持子查询, 方式2不支持

```sql
insert into table1(id,name,phone)
select id,name,phone # 必须与插入的列字段一致
from table2
# 将从table2中查询到的结果添加到table1中
```

## 3.4 修改(update)表单数据

```sql
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

## 3.5 修改(update)多个表的数据

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

## 3.6 删除(delete)表单数据方式1

```sql
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

## 3.7 删除(delete)表单数据方式2

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

# 4 DDL语言(数据库模式定义语言)

```
- 库的管理
  - 创建,修改,删除
- 表的管理
  - 创建,修改,删除
- 创建 create
- 修改 alter
- 删除 drop
```

## 4.1 库的管理

```sql
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

## 4.2 创建表单

```sql
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

## 4.3 修改表单

```
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

## 4.4 删除表单

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

## 4.5 复制表单

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

## 4.6 库和表的管理案例

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

# 5 数据类型介绍

```
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

## 5.1 整型

```
整型
	tinyint:1字节. 有符号:-128~127; 无符号: 0~255;
	smallint:2字节
	mediumint: 3字节
	int\integer:4字节
	bigint:8字节
```

如何设置无符号和有符号

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

## 5.2 浮点型

- ```
  浮点型类型    字节     范围
  float		4		很大,不记
  double		8		很大,不记
  定点数类型		字节		范围
  DEC(M,D)		m+2		最大取值范围与double相同, 给定decimad的有效取值范围由m和d决定
  DECIMAL(M,D) 这是全写
  ```

- 保存精确度较高的小数使用定点型

- m和d的意思

  - d: 小数点后的位数

  - m: 整数部位的数

  - ```sql
    CREATE TABLE tab_float(
    	f1 FLOAT(5,2),
    	f2 DOUBLE(5,2),
    	f3 DECIMAL(5,2)
    );
    INSERT INTO tab_float VALUES(123.45,123.45,123.45) # 123.45
    INSERT INTO tab_float VALUES(123.4,123.4,123.4) # 123.40
    INSERT INTO tab_float VALUES(1523.4,1523.4,1523.4) # 报错out of range 不予执行, 这里和老师的不一样, 应该是版本问题, 老师的结果是插入了999.99, 即范围上限
    ```

  - m和d均可省略

    - ```sql
      如果是decimal, 则m默认是10,d默认是0
      如果是float和double, 则会根据插入的数值的精度来决定精度
      定点型的精确度较高, 如果要求插入的数值精度较高如货币运算等则考虑使用decimal
      ```

    - ```sql
      CREATE TABLE tab_float(
      	f1 FLOAT,
      	f2 DOUBLE,
      	f3 DECIMAL
      )
      
      INSERT INTO tab_float
      VALUES(123.4567,123.4567,123.4567)
      # 123.457 123.4567 123
      
      INSERT INTO tab_float
      VALUES(1234.567,1234.567,1234.567)
      # 1234.57 1234.567 1235
      
      INSERT INTO tab_float
      VALUES(12345678,12345678,12345678)
      #12345700 12345678 12345678
      ```

- 所选择的类型越简单越好, 能保存数值的类型越小越好

##### 单精度浮点数与双精度浮点数

- 单精度浮点数由三部分组成
  - 符号位, 在首位, 用1表示负数, 0表示正数
  - 阶码部分, 共8位,采用移码形式, 偏置常数127
  - 尾数部分,一共23位

## 5.3 字符型

- 较短的文本: char, varchar

- 较长的文本: text, blob(较大的二进制)

- ```
  字符串类型		最大字符数		描述及存储需求
  char(M)			M				M为0-255之间的整数
  varchar(M)		M				M为0-65535之间的整数
  ```

  - 区别字符数: utf8字符集中, 一个汉字占3个字节, 一个英文字母占1个字节, 但是字符描述的话, 一个汉字与一个英文字母都是一个字节
  - char代表固定长度的字符, varchar代表可变类型的字符
    - char相对于varchar在空间上耗费更高, 但也在效率上更高
    - char(M)的M可以省略, 默认为1,但是varcahr的m不可省略

- enum类型

  - 枚举类型, 要求插入的值必须属于列表中指定值之一

  - ```sql
    CREATE TABLE tab_char(
    	c1 ENUM('a','b','c')
    );
    
    INSERT INTO tab_char VALUES('a') # a
    INSERT INTO tab_char VALUES('b') # b
    INSERT INTO tab_char VALUES('c') # c
    INSERT INTO tab_char VALUES('m') # 错误代码： 1265 Data truncated for column 'c1' at row 1
    INSERT INTO tab_char VALUES('A') # a
    INSERT INTO tab_char VALUES('a,b') # 错误代码： 1265 Data truncated for column 'c1' at row 1
    ```

- set类型

  - 和enum类似; 但是set类型一次可以选取多个成员, 而enum只能选一个

  - ```sql
    CREATE TABLE tab_set(
    	s1 SET('a','b','c','d')
    );
    
    
    INSERT INTO tab_set VALUES('a') # a
    INSERT INTO tab_set VALUES('a,b') # a,b
    INSERT INTO tab_set VALUES('a,c,d') # a,c,d
    INSERT INTO tab_set VALUES('A,B') # a,b
    INSERT INTO tab_set VALUES('M') # 错误代码： 1265 Data truncated for column 's1' at row 1
    ```



## 5.4 日期型

- | 日期和时间类型 | 字节 | 最小值              | 最大值              |
  | -------------- | ---- | ------------------- | ------------------- |
  | date           | 4    | 1000-01-01          | 9999-12-31          |
  | datetime       | 8    | 1000-01-01 00:00:00 | 9999-12-31 23:59:59 |
  | timestamp      | 4    | 19700101080001      | 2038年的某个时刻    |
  | time           | 3    | -838:59:59          | 838:59:59           |
  | year           | 1    | 1901                | 2155                |

- timestamp和实际时区油管, 更能反应实际的日期, 而datetime则只能反映出插入时的当地时区

- timestamp的属性受mysql版本和SQLMode的影响很大

- ```sql
  CREATE TABLE tab_date(
  	t1 DATETIME,
  	t2 TIMESTAMP
  );
  INSERT INTO tab_date VALUES(NOW(),NOW())
  SELECT * FROM tab_date
  # t1	t2
  #2022-05-24 20:27:44	2022-05-24 20:27:44
  ```

- ```sql
  SHOW VARIABLES LIKE 'time_zone'
  # Variable_name	Value
  # time_zone	SYSTEM
  ```

- ```sql
  SET time_zone='+9:00';
  select * from tab_date
  # t1	t2
  # 2022-05-24 20:27:44	2022-05-24 21:27:44
  # 因此通过修改时区, timestamp可以自动修改时间为指定时区的时间
  ```

- 分类

  - date 只保存日期
  - time 只保存时间
  - year只保存年
  - datetime保存日期+时间
  - timestamp保存日期+时间

- 特点

  - datetime: 8字节; 1000-9999';不受时区影响
  - timestamp:4字节,1970-2038;受时区影响

# 6 约束

## 6.1 常见约束介绍

- ```sql
  CREATE TABLE 表名(
  	字段名 字段类型 约束
  )
  ```

- 一种限制, 用于限制表中的数据, 为了保证表中的数据的准确和可靠性

- 六大约束

  - ```
    NOT NULL : 非空约束, 保证字段的值不能为空和null值, 比如姓名, 学号等; 一般默认可以为空, 可以写成null或者不写
    DEFAULT: 默认约束, 用于保证该字段的值有默认值;
    PRIMARY KEY:主键, 用于保证该字段的值具有唯一性, 并且非空. 比如学号, 员工编号等
    UNIQUE: 唯一约束; 用于保证该字段的值具有唯一性, 可以为空
    CHECK: 检查约束[mysql中不支持,即加了这个约束与不加这个约束没有区别; 但是不会报错]
    FOREIGN KEY: 用于限制两个表的关系; 用于保证该字段值必须来自于主表的关联列的值
    	在从表添加外键约束, 用于限制主表中的某列的值;比如学生表的major_id字段的值必须来自于major表的major_id的值
    ```

  - 在创建表的时候添加约束; 在修改表时修改约束

  - 约束的添加分类:

    - 列级约束

      - 六种约束都支持, 但外键约束没有效果

    - 表级约束

      - 除了非空, 默认, 其它约束都支持

    - ```sql
      CREATE TABLE 表名(
      	字段名 字段类型 约束,
      	字段名 字段类型,
      	表级约束
      )
      ```

## 6.2 添加表时添加列级约束

- ```sql
  CREATE TABLE stuinfo(
  	id INT PRIMARY KEY, # 主键
  	stuName VARCHAR(20) NOT NULL, #非空
  	gender CHAR(1) CHECK(gender='男' OR gender='女') ,# 检查, 不起任何作用
  	seat INT UNIQUE, # 唯一
  	age INT DEFAULT 18, # 默认18
  	marjorId INT REFERENCES major(id) #引用外键, 在列级约束写外键没有任何作用
  );
  ```

- ```sql
  语法:
  	直接在字段名和类型后面嘴角约束类型即可
  	只支持: 默认, 非空, 主键, 唯一
  ```

- 查看约束

  - ```
    SHOW INDEX FROM stuinfo
    ```

## 6.3 添加表级约束

- ```sql
  create table stuinfo(
  	id int,
  	stuname varchar(20),
  	gender char(1),
  	seat int,
  	age int,
  	marjorId int,
  	constraint pk primary key(id), # 主键
  	constraint uq unique(seat), #唯一键
  	constraint ck check(gender='男' or gender='女'),# 检查
  	constraint fk_stuinfo_major foreign key(marjorId) references major(majorid) # 外键
  )
  ```

- `SHOW INDEX FROM stuinfo` 用于查询约束

- ```
  语法: 
  在各个字段的最下面
  [constraint 约束名] 约束类型(字段名)
  ```

- ```sql
  CREATE TABLE stuinfo(
  	id INT,
  	stuname VARCHAR(20),
  	gender CHAR(1),
  	seat INT,
  	age INT,
  	special INT,
  	marjorId INT,
  	PRIMARY KEY(id), # 主键
  	UNIQUE(seat), #唯一键
  	UNIQUE(special), # 又一个唯一键
  	CHECK(gender='男' OR gender='女'),# 检查
  	FOREIGN KEY(marjorId) REFERENCES major(majorid) # 外键
  )
  # 去掉前面设置约束名也可以, 默认使用字段名来作为约束名(除了主键, 主键的约束名一直都是PRIMARY)
  ```

- 什么时候用列? 什么时候用表?

  - 通用的写法

    - ```sql
      create table if not exists stuinfo(
      	id int primary key,
          stuname varchar(20) not null,
          sex char(1),
          age int default 18,
          seat int unique,
          marjorId int,
          constraint fk_stuinfo_major from foreign key(marjorId) references major(marjorid) # 给外键可以用表约束, 并且给约束命名
      )
      ```

## 6.4 primary key 与 unique的区别

```
		保证唯一性	是否允许为空		一个表中可以有多个
主键		唯一			不允许			至多有一个主键
唯一		唯一			允许			可以有多个唯一键
```

- 唯一键允许为空, 那么可以允许有多个null吗

  - 允许为null值, 但是不允许多个null值

- 组合主键与组合唯一键

  - 可以多个字段设置成一个主键

    - ```sql
      create table stuinfo(
      	id int,
      	stuname varchar(20),
      	gender char(1),
      	seat int,
      	age int,
      	marjorId int,
      	primary key(id,stuname), # 将多个字段组合设置成主键
      	unique(seat), #唯一键
      	check(gender='男' or gender='女'),# 检查
      	foreign key(marjorId) references major(majorid) # 外键
      );
      insert into stuinfo values(1,'john','男',154,16,3); # 成功
      insert into stuinfo values(2,'john','男',227,16,2); # 成功
      # 只要id和stuname字段的值不一致就可以, 一致会报错
      ```

    - unique非空字段也可以像上面一样组合

    - 不推荐多个字段组合设置主键或唯一键

## 6.5 外键的特点

- 要求在从表设置外键关系
- 从表的外键列的类型和主表的对应列的类型要求一致或兼容, 名称无要求
- 要求主表中的关联列必须是一个key(一般是主键或唯一键)

- 插入数据时应该先插入主表, 再插入从表( 学员表和专业表, 必须有了专业表才有学员表 )
- 删除数据时, 先删除从表, 再删除主表( 先删除学员表, 再删除专业表 )
- 多个约束 空格 隔开, 没有顺序要求

## 6.6 修改表时添加约束/删除约束

- 添加非空约束

  - ```sql
    alter table stuinfo modify column  stuname varchar(20) not null
    ```

- 添加默认约束

  - ```sql
    alter table stuinfo modify column  age int default 20
    ```

- 添加主键

  - ```sql
    alter table stuinfo modify column id int primary key
    ```

- 修改列级约束语法

  - ```sql
    alter table [表名] modify column [字段名] [字段类型] [约束]
    ```

- 修改表级约束语法

  - ```sql
    alter table [表名] add [约束]
    ```

    - 修改stuinfo的id为主键

      - ```sql
        alter table stuinfo add primary key(id)
        ```

    - 添加外键

      - ```sql
        alter table stuinfo add constraint fk_stuinfo_major foreign key(majorid) reference major(majorId)
        ```

      - ```sql
        alter table stuinfo add [constraint [约束名]] foreign key([从表字段名]) reference [主表名(主表字段名)]
        ```

- 删除非空约束

  - ```sql
    alter table stuinfo modify column stuname varchar(20) null;
    ```

- 删除默认约束

  - ```sql
    alter table stuinfo modify column age int;
    ```

- 删除主键

  - ```sql
    alter table stuinfo drop primary key;
    ```

- 删除唯一

  - ```sql
    alter table stuinfo drop index seat;
    ```

- 删除外键

  - ```sql
    alter table stuinfo drop foreign key fk_stuinfo_major
    ```

## 6.7 案例讲解_