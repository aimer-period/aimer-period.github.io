---
title: mybatis
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-05 00:28:13
password:
summary:
tags:
categories:
 -Java
---

<img src="https://cdn.jsdelivr.net/gh/aimer-period/pic/img/image-20220305005511657.png" />

# 1.MyBatis介绍

## 环境说明

- jdk 8 +
- MySQL 5.7.19
- maven-3.6.1
- IDEA

学习前需要掌握：

- JDBC
- MySQL
- Java 基础
- Maven
- Junit

## 什么是MyBatis

- MyBatis 是一款优秀的**持久层框架**
- MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程
- MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【Plain Old Java Objects,普通的 Java对象】映射成数据库中的记录。
- MyBatis 本是apache的一个开源项目ibatis, 2010年这个项目由apache 迁移到了google code，并且改名为MyBatis 。
- 2013年11月迁移到**Github** .
- Mybatis官方文档 : http://www.mybatis.org/mybatis-3/zh/index.html
- GitHub : https://github.com/mybatis/mybatis-3

## 持久化

**持久化是将程序数据在持久状态和瞬时状态间转换的机制。**

- 即把数据（如内存中的对象）保存到可永久保存的存储设备中（如磁盘）。持久化的主要应用是将内存中的对象存储在数据库中，或者存储在磁盘文件中、XML数据文件中等等。
- JDBC就是一种持久化机制。文件IO也是一种持久化机制。
- 在生活中 : 将鲜肉冷藏，吃的时候再解冻的方法也是。将水果做成罐头的方法也是。

**为什么需要持久化服务呢？那是由于内存本身的缺陷引起的**

- 内存断电后数据会丢失，但有一些对象是无论如何都不能丢失的，比如银行账号等，遗憾的是，人们还无法保证内存永不掉电。
- 内存过于昂贵，与硬盘、光盘等外存相比，内存的价格要高2~3个数量级，而且维持成本也高，至少需要一直供电吧。所以即使对象不需要永久保存，也会因为内存的容量限制不能一直呆在内存中，需要持久化来缓存到外存。

## 持久层

- 完成持久化工作的代码块 . ----> dao层 【DAO (Data Access Object) 数据访问对象】
- 大多数情况下特别是企业级应用，数据持久化往往也就意味着将内存中的数据保存到磁盘上加以固化，而持久化的实现过程则大多通过各种**关系数据库**来完成。
- 不过这里有一个字需要特别强调，也就是所谓的“层”。对于应用系统而言，数据持久功能大多是必不可少的组成部分。也就是说，我们的系统中，已经天然的具备了“持久层”概念？也许是，但也许实际情况并非如此。之所以要独立出一个“持久层”的概念,而不是“持久模块”，“持久单元”，也就意味着，我们的系统架构中，应该有一个相对独立的逻辑层面，专注于数据持久化逻辑的实现.
- 与系统其他部分相对而言，这个层面应该具有一个较为清晰和严格的逻辑边界。【说白了就是用来操作数据库存在的！】

## 为什么需要MyBatis

- Mybatis就是帮助程序猿将数据存入数据库中 , 和从数据库中取数据 .
- 传统的jdbc操作 , 有很多重复代码块 .比如 : 数据取出时的封装 , 数据库的建立连接等等... , 通过框架可以减少重复代码,提高开发效率 .
- MyBatis 是一个半自动化的**ORM框架 (Object Relationship Mapping) -->对象关系映射**
- 所有的事情，不用Mybatis依旧可以做到，只是用了它，所有实现会更加简单！**技术没有高低之分，只有使用这个技术的人有高低之别**
- MyBatis的优点
  - 简单易学：本身就很小且简单。没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件就可以了，易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。
  - 灵活：mybatis不会对应用程序或者数据库的现有设计强加任何影响。sql写在xml里，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求。
  - 解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。
  - 提供xml标签，支持编写动态sql。

# 2、第一个程序

思路流程：搭建环境-->导入Mybatis--->编写代码--->测试

## 1、搭建实验数据库

```sql
CREATE DATABASE `mybatis`;
USE `mybatis`;

DROP TABLE IF EXISTS `user`;

CREATE TABLE `user`(
`id` INT(20) NOT NULL,
`name` VARCHAR(30) DEFAULT NULL,
`pwd` VARCHAR(30) DEFAULT NULL,
PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO USER VALUES(1,'安安',123),(2,'姜芃姬',123),(3,'东姝',123);
```

## 2、导入MyBatis相关jar包

```java
<dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
    
    
 
```

```xml
   #过滤静态资源
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

 

## 3、编写MyBatis核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
        <package name="com.aimer.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="106606"/>
            </dataSource>
        </environment>
    </environments>
    
</configuration>
```



## 4、MyBatis工具类

```java
//连接数据库
public class MyBatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    //获取SqlSession连接
    public static SqlSession getSession(){
        return sqlSessionFactory.openSession();
    }
}
```

# 3、CRUD

## 1、namespace

配置文件中namespace中的名称为对应Mapper接口或者Dao接口的完整包名,必须一致

## 2、select

- select标签是mybatis中最常用的标签之一
- select语句有很多属性可以详细配置每一条SQL语句
  - SQL语句返回值类型。【完整的类名或者别名】
  - 传入SQL语句的参数类型 。【万能的Map，可以多尝试使用】
  - 命名空间中唯一的标识符
  - 接口中的方法名与映射文件中的SQL语句ID 一一对应
  - id
  - parameterType
  - resultType

**需求：根据id查询用户**

1、在UserMapper中添加对应方法

```
public interface UserMapper {
   //查询全部用户
   List<User> selectUser();
   //根据id查询用户
   User selectUserById(int id);
}
```

2、在UserMapper.xml中添加Select语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.aimer.mybatis.mapper.UserMapper">
    <select id="selectUserById" parameterType="int" resultType="com.aimer.mybatis.pojo.User">
        select * from user where id = #{id}
    </select>

</mapper>
```

3、测试类中测试

```
@Test
public void tsetSelectUserById() {
   SqlSession session = MybatisUtils.getSession();  //获取SqlSession连接
   UserMapper mapper = session.getMapper(UserMapper.class);
   User user = mapper.selectUserById(1);
   System.out.println(user);
   session.close();
}
```

思路一：直接在方法中传递参数

1、在接口方法的参数前加 @Param属性

2、Sql语句编写的时候，直接取@Param中设置的值即可，不需要单独设置参数类型

```java
//通过密码和名字查询用户User selectUserByNP(@Param("username") String username,@Param("pwd") Stringpwd);/*   <select id="selectUserByNP" resultType="com.kuang.pojo.User">     select * from user where name = #{username} and pwd = #{pwd}   </select>*/
```

思路二：使用万能的Map

1、在接口方法中，参数直接传递Map；

```java
User selectUserByNP2(Map<String,Object> map);
```

2、编写sql语句的时候，需要传递参数类型，参数类型为map

```java
<select id="selectUserByNP2" parameterType="map"resultType="com.kuang.pojo.User">select * from user where name = #{username} and pwd = #{pwd}</select>
```

3、在使用方法的时候，Map的 key 为 sql中取的值即可，没有顺序要求！

```java
Map<String, Object> map = new HashMap<String, Object>();map.put("username","小明");map.put("pwd","123456");User user = mapper.selectUserByNP2(map);
```

总结：如果参数过多，我们可以考虑直接使用Map实现，如果参数比较少，直接传递参数即可

## 3、insert

我们一般使用insert标签进行插入操作，它的配置和select标签差不多！

**需求：给数据库增加一个用户**

1、在UserMapper接口中添加对应的方法

```
//添加一个用户int addUser(User user);
```

2、在UserMapper.xml中添加insert语句

```
<insert id="addUser" parameterType="com.kuang.pojo.User">    insert into user (id,name,pwd) values (#{id},#{name},#{pwd})</insert>
```

3、测试

```
@Testpublic void testAddUser() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   User user = new User(5,"王五","zxcvbn");   int i = mapper.addUser(user);   System.out.println(i);   session.commit(); //提交事务,重点!不写的话不会提交到数据库   session.close();}
```

**注意点：增、删、改操作需要提交事务！**

## 4、update

我们一般使用update标签进行更新操作，它的配置和select标签差不多！

**需求：修改用户的信息**

1、同理，编写接口方法

```
//修改一个用户int updateUser(User user);
```

2、编写对应的配置文件SQL

```
<update id="updateUser" parameterType="com.kuang.pojo.User">  update user set name=#{name},pwd=#{pwd} where id = #{id}</update>
```

3、测试

```
@Testpublic void testUpdateUser() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   User user = mapper.selectUserById(1);   user.setPwd("asdfgh");   int i = mapper.updateUser(user);   System.out.println(i);   session.commit(); //提交事务,重点!不写的话不会提交到数据库   session.close();}
```

## 5、delete

我们一般使用delete标签进行删除操作，它的配置和select标签差不多！

**需求：根据id删除一个用户**

1、同理，编写接口方法

```
//根据id删除用户int deleteUser(int id);
```

2、编写对应的配置文件SQL

```
<delete id="deleteUser" parameterType="int">  delete from user where id = #{id}</delete>
```

3、测试

```
@Testpublic void testDeleteUser() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   int i = mapper.deleteUser(5);   System.out.println(i);   session.commit(); //提交事务,重点!不写的话不会提交到数据库   session.close();}
```

**小结：**

- 所有的增删改操作都需要提交事务！
- 接口所有的普通参数，尽量都写上@Param参数，尤其是多个参数时，必须写上！
- 有时候根据业务的需求，可以考虑使用map传递参数！
- 为了规范操作，在SQL的配置文件中，我们尽量将Parameter参数和resultType都写上！

## 6、模糊查询

**模糊查询like语句该怎么写?**

第1种：在Java代码中添加sql通配符。

```
string wildcardname = “%smi%”;list<name> names = mapper.selectlike(wildcardname);<select id=”selectlike”>select * from foo where bar like #{value}</select>
```

第2种：在sql语句中拼接通配符，会引起sql注入

```
string wildcardname = “smi”;list<name> names = mapper.selectlike(wildcardname);<select id=”selectlike”>    select * from foo where bar like "%"#{value}"%"</select>
```

# 4、配置解析

## 1、核心配置文件

- mybatis-config.xml 系统核心配置文件
- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。
- 能配置的内容如下：

```
configuration（配置）properties（属性）settings（设置）typeAliases（类型别名）typeHandlers（类型处理器）objectFactory（对象工厂）plugins（插件）environments（环境配置）environment（环境变量）transactionManager（事务管理器）dataSource（数据源）databaseIdProvider（数据库厂商标识）mappers（映射器）<!-- 注意元素节点的顺序！顺序不对会报错 -->
```

我们可以阅读 mybatis-config.xml 上面的dtd的头文件！

## 2、environments元素

```java
<environments default="development"> <environment id="development">   <transactionManager type="JDBC">     <property name="..." value="..."/>   </transactionManager>   <dataSource type="POOLED">     <property name="driver" value="${driver}"/>     <property name="url" value="${url}"/>     <property name="username" value="${username}"/>     <property name="password" value="${password}"/>   </dataSource> </environment></environments>
```

- 配置MyBatis的多套运行环境，将SQL映射到多个不同的数据库上，必须指定其中一个为默认运行环境（通过default指定）

- 子元素节点：

  environment

  - dataSource 元素使用标准的 JDBC 数据源接口来配置 JDBC 连接对象的资源。

  - 数据源是必须配置的。

  - 有三种内建的数据源类型

    ```java
    type="[UNPOOLED|POOLED|JNDI]"）
    ```

  - unpooled：这个数据源的实现只是每次被请求时打开和关闭连接。

  - **pooled**：这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得并发 Web 应用快速响应请求的流行处理方式。

  - jndi：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

  - 数据源也有很多第三方的实现，比如dbcp，c3p0，druid等等....

  - 这两种事务管理器类型都不需要设置任何属性。

  - 具体的一套环境，通过设置id进行区别，id保证唯一！

  - 子元素节点：transactionManager - [ 事务管理器 ]

    ```
    <!-- 语法 --><transactionManager type="[ JDBC | MANAGED ]"/>
    ```

  - 子元素节点：**数据源（dataSource）**

## 3、mappers元素

**mappers**

- 映射器 : 定义映射SQL语句文件
- 既然 MyBatis 的行为其他元素已经配置完了，我们现在就要定义 SQL 映射语句了。但是首先我们需要告诉 MyBatis 到哪里去找到这些语句。Java 在自动查找这方面没有提供一个很好的方法，所以最佳的方式是告诉 MyBatis 到哪里去找映射文件。你可以使用相对于类路径的资源引用， 或完全限定资源定位符（包括 `file:///` 的 URL），或类名和包名等。映射器是MyBatis中最核心的组件之一，在MyBatis 3之前，只支持xml映射器，即：所有的SQL语句都必须在xml文件中配置。而从MyBatis 3开始，还支持接口映射器，这种映射器方式允许以Java代码的方式注解定义SQL语句，非常简洁。

**引入资源方式**

```xml
<!-- 使用相对于类路径的资源引用 --><mappers> <mapper resource="org/mybatis/builder/PostMapper.xml"/>    <mapper resource="org/mybatis/builder/*Mapper.xml"/>    //可一次通配引入所有资源，但接口不可以</mappers><!-- 使用完全限定资源定位符（URL） --><mappers> <mapper url="file:///var/mappers/AuthorMapper.xml"/></mappers><!--使用映射器接口实现类的完全限定类名需要配置文件名称和接口名称一致，并且位于同一目录下--><mappers> <mapper class="org.mybatis.builder.AuthorMapper"/></mappers><!--将包内的映射器接口实现全部注册为映射器但是需要配置文件名称和接口名称一致，并且位于同一目录下--><mappers> <package name="org.mybatis.builder"/></mappers>
```

**Mapper文件**

```java
<?xml version="1.0" encoding="UTF-8" ?><!DOCTYPE mapper       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"       "<http://mybatis.org/dtd/mybatis-3-mapper.dtd>"><mapper namespace="com.aimer.mapper.UserMapper"></mapper>
```

- namespace中文意思：命名空间，作用如下：

  - namespace的命名必须跟某个接口同名
  - 接口中的方法与映射文件中sql语句id应该一一对应

  1. namespace和子元素的id联合保证唯一 , 区别不同的mapper
  2. 绑定DAO接口
  3. namespace命名规则 : 包名+类名

MyBatis 的真正强大在于它的映射语句，这是它的魔力所在。由于它的异常强大，映射器的 XML 文件就显得相对简单。如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 95% 的代码。MyBatis 为聚焦于 SQL 而构建，以尽可能地为你减少麻烦。

## 4、properties优化

数据库这些属性都是可外部配置且可动态替换的，既可以在典型的 Java 属性文件中配置，亦可通过 properties 元素的子元素来传递。具体的官方文档

我们来优化我们的配置文件

第一步 ; 在资源目录下新建一个db.properties

```java
driver=com.mysql.jdbc.Driverurl=jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=utf8username=rootpassword=123456
```

第二步 : 将文件导入properties 配置文件

```java
<configuration>   <!--导入properties文件-->   <properties resource="db.properties"/>   <environments default="development">       <environment id="development">           <transactionManager type="JDBC"/>           <dataSource type="POOLED">               <property name="driver" value="${driver}"/>               <property name="url" value="${url}"/>               <property name="username" value="${username}"/>               <property name="password" value="${password}"/>           </dataSource>       </environment>   </environments>   <mappers>       <mapper resource="mapper/UserMapper.xml"/>   </mappers></configuration>
```

## 5、typeAliases优化

类型别名是为 Java 类型设置一个短的名字。它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余。

```
<!--配置别名,注意顺序--><typeAliases>   <typeAlias type="com.kuang.pojo.User" alias="User"/></typeAliases>
```

当这样配置时，`User`可以用在任何使用`com.kuang.pojo.User`的地方。

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如:

```
<typeAliases>   <package name="com.kuang.pojo"/></typeAliases>
```

每一个在包 `com.kuang.pojo` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。

若有注解，则别名为其注解值。见下面的例子：

```
@Alias("user")public class User {  ...}
```

去官网查看一下Mybatis默认的一些类型别名！

## 6、其他设置

**设置**

- 设置（settings）相关 => 查看帮助文档

  - 懒加载
  - 日志实现
  - 缓存开启关闭

- 一个配置完整的 settings 元素的示例如下：

  ```xml
  <settings> <setting name="cacheEnabled" value="true"/> <setting name="lazyLoadingEnabled" value="true"/> <setting name="multipleResultSetsEnabled" value="true"/> <setting name="useColumnLabel" value="true"/> <setting name="useGeneratedKeys" value="false"/> <setting name="autoMappingBehavior" value="PARTIAL"/> <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/> <setting name="defaultExecutorType" value="SIMPLE"/> <setting name="defaultStatementTimeout" value="25"/> <setting name="defaultFetchSize" value="100"/> <setting name="safeRowBoundsEnabled" value="false"/> <setting name="mapUnderscoreToCamelCase" value="false"/> <setting name="localCacheScope" value="SESSION"/> <setting name="jdbcTypeForNull" value="OTHER"/> <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/></settings>
  ```

**类型处理器**

- 无论是 MyBatis 在预处理语句（PreparedStatement）中设置一个参数时，还是从结果集中取出一个值时， 都会用类型处理器将获取的值以合适的方式转换成 Java 类型。
- 你可以重写类型处理器或创建你自己的类型处理器来处理不支持的或非标准的类型。【了解即可】

**对象工厂**

- MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成。
- 默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时候通过有参构造方法来实例化。
- 如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现。【了解即可】

## 7、生命周期



生命周期，和作用域，是至关重要的，因为错误的使用会导致非常重要的并发问题

**作用域理解**

- SqlSessionFactoryBuilder 的作用在于创建 SqlSessionFactory，创建成功后，SqlSessionFactoryBuilder 就失去了作用，所以它只能存在于创建 SqlSessionFactory 的方法中，而不要让其长期存在。因此 **SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域**（也就是局部方法变量）。
- SqlSessionFactory 可以被认为是一个数据库连接池，它的作用是创建 SqlSession 接口对象。因为 MyBatis 的本质就是 Java 对数据库的操作，所以 SqlSessionFactory 的生命周期存在于整个 MyBatis 的应用之中，所以一旦创建了 SqlSessionFactory，就要长期保存它，直至不再使用 MyBatis 应用，所以可以认为 SqlSessionFactory 的生命周期就等同于 MyBatis 的应用周期。
- 由于 SqlSessionFactory 是一个对数据库的连接池，所以它占据着数据库的连接资源。如果创建多个 SqlSessionFactory，那么就存在多个数据库连接池，这样不利于对数据库资源的控制，也会导致数据库连接资源被消耗光，出现系统宕机等情况，所以尽量避免发生这样的情况。
- 因此在一般的应用中我们往往希望 SqlSessionFactory 作为一个单例，让它在应用中被共享。所以说 **SqlSessionFactory 的最佳作用域是应用作用域。**
- 如果说 SqlSessionFactory 相当于数据库连接池，那么 SqlSession 就相当于一个数据库连接（Connection 对象），你可以在一个事务里面执行多条 SQL，然后通过它的 commit、rollback 等方法，提交或者回滚事务。所以它应该存活在一个业务请求中，处理完整个请求后，应该关闭这条连接，让它归还给 SqlSessionFactory，否则数据库资源就很快被耗费精光，系统就会瘫痪，所以用 try...catch...finally... 语句来保证其正确关闭。
- **所以 SqlSession 的最佳的作用域是请求或方法作用域。**



# 5、解决属性名和字段名不一致的问题



## 1、问题 （查询为null）

2、Java中的实体类设计

```
public class User {   private int id;  //id   private String name;   //姓名   private String password;   //密码和数据库不一样！      //构造   //set/get   //toString()}
```

3、接口

```
//根据id查询用户User selectUserById(int id);
```

4、mapper映射文件

```
<select id="selectUserById" resultType="user">  select * from user where id = #{id}</select>
```

5、测试

```java
@Testpublic void testSelectUserById() {   SqlSession session = MybatisUtils.getSession();  //获取SqlSession连接   UserMapper mapper = session.getMapper(UserMapper.class);   User user = mapper.selectUserById(1);   System.out.println(user);   session.close();}
```

**结果:**

- User{id=1, name='狂神', password='null'}
- 查询出来发现 password 为空 . 说明出现了问题！

**分析：**

- select * from user where id = #{id} 可以看做

  select  id,name,pwd  from user where id = #{id}

- mybatis会根据这些查询的列名(会将列名转化为小写,数据库不区分大小写) , 去对应的实体类中查找相应列名的set方法设值 , 由于找不到setPwd() , 所以password返回null ; 【自动映射】

##  2、解决方案

方案一：为列名指定别名 , 别名和java实体类的属性名一致 .

```sql
<select id="selectUserById" resultType="User">  select id , name , pwd as password from user where id = #{id}</select>
```

**方案二：使用结果集映射->ResultMap** 【推荐】

```sql
<resultMap id="UserMap" type="User">   <!-- id为主键 -->   <id column="id" property="id"/>   <!-- column是数据库表的列名 , property是对应实体类的属性名 -->   <result column="name" property="name"/>   <result column="pwd" property="password"/></resultMap><select id="selectUserById" resultMap="UserMap">  select id , name , pwd from user where id = #{id}</select>
```

> ResultMap

**自动映射**

- `resultMap` 元素是 MyBatis 中最重要最强大的元素。它可以让你从 90% 的 JDBC `ResultSets` 数据提取代码中解放出来。
- 实际上，在为一些比如连接的复杂语句编写映射代码的时候，一份 `resultMap` 能够代替实现同等功能的长达数千行的代码。
- ResultMap 的设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行了。

你已经见过简单映射语句的示例了，但并没有显式指定 `resultMap`。比如：

```
<select id="selectUserById" resultType="map">select id , name , pwd  from user  where id = #{id}</select>
```

上述语句只是简单地将所有的列映射到 `HashMap` 的键上，这由 `resultType` 属性指定。虽然在大部分情况下都够用，但是 HashMap 不是一个很好的模型。你的程序更可能会使用 JavaBean 或 POJO（Plain Old Java Objects，普通老式 Java 对象）作为模型。

`ResultMap` 最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显式地用到他们。

**手动映射**

1、返回值类型为resultMap

```
<select id="selectUserById" resultMap="UserMap">  select id , name , pwd from user where id = #{id}</select>
```

2、编写resultMap，实现手动映射！

```
<resultMap id="UserMap" type="User">   <!-- id为主键 -->   <id column="id" property="id"/>   <!-- column是数据库表的列名 , property是对应实体类的属性名 -->   <result column="name" property="name"/>   <result column="pwd" property="password"/></resultMap>
```

如果世界总是这么简单就好了。但是肯定不是的，数据库中，存在一对多，多对一的情况，我们之后会使用到一些高级的结果集映射，association，collection这些，我们将在之后讲解，今天你们需要把这些知识都消化掉才是最重要的！理解结果集映射的这个概念！



# 6、日志

思考：我们在测试SQL的时候，要是能够在控制台输出 SQL 的话，是不是就能够有更快的排错效率？

如果一个 数据库相关的操作出现了问题，我们可以根据输出的SQL语句快速排查问题。

对于以往的开发过程，我们会经常使用到debug模式来调节，跟踪我们的代码执行过程。但是现在使用Mybatis是基于接口，配置文件的源代码执行过程。因此，我们必须选择日志工具来作为我们开发，调节程序的工具。

Mybatis内置的日志工厂提供日志功能，具体的日志实现有以下几种工具：

- SLF4J
- Apache Commons Logging
- Log4j 2
- Log4j
- JDK logging

具体选择哪个日志实现工具由MyBatis的内置日志工厂确定。它会使用最先找到的（按上文列举的顺序查找）。如果一个都未找到，日志功能就会被禁用

**标准日志实现**

指定 MyBatis 应该使用哪个日志记录实现。如果此设置不存在，则会自动发现日志记录实现。

```xml
<settings>       <setting name="logImpl" value="STDOUT_LOGGING"/></settings>
```



## 1、log4j

- Log4j是Apache的一个开源项目
- 通过使用Log4j，我们可以控制日志信息输送的目的地：控制台，文本，GUI组件....
- 我们也可以控制每一条日志的输出格式；
- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。最令人感兴趣的就是，这些可以通过一个配置文件来灵活地进行配置，而不需要修改应用的代码。



**使用步骤：**

1、导入log4j的包

```xml
<dependency>   <groupId>log4j</groupId>   <artifactId>log4j</artifactId>   <version>1.2.17</version></dependency>
```

2、配置文件编写

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码log4j.rootLogger=DEBUG,console,file#控制台输出的相关设置log4j.appender.console = org.apache.log4j.ConsoleAppenderlog4j.appender.console.Target = System.outlog4j.appender.console.Threshold=DEBUGlog4j.appender.console.layout = org.apache.log4j.PatternLayoutlog4j.appender.console.layout.ConversionPattern=[%c]-%m%n#文件输出的相关设置log4j.appender.file = org.apache.log4j.RollingFileAppenderlog4j.appender.file.File=./log/kuang.loglog4j.appender.file.MaxFileSize=10mblog4j.appender.file.Threshold=DEBUGlog4j.appender.file.layout=org.apache.log4j.PatternLayoutlog4j.appender.file.layout.ConversionPattern=[%p][%d{yy-MM-dd}][%c]%m%n#日志输出级别log4j.logger.org.mybatis=DEBUGlog4j.logger.java.sql=DEBUGlog4j.logger.java.sql.Statement=DEBUGlog4j.logger.java.sql.ResultSet=DEBUGlog4j.logger.java.sql.PreparedStatement=DEBUG
```

3、setting设置日志实现

```xml
<settings>   <setting name="logImpl" value="LOG4J"/></settings>
```

4、在程序中使用Log4j进行输出！

```java
//注意导包：org.apache.log4j.Loggerstatic Logger logger = Logger.getLogger(MyTest.class);@Testpublic void selectUser() {   logger.info("info：进入selectUser方法");   logger.debug("debug：进入selectUser方法");   logger.error("error: 进入selectUser方法");   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   List<User> users = mapper.selectUser();   for (User user: users){       System.out.println(user);  }   session.close();}
```

5、测试，看控制台输出！

- 使用Log4j 输出日志
- 可以看到还生成了一个日志的文件 【需要修改file的日志级别】



# 7、分页

在学习mybatis等持久层框架的时候，会经常对数据进行增删改查操作，使用最多的是对数据库进行查询操作，如果查询大量数据的时候，我们往往使用分页进行查询，也就是每次处理小部分数据，这样对数据库压力就在可控范围内。

## 1、limit实现分页

``` mysql
#语法SELECT * FROM table LIMIT stratIndex，pageSizeSELECT * FROM table LIMIT 5,10; // 检索记录行 6-15  #为了检索从某一个偏移量到记录集的结束所有的记录行，可以指定第二个参数为 -1：   SELECT * FROM table LIMIT 95,-1; // 检索记录行 96-last.  #如果只给定一个参数，它表示返回最大的记录行数目：   SELECT * FROM table LIMIT 5; //检索前 5 个记录行  #换句话说，LIMIT n 等价于 LIMIT 0,n。
```

**步骤：**

1、修改Mapper文件

```xml
<select id="selectUser" parameterType="map" resultType="user">  select * from user limit #{startIndex},#{pageSize}</select>
```

2、Mapper接口，参数为map

```
//选择全部用户实现分页List<User> selectUser(Map<String,Integer> map);
```

3、在测试类中传入参数测试

- 推断：起始位置 =  （当前页面 - 1 ） * 页面大小

```java
//分页查询 , 两个参数startIndex , pageSize@Testpublic void testSelectUser() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   int currentPage = 1;  //第几页   int pageSize = 2;  //每页显示几个   Map<String,Integer> map = new HashMap<String,Integer>();   map.put("startIndex",(currentPage-1)*pageSize);   map.put("pageSize",pageSize);   List<User> users = mapper.selectUser(map);   for (User user: users){       System.out.println(user);  }   session.close();}
```



## 2、RowBounds分页

我们除了使用Limit在SQL层面实现分页，也可以使用RowBounds在Java代码层面实现分页，当然此种方式作为了解即可。我们来看下如何实现的！

```java
 public void selectUserByRowBounds(){        SqlSession session = MyBatisUtils.getSession();        //RowBounds的实现        RowBounds rowBounds = new RowBounds(1,2);//从第一个二开始，每页查询两个        //传统的实现方法        List<User> userList = session.selectList("com.aimer.Dao.UserMapper.selectUserByRowBounds",null,rowBounds) ;        for (User user : userList) {            System.out.println(user);        }        session.close();    }
```



# 8、注解开发

## 8.1、面向接口编程

- 大家之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程
- **根本原因 :  解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好**
- 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；
- 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。



**关于接口的理解**

- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

- 接口的本身反映了系统设计人员对系统的抽象理解。

- 接口应有两类：

- - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；
  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

- 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。



**三个面向区别**

- 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .
- 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .
- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构



## 8.2、利用注解开发

- **mybatis最初配置信息是基于 XML ,映射语句(SQL)也是定义在 XML 中的。而到MyBatis 3提供了新的基于注解的配置。不幸的是，Java 注解的的表达力和灵活性十分有限。最强大的 MyBatis 映射并不能用注解来构建**

- sql 类型主要分成 :

- - @select ()
  - @update ()
  - @Insert ()
  - @delete ()



**注意：**利用注解开发就不需要mapper.xml映射文件了 .

1、我们在我们的接口中添加注解

```Java
//查询全部用户@Select("select id,name,pwd password from user")public List<User> getAllUser();
```

2、在mybatis的核心配置文件中注入

```Java
<!--使用class绑定接口--><mappers>   <mapper class="com.kuang.mapper.UserMapper"/></mappers>
```

3、我们去进行测试

```java
@Testpublic void testGetAllUser() {   SqlSession session = MybatisUtils.getSession();   //本质上利用了jvm的动态代理机制   UserMapper mapper = session.getMapper(UserMapper.class);   List<User> users = mapper.getAllUser();   for (User user : users){       System.out.println(user);  }   session.close();}
```

4、利用Debug查看本质

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LZwwtchlelS8kzAAyVia5uNnMiahVkdvictXZkDDWHQCwob9rMlKtxnhiaQee5Kxa6K0BCbHH2ibRERibQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



5、本质上利用了jvm的动态代理机制

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LZwwtchlelS8kzAAyVia5uNeukjWMleICg2Jsm8hTI63hvVLiarGmD7zT1CmgXlUXSUbmdhialeIjpA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



6、Mybatis详细的执行流程

![图片](https://mmbiz.qpic.cn/mmbiz_png/uJDAUKrGC7LZwwtchlelS8kzAAyVia5uNvhic22X8ahJy5BdOfjy1LlDRfo8Nf3GOAzwALgvriau4SzmXZIhUUd2A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



##   8.3、CRUD

改造MybatisUtils工具类的getSession( ) 方法，重载实现。

```
  //获取SqlSession连接  public static SqlSession getSession(){      return getSession(true); //事务自动提交  }   public static SqlSession getSession(boolean flag){      return sqlSessionFactory.openSession(flag);  }
```

【注意】确保实体类和数据库字段对应

### **查询：**

1、编写接口方法注解

```Java
//根据id查询用户@Select("select * from user where id = #{id}")User selectUserById(@Param("id") int id);
```

2、测试

```Java
@Testpublic void testSelectUserById() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   User user = mapper.selectUserById(1);   System.out.println(user);   session.close();}
```

### **新增：**

1、编写接口方法注解

```java
//添加一个用户@Insert("insert into user (id,name,pwd) values (#{id},#{name},#{pwd})")int addUser(User user);
```

2、测试

```java
@Testpublic void testAddUser() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   User user = new User(6, "秦疆", "123456");   mapper.addUser(user);   session.close();}
```

### **修改：**

1、编写接口方法注解

```java
//修改一个用户@Update("update user set name=#{name},pwd=#{pwd} where id = #{id}")int updateUser(User user);
```

2、测试

```java
@Testpublic void testUpdateUser() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   User user = new User(6, "秦疆", "zxcvbn");   mapper.updateUser(user);   session.close();}
```

### **删除：**

1、编写接口方法注解

```Java
//根据id删除用@Delete("delete from user where id = #{id}")int deleteUser(@Param("id")int id);
```

2、测试

```Java
@Testpublic void testDeleteUser() {   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   mapper.deleteUser(6);      session.close();}
```

【注意点：增删改一定记得对事务的处理】



## 8.4、关于@Param

有多个参数的时候使用@Param

@Param注解用于给方法参数起一个名字。以下是总结的使用原则：

- 在方法只接受一个参数的情况下，可以不使用@Param。
- 在方法接受多个参数的情况下，建议一定要使用@Param注解给参数命名。
- 如果参数是 JavaBean ， 则不能使用@Param。
- 不使用@Param注解时，参数只能有一个，并且是Javabean。

例如：

```java 
 User getUserByID(@Param("id") int id,@Param("name") String name)
```

Param中的名字优先级高于后面的int定义的名字



## 8.5、#{} 和 ${}  的区别：

- \#{} 的作用主要是替换预编译语句(PrepareStatement)中的占位符? 【推荐使用】

  ```sql
  INSERT INTO user (name) VALUES (#{name});INSERT INTO user (name) VALUES (?);
  ```

  

- ${} 的作用是直接进行字符串替换

  ```sql
  INSERT INTO user (name) VALUES ('${name}');INSERT INTO user (name) VALUES ('kuangshen');
  ```



# 9、Lombok

使用前先下载插件

导入jar包

````java 
<dependency>            <groupId>org.projectlombok</groupId>            <artifactId>lombok</artifactId>            <version>1.18.20</version>            <scope>provided</scope>        </dependency>
````

```java
val: final 像动态语言一样，声明一个fianl的变量。var: 同JDK10@Data：注解在类上，将类提供的所有属性都添加get、set方法，并添加、equals、canEquals、hashCode、toString方法@Setter：注解在类上，为所有属性添加set方法、注解在属性上为该属性提供set方法@Getter：注解在类上，为所有的属性添加get方法、注解在属性上为该属性提供get方法@NotNull：在参数中使用时，如果调用时传了null值，就会抛出空指针异常@Synchronized 用于方法，可以锁定指定的对象，如果不指定，则默认创建一个对象锁定@Log作用于类，创建一个log属性@Builder：使用builder模式创建对象@NoArgsConstructor：创建一个无参构造函数@AllArgsConstructor：创建一个全参构造函数@ToString：创建一个toString方法@Accessors(chain = true)使用链式设置属性，set方法返回的是this对象。@RequiredArgsConstructor：创建对象, 例: 在class上添加@RequiredArgsConstructor(staticName = "of")会创建生成一个静态方法@UtilityClass:工具类@ExtensionMethod:设置父类@FieldDefaults：设置属性的使用范围，如private、public等，也可以设置属性是否被final修饰。@Cleanup: 关闭流、连接点。@EqualsAndHashCode：重写equals和hashcode方法。@toString：创建toString方法。@Cleanup: 用于流等可以不需要关闭使用流对象.
```



**常用的就的@Data，可以直接生成：无参构造、hashcode、get、set、toString、equals**





# 10、多对一



多对一的理解：

- 多个学生对应一个老师
- 如果对于学生这边，就是一个多对一的现象，即从学生这边关联一个老师！

## 10.1、数据库设计

```mysql
CREATE TABLE `teacher` (`id` INT(10) NOT NULL,`name` VARCHAR(30) DEFAULT NULL,PRIMARY KEY (`id`)) ENGINE=INNODB DEFAULT CHARSET=utf8INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');CREATE TABLE `student` (`id` INT(10) NOT NULL,`name` VARCHAR(30) DEFAULT NULL,`tid` INT(10) DEFAULT NULL,PRIMARY KEY (`id`),KEY `fktid` (`tid`),CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)) ENGINE=INNODB DEFAULT CHARSET=utf8INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1');INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1');INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1');INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1');INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```



## 10.2、环境搭建

#### 1、IDEA安装Lombok插件

#### 2、引入Maven依赖

```xml
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok --><dependency> <groupId>org.projectlombok</groupId> <artifactId>lombok</artifactId> <version>1.16.10</version></dependency>
```

#### 3、在代码中增加注解

```java
@Data //GET,SET,ToString，有参，无参构造public class Teacher {   private int id;   private String name;}@Datapublic class Student {   private int id;   private String name;   //多个学生可以是同一个老师，即多对一   private Teacher teacher;}
```

#### 4、编写实体类对应的Mapper接口 【两个】

- **无论有没有需求，都应该写上，以备后来之需！**

```java
public interface StudentMapper {}public interface TeacherMapper {}
```

#### 5、编写Mapper接口对应的 mapper.xml配置文件 【两个】

- **无论有没有需求，都应该写上，以备后来之需！**

```xml
<?xml version="1.0" encoding="UTF-8" ?><!DOCTYPE mapper       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"       "http://mybatis.org/dtd/mybatis-3-mapper.dtd"><mapper namespace="com.kuang.mapper.StudentMapper"></mapper><?xml version="1.0" encoding="UTF-8" ?><!DOCTYPE mapper       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"       "http://mybatis.org/dtd/mybatis-3-mapper.dtd"><mapper namespace="com.kuang.mapper.TeacherMapper"></mapper>
```



## 10.3、按查询嵌套处理

1、给StudentMapper接口增加方法

```
//获取所有学生及对应老师的信息public List<Student> getStudents();
```

2、编写对应的Mapper文件

```xml
<?xml version="1.0" encoding="UTF-8" ?><!DOCTYPE mapper       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"       "http://mybatis.org/dtd/mybatis-3-mapper.dtd"><mapper namespace="com.kuang.mapper.StudentMapper">   <!--   需求：获取所有学生及对应老师的信息   思路：       1. 获取所有学生的信息       2. 根据获取的学生信息的老师ID->获取该老师的信息       3. 思考问题，这样学生的结果集中应该包含老师，该如何处理呢，数据库中我们一般使用关联查询？           1. 做一个结果集映射：StudentTeacher           2. StudentTeacher结果集的类型为 Student           3. 学生中老师的属性为teacher，对应数据库中为tid。              多个 [1,...）学生关联一个老师=> 一对一，一对多           4. 查看官网找到：association – 一个复杂类型的关联；使用它来处理关联查询   -->   <select id="getStudents" resultMap="StudentTeacher">    select * from student   </select>   <resultMap id="StudentTeacher" type="Student">       <!--association关联属性 property属性名 javaType属性类型 column在多的一方的表中的列名-->       <association property="teacher"  column="tid" javaType="Teacher" select="getTeacher"/>   </resultMap>   <!--   这里传递过来的id，只有一个属性的时候，下面可以写任何值   association中column多参数配置：       column="{key=value,key=value}"       其实就是键值对的形式，key是传给下个sql的取值名称，value是片段一中sql查询的字段名。   -->   <select id="getTeacher" resultType="teacher">      select * from teacher where id = #{id}   </select></mapper>
```

3、编写完毕去Mybatis配置文件中，注册Mapper！

4、注意点说明：

```xml
<resultMap id="StudentTeacher" type="Student">   <!--association关联属性 property属性名 javaType属性类型 column在多的一方的表中的列名-->   <association property="teacher"  column="{id=tid,name=tid}" javaType="Teacher" select="getTeacher"/></resultMap><!--这里传递过来的id，只有一个属性的时候，下面可以写任何值association中column多参数配置：   column="{key=value,key=value}"   其实就是键值对的形式，key是传给下个sql的取值名称，value是片段一中sql查询的字段名。--><select id="getTeacher" resultType="teacher">  select * from teacher where id = #{id} and name = #{name}</select>
```

5、测试

```java
@Testpublic void testGetStudents(){   SqlSession session = MybatisUtils.getSession();   StudentMapper mapper = session.getMapper(StudentMapper.class);   List<Student> students = mapper.getStudents();   for (Student student : students){       System.out.println(               "学生名:"+ student.getName()                       +"\t老师:"+student.getTeacher().getName());  }}
```



# 11、多对一

多对一的理解：

- 多个学生对应一个老师
- 如果对于学生这边，就是一个多对一的现象，即从学生这边关联一个老师！



# 12、动态SQL

## 12.1 if 语句

```mysql
<!--需求1：根据作者名字和博客名字来查询博客！如果作者名字为空，那么只根据博客名字查询，反之，则根据作者名来查询select * from blog where title = #{title} and author = #{author}--><select id="queryBlogIf" parameterType="map" resultType="blog">  select * from blog where   <if test="title != null">      title = #{title}   </if>   <if test="author != null">      and author = #{author}   </if></select>
```



## 12.2 where

```
<select id="queryBlogIf" parameterType="map" resultType="blog">  select * from blog   <where>       <if test="title != null">          title = #{title}       </if>       <if test="author != null">          and author = #{author}       </if>   </where></select>
```

这个“where”标签会知道如果它包含的标签中有返回值的话，它就插入一个‘where’。此外，如果标签返回的内容是以AND 或OR 开头的，则它会剔除掉。

## 12.3 set

```
<!--注意set是用的逗号隔开--><update id="updateBlog" parameterType="map">  update blog     <set>         <if test="title != null">            title = #{title},         </if>         <if test="author != null">            author = #{author}         </if>     </set>  where id = #{id};</update>
```



## 12.4 choose语句

```
<select id="queryBlogChoose" parameterType="map" resultType="blog">  select * from blog   <where>       <choose>           <when test="title != null">                title = #{title}           </when>           <when test="author != null">              and author = #{author}           </when>           <otherwise>              and views = #{views}           </otherwise>       </choose>   </where></select>
```

## 12.5 SQL片段

**提取SQL片段：**

```
<sql id="if-title-author">   <if test="title != null">      title = #{title}   </if>   <if test="author != null">      and author = #{author}   </if></sql>
```

**引用SQL片段：**

```
<select id="queryBlogIf" parameterType="map" resultType="blog">  select * from blog   <where>       <!-- 引用 sql 片段，如果refid 指定的不在本文件中，那么需要在前面加上 namespace -->       <include refid="if-title-author"></include>       <!-- 在这里还可以引用其他的 sql 片段 -->   </where></select>
```

注意：

①、最好基于 单表来定义 sql 片段，提高片段的可重用性

②、在 sql 片段中不要包括 where

## 12.6 Foreach

将数据库中前三个数据的id修改为1,2,3；

需求：我们需要查询 blog 表中 id 分别为1,2,3的博客信息

1、编写接口

```
List<Blog> queryBlogForeach(Map map);
```

2、编写SQL语句

```
<select id="queryBlogForeach" parameterType="map" resultType="blog">  select * from blog   <where>       <!--       collection:指定输入对象中的集合属性       item:每次遍历生成的对象       open:开始遍历时的拼接字符串       close:结束时拼接的字符串       separator:遍历对象之间需要拼接的字符串       select * from blog where 1=1 and (id=1 or id=2 or id=3)     -->       <foreach collection="ids"  item="id" open="and (" close=")" separator="or">          id=#{id}       </foreach>   </where></select>
```

# 13、缓存

- MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存。缓存可以极大的提升查询效率。

- MyBatis系统中默认定义了两级缓存：**一级缓存**和**二级缓存**

- - 默认情况下，只有一级缓存开启。（SqlSession级别的缓存，也称为本地缓存）
  - 二级缓存需要手动开启和配置，他是基于namespace级别的缓存。
  - 为了提高扩展性，MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存

## 13.1 一级缓存

一级缓存也叫本地缓存：

- 与数据库同一次会话期间查询到的数据会放在本地缓存中。
- 以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库；



> 测试

1、在mybatis中加入日志，方便测试结果

2、编写接口方法

```
//根据id查询用户User queryUserById(@Param("id") int id);
```

3、接口对应的Mapper文件

```
<select id="queryUserById" resultType="user">  select * from user where id = #{id}</select>
```

4、测试

```
@Test
public void testQueryUserById(){
   SqlSession session = MybatisUtils.getSession();
   UserMapper mapper = session.getMapper(UserMapper.class);

   User user = mapper.queryUserById(1);
   System.out.println(user);
   User user2 = mapper.queryUserById(1);
   System.out.println(user2);
   System.out.println(user==user2);

   session.close();
}
```

> 一级缓存失效的四种情况

一级缓存是SqlSession级别的缓存，是一直开启的，我们关闭不了它；

一级缓存失效情况：没有使用到当前的一级缓存，效果就是，还需要再向数据库中发起一次查询请求！

1、sqlSession不同

```
@Testpublic void testQueryUserById(){   SqlSession session = MybatisUtils.getSession();   SqlSession session2 = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   UserMapper mapper2 = session2.getMapper(UserMapper.class);   User user = mapper.queryUserById(1);   System.out.println(user);   User user2 = mapper2.queryUserById(1);   System.out.println(user2);   System.out.println(user==user2);   session.close();   session2.close();}
```

观察结果：发现发送了两条SQL语句！

结论：**每个sqlSession中的缓存相互独立**

2、sqlSession相同，查询条件不同

```
@Testpublic void testQueryUserById(){   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   UserMapper mapper2 = session.getMapper(UserMapper.class);   User user = mapper.queryUserById(1);   System.out.println(user);   User user2 = mapper2.queryUserById(2);   System.out.println(user2);   System.out.println(user==user2);   session.close();}
```

观察结果：发现发送了两条SQL语句！很正常的理解

结论：**当前缓存中，不存在这个数据**

3、sqlSession相同，两次查询之间执行了增删改操作！

增加方法

```
//修改用户int updateUser(Map map);
```

编写SQL

```
<update id="updateUser" parameterType="map">  update user set name = #{name} where id = #{id}</update>
```

测试

```
@Testpublic void testQueryUserById(){   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   User user = mapper.queryUserById(1);   System.out.println(user);   HashMap map = new HashMap();   map.put("name","kuangshen");   map.put("id",4);   mapper.updateUser(map);   User user2 = mapper.queryUserById(1);   System.out.println(user2);   System.out.println(user==user2);   session.close();}
```

观察结果：查询在中间执行了增删改操作后，重新执行了

结论：**因为增删改操作可能会对当前数据产生影响**

4、sqlSession相同，手动清除一级缓存

```
@Testpublic void testQueryUserById(){   SqlSession session = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   User user = mapper.queryUserById(1);   System.out.println(user);   session.clearCache();//手动清除缓存   User user2 = mapper.queryUserById(1);   System.out.println(user2);   System.out.println(user==user2);   session.close();}
```

一级缓存就是一个map



## 13.2 二级缓存

- 二级缓存也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存

- 基于namespace级别的缓存，一个名称空间，对应一个二级缓存；

- 工作机制

- - 一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中；
  - 如果当前会话关闭了，这个会话对应的一级缓存就没了；但是我们想要的是，会话关闭了，一级缓存中的数据被保存到二级缓存中；
  - 新的会话查询信息，就可以从二级缓存中获取内容；
  - 不同的mapper查出的数据会放在自己对应的缓存（map）中；

> 使用步骤

1、开启全局缓存 【mybatis-config.xml】

```
<setting name="cacheEnabled" value="true"/>
```

2、去每个mapper.xml中配置使用二级缓存，这个配置非常简单；【xxxMapper.xml】

```
<cache/>官方示例=====>查看官方文档<cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>这个更高级的配置创建了一个 FIFO 缓存，每隔 60 秒刷新，最多可以存储结果对象或列表的 512 个引用，而且返回的对象被认为是只读的，因此对它们进行修改可能会在不同线程中的调用者产生冲突。
```

3、代码测试

- 所有的实体类先实现序列化接口
- 测试代码

```
@Testpublic void testQueryUserById(){   SqlSession session = MybatisUtils.getSession();   SqlSession session2 = MybatisUtils.getSession();   UserMapper mapper = session.getMapper(UserMapper.class);   UserMapper mapper2 = session2.getMapper(UserMapper.class);   User user = mapper.queryUserById(1);   System.out.println(user);   session.close();   User user2 = mapper2.queryUserById(1);   System.out.println(user2);   System.out.println(user==user2);   session2.close();}
```

> 结论

- 只要开启了二级缓存，我们在同一个Mapper中的查询，可以在二级缓存中拿到数据
- 查出的数据都会被默认先放在一级缓存中
- 只有会话提交或者关闭以后，一级缓存中的数据才会转到二级缓存中



# 14、逆向工程

mybaits需要程序员自己编写sql语句，mybatis官方提供逆向工程，可以针对单表自动生成mybatis执行所需要的代码（mapper.java,mapper.xml、po..）

## 1、下载逆向工程文件

```xml
<build>
    <finalName>SpringMVCBasic</finalName>
    <!-- 添加mybatis-generator-maven-plugin插件 -->
    <plugins>
      <plugin>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>1.3.2</version>
        <configuration>
          <verbose>true</verbose>
          <overwrite>true</overwrite>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

## 2、生成代码配置文件

在maven项目下的src/main/resources 目录下新建`generatorConfig.xml`和`generator.properties`配置文件

### **generator.properties**：

```properties
jdbc.driverLocation=C:\\Users\\Yvettee\\.m2\\repository\\mysql\\mysql-connector-java\\5.1.18\\mysql-connector-java-5.1.18.jar
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=utf-8
jdbc.userId=root
jdbc.password=rootjdbc.driverLocation=C:\\Users\\Yvettee\\.m2\\repository\\mysql\\mysql-connector-java\\5.1.18\\mysql-connector-java-5.1.18.jar
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=utf-8
jdbc.userId=root
jdbc.password=root
```

### **generatorConfig.xml**:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--导入属性配置-->
    <properties resource="generator.properties"></properties>

    <!--指定特定数据库的jdbc驱动jar包的位置(绝对路径)-->
    <classPathEntry location="${jdbc.driverLocation}"/>

    <context id="default" targetRuntime="MyBatis3">

        <!-- optional，旨在创建class时，对注释进行控制 -->
        <commentGenerator>
            <!--是否去掉自动生成的注释 true:是-->
            <property name="suppressDate" value="true"/>
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!--jdbc的数据库连接：驱动类、链接地址、用户名、密码-->
        <jdbcConnection
                driverClass="${jdbc.driverClass}"
                connectionURL="${jdbc.connectionURL}"
                userId="${jdbc.userId}"
                password="${jdbc.password}">
        </jdbcConnection>


        <!-- 非必需，类型处理器，在数据库类型和java类型之间的转换控制-->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>


        <!-- Model模型生成器,用来生成含有主键key的类，记录类 以及查询Example类
            targetPackage     指定生成的model生成所在的包名
            targetProject     指定在该项目下所在的路径
        -->
        <javaModelGenerator targetPackage="com.eurasia.pojo"
                            targetProject="src/main/java">

            <!-- 是否允许子包，即targetPackage.schemaName.tableName -->
            <property name="enableSubPackages" value="false"/>
            <!-- 是否对model添加 构造函数 -->
            <property name="constructorBased" value="true"/>
            <!-- 是否对类CHAR类型的列的数据进行trim操作 -->
            <property name="trimStrings" value="true"/>
            <!-- 建立的Model对象是否 不可改变  即生成的Model对象不会有 setter方法，只有构造方法 -->
            <property name="immutable" value="false"/>
        </javaModelGenerator>

        <!--Mapper映射文件生成所在的目录 为每一个数据库的表生成对应的SqlMap文件 -->
        <sqlMapGenerator targetPackage="com.eurasia.dao"
                         targetProject="src/main/java">
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- 客户端代码，生成易于使用的针对Model对象和XML配置文件 的代码
                type="ANNOTATEDMAPPER",生成Java Model 和基于注解的Mapper对象
                type="MIXEDMAPPER",生成基于注解的Java Model 和相应的Mapper对象
                type="XMLMAPPER",生成SQLMap XML文件和独立的Mapper接口
        -->
        <javaClientGenerator targetPackage="com.eurasia.mapper"
                             targetProject="src/main/java" type="XMLMAPPER">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <!-- 数据表进行生成操作 tableName:表名; domainObjectName:对应的DO -->
        <table tableName="items" domainObjectName="Items"
               enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>

        <table tableName="orderdetail" domainObjectName="Orderdetail"
               enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>
        <table tableName="orders" domainObjectName="Orders"
               enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>
        <table tableName="user" domainObjectName="User"
               enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>

    </context>
</generatorConfiguration>
```

## 3. 在Intellij IDEA添加一个“Run运行”选项

使用maven运行mybatis-generator-maven-plugin插件

![img](https://upload-images.jianshu.io/upload_images/1037735-86549bcd76a36fe1.png?imageMogr2/auto-orient/strip|imageView2/2/w/530/format/webp)

![img](https://upload-images.jianshu.io/upload_images/1037735-298dc7614958358f.png?imageMogr2/auto-orient/strip|imageView2/2/w/317/format/webp)

## 4. 在name和Commond line分别填上如上图所示，再点击Apply和OK。

![img](https://upload-images.jianshu.io/upload_images/1037735-40a49e1e6f7b796e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)
