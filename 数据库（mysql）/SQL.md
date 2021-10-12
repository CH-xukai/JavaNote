# SQL

Structured Query Language：结构化查询语言 其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。



> CRUD ---> C(Create 创建数据库)、R(Retrieve 查询数据库)、U(Update 修改数据库)、D(Delete 删除数据库) 



> 1. SQL 语句可以单行或多行书写，以分号结尾。
> 2. 可使用空格和缩进来增强语句的可读性。
> 3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
> 4. 3 种注释
>     * 单行注释: -- 注释内容 或 # 注释内容(mysql 特有)
>     * 多行注释: `/* 注释 */`

**SQL语句分类**

> 1) DDL(Data Definition Language)数据定义语言
>     用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等
> 2) DML(Data Manipulation Language)数据操作语言
>     用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等
> 3) DQL(Data Query Language)数据查询语言
>     用来查询数据库中表的记录(数据)。关键字：select, where 等
> 4) DCL(Data Control Language)数据控制语言(了解)
>     用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等

---



## DDL(Data Definition Language)数据定义语言

>  用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等



### 操作数据库

#### 查询数据库

```sql
-- 查询所有数据库的名称
show databases;
-- 显示创建数据库的语句，可用于查看数据库的字符集等信息
show create database 数据库名称;
```

#### 创建数据库

```sql
-- 创建数据库基本格式
create database 数据库名称;
-- 创建数据库，判断不存在，再创建
create database if not exists 数据库名称;
-- 创建数据库，并指定字符集
create database 数据库名称 character set 字符集名;
```

#### 修改数据库

```sql
-- 修改数据库的字符集
alter database 数据库名称 character set 字符集名称;
-- 例：修改db1数据库 的字符集为 utf8
alter database db1 character set utf8;
```

#### 删除数据库

```sql
-- 删除指定数据库
drop database 数据库名称;

-- 判断数据库若存在则删除
drop database if exists 数据库名称;
```

#### 使用数据库

```sql
-- (切换)当前使用的数据库
use 数据库名称;

-- 查询当前正在使用的数据库名称
select database();
```

---



### 操作数据表

#### 查询数据表

```sql
-- 查询所有数据表
show tables;
-- 查询表结构
desc 表名;
-- 查询表状态
SHOW TABLE STATUS FROM mysql LIKE '表名称';
```

#### 创建数据表

```sql
-- 创表格式,最后一列不要加逗号
create table 表名(
    列名1 数据类型1,
    列名2 数据类型2,
    列名3 数据类型2
); 
```

##### mysql常见数据类型

| 关键字      | 数据类型   | 备注                                                         |
| ----------- | ---------- | ------------------------------------------------------------ |
| int         | 整数类型   | 无符号[0,2^32-1]，有符号[-2^31,2^31-1]                       |
| Double(M,D) | 小数类型   | 双精度浮点。                                                 |
| date        | 日期       | 以YYYY-MM-DD的格式显示，比如：2009-07-19                     |
| datetime    | 日期       | 以YYYY-MM-DD HH:MM:SS的格式显示，比如：2009-07-19 11：22：30 |
| timestamp   | 时间戳类型 | 包含年月日时分秒 yyyy-MM-dd HH:mm:ss,不赋值默认为系统时间毫秒值 |
| VarChar(M)  | 字符串     | 变长字符串，要求M<=255                                       |

#### 修改数据表

修改表主要是对表结构的修改，如修改表名、修改表的字符集、修改表的字段(列)等

```sql
-- 修改表名
alter table 表名 rename to 新的表名;

-- 修改表的字符集格式
alter table 表名 character set 字符集名称;

-- 添加表字段(列)
alter table 表名 add 列名 数据类型;

-- 修改列名以及数据类型格式
alter table 表名 change 列名 新列别 新数据类型;

-- 修改指定列数据类型
alter table 表名 modify 列名 新数据类型;

-- 删除表字段(列)
alter table 表名 drop 列名;
```

#### 删除数据表

```sql
-- 删除表格式
drop table 表名;
-- 先判断存在，再删除表
drop table if exists 表名 ;
```

---



## DML(Data Manipulation Language)数据操作语言

### 添加表数据

```sql
-- 给指定列添加数据
INSERT INTO 表名(列名1,列名2,...) VALUES (值1,值2,...);
-- 给所有列添加数据
INSERT INTO 表名 VALUES (值1,值2,...);
```

### 删除表数据

```sql
DELETE FROM 表名 [WHERE 条件];
```

### 修改表数据

```sql
UPDATE 表名 SET 列名1 = 值1,列名2 = 值2,... [where 条件]; 
```

---



## DQL(Data Query Language)数据查询语言

> ```sql
> select 		
> 	查询的字段列表
> from		
> 	表名1 as 别名1,表名2 as 别名2
> where		
> 	查询的条件
> group by 	
> 	分组的列
> having 		
> 	分组后的条件
> order by	 
> 	排序条件
> limit		
> 	分页
> ```

### 基本查询格式

```sql
-- 查询表中所有数据
SELECT * FROM 表名;

-- 查询指定列的记录
select 列名1,列名2,... from 表名；
```

### 花里胡哨查询

```sql
-- 去重查询
SELECT DISTINCT 列名1,列名2,... FROM 表名;

-- 列运算查询
SELECT NAME,IFNULL(stock,0)+10 FROM product;

-- 起别名查询
SELECT 列名1 AS 别名 FROM 表名;

-- 条件查询
SELECT 列名列表 FROM 表名 WHERE 条件;

-- 聚合查询 count、sum、max、min、avg
SELECT 函数名(列名) FROM 表名 [WHERE 条件];

-- 排序查询 升序（默认）：ASC；降序：DESC
SELECT 列名 FROM 表名 [WHERE 条件] ORDER BY 列名1 排序方式1,列名2 排序方式2;

-- 分组查询
SELECT 列名 FROM 表名 GROUP BY 分组列名

-- 分页查询 LIMIT：限制查询结果返回的数量
SELECT * FROM tableName LIMIT i,n
# tableName：表名
# i：为查询结果的索引值(默认从0开始)，当i=0时可省略i
# n：为查询结果返回的数量
	-- 例：
	# 查询10条数据，索引从0到9，第1条记录到第10条记录
	select * from t_user limit 10;
	select * from t_user limit 0,10;
	# 查询8条数据，索引从5到12，第6条记录到第13条记录
	select * from t_user limit 5,8;
```

### 多表查询

#### 内连接查询

```sql
-- 显示内连接查询
SELECT 列名 FROM 表名1 [INNER] JOIN 表名2 ON 关联条件;

-- 隐式内连接查询
SELECT 列名 FROM 表名1,表名2 WHERE 关联条件;
```



#### 外连接查询

```sql
-- 左外连接查询 显示左表的所有数据，即使右表有null行
SELECT 列名 FROM 表名1 LEFT [OUTER] JOIN 表名2 ON 条件;

-- 右外连接查询 显示右表的所有数据，即使左表有null行
SELECT 列名 FROM 表名1 RIGHT [OUTER] JOIN 表名2 ON 条件;
```



#### 子查询

子查询就是利用SQL语句的查询结果，再结合其他SQL条件再次进行查询。套娃查询。

**情况有三**

##### 子查询结果为一行一列

```sql
select 列名 from 表名 where 字段=(查询语句);
select name ,age from user where age=(select max(age) from user);
```

##### 子查询结果为一行多列

```sql
SELECT 列名 FROM 表名 where 字段 in (查询语句);
select * from orderlist where uid in (select id from user where name in('张三','李 四'));
```

##### 子查询结果为多行多列

```sql
SELECT 列名 FROM 表名,(查询语句) AS 别名 where 查询条件;
select u.name,o.number from user u ,(select * from orderlist where id>4) o where o.uid=u.id;
```



#### 自关联查询

```sql
SELECT 列名 FROM 表名 别名1, 表名 别名2 WHERE 查询条件;
#同一张表其两个别名，相当于查询了两张内容相同的表
```



