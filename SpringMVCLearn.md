# SpringMVC

+ 纯注解式开发 
+ 跳转方式有优化
+ 日期处理时相对麻烦的
+ 拦截器：更多的处理请求和响应的时机 感觉类似于监听器
+ SSM整合(Spring SpringMVC mybatis) 整合后台的功能

# 1. 什么是SpringMVC

+ 它是用来mvc开发模式的框架用来优化控制器，他是spring家族的一员，他也具备AOP和IOC。
+ 什么是MVC？
  + 他是一种开发模式
  + 他是模型视图控制器的简称 (module view controller)
  + 所有的web应用都是基于MVC开发模式的
    + 模型层：包含实体类，业务逻辑，数据访问
    + 视图层：html，javascript，vue 都是视图层的 用来呈现数据的就是试图层
    + 控制层：用来接收用户端的请求，并且返回响应到客服端的组件 servlet就是controller层
  + ![lTnlB.png](https://s1.328888.xyz/2022/05/25/lTnlB.png)

## 1.1 springMVC的优点

1. 轻量级，基于MVC框架
2. 易于上手，容易理解，功能强大
3. 它具备IOC和AOP
4. 完全基于注解开发

+ ![lTZT7.png](https://s1.328888.xyz/2022/05/25/lTZT7.png)
+ ![lTNVg.png](https://s1.328888.xyz/2022/05/25/lTNVg.png)

## 1.2  基于注解的SpringMVC框架的开发步骤：

1. 新建项目，选择webapp的模板
2. 添加缺失的目录(test,java,resource) 并修改目录属性
3. 修改pom文件，添加SpringMVC的依赖，添加servlet的依赖
4. 添加springmvc.xml配置文件,指定包扫描，添加视图解析器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--添加包扫描-->
    <context:component-scan base-package="com.sea.Controller"></context:component-scan>
    <!--添加视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!----------------------------------------------容易出错的点------------------------------------------------>
        <!--配置一个前缀-->
        <property name="prefix" value="/admin/"></property> <!--前缀是要夹在//中间的-->
        <!--------------------------------------------------------------------------------------------------------->
        <!--配置一个后缀-->
        <property name="suffix" value=".jsp"></property>
    </bean>

    <bean class="org.springframework.web.servlet.DispatcherServlet"></bean>
</beans>
```

5. 删除web.xml文件，新建web.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--注册springmvc的框架-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--
            指定拦截什么样的请求
            http://localhost:8080/one
            http://localhost:8080/demo.action
            <a href="${pageContext.request.ContextPath}/demo.action">访问服务器</a>
        -->
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>
</web-app>
```

6. 在web.xml文件中注册springmvc框架（所有的web请求都是基于servlet的）
7. 在webapp目录下新建admin目录，在admin目录下新建main.jsp页面，删除index.jsp页面,并新建，发送请求给服务器
8. 开发控制器(servlet),他是一个普通的类。(普通的类要担当起servlet的功能)

```java
package com.sea.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import java.sql.Driver;
import java.sql.DriverManager;

@Controller //交给spring创建对象
public class DemoAction {
    /*
    * action中所有的功能实现都是由方法来实现的
    * action的方法是有规范的
    *   1.访问权限是public
    *   2.返回值时任意的
    *   3.方法的名称任意
    *   4.方法可以没有参数 如果有可以任意类型
    *   5.要使用@requestMapping的注解来声明一个访问的路径(名称)
    * */
    @RequestMapping("/demo1")
    public String demo()
    {

        System.out.println("服务器访问成功............");
        return "mainkklt";  //直接跳到/admin/main.jsp 这里是有一定的磨法的感觉的 
        //磨法已破解 <property name="prefix" value="/admin/"></property> <!--前缀是要夹在//中间的-->
    }
}
```

9. 添加tomcat进行测试功能

+ ```xml
  <build>
    <resources>
      <resource>
        <directory>/src/main/java</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>
      <resource>
        <directory>/src/main/resources</directory>
        <includes>
          <include>**/*.xml</include>
          <include>**/*.properties</include>
        </includes>
      </resource>
    </resources>
  </build>
  ```

## 1.3 分析web请求

​    index.jsp <----------------------->Dispatvhersrevlet<--------------------------> springMVC的处理器是一个普通的方法不再是一个servlet
						        核心处理器

+ 因此我们需要在spring的配置文件中添加一个核心处理器的bean

### 1.3.1 注解的解读

 @RequestMapping可以加在类上 也可以加在方法上

```java
@RequestMapping("/demo1")
public String demo(){
    {System.out.println("服务器访问成功............");
    return "mainkklt";  //直接跳到/admin/main.jsp 这里是有一定的磨法的感觉的 磨法已破解 
}
```
```xml
<a href="${pageContext.request.contextPath}/demo.action">访问服务器</a>
```

（1） 加在方法上，是为此方法注册一个可以访问的名称

（2） 此注解可以加在类上面，相当于是包名

```java
package com.sea.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import java.sql.Driver;
import java.sql.DriverManager;

@Controller //交给spring创建对象
public class DemoAction {
    /*
    * action中所有的功能实现都是由方法来实现的
    * action的方法是有规范的
    *   1.访问权限是public
    *   2.返回值时任意的
    *   3.方法的名称任意
    *   4.方法可以没有参数 如果有可以任意类型
    *   5.要使用@requestMapping的注解来声明一个访问的路径(名称)
    * */
    @RequestMapping("/demo1")
    public String demo()
    {

        System.out.println("服务器访问成功............");
        return "mainkklt";  //直接跳到/admin/main.jsp 这里是有一定的磨法的感觉的
    }
}
```

+ 提交数据到action
+ 各种数据携带的返回值
+ 页面跳转的四种跳转
+ 携带数据跳转

### 1.3.2 五种数据提交方式数据的方式

1. ## 单个数据提交

```xml
<form action="${pageContext.request.contextPath}/one.action" >
    姓名：<input name="myname"> <br>
    年龄：<input name="myage"> <br>
    <input type="submit" value="提交">
</form>
```

```java
@RequestMapping(value = "/req",method = RequestMethod.POST)
public String Test1(){
    System.out.println("this is PostRequest");
    return "Unique";
}
```

2. ##  对象封装提交

   再提交中使用参数的名称与实体类中的变量名一致，这可以自动提交数据，自动创建对象，自动类型转换，自动封装数据到对象中。

   ```xml
   <form action="${pageContext.request.contextPath}/two.action">
       姓名：<input name="uname"> <br>
       年龄：<input name="uage"> <br>
       <input type="submit" value="提交"/>
   </form>
       
   --------------------------------------------------------------------------------
   package com.sea.Controller.paramterContain;
   public class Users {
       private String uname;
       private int uage;
   
       public String getUname() {
           return uname;
       }
   
       public void setUname(String uname) {
           this.uname = uname;
       }
   
       public int getUage() {
           return uage;
       }
   
       public void setUage(int uage) {
           this.uage = uage;
       }
   
       public Users() {
       }
   
       public Users(String uname, int uage) {
           this.uname = uname;
           this.uage = uage;
       }
   
       @Override
       public String toString() {
           return "Users{" +
                   "uname='" + uname + '\'' +
                   ", uage=" + uage +
                   '}';
       }
   }
       _________________________________________________________________________________________________________________________
           @RequestMapping("/two")
       public String jump2(Users users){
           System.out.println(users);
           return "Unique";
       }
   
   ```

3. ## 动态占位符提交 

   仅限于超链接或者地址栏提交数据，他是一杠一值，一杠一大括号。

#### @PathVariable

```xml
<a href="${pageContext.request.contextPath}/three/张三/22.action">动态提交</a>
```

```java
@RequestMapping("/three/{uname}/{uage}")
public String jump3(@PathVariable("uname") String name,@PathVariable("uage") int age){
    System.out.println("name:"+name+"  age:"+age);
    return "Unique";
}
```

4. ## 映射名称不一致

#### @RequestParam

提交请求参数与action方法的形参的名称不一致，使用注解@RequestParam来解析

```xml
<h2>参数不一致的解决方案</h2>
<form action="${pageContext.request.contextPath}/four.action">
    姓名：<input name="name"> <br>
    年龄：<input name="age"> <br>
    <input type="submit" value="提交"/>
</form>
```

```java
@RequestMapping("/four")
public String jump4(@RequestParam("name") String uname,@RequestParam("age") int uage){
    System.out.println("uname:"+uname+"  uage:"+uage);
    return "Unique";
}
```

+ 该注解专门用来结果名称不一致的问题

5. ## 手工提取数据

+ 自己手工提取数据

```java
@RequestMapping("/five")
public String jump5(HttpServletRequest request){
    String name = request.getParameter("name");
    String age = request.getParameter("age");
    System.out.println("name:"+name+"  age:"+Integer.parseInt(age));
    return "Unique";
}
```

```Xml
<form action="${pageContext.request.contextPath}/five.action">
    姓名：<input name="name"> <br>
    年龄：<input name="age"> <br>
    <input type="submit" value="提交"/>
</form>
```

### 1.3.3 中文乱码的解决方式

配置过滤器.在web.xml中配置文件。 过滤器和监听器在web.xml文件中配置

```xml
    <filter>
        <filter-name>encode</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <!--
                private String encoding;
                private boolean forceRequestEncoding;
                private boolean forceResponseEncoding;
    -->
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encode</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

### 1.3.4 返回值的问题

+ 我们的返回值是任意的，但是当前我们仅仅使用的是String 剩下的返回值是什么呢？如何使用呢？

  1. String：客户端资源的地址，自动拼接前缀后缀，还可以屏蔽自动拼接字符串，可以指定返回的路径

  2. Object：返回json格式的对象，自动将对象或集合转换为json，使用的是jackson工具必须要添加依赖 

     一般用于Ajax请求，如何加、jackson的依赖呢？

  3. void：无返回值，一般用于ajax的请求

  4. 基本数据类型，用于ajax请求。

  5. ModelAndView：返回数据和视图对象，现在已经很少使用了

+ 完成ajax请求访问服务器，返回学生集合。

  1. 添加ajax的依赖

  ```xml
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.8</version>
  </dependency>
  ```

  2. 在webapp的目录下新建js目录，添加jquery函数库

  3. 在index.jsp页面上导入函数库

  4. 在action上添加注解@ResponseBody，用来处理ajax的请求

  5. 在springmvc.xml文件中添加注解驱动<mvc:annotationdriver/>，它用来解析@ReSponseBody注解

     

  ```jsp
  <%--
    Created by IntelliJ IDEA.
    User: Administrator
    Date: 2022/5/28
    Time: 8:41
    To change this template use File | Settings | File Templates.
  --%>
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <html>
  <head>
      <title>Title</title>
      <%--导入jquery函数库--%>
      <script src="js/jquery-3.1.1.js"></script>
  </head>
  <body>
  <br><br><br><br><br>
  <a href="javascript:showStu()">return a set of students from the server port</a>
  <div>waiting......</div>
  <script type="text/javascript">
      function showStu(){
          //使用jquery封装的ajax()方法发送请求
          $.ajax({
              url:"${pageContext.request.contextPath}/list.action",
              type:"get",
              dataType:"json",
              success:function (stuList){
                  var s = "";
                  $.each(stuList,function (i,stu){
                      s+=stu.name + "----"+stu.age+"<br>" //stu.age和stu.name 会调用students的get方法
                  });
                  $("#mydiv").html(s);
              }
          });
      }
  </script>
  
  </body>
  </html>
  ```

### 1.3.5 四种跳转方式

+ 请求转发 
+ 重定向

```java
package com.sea.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class JumpAction {
    @RequestMapping("/one")
    public String jump1(){
        System.out.println("this is a page from jumped via relay");
        return "last"; //默认使用视图解析器拼接前后缀进行页面跳转
    }
    @RequestMapping("/two")
    public String jump2(){
        System.out.println("this is a action from jumped via relay");
        //默认使用视图解析器拼接前后缀进行页面跳转 /admin/other.action.jsp是不可以的 所以我们使用了一个特殊的标记 forward 转发
        //forward:字符串可以屏蔽字符串的拼接
        return "forward:/other.action";
    }
    @RequestMapping("/three")
    public String jump3(){
        System.out.println("this is a page from jumped via redirect");
        return "redirect:/admin/last.jsp"; //direct：会屏蔽字符串的拼接 所以要写全部的在服务器中的路径
    }
    @RequestMapping("/four")
    public String jump4(){
        System.out.println("this is a action from jumped via redirect");
        return "redirect:/other.action";
    }

}
```

+ 注意地址栏的变化哦
+ forward和redirect都会屏蔽视图解析器的字符串拼接

#### springmvc默认参数

+ 不需要创建直接使用就好

1. HttpServletServlet
2. HttpServletResponse
3. HttpSession
4. Model
5. Map
6. ModelMap

注意：Map，ModelMap和request一样，都是用请求作用域进行数据的传递，使用服务器的跳转必须是请求转发的。

```jsp
<%--
  Created by IntelliJ IDEA.
  User: Administrator
  Date: 2022/5/28
  Time: 10:56
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<br><br><br><br><br>
<h1>access success</h1>

<%--        req.setAttribute("requestUser",users);
        session.setAttribute("session",users);
        model.addAttribute("model",users);
        map.put("mapuser",users);
        mmp.addAttribute("ModelMap",users);
        --%>
RequestUser:${requestUser}<br>
session:${session}<br>
model:${model}<br>
map:${mapuser}<br>
mmp:${ModelMap}<br> <%!--EL表达式的使用 但是我没有学过呢--%> 


</body>
</html>

```

### 1.3.6 日期的处理

#### @DateTimeFormat

+ 日期的提交处理

  1. 单个日期处理

     要使用注解@DateTimeFormat，此注解必须搭配springmvc.xml文件中<mvc:annotation-driven> > :yum:

     ```java
     SimpleDateFormat sf = new SimpleDateFormat("yyyy-MM-dd"); //别在腰上
     @RequestMapping("/mydate")
     public String mydate(@DateTimeFormat(pattern = "yyyy-MM-dd") Date mydate){ //date 无法以参数的形式 传入服务端
         System.out.println(mydate);
         System.out.println(sf.format(mydate));
         return "show";
     }
     ```

  2. 类中全局日期处理

     注解开发:注册一个注解，用来解析本类中的所有日期类型，自动转换。

     ```java
     SimpleDateFormat sf = new SimpleDateFormat("yyyy-MM-dd");
     @InitBinder
     public void initbind(WebDataBinder binder){
         binder.registerCustomEditor(Date.class,new CustomDateEditor(sf,true));
     }
     ```

+ 日期的显示处理

  在页面上显示好看的日期，必须使用JSTL.

  1. 添加依赖jstl
  2. 在页面上导入标签库
  3. 使用标签显示数据

  + 如果是一个单个的日期对象，直接转为想要的格式进行显示，如果是list集合中的实体对象的成员变量是日期，则必须使用JSPL进行显示。

  ```xml
  <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  <%--导入jstl的核心标签库--%>
  <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
  <%--导入jstl格式化标签库--%>
  <%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
  ```

+ 资源放在WEB-INF的文件夹下 WEB-INF的资源对外不可直接访问 增强了webapp的安全性（只有使用请求转发才能进入到WEB-INF中访问资源）

  + INF目录下的动态资源是无法直接访问的，（只有使用请求转发才能进入到WEB-INF中访问资源）
  + 去掉后缀的action匹配

+ 拦截器实现权限的验证

  + 实现登录处理
    + 登录了可以跳转页面 否则跳回。

### 1.3.7 springmvc的拦截器

+ 针对请求和响应进行额外的处理，在请求和响应的过程中添加预处理，后处理和最终处理。
+ 拦截器的执行时机
  1. prehandle()：请求被处理之前进行操作
  2. posthandle()：在请求处理之后，但是结果还没有渲染前进行操作，可以改变相应的结果
  3. afterhandle()：所有的请求响应结束后执行善后工作，清理对象，关闭之源
+ 拦截器实现的两种方式
  + 实现接口 HandleInterceptor接口
  + 实现父类 HandleInterCeptorAdopter()父类
  + 父类是单继承 接口是多实现
+ 拦截器的实现步骤
  1. 改造登录的方法，在session中存储用户的信息，用于进行权限验证。
  2. 开发拦截器的功能，实现handleInterceptor接口，重写prehandle()方法。
  3. 在springmvc文件中注册拦截器

配置拦截器

```xml
<!--mvc注解驱动 ajax相关的有些注释需要使用-->
<mvc:annotation-driven/>
<mvc:interceptors>
    <mvc:interceptor>
        <!--映射要拦截的请求-->
        <mvc:mapping path="/**"/>
        <mvc:exclude-mapping path="/showLogin"/>
        <mvc:exclude-mapping path="/login"/>
        <bean class="com.sea.interceptor.LoginInterceptor"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

```java
package com.sea.interceptor;

import jdk.nashorn.internal.ir.RuntimeNode;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.util.HashMap;
import java.util.Map;

public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //完成是否登陆过的判断
        HttpSession session = request.getSession();
        if(session.getAttribute("UserInformation") == null ){
            request.setAttribute("msg","account or password is not correct");
            request.getRequestDispatcher("/WEB-INF/JSP/login.jsp").forward(request,response); //请求转发 前面的字符串写的是webapp中的路径
            return false;
        }
        return true;

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

# 2. SSM整合

1. 建表
2. 新建maven项目 选择webapp的模板 添加缺失的目录
3. 在pom文件中加入依赖 (使用自己使用的) 不需要默写 看懂就行
4. 添加jdbc.properties属性文件
5. 添加sqlMapConfig.xml文件
6. 添加applicationContext_mapper.xml文件 (数据访问层的核心配置文件)
7. 添加applicationContext_service.xml文件（业务逻辑层的核心配置文件）
8. 添加springmvc.xml文件
9. 删除web.xml文件，新建，改名，设置中文编码，并注册springmvc框架
10. 新建实体类 user
11. 新建userMapper.java接口
12. 新建usermapper.xml实现增删查的所有的内容
13. 新建service接口和实现类
14. 新建测试类，完成所有功能的测试
15. 新建控制器，完成所有功能
16. 浏览器设置功能













lan'ji