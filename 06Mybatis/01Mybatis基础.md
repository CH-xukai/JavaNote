## 基础

> **简介**
>
> MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
>
> **ORM**
>
> 对象关系映射模型。主要强调面向对象思想操作数据库。

https://mybatis.org/mybatis-3/zh/getting-started.html

### 编写流程

1. 导入jar包
2. 编写核心配置文件，主要设置mybatis整体的一些参数。
3. 编写映射配置文件。主要设置sql语句及操作的对象的映射关系。
4. 通过mybatis的API，调用映射配置文件中设置的sql。mybatis自动完成结果处理。

### API

- `Resources`

  - `InputStream getResourceAsStream()`

  底层使用类加载器加载类路径下的配置文件。返回一个输入流对象

- `SqlSessionFactoryBuilder`

  - `SqlSessionFactory build() `

  采用构建器模式，用于构建工厂对象。

  *构建器模式，把特定某个对象的复杂的构建过程进行了封装，一般用于创建单个复杂对象。*

- `SqlSessionFactory`

  - `SqlSession openSession()`

  采用工厂模式，用于创建SqlSession对象。

  *工厂模式，用于批量生产特定的复杂对象*

- `SqlSession`

  代表了mybatis和数据库的会话对象，底层封装了一个数据库连接对象，也就是Connection对象。

#### 执行SQL相关的API

​	`selectList`、`selectOne`、`insert`、`delete`、`update`

#### 事务相关API

​	`rollback`、`commit`

#### 代理DAO的API

​	`getMapper`

### 配置文件

#### 核心配置

只能有一个

##### 配置数据库连接环境

​		数据库连接的参数

​		数据库连接池的管理

​		事务的管理方式

##### 配置映射配置文件的加载

```xml
<mappers>
    <!-- mapper 引入指定的映射配置文件   resource属性指定映射配置文件的名称 -->
    <!--resource属性指定的是路径的字符串。所以文件夹之间用/隔开。而不是写包名的. -->
    <mapper resource="com/mapper/StudentMapper.xml"/>
</mappers>
```

##### 加载外部属性文件

```xml
<!--引入数据库连接的配置文件-->
<properties resource="jdbc.properties"/>
```

一旦加载完毕之后，可以使用  ${属性的key}  的方式获取属性文件中的值

##### 配置mybatis的全局设置参数

通过settings标签设置。都是通过键值对的形式进行配置。具体的setting设置参见mybatis官方文档即可。

官方文档：https://mybatis.org/mybatis-3/zh/configuration.html#settings

##### 配置别名

通过包扫描的方式，指定实体类所在的包。该包下的所有类都会自动起别名。别名就是类的短类名，不区分大小写。

```xml
<typeAliases>
        <package name="com.itheima.bean"/>
</typeAliases>
```

在实际开发中，Mybatis习惯于把持久层的类和接口称为mapper。实际上就相当于之前的DAO。看到叫做StudentMapper的类或接口，把它看成叫做StudentDao即可

##### 集成LOG4J

```xml
<!--配置LOG4J-->
<settings>
    <setting name="logImpl" value="log4j"/>
</settings>
```



---

#### 映射配置

可以有多个

```xml
mapper：核心根标签
	namespace属性：名称空间。一般指定为dao接口的类名。用于唯一标识一个映射配置文件
子标签
	select、update、delete、insert：增删改查功能的标签，标签体中编写sql语句
        id属性：唯一标识，一般指定为dao接口中的方法名。
        resultType属性：指定结果映射对象类型。即使返回多个数据，结果类型也不用指定为list，而是指定泛型的类型即可。用于指定单条记录封装的类型。
        parameterType属性：指定参数映射对象类型
```

##### 参数类型

- 参数类型为简单类型：int、double、String...
  - 只有一个参数：参数名称任意指定即可。
  - 如果有多个参数：把多个参数封装为对象或者map集合，再使用参数

- 参数类型为对象或者map集合
  - 参数名称使用对象的属性名或者map集合的key的名称。

> 对象的属性名，仅和getter和setter方法相关。和对象的成员没有直接关系。

> #和$传参的区别
>
> - \#  使用预编译方式传参，底层用?占位符参数预编译，然后再设置参数。
>
> - $  使用字符串拼接的方式传参

##### 欲获取自增的ID值

- 方法一

  - >  使用`useGeneratedKeys="true" keyProperty="id" keyColumn="id"`这三个属性进行配置。insert操作完成后，会自动把自增的主键值设置到参数对象的指定属性上。
    >
    > `useGeneratedKeys`：指定是否使用自增的主键，设置为`true`即可。
    >
    > `keyProperty`：指定参数对象中主键对应的属性名称
    >
    > `keyColumn`：指定数据库表中主键列的名称

    ```xml
    <insert id="insert" parameterType="student" useGeneratedKeys="true" keyProperty="id" keyColumn="id">
        INSERT INTO student VALUES (null,#{name},#{age})
    </insert>
    ```

- 方法二

  - > 使用selectKey子标签，在insert标签的内部，通过在insert操作之后执行last_insert_id()函数，获取自增的id值。然后封装到参数对象的属性中。

    ```xml
    <insert id="insert" parameterType="student">
        <selectKey keyColumn="id" keyProperty="id" resultType="int" order="AFTER">
            select last_insert_id()
        </selectKey>
        INSERT INTO student VALUES (null,#{name},#{age})
    </insert>
    ```

##### 模糊查询

- 方法一

  - 使用concat()函数进行字符串拼接

  ```xml
  <select id="selectByCondition" resultType="Student" parameterType="string">
      select * from student where name like concat(#{name},"%")
  </select>
  ```

- 方法二

  - 使用${}的形式获取参数

  ```xml
  <select id="selectByCondition" resultType="Student" parameterType="string">
      select * from student where name like "${name}%"
  </select>
  ```

  > 注意：使用$获取参数实际上底层走的是字符串拼接，参数不会作为占位符进行预编译，所以有sql注入的风险

