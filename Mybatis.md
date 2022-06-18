# 1 三层架构

1. 界面层
2. 业务逻辑层
3. 数据访问层

+ 常用的框架SSM
  + spring：核心是IOC和AOP 跨领域提供了好的解决方案
  + springMVBC：专门用来优化控制器的，提供了既简单的数据提交，数据携带，页面跳转的功能
  + Mybatis：是一个持久层的框架，用来进行数据库访问的优化，专注于sql语句，极大的优化了JDBC的访问。

# 2 框架

+ JDBC优化的框架，
+ 什么是框架(MyBatis)
  + MyBatis是一个半成品，将重复的功能解决掉，帮助程序快速高效的进行开发，他是可复用的，可扩展的。前身叫iBatis 是APache开发的
  + 简化了JDBC的访问
+ 添加框架的步骤
  + 添加依赖
  + 添加配置文件

## 2.1 MyBatis

### 2.1.1  mybatis查询操作

1. 新建一个students表格

2. 加入maven的mybatis坐标，mysql驱动的坐标

3. 创建实体类，students--保存在表中的一行数据

4. 创建持久层的dao接口

5. 创建一个mybatis配置文件

   叫做sql映射文件：写sql语句。一般一个表一个隐射文件

   则个文件是xml文件 

   需要特别注意的是：文件写在接口所在的目录中 文件的名称要和接口保持一致

   

   + 接口的模式 实例

   ```java
   package com.sea.case_1;
   
   import com.sea.domain_case_1.Students;
   
   import java.util.List;
   
   //接口操作students表
   public interface StudentsDao{
       //查询students表单所有的数据
       public List<Students> selectStudents();
       //students标识要插入的数据 返回的是受影响的行数
       public int addStudents(Students students);
   
   }
   ```

   + sql的配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.sea.case_1.StudentsDao">
       <select id="selectStudents" resultType="com.sea.domain_case_1.Students">
           select stdtno,name,email from students order by stdtno;
       </select>
       <!--resulType:表示结构类型的，是sql语句执行后得到resultSet，遍历这个ResultSet得到的对象类型 需要写的是类型的全限定名称-->
               <!--id表示你要执行的sql语句的唯一表示 你的mybatis会使用设个id之找到要执行的sql语句
               可以自定义，但是要求你使用接口中的方法名称-->
   </mapper>
   <!--
   sql映射文件：写sql语句，mybatis会执行执行sql
       1. 指定约束文件
       <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   
       mybatis-3-mapper.dtd是约束文件的名称，扩展名是dtd的。
       2. 约束文件的作用：限制和检查当前文件中出现的标签，属性必须符合mybatis要求
       <mapper namespace="org.mybatis.example.BlogMapper">
       <select id="selectBlog" resultType="Blog">
           select * from Students where stdtno = #{id}
       </select>
       </mapper>
       3.mapper 他是当前文件的根标签
       namespace 命名空间 是唯一值 可以是自定义的字符串 要求使用dao的全限定名称
       4.在当前文件中，可以使用特定的标签，表示数据库的特定操作。
           <select>：表示执行查询操作
           <update>:表示执行更新操作 就是说执行的是update的sql语句
           <insert>:查入
           <delete>:删除
   
   -->
   ```

6. 创建mybatis的主配置文件：

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <environments default="development">
           <!--数据库环境的配置
               id：唯一值表示环境的名称
               default:必须和某个environment的id值一样
               告诉你的mybatis使用那个连接信息 也即是用哪一个数据库
               -->
           <environment id="development">
               <!--transactionManager 事务的类型
               其中type的值有两种：
                   1.JDBC（表示使用JBDC中的对象进行事务处理）-->
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <!--dataSource 表示数据员 type是数据源的类型 pooled表示连接池-->
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3066/mybase?serverTimezone=GMT"/>
                   <property name="username" value="root"/>
                   <property name="password" value="why123456"/>
               </dataSource>
           </environment>
   
   
   
   
   
       </environments>
   
   
       <mappers>
           <mapper resource="org/mybatis/example/BlogMapper.xml"/>
       </mappers>
   </configuration>
   <!--第一眼 感觉是连接数据库
       mybatis的主配置文件：主要定义了数据库的配置信息，sql映射文件的位置。
       1约束文件的说明：
       <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
           dtd/mybatis-3-config.dtd是约束文件的名称 是固定值
       2configuration 跟标签 表示各种配置信息
   
       -->
   ```

   一个项目就一个主配置文件

   主配置文件提供了数据库的连接信息和sql隐射文件的位置信息

   ```xml
   <build>
     <resources>
       <resource>
         <directory>src/main/java</directory>
         <includes>
           <include>**/*.properties</include>
           <include>**/*.xml</include>
         </includes>
         <filtering>false</filtering>
       </resource>
     </resources>
   </build>
   ```

   + 在pom文件中的build标签中加上上面的语句可以编译非resource目录中的xml和properties文件

7. 创建使用mybatis的类

   提供mybatis访问数据库

   ```java
   package com.sea;
   
   
   import com.sea.domain_case_1.Students;
   import org.apache.ibatis.io.Resources;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   import org.apache.ibatis.session.SqlSessionFactoryBuilder;
   
   import java.io.IOException;
   import java.io.InputStream;
   import java.util.List;
   
   public class Myapp {
       public static void main(String[] args) throws IOException {
           //访问mybatis读取students数据
           //1.定义mybatis主配置文件的名称:从类路径的根开始
           String config="MyBatisX.xml";
           //读取config表示的文件
           InputStream in = Resources.getResourceAsStream(config);
           //3.创建sqlSessionFactoryBuilder对象
           SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
           //4.创建sqlSessionFactory对象
           SqlSessionFactory factory = builder.build(in);
           //5.【重要】获取sqlSession对象，从sqlSessionFactory中获取sqlSession
           SqlSession sqlSession = factory.openSession();
           //6.【重要】指定要执行的sql语句的标识。 使用的sql映射文件的namespace+"."+标签的id
           String SqlId = "com.sea.case_1.StudentsDao.selectStudents";
           //7.执行sql语句 提供sqlId找到语句
           List<Students> studentsList = sqlSession.selectList(SqlId);
           //8 输出结果
           for (Students stdt: studentsList){
               System.out.println(stdt);
           }
           //9.关闭sqlSession
           sqlSession.close();
   
   
       }
   }
   ```

   ```java
   public class MyConclusion {
       @Test
       public void Test() throws IOException {
           InputStream is = Resources.getResourceAsStream("MyBatisX.xml"); //导入全局配置文件
           SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();//创建一个构造
           SqlSessionFactory build = builder.build(is);//使用构造和流创建一个session工厂类
           SqlSession session = build.openSession();//使用session工程类打开session
           List<Students> list = session.selectList("com.sea.case_1.StudentsDao.selectStudents"); //写sql语句的文件地址 执行后会返回结果
           session.close(); //关闭session
           //遍历得到的容器
           for (Students stdt:
                list) {
               System.out.println(stdt);
           }
       }
   }
   ```



需要注意的是mysql的端口号是3306；

### 2.1.1  mybatis的insert操作

```html
<insert id="addStudents">
    insert into students(stdtno,name,email) value(#{stdtno},#{name},#{email}); //{}里面填写的是类内属性的名称
</insert>
```

```java
package com.sea;

import com.sea.domain_case_1.Students;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MyConclusion {
    @Test
    public void Test1() throws IOException {
        InputStream is = Resources.getResourceAsStream("MyBatisX.xml"); //导入全局配置文件
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();//创建一个构造
        SqlSessionFactory build = builder.build(is);//使用构造和流创建一个session工厂类
        SqlSession session = build.openSession();//使用session工程类打开session
        List<Students> list = session.selectList("com.sea.case_1.StudentsDao.selectStudents"); //写sql语句的文件地址 执行后会返回结果
        session.close(); //关闭session
        //遍历得到的容器
        for (Students stdt:
             list) {
            System.out.println(stdt);
        }


    }
    @Test
    public void Test2() throws IOException {
        Students stdt15 = new Students(15, "姜维", "jiangwei@gmail.com");
        InputStream is = Resources.getResourceAsStream("MyBatisX.xml"); //导入全局配置文件
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();//创建一个构造
        SqlSessionFactory build = builder.build(is);//使用构造和流创建一个session工厂类
        SqlSession session = build.openSession();//使用session工程类打开session
        int insert = 0;//写sql语句的文件地址 执行后会返回结果
        try {
            insert = session.insert("com.sea.case_1.StudentsDao.addStudents",stdt15);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }finally {
            if(insert == 1) {
                session.commit();
                System.out.println("affect row number "+insert);
            }else{
                System.out.println("insert false");
            }
        }

        List<Students> list = session.selectList("com.sea.case_1.StudentsDao.selectStudents");
        //遍历得到的容器

        session.close();
        for (Students stdt:
                list) {
            System.out.println(stdt);
        }


    }
}
```

### 2.2.3 打开日志

实际上mybatis文档里面是直接提供了的

```xml
<!--setting :控制mybatis全局行为-->
<settings>
    <!--设置，ybatis输出日志-->
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

# 3 主要的类的介绍

## 3.1主要类的介绍

 1. Resources：mybatis中的一个类。负责读取主配置文件

    ```java
        InputStream is = Resources.getResourceAsStream("MyBatisX.xml"); //导入全局配置文件
    ```

	2. SqlSessionFactoryBuilder:创建SqlSessionFactory对象 使用主配置转化的流来创建

    ```java
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(is);
    ```

	3. SqlSessionFactory：重量级对象，程序创建一个对象耗时比较长，使用资源比较多

    整个项目有一个就好

    ```java
    SqlSessionFactory build = new SqlSessionFactoryBuilder().build(is);
    //方法说明
    build.openSession(); //无参数的，获取是自动提交事务的sqlSession对象
    build.openSession(boolean);// 如果是true的话获取的是非自动提交事务的对象 如果是false的话获取的是自动提交事务。
    ```

    用于获取SqlSession对象

    

	4. sqlsession:

    + 接口：定义了操作数据的方法：例如 selectOne(),selectList(),insert(),update(),delete(),commit(),rollback()
    + sqlSession接口的实现类DefualtSqlSession

    

    + 使用要求：sqlsession不是线程安全的，需要在方法的内部执行，在执行sql语句之情，使用opensession()获取sqlsession对象

      在执行完sql语句后，需要关闭它，执行sqlsession.close() 这样才能保证线程是安全的。

    

    + 工具类

    ```java
    package com.sea.domain_case_1;
    
    import org.apache.ibatis.io.Resources;
    import org.apache.ibatis.session.SqlSession;
    import org.apache.ibatis.session.SqlSessionFactory;
    import org.apache.ibatis.session.SqlSessionFactoryBuilder;
    
    import java.io.IOException;
    import java.io.InputStream;
    import java.util.ResourceBundle;
    
    public class MyUtil {
        private static SqlSessionFactory build =null;
        static{
            try {
                InputStream Stream = Resources.getResourceAsStream("MyBatisX.xml");
                //创建sqlsessionfactory对象，使用SqlSessionFactoryBuild
                build = new SqlSessionFactoryBuilder().build(Stream);
    
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
    
        }
    
        public static SqlSession SqlSession(){
            return build.openSession();
        }
    
    }
    ```

## 3.2 传统的DAO操作方式：

```java
package com.DAO.Impl;

import com.DAO.StudentsDAO;
import com.sea.Students;
import org.apache.ibatis.session.SqlSession;

import java.util.List;

public class StudentsDAOImpl implements StudentsDAO {
    @Override
    public List<Students> selectStudents() {
        //获取sqlSession对象
        SqlSession sqlsession = MyUtil.SqlSession();
        List<Students> studentsList = sqlsession.selectList("com.DAO.StudentsDAO.selectStudents");
        for (Students stdt:
                studentsList
             ) {
            System.out.println(stdt);
        }
        sqlsession.close();

        return studentsList;
    }

    @Override
    public int insertStudent(Students stu) {
        SqlSession sqlsession = MyUtil.SqlSession();
        Students students = new Students();
        int i = sqlsession.insert("com.DAO.StudentsDAO.insertStudent",stu);
        sqlsession.commit();
        sqlsession.close();
        return i;
    }
}
```

+ mybatis的动态代理：mybatis根据dao的方法调用，获取执行sql语句的信息
  + mybatis根据你的接口，创建一个dao接口的实现类，并且创建这个类的对象。 用反射获取接口类的全路径名和方法名  配置namespace和id 即实现类不需要再创建了。
  + 完成sqlsession调用方法，访问数据库。
  + session.getMapper(studentsDao.class);

1. 动态代理：使用SqlSession.getMapper(DAO接口.class)获取这个DAO接口的对象

2. 传入参数：从java代码中把数据传入到mapper的sql语句中。

3. parameterType：是写在mapper文件中的一个属性 表示dao接口中的方法的参数数据类型

   例如：StudentsDAO接口

   public Students selectStudents(String id)

   + 需要注意的是parameter()是不必要的 不写mybatis也可以通过反射机制知道这个参数

   + 传入一个简单参数：

   + ```java
     /* 传一个简单类型的参数：
     *   简单类型：mybatis把java中的基本数据类型和String都叫做简单数据类型
     *
     * 在mapper中要获取简单类型的一个参数使用#{任意字符};
     *    List<Students> selectStudents(Integer stdtno);
     *         select stdtno,name,email from students where stdtno=#{Studentsid};
     * 使用#{}之后，mybatis执行sql是使用的jdbc中的preparestatement对象
     * 由mybatis执行下面的代码：
     * #{}可以防止sql注入 比较安全
     */
     ```

4. ## 如何传入多个参数？

   1. ### 多个参数，使用@param命名参数

      + 接口 public List<Studuents> selectMulitParam(@param("myname") String name,@param("myage") Integer age);

      + 使用@param("参数名") 命名参数

      + ```xml
        <select>
        	select * from students where name=#{myname,javaType=string,jdbcType=VARCHAR} or age=#{myage};
        </select>
        ```

      + ### 使用对象传参

        + 创建一个类保存需要查询的信息 在mapper中使用
          + 使用对象的属性值作为参数的语法标准
            + 使用对象的语法：#{属性名,javaType=类型名称，jdbcType=数据类型} 最完整的语法格式 很少用
              + 常常只写属性名

+ java类的别名全是小写 string int integer  
+ database类的别名都是大写的 VARCHAR INT CLOB
+ 总结：#{}是用来匹配参数的 

# 4 mybatis

## 4.1 ResultType

+ ### （1）ResultType：mybatis的输出结果类型，指sql语句执行完之后，数据转化为的java对象。java类型是任意的。

+ ResultType： 1. 结果类型的全限定名称  2. 类的别名

+ 处理的方式：

  1. mybatis执行sql语句，然后mybatis调用类的无参构造方法，创建对象。

  2. mybatis把ResultSet指定的列值 赋值给同名的属性。

     ```xml
     <select id="selectStudents" parameterType="int" resultType="com.sea.Students">
         select stdtno,name,email from students where stdtno=#{Studentsid};
     </select>
     ```

相当于

```java
ResultSet rs = executeselete("select stdtno,name,email from students where stdtno=#{Studentsid};")
while(re.next()){
    Students student = new Students();
    student.setName();
    student.setstdtno();
    ...
}
```

## 4.2 定义自定义类的别名

1. 在mybatis主配置文件中定义，使用<TypeAllas>定义别名
2. 可以在mapper的ResultType中使用自定义的别名

```xml
<typeAliases>
    <!--<typeAlias type="com.sea.Hero" alias="hero"/>-->
    <package name="com.sea"/>
    <!--<package>name是包名，这个包中的所有类，类名就是别名（类名不区分大小写） 这个好-->
</typeAliases>
```

+ 返回Map
  + 不建议使用 使用手册说了

## 4.3 ResultMap

+ ResultMap 叫做结果映射，指定列名和java对象属性对应关系。

  + 你可以自定义列值赋给那个属性

  + 当你的列名和属性不同时，一定要使用resultmap

  + ```xml
    <resultMap id="studentsToHeroMap" type="Hero">   map 映射
        <result column="stdtno" property="Herono"></result>
    </resultMap>
    <select id="transformToHero" resultType="hero" resultMap="studentsToHeroMap">
        select name,stdtno from students;
    </select>
    ```

## 4.4 模糊匹配

### 4.4.1 method 1

```java
//第一种like
List<Students> NotClearSearch(String name);
```

```xml
<select id="NotClearSearch" resultType="com.sea.Students">
    select stdtno,name,email from students where name like #{aimmyname};
</select>
```

### 4.4.1 method 2

```java
List<Students> NotClearSearch(String name);
```

```xml
<select id="NotClearSearch" resultType="com.sea.Students">
    select stdtno,name,email from students where name like "%"#{aimname}"%";
</select>
```

## 5 动态sql  if-where-forEach 

+ 动态sql的内容是变化的，可以根据条件获取到不同的sql语句。

  + 主要是where部分变化

+ 动态sql的实现，使用的是mybatis提供的标签，<if>,<where>,<foreach>

  1. if 是判断条件：

     + 语法

     ```xml
     <if test="判断java对象的属性值">部分sql语句</if>
     ```

     ```xml
     <mapper namespace="com.DAO.StudentsDAO">
         <select id="selectStudentsId" resultType="com.sea.Students">
             select name,email,stdtno from students
             where
             <if test="name!=null and name!=''"> 注意左边的那么是指参数的 而不是数据库列属性名
                 name=#{name}
             </if>
             <if test="stdtno>0">
                 and stdtno>#{stdtno}
             </if>
     	
         </select>
     </mapper>
     ```

2. <where> 
   + 用来包含多个<if>的，当多个if有一个成立的，<where>会自动增加一个where 关键字，并去掉if中多余的and，or等

```xml
<where>
    <if test="name!=null and name!=''">
        name=#{name}
    </if>
    <if test="stdtno>0">
        and stdtno>#{stdtno}
    </if>
</where>
```

3. <for-each>

   + 循环java中的数组，list集合的。主要用在sql的in语句中。
     + 学生stdtno是1，2，3的三个学生

   ```sql
   select * from students where stdtno in (1,2,3)
   ```

   ```java
   public List<Students> mutipleselect(){List<Integer> idlist}
   
   List<Integer> list = new ...;
   
   list.add(1);
   
   list.add(2);
   
   list.add(3);
   
   dao.multipleselect(list)
   ```

   ```xml
   <select id="selectStudentsForEach" resultType="com.sea.Students">
       select * from students where id in 
                              <foreach collection="" item="" open="" close="" separator="">
                                  
                              </foreach>
   </select>
   ```



+ collcation 表示接口中的方法的参数的类型，如果 是数组使用array，如果是list集合使用list

+ item 自定义的表示数组和集合成员的变量

+ open 循环开始时的字符

+ close 循环结束时的字符

+ separator 集合成员之间的分隔符

  + 参数为简单数据类型

  ```xml
   </select>
      <!--
      foreash 使用1-->
      <select id="selectStudentsForEach" resultType="com.sea.Students">
          select * from students where stdtno in
                                 <foreach collection="list" item="stdtno" open="(" close=")" separator=",">
                                  #{stdtno}
                                 </foreach>
      </select>
  ```

  + 参数为自定义类型 (stu)

  ```xml
   </select>
      <!--
      foreash 使用1-->
      <select id="selectStudentsForEach" resultType="com.sea.Students">
          select * from students where stdtno in
                                 <foreach collection="list" item="stu" open="(" close=")" separator=",">
                                  #{stu.stdtno}
                                 </foreach>
      </select>
  ```

+ sql代码片段，就是服用一些方法

  + 步骤
  + 先定义<sql id="自定义唯一标识符" > sql语句</sql>
  + 在使用<include refid="目标id"/>

  ```xml
  <sql id="select">
      select name,email,stdtno from students
  </sql>
  <select id="selectStudentsId" resultType="com.sea.Students">
      <include refid="select"/>
      where 1=1
      <if test="name!=null and name!=''">
          and name=#{name}
      </if>
      <if test="stdtno>0">
          and stdtno>#{stdtno}
      </if>
  ```

# 5 主配置文件

+ transaction事务处理的类型：

  + JDBC：表示mybatis底层使用的是调用jdbc中的connection对象的，commit和rollback
  + MANAGER：把mybatis的事务管理委托给其他容器(一个服务器软件，一个框架叫spring)

+ dataSource:表示数据源，java体系中，规定了java.sql.DataSource接口都是数据原

  + 数据源表示connection对象的

  + type：指定数据源的类型

    + POOLED：使用连接池，mybatis会创建pooleddataSource类

    + upooled：不使用连接池，在每次执行sql语句时，先创建连接，再执行sql，在关闭连接

      ​		mybatis会创建一个unpooledDataSource，管理Connection对象的使用

    + JNDI：java命名和目录服务(Windows注册表)

+ 数据库的属性配置文件：把数据库连接信息放到一个单独的文件中，和mybatis主配置文件分开

+ 目的是便于修改，保存和处理数据库的信息

  + 在recources目录中定义一个属性配置文件，xxx.properties例如 jdbc,properties在属性配置文件中，定义数据，格式是key=value
  + key一般使用多级目录的

### ${}的使用

+ 在mybatis的主配置文件中，使用<property>指定文件的位置 property resource
  + 在需要使用的地方，${key}

### 多mapper的处理方式

+ 第一种是在主配置文件中配置多个mapper  resource

+ 第二种使用包名 package name

  + name：xml文件（mapper文件）所在的包名，这个包所有的xml文件一次都能加载到mybatis内
    + 使用package 的要求
      1. mapper文件名称要和接口名称一样，区分的小写的一样
      2. mapper文件和DAO的接口要在同一个目录中

+ pagehelper数据分页的

+ 添加依赖

  ```maven
  <dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.1.10</version>
  </dependency>
  ```

```java
PageHelper.startPage(1,3);  //startPage(pageNum,pageSize)  pageNum第几页 pageSize 页面大小
List StudentsList = studentsDAO.selectStuendts();
studentsList( stu -> sysout.out.println(stu));

```









































































