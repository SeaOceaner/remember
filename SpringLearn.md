# Spring简介

+ Spring全家桶：spring，springmvc，spring boot，spring cloud
+ Spring：出现在2002左右，解决企业开发的难度。减轻对项目模块之间的管理类和类之间的关系，帮助开发人员创建对象，管理对象之间的关系。
+ Spring核心技术：IOC AOP。能实现模块之间，类之间的解耦合。
+ 依赖：classA中使用classB的属性或者方法，叫做classA依赖classB
+ 框架怎么学？
  + 知道框架可以做什么？mybatis------>访问数据库，对表中的数据进行增删查改
  + 框架的语法，框架要完成一个功能，需要一定的步骤支持。
  + 框架内的实现原理。
  + 通过学习实现一个框架
  + 写框架，实现一个框架

# 1 IOC(inverse of control)

+ 把对象的创建，赋值，管理工作都交给代码外的容器实现的，也就是说对象的创建是由其他的外部资源来实现的。

  + 正转：有开发人员在代码中用new的方式创建对象

  + ```java
    public static void main(){
        Students stu = new Students(); //在代码中创建对象
        
    }
    ```

  + 反转：把原来开发人员管理的创建的对象的权限转交给代码之外的容器实现，由容器替代发发人员管理对象，创建对象，给对象赋值等工作。

+ 容器：是一个服务器软件，一个框架(Spring)

+ 为什么要使用IOC呢？

  + 能够减少对代码的改动，也能实现不同的功能。 实现解耦合。

+ java中创建对象有哪些方式？

  + 构造方法 new students
  + 反射 class.forname
  + 序列化
  + 克隆
  + IOC 容器创建对象
  + 动态代理

+ IOC的一个体现：

  + servlet 创建一个类继承Httpservlet

  + 在web.xml注册servlet.

    + ```xml
      <servlet>
          <servlet-name>myservlet</servlet-name>
          <servelt-class>com.sea.Myservlet</servelt-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>myservlet</servlet-name>
          <url-parttern>/welcome</url-parttern>
      </servlet-mapping>
      
      ```

  + .我们没有创建过servlet对象 没有 Myservlet servlet= new Myservlet();

    + 为什么没有创建servlet为什么还能使用呢？
      + 是tomcat服务器创建的 tomcat也称为容器 放的是servlet对象 当然还有其他的 Listener Filter

+ IOC的技术实现

  + 主要是用到了DI(Dependency injection)，DI是IOC的技术实现

  + DI依赖注入

  + DI是指我们只需要在程序中提供要使用的对象的名称就可以了至于对象如何在容器中创建和赋值查找都有容器内部实现。

+ Spring是使用的DI实现了IOC的功能，Spring底层创建对象使用的原理是反射机制。



+ 实现步骤
  + 创建maven项目
  + 加入maven依赖
    + spring的依赖，版本5.2.5版本
    + junit依赖

-----------------------------------------------

+ 创建类(接口和她的实现类)	
  + 和没有使用框架一样，就是普通的类
+ 创建spring需要使用的配置文件
  + 声明类的信息，这些类由spring创建和管理
  + 通过spring的语法，完成属性的配置

---------------------------------------------------

+ 测试spring是否创建了对象

  ```html
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd">
      <!--告诉spring创建对象
      声明bean 就是告诉spring要创建某个类的对象
      id：对象的自定义名称，唯一值。spring通过这个名称找到对象
      class：类的全路径名(不能是接口，因为spring创建对象是反射机制创建对象)
      spring就完成了 Someservice someService = new SomeserviceImpl()；
      spring会把创建好的对象放入到map中，
      springMap.(id值,对象);
      springMap.("someservices",new SomeserviceImpl());
      一个bean标签生成一个对象
      -->
  
      <bean id="someServices" class="com.sea.impl.SomeServicesImpl">
  
      </bean>
  
  </beans>
  <!--
  spring配置文件
      1.beans：是根目录，spring把java对象成为bean。
      2.spring-beans.xsd是约束文件 和mybatis指定 dtd是一样的
  -->        
  
  
  ```

  ```java
  @Test
  public void test2(){
      //使用spring容器创建对象
      //指定spring配置文件的名称
      String config="beans.xml";
      //2 创建表示spring容器的对象 ApplicationContext
      //Application就是spring存放对象的容器，通过容器获取对象了 他是一个接口
      //ClassPathXmlApplicationContext：表示从类路径中加载spring的配置文件
      ApplicationContext context = new ClassPathXmlApplicationContext(config);
      //从容器中要获取对象，要调用对象的方法
      //getbean("bean id")
      SomeServices someServices = (SomeServicesImpl)context.getBean("someServices");
      //使用spring创建好的对象执行方法
      someServices.dosome();
  
  
  
  }
  ```

  + 三部曲：
    +  1 创建容器
    + 2 使用getbean()，获取对象
    + 3 调用方法执行功能

  		

  + 通过给对象添加无参构造方法 可知对象实例实在ClassPathXmlApplicationContext()时创建的；
  + ClassPathXmlApplicationContext()执行时 会将xml中配置的所有bean一并初始化

## 1.1 如何给java对象赋值

+ 两种配置方式
  + 1 xml文件配置
  + 2 基于注解配置
+ di语法分类
  + set注入：spring调用类的set方法，在set方法可以实现属性的赋值
  + 构造注入：spring调用类的有参方法，创建对象，在构造方法中实现赋值

### 1.1.1 set注入

+ ```html
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
      <bean id="myApp" class="com.sea.App">
          <property name="name" value="wanghaiyang"></property>
          <property name="age" value="23"></property>
      </bean>
      <!--需要特别注意的是当你的类中含有有参构造方法时 一定要写一个无参构造方法
  	set注入中 类中需要有set方法 不然是会报错的-->
  
  
      
  </beans>
  ```

+ 非简单参数的注入

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="myApp" class="com.sea.App">
        <property name="name" value="wanghaiyang"></property>
        <property name="age" value="23"></property>
        <property name="phone" ref="phone"></property>
    </bean>

    <bean id="myDate" class="java.util.Date">
        <property name="time" value="123151564646"></property>
    </bean>

    <bean id="phone" class="com.sea.Phone">
        <property name="name" value="apple15"></property>
        <property name="IP" value="156:98:154:2"></property>
    </bean>



</beans>
```

### 1.1.2  构造注入

示例：

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
    <constructor-arg>
      <constructor-arg>标签：一个<constructor-arg>标签表示一个构造方法的一个参数
      <constructor-arg>标签属性：
        name：表示构造方法的形参名
        index：表示构造方法的参数的位置(从左向右是0 1 2 3 ...)
        value：构造的形参类型是简单类型的可以使用
        ref：构造方法是一个引用类型的可以使用
    -->
    <bean id="myApp" class="com.sea.App">
        <constructor-arg name="age" value="22"></constructor-arg>
        <constructor-arg name="name" value="lisi"></constructor-arg>
        <constructor-arg name="phone" ref="phone"></constructor-arg>
    </bean>

    <bean id="myDate" class="java.util.Date">
        <property name="time" value="123151564646"></property>
    </bean>

    <bean id="phone" class="com.sea.Phone">
        <constructor-arg name="IP" value="123:123:123:5"></constructor-arg>
        <constructor-arg name="name" value="Apple_Maxpro"></constructor-arg>
    </bean>



</beans>
```

+ 注意：写一个Constructor-arg就会调用有对应参数数目的构造器 所有一定把构造器需要的参数写全 不然会报错

+ 引用参数的自动注入 

+ byname()

+ ```xml
         <bean id="app" class="com.sea.App" autowire="byName">
         <property name="name" value="lisi"></property>
         <property name="age" value="23"></property>
         <!--<property name="phone" ref="phone"></property>-->
         <!--引用类型的自动注入 只对引用类型有效-->
         <!--使用的规则是byname，bytype-->
         <!--byname：java类中引用类型的属性名和spring容器中（配置文件）<bean>的id名称一样，且数据类型是一致的，这样的容器中的bean，spring能够赋值给引用类型
                     语法规则：<bean>标签里面加一个 autowire="byname"-->
     ```


      </bean>
      
      <bean id="phone" class="com.sea.Phone">
          <constructor-arg name="IP" value="123:123:123:5"></constructor-arg>
          <constructor-arg name="name" value="Apple_Maxpro"></constructor-arg>
      </bean>
  ```

  + bytype()

  + ```html
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="app" class="com.sea.App" autowire="byType">
            <property name="name" value="lisi"></property>
            <property name="age" value="23"></property>
            <!--2.bytype
                java类中引用的数据类型和spring容器中bean的class属性是同源关系的，这样的bean能够赋值给引用关系(同类 父子 接口)
                            语法规则：<bean>标签里面加一个 autowire="byType"-->
    
    
        </bean>
    
        <bean id="phone" class="com.sea.Phone">
            <constructor-arg name="IP" value="123:123:123:5"></constructor-arg>
            <constructor-arg name="name" value="Apple_Maxpro"></constructor-arg>
        </bean>
    
    </beans>
  ```

    + 自动注入如果有两个符合标准的bean则会出现异常(byType)

## 补充：

+ Junit单元测试 一个工具类库，做测试方法使用的。
  + 单元：指定的是方法，一个类中有很多方法，一个方法称为单元
  + 当然也可以使用一个main方法



# 2 配置文件

+ 多配置文件优势
  + 每一个文件比一个文件要小很多，效率高
  + 避免多人带来的冲突
+ 如果你的项目有多个模块(相关功能在一起) 一个模块配置一个文件。
  + 学生考勤模块配置一个文件
  + 学生成绩一个配置文件
+ 多文件配置方式
  + 按功能模块，一个模块一个配置文件
  + 按照类的功能，数据库相关的配置一个配置文件，做事务的功能一个配置文件，做servlet功能的一个配置文件等
+ 包含关系的配置文件

# 3 注解

+ 基于注解的DI:通过注解完成java对象的创建，属性赋值
+ 注解的使用步骤
  1. 使用注解必须使用spring-apo依赖
  2. 在类中加入spring的注解(多个不同功能的注解)
  3. 在spring的配置文件中，加入一个组件扫描标签，说明注解在你项目中的位置
+ 学习的注解有以下这些
  1. @component
  2. @Respotory
  3. @Service
  4. @Controller
  5. @value
  6. @Autowired
  7. @Resource

```java
import org.springframework.stereotype.Component;


/*
* @component:创建对象，等同于<bean>的功能
*   属性：value 就是对象的名称 也就是等同于bean的id
*   value的值是唯一的
* 位置：在类上
* @Component(value=”myStudent“)等同于
* <bean id="myStudent" class="com.sea.student"/>
*
* Spring中还有这样几个注释
*   创建对象的注解
*   1.@Repository（使用在持久层类的上面的）：放在dao的实现类上面，表示创建dao对象，dao对象是可以访问数据库的。
*   2.@Service（放在业务层上面的）：放在service的实现类上面的，创建service对象，service对象是做业务处理，可以有事务处理等功能
*   3.@Controller：放在控制器对象上面的，创建控制器对象的，能够提交用户的参数，显示请求的处理结果
*   以上三个注解的使用和@component一样，都能创建对象，但是这三个注释还有额外的功能。
* */
//@Component(value = "myStudent")
@Component(value = "myStudent")
public class Student {
    private String name;
    private int age;

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```



```html
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"                                                  名称
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context  命名空间https://www.springframework.org/schema/context/spring-context.xsd">                                                   url
    <context:component-scan base-package="com.ocean"/>
</beans>
```

+ 注解的注入

+ 简单类型的注解注入

+ ```java
  package com.sea;
  
  import org.springframework.beans.factory.annotation.Value;
  import org.springframework.stereotype.Component;
  
  /*
  * @value:简单类型的属性赋值
  *   属性：value是string 类型的，表示简单类型的属性值
  *   位置：1 在属性定义的上面，无需set方法。推荐使用。
  *        2 在set方法的上面
  * */
  
  @Component("myStudent")
  public class Student {
      @Value("seaOcean") //赋值
      private String name;
      @Value("23")	//赋值
      private int age;
  
      @Override
      public String toString() {
          return "Student{" +
                  "name='" + name + '\'' +
                  ", age=" + age +
                  '}';
      }
  	@Value("seaOcean") //也可以
      public void setName(String name) {
          this.name = name;
      }
  	@Value("23")//也可以
      public void setAge(int age) {
          this.age = age;
      }
      
  }
  
  ```

  + 引用类型的注解赋值
  + bytype

  ```java
  package com.sea;
  
  import org.springframework.beans.factory.annotation.Value;
  import org.springframework.stereotype.Component;
  
  @Component("mySchool")
  public class School {
      @Value("北京大学")
      private String name;
      @Value("北京")
      private String address;
  
      @Override
      public String toString() {
          return "School{" +
                  "name='" + name + '\'' +
                  ", address='" + address + '\'' +
                  '}';
      }
  }
  ------------------------------------------------------------------------------------------------------
  package com.sea;
  
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.beans.factory.annotation.Value;
  import org.springframework.stereotype.Component;
  
  /*
  * @value:简单类型的属性赋值
  *   属性：value是string 类型的，表示简单类型的属性值
  *   位置：1 在属性定义的上面，无需set方法。推荐使用。
  *        2 在set方法的上面
  * */
  
  @Component("myStudent")
  public class Student {
      //@Value("seaOcean")
      private String name;
    //  @Value("23")
      private int age;
  
      /*
      * 引用类型的赋值
      * @Autowired：spring框架提供的注解，实现引用类的赋值
      * spring中通过注解给引用类型赋值，使用的是自动注入原理
      *
      * 位置：
      *     1 属性定义的上面，无需set方法
      *     2 在set方法的上面
      * */
      @Autowired
      private School school;
  
      @Override
      public String toString() {
          return "Student{" +
                  "name='" + name + '\'' +
                  ", age=" + age +
                  ", school=" + school +
                  '}';
      }
  
      @Value("seaOcean")
      public void setName(String name) {
          this.name = name;
      }
      @Value("23")
      public void setAge(int age) {
          this.age = age;
      }
  
  }
  
  
  ```

  + byname

  ```java
    @Autowired
      @Qualifier("mySchool")
      private School school;
  ```

  + @resource：byname

  ```
      @Resource(name = "mySchool")
      private School school;
  ```

  + @resource：bytype

  ```
    @Resource
      private School school;
  ```

  

+ 注意：需要修改的可以使用配置文件

+ 不经常修改的可以不用配置文件 使用注解的方式

+ ${} 

  + 需要在resource目录下创建一个properties文件  

    + ```
      myname=lisi
      myage=23
      ```

  + 在xml文件中配置properties文件

    ```html
    <context:property-placeholder location="classpath:/Learn/test.properties"/>
    ```

  + 在java文件的类中@Value注解中 如此使用@value("${ myname}") ;

    + ```java
          @Value("${myname}")
          public void setName(String name) {
              this.name = name;
          }
          //@Value("23")
          @Value("${myage}")
          public void setAge(int age) {
              this.age = age;
          }
      ```

# 4 AOP

## 4.1 AOP和动态代理

+ AOP(Aspect Orient Programming)面向切口编程,基于动态代理的技术，可以使jdk，cglib两种方式。
+ AOP是动态代理的规范化，把动态代理的实现步骤，方式定义好了，让开发人员使用统一的方式，使用动态代理
+ AOP(Aspect Orient Programming) 
+ 切面：给你的目标类增加的功能，就是切面，向上面用的日志，事务都是切面。
  + 切面的特点：一般是非业务方法，独立使用的。
+ 怎么理解面向切面编程？
  + 需要在分析项目功能时，找出切面
  + 合理的安排切面的执行（与目标方法的相对位置）
  + 合理安排切面的执行位置，在哪个类，哪个方向增加功能
+ 术语
  + Aspect切面：表示增强的功能 通常是非业务功能：事务，日志，统计信息，参数检查，权限验证
  + JoinPoint:连接点，链接业务方法和切面的位置，就某类中的业务方法
  + pointcut：切入点，指多个连接方点的方法的集合，多个方法
  + 目标对象：给哪个类的方法增强功能，这个类就是目标对象
  + Advice：通知，通知表示切面功能执行的时间
+ 一个切面的三个关键要素
  + 切面的功能代码，切面干什么
  + 切面的执行位置，使用Pointcut表示切面执行的位置
  + 切面的执行时间，使用advice表示时间，在目标方法前还是目标方法后

## 4.2  AOP的实现

+ AOP是一个规范，是一个规范化的，标准的动态代理

+ AOP的技术实现框架

  1. spring：spring在内部实现了AOP的规范，能做AOP的工作。

     1. spring主要在事务处理中使用了AOP
     2. 在项目开发中很少使用spring的AOP实现，因为spring的AOP比较笨重。

  2. aspectJ：一个开源的专门做AOP的框架。 spring框架中已经集成了aspectJ框架，通过spring就能使用aspectJ的功能

     1. 来自于eclipse基金会支持成立的
     2. 实现AOP有两种方法
        1. 一种是xml配置文件的方式 ：配置全局事务 （事务）
        2. 一种是使用注解的方式
           1. 一般都是使用注解的
              1. 一共有五个注解这五个表示界面的切入时间

  3. 学习aspectJ的相关语法

     1. 切面的执行时间，这个执行时间在规范中叫做advice(通知，增强)

        在aspectJ中是使用注解来表示的.也可以在xml配置文件中使用标签

        (1) @Before

        (2) @afterReturning

        (3) @Around

        (4) @AfterThrowing

        (5) @After

## 4.3 切入点表达式

1. 表示切面执行的位置，使用的是接入点表达式。

   execution(modifies-pattern? ret-type-pattern

   declaring-type-pattern?name-pattern(param-pattern)

   throws-pattern?)

   + 类型 返回 路径 方法&参数 异常 
     +  

   + 下面是最重要的两个

   -------------------------------------

   ret-type-pattern

   name-pattern(param-pattern)

   ----------------------------------------------

   + /* 表示 0到多个任意字符
   + .. 用在包名中表示当前包及其子类 用在方法参数中，表示任意多个参数

------------------------------

## 4.4 IDEA中是使用AOP

1. 新建maven项目

2. 加入依赖

   1. spring依赖
   2. junit依赖
   3. sapectJ依赖

3. 创建目标类：接口和他的实现类。

   要做的是给类中的方法添加功能

4. 创建切面类：普通类

   1. 在类的定义方法，方法就是切面的功能代码

      在方法的上面加入aspectJ中的通知注解，例如@Before
      有需要指定的切入点表达式execution()

5. 创建spring的配置文件：声明对象，把对象交给容器统一管理

   声明对象你可以使用注解或者xml文件<bean>

   1. 声明目标类

   2. 沈明切面类对象

   3. 声明aspectj框架中的自动代理生成器标签。

      自动代理的生成器：用来完成代理对象的自动创建功能的

6. 创建测试类，冲spring容器中获取目标对象(实际就是代理的对象)

   通过代理的执行方法，实现aop的功能增强

```java
@Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("ApplicationContextProxy.xml");
        //冲容器中获取目标对象
        myService service =(myService) context.getBean("service"); //两个全部写接口 都是接口的实现 tnnd 找了半天
        //通过代理对象执行方法，实现目标方法执行时，增强的功能。
        service.dosomething(23,"hello");

    }
```

+ 总结：

  + 加依赖

  + 目标类和接口

  + 切面类

  + ```java
    package com.annot_6;
    
    import org.aspectj.lang.annotation.After;
    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;
    import org.aspectj.lang.annotation.Pointcut;
    
    /*
    * @Aspect是框架中的注解
    *   作用:表示当前类是切面类
    *   切面类：是用来给业务方法添加功能的类
    *   位置：在类的上面
    * */
    @Aspect
    public class myAspect {
        @After(value="mypt()")
        public void myFinallyNotice(){
            System.out.println("==========Delete Resource============");
        }
        @Before(value="mypt()")
        public void myAfter(){
            System.out.println("==========Service Start============");
        }
        @Pointcut(value = "execution(* *..*Impl.doThird(..))")
        private void mypt(){
    
        }
    
    }
    ```

  + 配置目标类和切面类

  + ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:aop="http://www.springframework.org/schema/aop"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/aop
           https://www.springframework.org/schema/aop/spring-aop.xsd">
    
        <!--把对象交给spring容器-->
        <!--申明目标对象-->
        <bean id="service" class="com.annot_1.myServiceImpl" />
        <!--申明切面类对象-->
        <bean id="myAspect" class="com.annot_1.myAspect" />
    
        <!--申明自动代理生成器 使用aspectJ框架的内部功能，创建目标对象的代理对象
        创建代理对象是内存中实现的，修改目标对象的内存中的结构。创建为代理对象
        所以目标对象就是被修改后的代理对象
        aspectj-autoproxy：会把spring容器中所有的目标对象，一次性全部生产代理对象。
        -->
        <aop:aspectj-autoproxy />
        <!--spring会自动当我们添加环境 就在第4行 和7 8行-->
    
    
    </beans>
    ```

    

+ AOP的实现框架

  + spring实现了AOP使用的是接口
  + aspectJ框架使用注解和配置文件都可以实现AOP功能

  

## 4.5 前置通知   

```java
/*
    * 定义方法的要求
    * public 公共方法
    * 方法没有返回值
    * 方法的名称自定义
    * 方法可以有参数，可以没有参数
    *   如果有参数 这个参数不是自定义的 有几个参数类型可以使用
    * */

    /*
    * @Before:前置通知
    *   属性value，是切面表达式，表示切面的功能执行位置。
    *   位置：方法上方
    * 特点：
    *   在目标方法之前执行的
    *   不会改变执行结果
    *   不会影响方法执行
    * */
```

```java
@Before(value = "execution(void *..dosomething(..))")
public void myBefore(){
    System.out.println("current_time:"+new Date());
}
```

## 4.6 后置通知   

+ @AfterRetueing(value="execution(* * *..*(..))"  returning="ret")

+ Joinpoint如何使用

  + 说明使用的语法

    + 注意一定要将接入点对象放在第一位

    + ```java
      * @AfterReturning：后置通知
      * 属性一：value 切入点表达式
      *    二：returning自定义的变量，表示目标方法的返回值的。
              自定义的变量名必须和通知方法的形参名一样
      *  位置：在方法上定义
      *  特点
      * 1.在目标方法之后执行的
      * 2.能够获取到目标方法的返回值，可以根据这个返回值做不同的处理功能
      * 3.可以修改这个返回值
      *
      * 后置通知的执行顺序
      *   Object res = doOther();
      *   myAfterReturing(res);
      *   System.out.println("res="+res)
      ```

    + ```java
      @AfterReturning(value="execution(* *..doOther(..))",
          returning = "res")
          public void myAfterReturning(JoinPoint jp,Object res){
              System.out.println("后置通知：方法的定义"+jp.getSignature());
              String res1 = (String) res;
              String s = res1.toLowerCase();
              System.out.println(s);
          }
      ```

## 4.7 环绕通知

+ ```java
  package com.annot_3;
  
  import org.aspectj.lang.ProceedingJoinPoint;
  import org.aspectj.lang.annotation.Around;
  import org.aspectj.lang.annotation.Aspect;
  import org.aspectj.lang.annotation.Before;
  
  import java.util.Date;
  
  /*
  * @Aspect是框架中的注解
  *   作用:表示当前类是切面类
  *   切面类：是用来给业务方法添加功能的类
  *   位置：在类的上面
  * */
  @Aspect
  public class myAspect {
   /*
   * 环绕通知方法的定义格式
   *  1.public
   *  2.必须有一个返回值
   *  3.方法名称自定义
   *  4.方法有参数，固定的参数
   * */
      /*
      * @Around:环绕通知
      *   属性：value 切入点表达式
      *   位置：在方法的定义上面
      * 特点：
      *   1.他是最强力的通知
      *   2.在我们的目标方法的前和后都能增强功能
      *   3.能控制目标方法是否被调用执行
      *   4.修改原来的目标方法的执行结果，影响最后的结果
      *
      * 环绕通知等同于jdk动态代理的，Invocationhandler接口
      *
      * 参数：proceedingJointPoint等同于 Method
      * 作用：执行目标方法的
      * 返回值：就是目标方法的执行结果，可以修改。
      *
      * */
      @Around(value = "execution(* *..myServiceImpl.doFirst(String,Integer))")
      public Object myAround(ProceedingJoinPoint pjp) throws Throwable {
          //1实现环绕通知
          System.out.println("ahead");
          Object proceed = null;
          proceed = pjp.proceed()+" hello!";
          //2在目标方法前或在目标方法后加入功能
          System.out.println("rear");
          return proceed;
      }
  
  
  }
  ```

## 4.8 异常通知

+ 异常通知方法的定义格式
  + public
  + 没有返回值
  + 方法名称自定义
  + 方法有一个异常，如果还有就是JoinPoint
+ @AfterThrowing：异常通知
  + 属性
    + value 切入点表达式
    + throwing 自定义变量，表示目标方法抛出的异常对象
      + 变量名必须和参数名一样
  + 特点
    + 在目标方法抛出异常是执行
    + 可以做异常的监控目标方法执行时是不是有异常
    + 如果有异常，可以发送邮件，短信进行通知

```java
@Aspect
public class myAspect {

@AfterThrowing(value = "execution(public void com.annot_4.myServiceImpl.doSecond(..))",throwing = "ex")
 public void myAfterThrowing(Exception ex){
    System.out.println("===========Discover Exception==============");
    System.out.println(ex.getStackTrace());
 }
}
```

## 4.9 最终通知 

+ 最终通知方法的定义格式
  - public
  - 没有返回值
  - 方法名称自定义
  - 没有参数，如果有就是JoinPoint
+ @After：最终通知
  - 属性
    - value 切入点表达式
  - 特点
    - 总是会执行
    - 在目标方法之后执行

```java
@Aspect
public class myAspect {
@After(value="execution(* *..*Impl.doThird(..))")
public void myFinallyNotice(){
    System.out.println("==========Delete Resource============");
}

}
```

```java
try {
        目标方法
    } catch (Exception e) {
        throw new RuntimeException(e);
    } finally {
    	myFinallyNotice()
    }
```

+ 以这种意思在里面 所以一般用于关闭资源

## 4.10 @Pointcut 

+ 这个注解是用于定义和管理切入点的，如果你的项目中有多个切入点表达式是重复的，可以复用的，可以使用pointcut
+ 属性
  + value 切入点表达式
  + 加在方法上方
+ 特点
  + 当使用@Pointcut定义在一个方法的上面，此时这个方法的名称就是切入点表达式的别名。其他的通知中，value属性就可以使用这个方法的名称来代替切入点表达式了

```java
@Aspect
public class myAspect {
    @After(value="mypt()")
    public void myFinallyNotice(){
        System.out.println("==========Delete Resource============");
    }
    @Before(value="mypt()")
    public void myAfter(){
        System.out.println("==========Service Start============");
    }
    @Pointcut(value = "execution(* *..*Impl.doThird(..))")
    public void mypt(){

    }

}
```

+ 不同形式下的不同代理
  + 目标类实现接口的时候使用的是jdk的动态代理
  + 目标类没有实现了接口使用的是CGLIB动态代理

+ 如何在有接口的情况下还是用CGLIB代理？

  + 需要修改配置文件

  + ```html
    <!--如果你希望目标类有接口使用CGLIB代理的话-->
    <!--proxy-target-class="true" 告诉框架cglib代理-->
    <aop:aspectj-autoproxy proxy-target-class="true"/>
    ```

    

# 5 关于spring和mybatis的使用

+ 把spring和mybatis集成到一起，像一个框架一样使用。

  + 使用的技术是IOC

+ 为什么IOC可以将mybatis和spring整合起来呢？

  + 以为IOC能够创建对象，可以将mybatis框架中的对象交给spring统一创建，开发人员从spring中获取对象。
  + 开发人员就不需要同时面对两个框架了，就面对一个spring就行了

+ mybatis使用步骤，对象

  + 定义接口，studentsDAO
  + 定义mapper文件，studentsDAO.xml
  + 创建DAO的代理对象 StudentsDAO dao = sqlsession.getMapper(StudentsDAO.class);
  + List<students> list = dao.selectStudents();

  

+ 要使用DAO对象，需要使用getmapper()方法：

  + 怎么获取getmapper()方法
    + 获取SqlSession对象，需要使用sqlsessionfactory的对象openSession()方法
    + 创建sqlsessionfactory对象。通过读取mybatis的主配置文件，能够创建sqlsessionfactory对象
  + 需要使用sqlsessionfactory对象，使用factory能获取sqlsession
  + factory的创建需要读取主配置文件

+ 主配置文件

  + 环境信息 不再使用原先的连接池 使用更高级的连接池 这个连接池交给spring来创建
  + mapper位置
  + 日志设置

  ===============

+ 通过以上的说明 spring需要创建以上的对象

  1. 独立的连接池的对象，使用阿里巴巴的德鲁伊连接池
  2. sqlsessionfactory对象
  3. 创建DAO对象

+ 创建出spring需要对象就可以使用spring和mybatis整合起来使用

+ 步骤：

  1. 新建maven项目
  2. 加入maven的依赖
     1. spring的依赖
     2. mybatis的依赖
     3. mysql的驱动
     4. spring的事务管理的依赖
     5. mybatis和spring集成的依赖：mybatis官方提供的，用来在spring项目中创建mybatis中的sqlsessionfactroy，DAO对象
  3. 创建实体类
  4. 创建接口和mapper文件
  5. 创建主配置文件
  6. 创建service接口和实现类，属性是dao
  7. 创建spring的配置文件：声明mybatis的对象交给spring创建
     1. 数据源
     2. sqlsessionfactory
     3. DAO对象
     4. 声明自定义的servce

  ```xml
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
      
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.9</version>
    </dependency>
      
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.28</version>
    </dependency>
      
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
      
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
      
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
      
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>
      
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.3.1</version>
      </dependency>
  
  </dependencies>
  ```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">
    <util:properties location="baseinf.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <property name="url" value="${jdbc_url}" />
        <property name="username" value="${jdbc_user}" />
        <property name="password" value="${jdbc_password}" />

        <property name="filters" value="stat" />

        <property name="maxActive" value="20" />
        <property name="initialSize" value="1" />
        <property name="maxWait" value="6000" />
        <property name="minIdle" value="1" />

        <property name="timeBetweenEvictionRunsMillis" value="60000" />
        <property name="minEvictableIdleTimeMillis" value="300000" />

        <property name="testWhileIdle" value="true" />
        <property name="testOnBorrow" value="false" />
        <property name="testOnReturn" value="false" />

        <property name="poolPreparedStatements" value="true" />
        <property name="maxOpenPreparedStatements" value="20" />

        <property name="asyncInit" value="true" />
    </bean>
</beans>
```

+ 由于我们不再使用POOLED所以我们使用配置bean的方式使用德鲁伊(druid)连接池

+ mybatis的配置文件

+ ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
      <settings>
          <!--设置，ybatis输出日志-->
          <setting name="logImpl" value="STDOUT_LOGGING"/>
      </settings>
      <!--<typeAliases>
          <package name="com.sea"/>
      </typeAliases>-->
      <mappers>
          <mapper resource="com/sea/DAO/studentsDAO.xml"/>
          <!--<package name="com.sea.DAO"/>-->
      </mappers>
  
  </configuration>
  ```

+ applicationcontext 里面包含  数据源、factory和DAO

+ ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:util="http://www.springframework.org/schema/util"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd">
  
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
          <property name="url" value="jdbc:mysql://localhost:3306/mybase?serverTimezone=GMT"/>
          <property name="username" value="root"/>
          <property name="password" value="why123456"/>
          <property name="maxActive" value="20"/>
      </bean>
      <bean id="SqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="configLocation" value="classpath:mybatis-main.xml"/>
          <property name="dataSource" ref="dataSource"/>
      </bean>
       <!--创建DAO对象，使用Sqlsession的getMapper(Students.class)-->
      <!--来自于官方提供的 住主要作用是在内部调用getMapper()生成每个DAO接口的代理对象-->
      <!--dao对象的默认名称是接口名首字母小写-->
      <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
          <!--指明sqlSessionFactoryBeanName-->
          <property name="sqlSessionFactoryBeanName" value="SqlSessionFactory"/>
          <!--指定一个包名，包名是DAO接口所在的包名,会扫描所有的接口 把每个接口都执行一次 getmapper()方法 得到每个接口的DAO对象 创建好的DAO对象会放的spring的容器中-->
          <property name="basePackage" value="com.sea.DAO"/>
      </bean>
  </beans>
  ```

+ jdbc.properties

+ ```properties
  jdbc.url=jdbc:mysql://localhost:3306/mybase?serverTimezone=GMT
  jdbc.username=root
  jdbc.password=why123456
  jdbc.maxActive=20                         //前面不加jbdc的话 username会和windows系统变量发生冲突
  ```



# 6 事务

+ 使用事务的情况

  + 当我的操作涉及到多个表或者是多个sql语句的delect update insert 我们需要保证这些语句全部成功才能完成
  + 或者全部失败才能保证操作是真确的
    + 例如：从A账户向B账户中汇款 A的减和B的加要同时成立

+ 在java的代码中应该如何使用事务呢？

  + DAO是sql语句的java代表 其中的函数是是sql语句的换回结果 DAO.xml文件记录了sql语句
  + 放在了service的业务方法中 因为service中会有多个DAO方法，执行多个sql语句

+ 不同方式的事务操作

  + JBDC代码中处理事务需要使用Connection需要提交事务需要conn.commit 回滚事务需要conn.rollback
  + mybatis访问数据库，处理事务sqlsession.commit 和 sqlsession.rollback()

+ spring提供了一种处理事务处理的同i模型，能够使用统一的步骤，方式完成多种不同数据库访问技术的事务处理。

  + 使用spring的事务处理机制，可以完成mybatis和hibernate的数据库事务处理

+ 申明式事务：

  1. 事务内部提交，回滚事务，使用事务管理器的对象，代替完成commit rollback

     事务管理器时一个接口和一套(很多)实现类组成了事务管理器

     + 接口：platformTransactionManger，定义了事务的重要方法commit rollback

     + 实现类：spring把每种数据库访问技术的事务处理类创建好了

     如何使用：需要告知spring你要是用的是那种访问技术

     需要声明数据库技术对于事务管理器的实现类，在spring的配置文件中使用<bean>声明

     ```xml
     <bean id="xxx" class="*..DataSourceTransactionManager"/>
     ```

  2. 你的业务需要什么样的事务呢？

     说明方法需要的事务

      1. 事务的隔离级别

         DEFAULT

         READ_UNCOMMITTED

         REPEATABLE_READ

         SERICALABLE

         2. 事务的超时时间：表示一个方法最长的执行时间，如果方法执行超过了时间，事务就会回滚。

         单位是秒，整数值，默认是-1

         3. 事务的传播行为 

         传播行为是用来控制你的方法是不是有事务，是什么样的事务

         事务在方法之间是如何使用的 就是事务行为

         全波行为有7种

         + 最重要的是1

         		1.  PROPAGETION_REQUIRE 	

         			最常见的事	务传播行为 也是默认的传播方式 一定会在事务中执行 如果已有就用 没有就自己创建

         		2. PROPAGETION_REQUIRE_NEW

         			总是信件事务，若当前存在事务，就挂起事务，知道新事务执行完成。

         		3. PROPAGETION_SUPPORTS

         			查询操作 有无事务均可

         4. 提交事务的时机

            1. 当你的事务方法，执行成功，没有异常抛出，当方法执行完毕，spring在方法执行后提交事务。事务管理器commit

            2. 当你的业务方法抛出异常或者error时，spring执行回滚，调用事务管理器的rollback

            3. 当你的业务方法抛出非运行时异常，主要是受到查异常时，提交事务

               受查异常：在你写代码中，必须进行异常的捕捉

         总结spring的事务：

          	1. 管理事务的时事务管理和它的实现类
         		2. spring的事务是一个统一的模型
               	1. 指定使用的事务管理器实现类，使用<bean>
               	2. 指定那些类，哪些方法需要加入事务的功能
               	3. 指定方法需要事务需要的隔离级别，转播行为，超时

# 6. 程序举例环境搭建

## 6.1 基本处理方式

举例：购买商品trans_sale项目
本例要实现购买商品，模拟用户下单，向订单表添加销售记录，从商品表减少库存。

实现步骤：

创建两个数据库表 sale,goods
sale 销售表
goods 商品表

+ 如何给我们已经写好的函数添加事务呢？(有两种方案)

  + 注解方案 spring框架自己用AOP实现给业务方法增加事务功能的，使用@Transactional注解增加事务

    + 这个书解是spring提供的放在public方法的上面，表示当前方法的具有事务
    + 可以给注解的属性赋值，表示具体的隔离级别，传播行为，异常信息等
    + propagation：用于设置事务的传播属性
    + isolate：用于设置事务的隔离级别
    + readonly：用于设置对数据的操作是不是只读的
    + timeout：用于设置操作与数据库连接的超时限制
    + rollbackFor：指定需要回滚的异常类
    + rollbackForClassName：指定需要回滚的异常类的类名
    + noRollbackFor：不需要回滚的异常类
    + noRollbackForClassName：指定不需要回滚的异常类的类名

  + 不管是那种事务一定需要事务管理器

    + 需要声明事务管理器对象

      1. <bean id="xx" class="datasourceTransactionManager">

      2. 开启事务注解驱动，告诉spring框架，要使用注解的方式管理事务。

         spring使用AOP机制，创建@Transactional所在的类的代理对象，给方法加入事务的功能。

         spring给业务方法加入事务：

         ​	在你的业务方法执行之前先开启事务，在业务方法之后提交或回滚事务，使用AOP的环绕通知

         Around环绕通知自己不用写了，spring内部已经实现了

         @Around(“你需要添加事务功能的业务方法的切入点表达式”)

         Object myAround(){

         ​	try{

         ​		buy(1001,10);

         ​		spring的事务管理.commit();

         ​	}catch(Exception e){

         ​		spring的事务管理.rollback

         ​	}

         }

```java
package com.sea.Service.Impl;

import com.sea.Contain.Goods;
import com.sea.Contain.Sale;
import com.sea.DAO.GoodsDAO;
import com.sea.DAO.SaleDAO;
import com.sea.Exception.NotEnoughtExcep;
import com.sea.Service.ByGoodService;

public class ByGoodServiceImpl implements ByGoodService {
    private SaleDAO saledao;
    private GoodsDAO goodsdao;

    public void setSaledao(SaleDAO saledao) {
        this.saledao = saledao;
    }

    public void setGoodsdao(GoodsDAO goodsdao) {
        this.goodsdao = goodsdao;
    }

    @Override
    public void buy(Integer goodsid, Integer nums) {
        System.out.println("=============start============");
        Sale sale = new Sale();
        sale.setGoodsid(goodsid);
        sale.setNums(nums);
        saledao.insertSaleRecord(sale);
        //更新库存
        Goods goods = goodsdao.selectGoodsById(goodsid);
        if(goods == null){
            //没有
            throw new NotEnoughtExcep("编号是为"+goodsid+"的商品已售空！");
        }else if(goods.getAmount() < nums){
            //商品不足够
            throw new NotEnoughtExcep("商品不足！");
        }
        //修改库存了
        Goods buygoods = new Goods();
        buygoods.setId(goodsid);
        buygoods.setAmount(nums);
        goodsdao.updateGoods(buygoods);
        System.out.println("============end=============");
    }
}

```

3. 在你的方法上面加上@Transactional的注解



+ 先在applicationcontext中配置我们需要使用到的一些bean  TransactionManager（事务管理） 和 annotation-driver （注解驱动） 在tx里面

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd
       http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
    <context:property-placeholder location="classpath:jdbc.properties"/>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <property name="url" value="${jdbc_url}"/>
        <property name="username" value="${jdbc_username}"/>
        <property name="password" value="${jdbc_password}"/>
        <property name="maxActive" value="${jdbc_maxActive}"/>
    </bean>
    <bean id="SqlsessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:Mybatis-config.xml"/>
    </bean>
    <!--使用MapperScanner创建接口代理类，service使用注入-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.sea.DAO"/>
        <property name="sqlSessionFactoryBeanName" value="SqlsessionFactory"/>
    </bean>
    <bean id="service" class="com.sea.Service.Impl.ByGoodServiceImpl">
        <property name="goodsdao" ref="goodsDAO"/>
        <property name="saledao" ref="saleDAO"/>
    </bean>
    <!-----------------------------------------重要 关键点------------------------------------------------------->
    <!--使用spring的事务处理-->
    <!--声明事务管理器-->
    <bean id="TransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--连接的数据库，指定数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!--开启事务注解驱动，告诉spring使用注解管理事务的代理对象
        transaction-manager:事务管理器对象的id
    -->
    <tx:annotation-driven transaction-manager="TransactionManager"/>
        <!-----------------------------------------重要 关键点------------------------------------------------------->

</beans>
```

```java
package com.sea.Service.Impl;

import com.sea.Contain.Goods;
import com.sea.Contain.Sale;
import com.sea.DAO.GoodsDAO;
import com.sea.DAO.SaleDAO;
import com.sea.Exception.NotEnoughtExcep;
import com.sea.Service.ByGoodService;
import org.springframework.transaction.annotation.Isolation;
import org.springframework.transaction.annotation.Propagation;
import org.springframework.transaction.annotation.Transactional;

public class ByGoodServiceImpl implements ByGoodService {
    private SaleDAO saledao;
    private GoodsDAO goodsdao;

    public void setSaledao(SaleDAO saledao) {
        this.saledao = saledao;
    }

    public void setGoodsdao(GoodsDAO goodsdao) {
        this.goodsdao = goodsdao;
    }

    /**
     *
     * rollbackFor:表示发生指定异常要回滚
     * @param goodsid
     * @param nums
     */
    /*@Transactional(
            propagation = Propagation.REQUIRED,
            isolation = Isolation.DEFAULT,
            readOnly = false,
            rollbackFor = {
                    NullPointerException.class,
                    NotEnoughtExcep.class
            }
    )*/
    //默认抛出运行时异常回滚事务
    @Transactional //这里的一行和上面的一大块是等效的
    @Override
    public void buy(Integer goodsid, Integer nums) {
        System.out.println("=============start============");
        Sale sale = new Sale();
        sale.setGoodsid(goodsid);
        sale.setNums(nums);
        saledao.insertSaleRecord(sale);
        //更新库存
        Goods goods = goodsdao.selectGoodsById(goodsid);
        if(goods == null){
            //没有
            throw new NullPointerException("编号是为"+goodsid+"商品不存在");
        }else if(goods.getAmount() < nums){
            //商品不足够
            throw new NotEnoughtExcep("商品不足！");
        }
        //修改库存了
        Goods buygoods = new Goods();
        buygoods.setId(goodsid);
        buygoods.setAmount(nums);
        goodsdao.updateGoods(buygoods);
        System.out.println("============end=============");
    }
}
```

## 6.2  在大型项目中如何处理事务

+ 使用aspectJ的AOP配置管理事务（掌握）

  + 适合大型项目，有很多的方法和类需要大量的配置事务，需要使用aspectJ框架功能，在spring配置文件中申明类，方法需要的事务。这种方式业务方法和事务完全分离。

+ 实现步骤：

  + 都是在xml配置文件中实现的

    1. 要使用的是aspectJ 故需要在maven中添加依赖

       ```xml
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-aspects</artifactId>
         <version>5.2.5.RELEASE</version>
       </dependency>
       ```

    2. 申明事务管理器的对象

    ```xml
    <bean id="TransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--连接的数据库，指定数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
    ```

    3. 申明方法需要的事务类型(配置方法的事务属性（隔离级别，传播行为，超时）)
    4. 主配置文件的内容

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:util="http://www.springframework.org/schema/util"
           xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
           xmlns:aop="http://www.springframework.org/schema/aop"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd
            http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd
            http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
            http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
        <context:property-placeholder location="classpath:jdbc.properties"/>
        <bean id="mydataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
            <property name="url" value="${jdbc_url}"/>
            <property name="username" value="${jdbc_username}"/>
            <property name="password" value="${jdbc_password}"/>
            <property name="maxActive" value="${jdbc_maxActive}"/>
        </bean>
        <bean id="SqlsessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
            <property name="dataSource" ref="mydataSource"/>
            <property name="configLocation" value="classpath:Mybatis-config.xml"/>
        </bean>
        <!--使用MapperScanner创建接口代理类，service使用注入-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="basePackage" value="com.sea.DAO"/>
            <property name="sqlSessionFactoryBeanName" value="SqlsessionFactory"/>
        </bean>
        <bean id="service" class="com.sea.Service.Impl.ByGoodServiceImpl">
            <property name="goodsdao" ref="goodsDAO"/>
            <property name="saledao" ref="saleDAO"/>
        </bean>
        <!--申明式事务处理 和源代码是完全分离的-->
        <!--事务管理器对象-->
        <bean id="TransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="mydataSource"/>
        </bean>
        <!--申明业务方法的事务属性-->
        <!--id:自定义名称 表示tx:advice 和 </tx:advice>之间的配置内容的
            transaction-manager：事务管理器对象的id
        -->
        <!---------------------------------------------关键点------------------------------------------------------>
        <tx:advice id="myAdvice" transaction-manager="TransactionManager">
            <!--表示属性-->
            <tx:attributes>
                <!--给不同的方法配置事务属性
                    name: 1. 方法的名称 1完整的方法名 不带有包和类
                          2.方法可以适合用通配符，*表示任意字符
                    propagation：传播行为
                    isolation：隔离级别
                    rollback-for：指定异常需要回滚的类 需要全限定的类名
                -->
                <tx:method name="buy" propagation="REQUIRED" isolation="DEFAULT" read-only="false" rollback-for="java.lang.NullPointerException,com.sea.Exception.NotEnoughtExcep" />
                <tx:method name="add*" propagation="REQUIRED" isolation="DEFAULT" read-only="false" />
                <tx:method name="query*" propagation="REQUIRED" isolation="DEFAULT" read-only="true"/>
                <tx:method name="remover*" propagation="REQUIRED" isolation="DEFAULT"/>
                <tx:method name="*" propagation="SUPPORTS" read-only="true"/>
            </tx:attributes>
        </tx:advice>
        <!--配置AOP-->
        <aop:config>
            <!--id：配置切入点表达式的名称，唯一值
                express：切入点表达式，指定哪些类要使用事务，aspectJ会创建代理对象-->
            <aop:pointcut id="servicePoint" expression="execution(* *..Service..*.*(..))"/>
            <!--配置增强器：关联advice和pointcut
                advice-ref:通知，上面tx:advice那里的配置
                pointcut-ref:切入点表达式的id
            -->
            <aop:advisor advice-ref="myAdvice" pointcut-ref="servicePoint" />
        </aop:config>
    <!-----------------------------------------------------关键点---------------------------------------------->
    
    </beans>
    ```

# 7.在web项目中如何使用容器对象：监听器的使用

+ 之前用的都是SE项目 在main方法中执行代码

+ Web不是怎么运行的，他是在Tomcat中运行的，项目一旦启动就会一直运行的。

  

+ 需求：
  web项目中的容器对象只需要创建一次，把容器的对象放到全局作用域servletContext中

  如何实现？

  + 使用监听器 当全局作用域对象创建时 创建容器 放入servletContext中

+ 监听器作用：

  + 创建容器对象，执行ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml")
  + 把容器对象放入到servletContext，servletContecxt.setAttribute(key,ctx)

+ 监听器可以自定义 亦可以使用框架中以提供的contextLoaderListenner.

+ 由于tomcat不同的servlet版本导致了冲突 使得有些内容是无法实现的

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>myservlet</servlet-name>
        <servlet-class>com.sea.Contorller.RegistStudents</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>myservlet</servlet-name>
        <url-pattern>/reg</url-pattern>
    </servlet-mapping>
    <!--注册监听器ContextLoaderListener
    监听器被创建对象后，会读取/web-inf/applicationContext_1.xml
    为什么要读取文件：因为在监听器中要创建applicationContext对象，需要加载配置文件
    /web-INF/applicationContext.xml就是监听器默认读取的spring配置文件路径

    可以修改默认的文件位置，使用context-param重新指定文件的位置
    -->
    <!----------------------------------------------------------important----------------------------------------------------->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext_1.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>
<!----------------------------------------------------------important----------------------------------------------------->
```































































