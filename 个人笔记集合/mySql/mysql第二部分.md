[Toc]

#### 118 数据类型介绍

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

#### 120 浮点型

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





#### 121 字符型

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



#### 日期型

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

#### 123 当天重点内容



#### 124 复习前一天的内容



#### 125 常见约束的介绍

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



#### 126 添加表时添加列级约束

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

#### 127 添加表级约束

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



#### 128 主键与唯一的区别

- ```
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

#### 129 外键的特点

- 要求在从表设置外键关系

- 从表的外键列的类型和主表的对应列的类型要求一致或兼容, 名称无要求

- 要求主表中的关联列必须是一个key(一般是主键或唯一键)

  - ```sql
    ```

- 插入数据时应该先插入主表, 再插入从表( 学员表和专业表, 必须有了专业表才有学员表 )

- 删除数据时, 先删除从表, 再删除主表( 先删除学员表, 再删除专业表 )

- 多个约束 空格 隔开, 没有顺序要求



#### 130 修改表时添加约束

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

  - 

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



#### 131 修改表时删除约束

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



#### 132 案例讲解_常见约束

- 向表emp2的id列中添加Primary key约束(my_emp_id_pk)

  - ```sql
    alter table emp2 modify column id int primary key
    ```

  - ```sql
    alter table emp2 constrain my_emp_id_pk primary key(id)
    ```

  - 

- 向表dept2中的id列中添加PRIMARY KEY 约束(my_dept_id_pk)

  - 和上面一样

- 向表emp2中添加列dept_id, 并在其中定义foreign key约束, 与之相关联的列是dept2表中的id列

  - ```sql
    alter table emp2 add column dept_id int
    ```

  - ```sql
    alter table emp2 add column constrain fk_emp2_dept2 foreign key(dept_id) references dept2(id)
    ```

- 补充列级约束与表级约束的区别

|          | 位置         | 支持的约束类型              | 是否可以起约束名   |
| -------- | ------------ | --------------------------- | ------------------ |
| 列级约束 | 列的后面     | 语法都支持, 但外键没有效果  | 不可以             |
| 表级约束 | 所有列的下面 | 默认和非空不支持,其它都支持 | 可以, 主键没有效果 |

 #### 133 标识列(自增列_设置自增列,修改自增步长)

```sql
/*
	又称为自增长列
	含义: 可以不用手动地插入值, 系统提供默认的序列值
	
	特点:
		1.自增列必须和主键搭配吗? 
			去掉primary key约束, 报错: 只允许存在一个自增列, 且它必须被定义为一个键(key, 主键,外键,唯一键都是key);
			因此, 不一定是主键, 也可以是unique;
		2.一个表中只能有一个自增列.
		3.自增列的类型只能是数值型
		4.自增列可以通过set auto_increment_increment=x来修改步长
*/
#一. 创建表时设置自增长列
create table tab_identity(
	id int primary key auto_increment, # 默认起始值为1,每次增长1
	name varchar(20)
);
insert into tab_identity values(null,'alice'); # 1 alice; 2 alice; 3 alice;

# 二 修改表时设置自增列
ALTER TABLE tab_identity MODIFY COLUMN id INT UNIQUE AUTO_INCREMENT; 

# 三 修改表时删除执自增列
ALTER TABLE tab_identity MODIFY COLUMN id INT UNIQUE; # 直接去掉auto_increment就可以了
```

- 注意: 自增列必须给予值null; 设定重复值或者不给予值都会报错. 每次都是从最后一行的数据作为自增起始值, 如果表中没有数据, 那么就从1开始;
- mysql不允许修改自增列的起始值, 但是允许修改步长

```sql
show variables like '%auto_increment%'; 
# 查看auto_increment列的设置, 有auto_incremnet_increment字段,值为1, 代表增长步长; 有auto_increment_offset, 代表起始值; 
set auto_increment_increment=3;
# 修改步长为3,自增列每次自增3
```

### 134 事务的介绍

```sql
/*
	TCL: Transaction Control Language 事务控制语言
	事务: 一个或一组sql语句组成的一个执行单元, 这个执行单元要么全部执行, 要么全部不执行
	案例: 转账
		A向B转账1000元,涉及到的逻辑:
			一.update A的账户余额 减去1000元 where name='A'
			二.update B的账户余额 增加1000元 where name='B'
			如果在执行一完成后, 执行二之前出了意外, 那么会造成整个银行系统结算出错
			
	概念: 事务由单独单元的一个或多个SQL语句组成，在这个单元中，每个MySQL语句是相互依赖的。而整个单独单元作为一个不可分割的整体，如果单元中某条SQL语句一旦执行失败或产生错误，整个单元将会回滚。所有受到影响的数据将返回到事物开始以前的状态；如果单元中的所有SQL语句均执行成功，则事物被顺利执行。
*/
```

```sql
/*
	存储引擎: 在mysql中的数据用各种不同的技术存储在文件（或内存）中, 通过show engines;来查看当前的存储引擎.默认是innodb, 它支持事务.
	经典面试题:
	事务的ACID属性
		1.原子性: 原子性是指事务是一个不可分割的工作单位，事务中的操作要么都发生，要么都不发生。
		2.一致性: 事务必须使数据库从一个一致性状态变换到另一个一致性状态
		3.隔离性:事务的隔离性是指一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对并发的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。
		4.永久性: 持久性是指一个事务一旦被提交，它对数据库中数据的改变就是永久性的，接下来的其他操作和数据库故障不应该对其有任何影响
*/
```

#### 135 演示事务的使用步骤

```sql
/*
	事务的创建:
		隐式事务: 事务没有明显的开启和结束的标记; 比如insert, update, delete;
			比如调用语句 show variables like 'autocommit'; 可以看到自动提交是开启的, 因此每条insert,update,delete语句一经调用立刻执行.
		显式事务: 事务具有明显的开启与结束的标记;前提: 必须先设置自动提交功能为禁用;
			set autocommit=0;
*/
# 步骤1:开启事务
set autocommit=0;
start transaction; #可选的
# 步骤2: 编写事务中的sql语句(select,insert,update,delete, 事务只有增删改查)
# 语句1,
# 语句2
# ...

# 步骤3:结束事务
commit; #提交事务
rollback; #回滚事务

# 演示事务的使用步骤
# 以account为例
drop table if exists account;

create table account(
	id int primary key auto_increment,
	username varchar(20),
	banlance double
);

insert into account(username,banlance)
values('user1',1000),('user2',1000);

# 开启事务
set autocommit=0;
# 编写一组事务的语句
update account set balance=500 where username='user1';
update account set balance=1500 where username='user2'; # 这里模拟了一个账户转账事务

# 结束事务_提交; 提交或回滚只能选一个
commit; -- 在结束事务之前, 上面的sql语句只是将数据保存在了内存中而不是磁盘文件, 只有执行commit之后才会提交到磁盘文件.

# 结束事务_回滚; 提交或回滚只能选一个
rollback;

# 附录: 发现如果取消了自动提交, 每次执行sql语句都会出现提示框
```

#### 136 事务并发问题的介绍

```sql
/*
	对于同时运行的多个事务, 当这些事务访问数据库中相同的数据时, 如果没有采取必要的隔离机制, 就会导	致各种并发问题.
	(多个线程访问同一数据资源可能会造成线程安全问题, 可以用加锁的方式实现线程同步)
	常见几种并发问题:
		1.脏读: 对于两个事务 T1, T2, T1 读取了已经被 T2 更新但还没有被提交的字段.之后, 若 T2 	回滚, T1读取的内容就是临时且无效的.
		2.不可重复读: 对于两个事务T1, T2, T1 读取了一个字段, 然后 T2 更新了该字段.之后,T1次读
	取同一个字段, 值就不同了
		3.幻读:对于两个事务T1, T2, T1 从一个表中读取了一个字段, 然后 T2 在该表中插入了一些新的	行. 之后, 如果 T1 再次读取同一个表, 就会多出几行.
		
		数据库事务的隔离性: 数据库系统必须具有隔离并发运行各个事务的能力, 使它们不会相互影响,避免		各种并发问题
		一个事务与其他事务隔离的程度称为隔离级别. 数据库规定了多种事务隔离级别, 不同隔离级别对应	不同的干扰程度, 隔离级别越高, 数据一致性就越好, 但并发性越弱
		
		mysql支持四种事务隔离级别.
	
*/
```



#### 137 演示mysql的四种隔离级别

- 第一个mysql服务器

```sql
# 查看当前默认的隔离级别
select @@transaction_isolation; # 这个是8.0以上的代码;显示REPEATABLE READ

# 修改当前的隔离级别
set session transaction isolation level read uncommitted; # 将隔离级别修改为read uncommitted

# 开启事务
set autocommit=0;

# 更新数据
update account set username='沈某某' where id='1'; # 执行后前往第二个mysql服务器查看数据; 此时第一个服务器并没有执行commit或者rollback;

# 前往第二个mysql服务器

# 在执行第二个mysql服务器的查询操作后,执行rollback回滚
rollback;

# 从第二个mysql回到当前

# 重新开启事务
set autocommit=0;

# 前往第二个开启事务
```

- 第二个mysql服务器

```sql
# 查看当前默认的隔离级别
select @@transaction_isolation; # 这个是8.0以上的代码;显示REPEATABLE READ

# 修改当前的隔离级别
set session transaction isolation level read uncommitted; # 将隔离级别修改为read uncommitted

# 开启事务
set autocommit=0;

# 在第一个mysql服务器更新数据后, 第二个服务器执行查询操作
select * from account; 
	# 查询结果为: 虽然第一个服务器没有执行事务完成命令, 但是第二个服务器依然获取到了更新后的数据库;
	# 如果查询的汉字为乱码, 那么调用命令 set names gbk;

# 回到第一个mysql服务器, 执行rollback

# 重新查询
select * from account; 
	# 查询结果为: id:1,username:'user1',与上一次查询结果不一致
	# 这个查询结果证明了最低等级的隔离级别下, 脏读,不可重复读和幻读问题都会出现
	
# 前往第一个mysql服务器

# 回到第二个,重新开启事务
set autocommit=0;

# 查询account
```

- 老师讲得太啰嗦了,自己根据一些博客总结了一下

```sql
/* 

事务的并发问题
　　1、脏读：事务A读取了事务B更新的数据，然后B回滚操作，那么A读取到的数据是脏数据

　　2、不可重复读：事务 A 多次读取同一数据，事务 B 在事务A多次读取的过程中，对数据作了更新并提交，导致事务A多次读取同一数据时，结果 不一致。

　　3、幻读：系统管理员A将数据库中所有学生的成绩从具体分数改为ABCDE等级，但是系统管理员B就在这个时候插入了一条具体分数的记录，当系统管理员A改结束后发现还有一条记录没有改过来，就好像发生了幻觉一样，这就叫幻读。
　　小结：不可重复读的和幻读很容易混淆，不可重复读侧重于修改，幻读侧重于新增或删除。解决不可重复读的问题只需锁住满足条件的行，解决幻读需要锁表

*/

```

- mysql事务隔离级别

| 事务隔离级别               | 脏读 | 不可重复读 | 幻读 |
| -------------------------- | ---- | ---------- | ---- |
| 读未提交(read-uncommitted) | 是   | 是         | 是   |
| 读已提交(read-committed)   | 否   | 是         | 是   |
| 可重复读(repeatable-read)  | 否   | 否         | 是   |
| 串行化(serializable)       | 否   | 否         | 否   |

##### 隔离级别_读未提交

```sql
/*
	1.创建2个mysql服务器
	2.查看2个服务的隔离级别
*/
```

