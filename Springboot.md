## 1. springboot

tips:start.spring.io是外国的网址 相对较慢 推荐使用 国内的网址:Start.springboot.io相对较快

### 1.1 基本注解

```java
@SpringBootApplication
public class MainApplicationClass {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(MainApplicationClass.class); //主配置类

    }
}
```



```java
@SpringBootApplication //主配置类注解
//以下的注解是主配置类注解的组成部分
@SpringBootConfiguration //主配置类也在容器中
@EnableAutoConfiguration
@ComponentScan
```

```java
@EnableAutoConfiguration //启动制动配置，把java对象配置好，注入到容器中。
```

```java
@ComponentScan //一个扫描器 用来找到注解 根据注解的功能可以创建对象 给属性赋值
```

````java
@ImportResource("classpath:beans.xml") //原生配置文件的导入
````

```java
@PropertySource("calsspath:jdbc.properties") //导入资源.properties
```

### 1.2 配置文件

application.properties------------application.yml
在实际开发中，我们的项目会经历很多个阶段(开发--测试--上线)，每个阶段的配置也会不同，例如：端口号，上下文，数据库等等，为了方便不同环境的切换springboot提供了多环境配置，具体步骤如下
	使用方式：
		创建多个配置文件：application-dev.properties,命名规则:application_环境名称.properties

​	创建开发环境的配置文件：application-dev.properties
	创建测试环境的配置文件：application-test.properties

## 2.springboot中使用applicationContext

+ commandLineRunner

+ ```java
  package com.example.demo;
  
  import com.example.demo.service.Impl.UserServiceImpl;
  import com.example.demo.service.UserService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.boot.CommandLineRunner;
  import org.springframework.boot.SpringApplication;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.context.ConfigurableApplicationContext;
  
  import javax.annotation.Resource;
  
  @SpringBootApplication
  public class MainApplicationClass implements CommandLineRunner {
      @Resource
      private UserService service;
  
      public static void main(String[] args) {
          System.out.println("创建容器");
          ConfigurableApplicationContext run = SpringApplication.run(MainApplicationClass.class);
  //        String[] beanDefinitionNames = run.getBeanDefinitionNames();
  //        for (String beanDefinitionName : beanDefinitionNames) {
  //            System.out.println(beanDefinitionName);
  //        }
          /*一般是自己测试的时候使用 我们返回值就是容器类型 就是@Serivce标注的类*/
          System.out.println("创建容器对象之后");
  
      }
  
      @Override
      public void run(String... args) throws Exception {
          //可做自定义的操作，不如文件的读取 和 数据库等等
  
          service.sayHello("lisi");
          System.out.println("在容器对象创建以后执行的方法");
      }
  }
  
  ```

+ 冲上面我们可以得到一些发现： 

  1. @AutoWired和@Resource 直接注解在接口上 不必是实现类 
  2. 如果使用run.getbean 使用的是实现类的名

## 3 web组件

三个内容：拦截器，过滤器，filter

### 3.1 拦截器

拦击器是springmvc中的一种对象，可拦截对controller的请求
拦击器框架中有系统的拦击器，还可以自定义拦截器。实现对请求的预处理

实现自定义拦截器：

1. 创建类实现springmvc框架中的HandlerInterceptor

   ```java
   package com.example.demo.interceptor;
   
   import org.springframework.web.servlet.HandlerInterceptor;
   import org.springframework.web.servlet.ModelAndView;
   
   import javax.servlet.http.HttpServletRequest;
   import javax.servlet.http.HttpServletResponse;
   
   public class CustomerInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           return HandlerInterceptor.super.preHandle(request, response, handler);
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
       }
   }
   
   ```

   2. 需要在springMVC的配置文件中，声明拦截器

      ```xml
      <mvc:interceptors>
          <mvc:interceptor>
              <mvc:path="url"/>
              <bean class="拦截器全限定名称"/>
          </mvc:interceptor>
      </mvc:interceptors>
      ```

      springboot中我们需要注册拦截器

      ```java
      package com.example.demo.config;
      
      import com.example.demo.interceptor.CustomerInterceptor;
      import org.springframework.context.annotation.Configuration;
      import org.springframework.web.servlet.config.annotation.InterceptorRegistration;
      import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
      import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
      
      @Configuration
      public class MyappConfig implements WebMvcConfigurer {
          //创建拦截器对象
          @Override
          public void addInterceptors(InterceptorRegistry registry) {
              String[] paths = {"/user/**"};
              String[] exPaths = {"/user/login"};
              InterceptorRegistration interceptorRegistration = registry.addInterceptor(new CustomerInterceptor()).addPathPatterns(paths).excludePathPatterns(exPaths);
          }
      }
      ```

      

### 3.2 servlet

#### 3.2.1 servet Native

在springboot框架内使用servlet对象
使用步骤：

1. 创建一个servlet
2. 实现doget dopost方法
3. 注册servlet 用来容纳servlet

```java
package com.example.demo.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

//创建servlet类
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().print("hello get");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=utf-8");
        resp.getWriter().print("hello post");
    }
}

```



#### 3.2.2 filter

```java
package com.example.demo.servlet;

import javax.servlet.*;
import java.io.IOException;

public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("执行了MyFilter,DoFilter");
        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}

```

#### 3.2.In_short

```java
package com.example.demo.config;

import com.example.demo.servlet.MyFilter;
import com.example.demo.servlet.MyServlet;
import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.boot.web.servlet.ServletRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.servlet.Filter;
import javax.servlet.Servlet;

@Configuration
public class ServletConfig {
    @Bean
    public ServletRegistrationBean registrationBean(){
        MyServlet myServlet = new MyServlet();
        //    public ServletRegistrationBean(T servlet, String... urlMappings) {
        //        this(servlet, true, urlMappings);
        //    }
        ServletRegistrationBean<MyServlet> servletServletRegistrationBean = new ServletRegistrationBean<>(myServlet,"/servlet","/test");
        return servletServletRegistrationBean;
    }
    @Bean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean<Filter> filterFilterRegistrationBean = new FilterRegistrationBean<>();
        filterFilterRegistrationBean.setFilter(new MyFilter());
        filterFilterRegistrationBean.addUrlPatterns("/user/*");
        return filterFilterRegistrationBean;
    }
}
```



### 3.3 字符集过滤器

respond的字符集过滤器是自动配置的，正常情况下不需要自己添加。
方式一：

```java
    @Bean
    public FilterRegistrationBean bean(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        //使用框架中的FIlter
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        //指定编码方式
        characterEncodingFilter.setEncoding("utf-8");
        //指定request，response都是用encoding的值
        characterEncodingFilter.setForceEncoding(true);

        bean.setFilter(characterEncodingFilter);
        bean.addUrlPatterns("/*");
        return bean;
    }
```

方式二：

```yaml
server:
  port: 8080
  servlet:
    encoding:
      enabled: true # 先买个关子 为什么呢？ 在springboot中默认已经配置了 字符集过滤器 编码默认ISO—8859-1 设置enable=false是关闭系统中配置好的过滤器 使用自定义的过滤器4
      charset: utf-8
      force: true
```

+ 方式二更加简单 推荐使用 主要用于自定义的servlet

## 4 ORM操作数据库

先看一下需要的配置：

![zxtEg.png](https://s1.328888.xyz/2022/06/09/zxtEg.png)

Web SQLDriver Mybatis

ORM(Object Relate Mapping) java对象与数据库表的映射关系。
使用步骤：

1. mybatis起步依赖：完成mybatis对象的自动配置，对象放在容器中
2. pom文件中指定src/main/java目录中的xml文件包含到classpath中
3. 创建实体类Student
4. 创建DAO接口，创建一个查询学生的方法
5. 创建一个对应的Mapper文件 写sql语句
6. 创建servlet层对象，创建studentService接口和他的实现类。去DAO对象的方法，完成数据库的操作
7. 创建controller对象，访问Service
8. 写application.properties文件 配置数据库信息

#### 第一种方式：@Mapper

```java
@Mapper
public interface StudentsDAO {
    Student selectById(@Param("stdtno") Integer Id);
}
```

第二种方式：@MapperScan()

+ 在主类上面添加使用

第三种方式：

+ 将mapper放入到resource文件夹中创建一个mapper子目录
+ 将已有的将来的mapper存放到src/main/resource/mapper目录下

+ 在配置文件中实现如下的配置

```yaml
mybatis:
  mapper-locations: classpath:mapper/*.xml #mapper的包的位置
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #添加一个日志类
```



## 5 事务

spring框架中的事务处理：

1. 事务管理的对象：事务管理器(接口，接口有许多实现类)
   例如：使用jdbc或者mybatis访问数据库，使用事务的管理器：DataSourceTransactionManager
2. 声明式事务：在xml文件中使用注解说明事务的内容
   控制事务的隔离级别，传播行为，超时时间
3. 事务的处理方式：
   1. spring框架中的@Transaction
   2. aspectJ框架可以在xml文件中，声明事务的内容

springboot中使用事务：上面的两种方式都是可以的

1. 在业务方法上面加入@Transaction，加入注解后，方法就有了事务功能了。
2. 明确的在主配置类上面，加入@EnableTransactionManager
   

```java
  @Transactional
    @Override
    public int recordNewStudent(Student stu) {
//        Student student = new Student();
//        student.setStdtno(24);
//        student.setCountry("群雄");
//        student.setName("貂蝉");
//        student.setEmail("diaochan@gmail.com");
        int insert = mapper.insert(stu);

        //抛出一个运行时异常  目的是回滚事务
//        int m = 10/0;

        return insert;
    }

@SpringBootApplication
@MapperScan(basePackages = "com.sea.transaction.dao")
@EnableTransactionManagement
public class TransactionApplication {

    public static void main(String[] args) {
        SpringApplication.run(TransactionApplication.class, args);
    }

}


```

## 6 接口的架构风格 REST-style

+ 接口：API:application programing interface 应用程序接口
+ 接口可以访问servlet，controller的url 调用其他程序的函数
+ 架构风格：api的组织方式

   + http://localhost:8090/transaction/stu/add?name=%E8%B2%82%E8%9D%89&stdtno=24
   + 在地址上提供了访问资源的名称addstudent，在其后使用了get方式传递参数

+ RESTful架构风格：

  + （1）REST Representational State Transfer 简称REST （表现层状态转义）

     是一种架构理念和设计原则 不是强制性的要求 使得接口更加简洁更有层次 不是标准

  + （2）REST中的要素

    ​	用REST表示资源和对资源的操作。在互联网中表示一个资源或者一个操作
    	资源使用俩表示，在互联网，使用的图片等资源都是有对应url表示资源

    + 在url中表示资源，以及访问资源的信息,在url中使用分隔符的方式，分离不同的信息
    + http://localhost:8080/mybase/myboot/1001

```java
GET, 查询资源  //处理单个资源使用单数方式 处理多个请求使用复试形式
HEAD, 
POST, 提交资源 
PUT, 更新资源
PATCH,
DELETE, 删除资源
OPTIONS,
TRACE;
```

但是浏览目前自是支持post和get方式

+ 在post请求中传递数据

```html
<form action="/students" method="post">
    name:<input name="stuname"/> <br>
    age:<input name="age"/> <br>
</form>
```

+ put:更新资源--sql update

```html
<form action="addStudent" method="post">
    name:<input name="name"/>
    age:<input name="age"/>
   <input type="hidden" name="_method" value="PUT"/>
</form>
```

+ delete:删除资源---sql delete

```html
<a href="http://localhost:8080/deleteStu">delate aim data</a>
```

+ 传递数据需要使用什么方式呢？

+ 分页 排序 等无关紧要的参数 依然是放在url的后面 
  case1：http://localhost:3306/listStuInfo?PerpageNum=5

+ 对资源的操作方式

+ REST 表现状态转移

  1. 表现层状态转义：表现层就是视图层，显示资源的，通过视图页面，jsp等等显示操作资源的结果

  2. 状态：指的是资源的变化

  3. 转移：资源可以变化的，可以创建，new一个状态，资源一创建后可以查询资源，并且在此之后可以查看他的内容，可以被修改，修改后资源和之前的是不一样的

     

### 6.1 REST style相关的注解

​	@Pathvariable:从路径中获取资源

​	@GetMapping：支持get请求的方式 == @RequestMapping(method="RequestMethod.GET")

​	...
	其他的我们在前面的课程里面已经用过 不再赘述了
	

```java
package com.sea.myapp.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class appController {
    //查询id=1001的学生

    /**
     * @PathVariable（路径变量）：获取url中的数据
     *  属性：value： 路径变量名
     *  位置：放在控制器的形参前面
     *
     */
    @GetMapping("/hello/{stdtno}") //需要特别注意的是格式是{}而不是${}   容易犯错的点
    public String test( @PathVariable("stdtno") String stdtno){
        return "hello world! students "+stdtno;
    }

}
```

### 6.2 在页面中或者ajax中，支持put，delete请求

+ 在springmvc中有一个过滤器，支持post请求转换为put,delete
+ 过滤器：org.spring.springframework.web.filter.HiddenHttpMethodFilter
  + 实现步骤：
    1. application.properties（yaml）：开启使用HiddenHttpMethodFIlter过滤器
    2. 在请求也中，包含_method参数，它的值是put,delete，发起这个请求使用的是post方式

```html
     <input type="hidden" name="_method" value="put"> <!--比较容易忘记的是name="_method"-->
```


















































