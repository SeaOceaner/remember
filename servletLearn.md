# 1  servlet：

```html
<!--注意一下的路径以斜杠开始带享目名不需要添加端口号和IP和协议名-->
<a href="/oa/login.html">user login</a>
```

+ 界面层：通常有美工来完成  html javascript  
+ 逻辑层：通常由程序员来完成 javaBean...servlet spring  
+ 数据层：数据库分析员来完成  jdbc jdbc-odbc  
+ 动态的数据需要JDBC技术来实现 我们需要写java程序的通信原理是什么样的呢？
+ 创建一个servlet然后再在配置文件内将其部署 

![hb6R7.png](https://s1.328888.xyz/2022/05/05/hb6R7.png)

webApp是具有独立性的一方 

## 

+ servlet规定了：
  - 一个合格的webapp应该是这样的文件目录结构

  + 一个合格的webapp应该是怎样的配置文件
  + 一个合格的webapp应该将配置文件存放在哪里
  + 一个合格的webappjava程序放在哪里

servlet规范是什么样的规范：

+ 遵寻servlet规范的webapp是可以放在WEB服务器中运行。
+ servlet规范了哪些接口
  + 规范了哪些接口
  + 规范了那些类
  + 规范了一个web应用中应该有哪些配置文件
  + 规范了一个web应用中配置文件的名字
  + 规范了一个web应用中配置文件的内容
  + 规范了一个合法有效的webapp合法有效的目录结构因该是什么样的

## 1.1  开发一个带有servlet的webapp

+ 开发步骤是怎样的？

  + 第一步：在webapp目录下新建一个目录，起名crm(这个crm就是webapp的名字) 当然也可以是其他名字 bank 此时创建的目录作为app的根目录存在

  + 第二步：在根目录下创建一个WEB-INF

    + 这个目录的名字是严格规定的，写死的，无条件的，无理由的。

  + 第三步：创建一个classes文件夹，这个目录下存放的一定是.class文件

  + 第四步：创建一个lib目录(非必须) 但是如果webapp需要第三方的jar文件时，将其放入此文件夹内

  + 第五步：在WEB-INF目录下新建一个web.xml文件

    + 注意这个文件是必须的 xml文件就是配置文件记录了请求路径和servlet之间的关系 可以从root文件夹下拷贝没必要手写

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                            https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
        version="5.0"
        metadata-complete="true">
      
        <display-name>Welcome to Tomcat</display-name>
        <description>
           Welcome to Tomcat
        </description>
      
      </web-app>
      
      ```

      

  + 第六步：这个小java程序要实现java接口，

    + 接口不再jdk中，在webservice里面。

  + 第七步：在xml.class文件中编写配置信息，让"请求路径"和"servlet名"关联在一起

    + 这一步的专业名叫注册

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                        https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
    version="5.0"
    metadata-complete="true">
  
    <display-name>Welcome to Tomcat</display-name>
    <description>
       Welcome to Tomcat
    </description>
    <!--servlet描述信息-->
    <servlet>
  		<servlet-name>asdf</servlet-name>
  		<!--这个位置是必须带有--包名的全限定名-->
  		<servlet-class>com.sea.hello</servlet-class>
  		
    </servlet>
    
      <!--servlet映射信息-->
    <servlet-mapping>
    <!--这里也是谁编写 但是要和上面的一样-->
  	<servlet-name>asdf</servlet-name>
  	<!--这里需要一个路径 
  	唯一的要求是必须以斜杠开始
  	当前这个路径可以随便写-->
  	<url-pattern>/bbls</url-pattern>
    </servlet-mapping>
    
  
  </web-app>
  
  ```

+ 第十步： 启动tomcat服务器

+ 第十一步： 打开浏览器在浏览器上输入一个url



## 1.2 如何将webapp中的信息输入到浏览器中：

```java
public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        /*向控制台打印输出*/
        System.out.println("hello servlet");
        /*向浏览器打印输出*/
        //设置相应内容类型是普通文本或者是html代码                特别注意设置获取流对象之前设置才有效 不要放在后面
        servletResponse.setContentType("text/html;charset=utf-8");
        PrintWriter out = servletResponse.getWriter();
        out.print("the people republic of china is forever, peace full around the world is forever");
        /*浏览器是可以识别html代码的所以我们也可以输出一段html代码*/
        out.print("<br><h1>the TIANNAN door</h1>");

    }
```

## 1.3 如何在servlet中连接数据库

+ 在servlet中直接编写java代码即可（JDBC）      如下

```java
package com.sea;

import jakarta.servlet.*;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.*;

public class studentsInfo implements Servlet {

    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        Connection connection = null;
        PreparedStatement ps = null;
        ResultSet set = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mybase?serverTimezone=GMT","root","why123456");
            PreparedStatement statement = connection.prepareStatement("select empno,ename,sal from emp");
            set = statement.executeQuery();

            servletResponse.setContentType("text/html");
            PrintWriter out = servletResponse.getWriter();

            while(set.next()){
                int empno = set.getInt("empno");
                String ename = set.getString("ename");
                double sal = set.getDouble("sal");
                out.print(empno+","+ename+","+sal+"<br>");
            }

        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            if (set != null) {
                try {
                    set.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (ps != null) {
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }

    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```



## 1.4 servlet对象的生命周期

## 1.5 在IDE中开发servlet程序

+ 第一步：New project创建一个新的空的项目
+ 第二步：新建一个新的module(javase)
+ 第三步：让模块变成javaee的模块           (目录结构要符合webapp的规范)
  + 在module上点击右键，添加框架支持 Add framework support...
  + 在弹出的窗口中选择webApplication，之后会自动生成合法的目录结构
  + 重点需要注意的 生成的目录中有一个web目录这个目录就对应webapp的根目录
+ 第四步：根据web application中生成的jsp文件，这里先将其删除。
+ 第五步：编写servlet(helloServlet)
+ 第六步：编写servlet程序
+ 第七步：在web.xml文件中注册你编写的servlet对象
+ 第八步：创建一个html文件，提供一个超文本连接，发送请求，tomcat执行后台的studentServlet
+ 第九步：让IDEA工具关联Tomcat服务器。关联的过程中将webapp部署到Tomcat服务器中
  + IDEA右上角的小锤子：Add configuration
  + 左上角加号，点击Tomcatserver->local
  + 在上面的选项卡中有deployment(点击用以部署webapp)
  + 修改进程上下文 写到直接的项目文件夹的名字
  + 应用
+ 第十步：启动tomcat服务器
  + debug模式可启动
+ 第十一步：打开浏览器

# 2  servlet生命周期：

## 2.1  什么是servlet的生命周期

+ servlet对象什么时候创建

+ servlet对象什么时候销毁

+ servlet对象创建了几个

+ servlet对象的生命周期是指一个servlet从出生到死亡的全过程

+ ##### servlet对象有谁来维护呢？

+ servlet对象有tomcat全权负责

+ tomcat服务器又被称为web容器

+ web容器来管理web的生死

+ ##### 我们自己new的servlet对象是不受web容器管理的

+ 在servlet中加一个无参数的构造器测试可知服务器中不会实例化servlet对象

+ 可以在启动之前创建对象但是需要在xml文件中填写整数

+ ```xml
  <load-on-startup>0</load-on-startup>
  内部的数值是一个优先级数 越小优先级越高
  ```

  

## 2.2 总结

+ servlet是一个单实例的对象 （但是servlet不满足单例模式，我们称之为单例 因为tomcat只创建了一个实例 别人管不着）
+ init和实例化只会进行一次 init还会在此处实例化一个对象 之后用户请求不会在进行init()函数了
+ server是每次请求都会进行一次
+ destory方法是在销毁之前调用的
+ 服务器刚启动的时候默认是不会启动的 用户第一次请求的时候tomcat会调用无参构造方法创建一个实例

# 3 适配器模式修改servlet

+ **<u>*接口方法太多但是子类使用的方法不多？如何才能减少子类的代码量？*</u>**

  接口类--->抽象类  实现了屏蔽效应
  编写一个抽象类继承servlet接口实现了简化代码

+ 思考一下：怎么进一步简化开发？

  + 子类的init还能执行吗？

    会，会默认调用父类的方法；

模拟一下tomcat的代码：

```java
public class tomcat{
    public static void main(String[] args){
        Class clazz = Class.forname("com.xxx.xxxx.xxxx.mySeriver");
        Object obj = clazz.newInstance();
        Servlet slt = (Servlet)obj;
        
        //创建一个Servletconfig实例化出来
        
        //调用servlet的init方法
        slt.init(servletconfig);
        //调用servlet的server方法
        
    }
}
```

final可以阻止子类重写方法 但是如果想让子类可以自定义方法的话 可以在final修饰的方法中加入一个重载方法 并在类中申明重载的方法 这样子类可以覆写没有final修饰的方法达到自定义的效果。

```java
....
    
private ServletConfig config;

@Override
public final void init(ServletConfig config) throws SerletException{
    this.config = config;
    this.init();
}

public void init(){
}
```

这是模板方法设计模式

+ 用transient修饰的属性是不可以序列化的
+ ServletConfig是什么？
  + 是servlet规范中的一员，servletConfig是一个接口
+ 谁去实现这个接口？
+ servletconfig对象里面是什么呢？
  + 包装的信息是web.xml中<servlet></servlet>中的信息。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_5_0.xsd"
         version="5.0">
    <servlet>
        <servlet-name>config</servlet-name>
        <servlet-class>com.sea.ConfigTest</servlet-class>
        <!--init-param可以初始化信息-->
        <init-param>
            <param-name>driver</param-name>
            <param-value>com.mysql.cj.jdbc.Driver</param-value>
        </init-param>
        <init-param>
            <param-name>url</param-name>
            <param-value>jdbc:mysql://localhost:3306/mybase?serverTimezone=GMT</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>config</servlet-name>
        <url-pattern>/config/a</url-pattern>
    </servlet-mapping>

</web-app>
```

+ 以上servlet标签中的init-param 是初始化参数 会自动被toncat装载到ServletConfig中；

  + 可以用一下方法获取config里面的内容

  ```java
  Enumeration<String> names = config.getInitParameterNames();
          while(names.hasMoreElements()){
              String s = names.nextElement();
              writer.print(s+"<br>");
          }
  
          String url = config.getInitParameter("url");
          writer.print(url);
  ```

  

+ 整个webapp的servletContext对象只有一个，被称为上下文对象或者环境

+ servletContext对象对应的是真个web.xml文件

```java
ServletContext context = this.getServletContext();
context.log("吾剑未尝不利 借汝头一用");
//为记录在tomcat的logs目录里
```

注意：以后编写servlet类的时候，实际上不会去直接继承genericservlet类的，因为我们是B/S的系统，这种系统使用HTTP超文本传输协议的，在servlet规范中提供了一个叫httpservlet的类。httpservlet是genericservlet的子类



# 4 HttpServlet

##     4.1 http的结构

+ http请求协议包含四个部分：
  + 请求协议 浏览器->服务器
    + 请求行
    + 请求头
    + 空白行
    + 请求体
  + 响应协议 服务器->浏览器
    + 状态行
    + 响应头
    + 空白行
    + 响应体
+ http的具体报文

### 4.1.1  get http

+ 响应头

  ```
  HTTP/1.1 200            				状态行
  Content-Type: text/html;charset=UTF-8   响应头
  Content-Length: 142
  Date: Sat, 07 May 2022 10:16:48 GMT
  Keep-Alive: timeout=20
  Connection: keep-alive
  									空白行
  <!DOCTYPE html>						 响应体
  <html lang='en'>
  <head>
  <meta charset='UTF-8'>
  <title>from get servlet</title></head>
  <body><h1>from get servlet</h1></body>
  </html>								
  								
  ```

  + 状态行
    + 第一部分：版本协议号 http/1.1
    + 第二部分：状态码 （HTTP协议中规定的响应状态号，不同的响应结果对应不同的号码）
      + 200 表示响应成功，正常结束
      + 404 表示资源不存在，通常是路径错误，或者是路径正确但是服务器中的资源没有启动
      + 405 表示前端发送的请求方式与后端处理方式不一致时发生的
        + 比如：前段是POST请求，后端的处理方式是get方式进行处理的
      + 500 表示服务器的程序出了异常，一般认为是服务器端的错误导致的
  + 怎么查看协议的内容
    + chrome F12

### 4.1.2 post http



```
Request URL: http://localhost:8080/httptest/postservlet 请求行
Request Method: POST								请求头
Status Code: 200 
Remote Address: [::1]:8080
Referrer Policy: strict-origin-when-cross-origin
Connection: keep-alive
Content-Length: 144
Content-Type: text/html;charset=UTF-8
Date: Sat, 07 May 2022 10:48:29 GMT
Keep-Alive: timeout=20
POST /httptest/postservlet HTTP/1.1
Host: localhost:8080
Connection: keep-alive
Content-Length: 26
Cache-Control: max-age=0
sec-ch-ua: " Not A;Brand";v="99", "Chromium";v="99", "Google Chrome";v="99"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: http://localhost:8080
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.82 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost:8080/httptest/index.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
												空白行
username: lisi&password: 123						请求体
```

+ 请求行有三部分
  + 请求方式
    + get
    + post
    + delete
    + put
  + URI
    + 什么是uri？同一资源标识符。
      + 代表某个资源的名字，但是通过URI是定位的
    + 什么是URL？
      + 同一资源定位符
    + URL包括URI
  + http版本号
+ 请求头
  + 请求的主机
  + 主机的端口号
  + 浏览历史
  + 平台信息
  + cookie等信息
+ 空白行
  + 空白行用来区分请求头和请求体
+ 请求体向服务器发送的具体数据

## 4.2  get请求和post请求有什么区别？

### 4.2.1 结构差异

+ 发送中的不同
  + 只有当你的form中method属性是post时，才是POST方式，其他的全是get方式
+ get方式常用方式
  + 直接点击超链接
  + 直接输入url访问website
  + 使用form时标签中的method属性是get
+ grt请求发送数据的时候 会挂在url后面，并且url后面会加一个?,?后面是数据。这样会导致返回的数据显示在url上。
+ post请求发送数据的时候在请求体中发送，不会回显到浏览器的地址栏中。
+ 但是不管是post还是get它们的数据格式是完全相同的 不过是位置不同
+ get请求只能发送普通的字符串，并且发送的长度是有长度限制的。
  + 无法发送大数据量
+ stop是可以发送任意类型的数据的，包括声音，视频，图像等等。
+ get请求在w3c中是这样描述的：get请求比较适合从服务端获取数据。
+ post请求在w3c中是这样描述的：post请求比较合适向服务器端传送。

### 4.2.2 安全性差异

+ get请求是绝对安全的，因为get请求是从服务器中获取数据
+ post请求是相对危险的，如果提交的数据通过后门的方式进入到服务器中，服务器是很危险的。所以大部分时候post请求是需要监听的。
+ get支持缓冲  每次get请求时会现在浏览器缓存中寻找，找不到才会请求。
  + 有没有这样一种需求，不想要get走缓存？
    + 只要每一次的get的请求路径不同 路径后面加一个每时每刻变化的时间戳，每次的路径不同，浏览器就不走缓存了
+ post不支持缓存



## 4.3 Httpservlet源码分析

+ httpservlet是专门为http协议准备的。比GeneticServlet跟加适合HTTP写一下的的开发。

+ servlet在那个包下？
  + jakarta.servlet.http.Httpservlet

+ 到目前为止我们接触了servlet规范中的哪些接口？
  + servlet			
  + servletContext
  + servletConfig
  + servletResponse
  + servletRequest
  + servletException

+ http包下有哪些接口呢？
  + HttpServlet
  + HttpservletResponse
  + HttpServletRequest

+ HttpServletRequest中封装了哪些信息呢？
  + 简称request对象
  + 封装了请求协议的所有内容

+ 总结：

  + 后端重写了doget方法，前端就要发送get请求
  + 后端重写了dopost方法，前端就要发送getpost请求

+ 实操

  + 编写一个类直接继承HttpServlet
  + 重写Doget和DoPost方法
    + 使用的时候一定记得删除super.do(post/get)();
  + 将servlet类配置到xml文件中
  + 准备前端的表单中(form表单),form指定请求路径即可
  + 准备前端的页面：在form中指定请求路径

+ 关于一个web站点的欢迎页面

  ```xml
  <welcome-file-list>
          <welcome-file>login.html</welcome-file>
      </welcome-file-list>
  ```

## 4.4 HttpServletRequest详解

+ httpServletRequest是一个接口
+ 是servlet规范的一员
+ 父接口叫servletrequest
  + Map<String,String[]> getParamentMap()
  + String getParamenter(String name) 最常用的
  + Servletrequest会记录用户使用时候提交的数据



+  如何将一个请求的信息传递到两个Servlet中呢？

### 4.4.1 Servlet的转发机制

+ 能否在AServlet中创建一个BServlet的对象 然后调用BServlet对象的doGet方法，把request对象传过去？
  + 不可以，因为processor自己实例化的对象TomCat是不会对他进行管理的
+ 正确操作
  + 跳转到
    + 转发（一次请求）

```java
		//第一步：获取请求转发器对象
        //相当于把/b包装到请求转发器中，实际上把下一个跳转的资源的路径告诉tomcat服务器
        RequestDispatcher dispatcher = req.getRequestDispatcher("/b");
        //第二步：调用请求转发器RequestDispatcher的forward方法，进行转发
        //转发的时候，这两个参数很重要，request和response都是要传递给下一个资源的
        dispatcher.forward(req,resp);

		其实第一步和第二步代码可以联合
		req.getRequestDispatcher("/b").forward(req,resq);

注意想要得到最终的页面需要输入的还是前序的url-pattern 但是最终显示的是后序的内容
```

+ 该方法实现了 两个servlet的之间的通信
+ 转发的下一个资源一定要是servlet吗？
  + 不是的，可以是任意的资源，需要注意的是路径的格式

```java
   String parameter = req.getParameter("name");		//获取前端提交的参数
   Object o = req.getAttribute("name");			   //获取req.getAttribute("name",new String("hello"));里面绑定的内容--->hello
```



## 4.5  使用纯servlet做一个单表的CRUD操作

+ 使用纯servlet实现单表的增删查改

+ 实现步骤

  + 1 创建一个数据库表

  + 2  准备一套HTML页面

    + 把html表格准备好在链接中可以跑的通

      + 添加页面

      + 修改页面

      + 详情页面

      + 欢迎页面

      + 列表页面

        

        +-------+----------+------------+
        | empno | ename | deptno |
        +-------+----------+------------+
        |  7369 | SMITH    |     20 |
        |  7499 | ALLEN    |     30 |
        |  7521 | WARD     |     30 |
        |  7566 | JONES    |     20 |
        |  7654 | MARTIN  |     30 |
        |  7698 | BLAKE    |     30 |
        |  7782 | CLARK   |     10 |
        |  7788 | SCOTT   |     20 |
        |  7844 | TURNER|     30 |
        |  7876 | ADAMS  |     20 |
        |  7900 | JAMES   |     30 |
        |  7902 | FORD     |     20 |
        |  7934 | MILLER  |     10 |
        |  8848 | SEAO     |     10 |
        +-------+--------------+-------+

+ 分析我们需要实现的功能：

  + 什么是功能？
    + 需要连接数据库的操作就是功能
  + 包括一个功能？
    + 查看部门列表
    + 保存用户需要功能
    + 删除
    + 修改用户信息

+ 在IDEA中搭建开发环境

  + 创建webapp
  + 添加依赖
  + jdbc工具类

+ 实现一个功能查看部门列表

  + 第一首先修改前端页面的超链接，因为用户先点击的就是超链接

  + ```html
    <a  href="/oa/dept/list" >员工列表</a>       <br>
    ```

  + 配置servlet

    + ```html
      <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
               version="4.0">
          <servlet>
              <servlet-name>list</servlet-name>
              <servlet-class>com.sea.List</servlet-class>
          </servlet>
          <servlet-mapping>
              <servlet-name>list</servlet-name>
              <url-pattern>/dept/list</url-pattern>
          </servlet-mapping>
      </web-app>
      ```

  + 写servlet

    + 在doget方法中查询所有员工信息

      + 所有的双引号要换成但引号，可能与java的字符串冲突。

      + sql语句可以先在DOS执行一下再写进去

      + ```java
         @Override
            protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                resp.setContentType("text/html;charset=utf-8");
                PrintWriter out = resp.getWriter();
         ```


                out.print("<!DOCTYPE html>");
                out.print("<html>");
                out.print("	<head>");
                out.print("		<meta charset='utf-8'>");
                out.print("		<title>员工信息</title>");
                out.print("	</head>");
                out.print("	<body>");
                out.print("		<h1 align='center'>部门列表</h1>");
                out.print("		<hr>");
                out.print("			<table border='1px' align='center' width='50%'>");
                out.print("				<tr>");
                out.print("					<th>员工序号</th>");
                out.print("					<th>员工姓名</th>");
                out.print("					<th>部门序号</th>");
                out.print("					<th>编辑</th>");
                out.print("				</tr>");
            
                Connection conn = null;
                PreparedStatement ps = null;
                ResultSet set = null;
                try {
                    conn = DBUtil.getConnection();
                    ps = conn.prepareStatement("select empno,ename,deptno from emppro");
                    set = ps.executeQuery();
                    while(set.next()){
                        String empno = set.getString("empno");
                        String ename = set.getString("ename");
                        String deptno = set.getString("deptno");
                        System.out.println(empno);
                        System.out.println(ename);
                        System.out.println(deptno);
                        out.print("				<tr>");
                        out.print("					<td>"+empno+"</td>");
                        out.print("					<td>"+ename+"</td>");
                        out.print("					<td>"+deptno+"</td>");
                        out.print("					<td>");
                        out.print("						<a href=''>删除</a>");
                        out.print("						<a href='update.html'>修改</a>");
                        out.print("						<a href='detail.html'>详细信息</a>");
                        out.print("					</td>");
                        out.print("				</tr>");
                    }
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }finally{
                    DBUtil.disConnection(set,ps,conn);
                }


​        

                out.print("			</table>");
                out.print("			<a href='Add.html'>添加员工</a>");
                out.print("	</body>");
                out.print("</html>");
            }
        ```
      
        + 需要注意的是<a href=" ">前端发送请求要加项目名 action和href都是需要加项目名的
      
        + ```java
          String path = req.getContextPath(); //可以得到请求webapp的地址
        ```
    
      ​    

+ 技巧

  + 重点：向服务器提交数据的格式中的问号必须是英文的？ 不能是中文的？

+ 配置empDetial信息

  + ```html
    <servlet>
            <servlet-name>detail</servlet-name>
            <servlet-class>com.Detail</servlet-class>
        </servlet>
        <servlet-mapping>
            <servlet-name>detail</servlet-name>
            <url-pattern>/dept/detail</url-pattern>
        </servlet-mapping>
    ```

    

+ 编写一个servlet：

  + 实现detail的展示

+ ```html
  <body>
      <script type="text/javascript">
          function del(empno){
              if(window.confirm("亲 删了无法恢复哦")){ //弹出确认框 有ok和no选项->返回boolean
                  /*	document.location.href="目标url"
                 		document.location ="目标url"
                 		Window.location.href="目标url"
                 		Window.location="目标url"
                  */ //四种方式可选
                  document.location.href="/oa/dept/delete?empno="+empno
              }
          }
      </script>    
     
  </body>
  
  <a href="javascript:void(0)" onclick="del("+empno+")">删除</a>
  ```

  

## 4.6转发与重定向

+ 在一个webapp中有两种跳转的方式：

  + 转发
  + 重定向

+ 转发和重定向的区别是什么？

  + 代码上有区别

    + 转发

    + ```java
      request.getRequestDispatcher("url").forward(req,resp);
      
      ```

    + 重定向

    + ```
      response.sendRedirect("url");
      ```

      

+ 形式上有什么区别？

  + 转发 一次请求
    + 在浏览器上发送的请求和跳转之后的请求地址没有发生变化，因为资源的跳转时在服务端完成的
  + 重定向 两次请求
    + 通过浏览器的response再次请求服务端跳转资源，内部是服务器内部的跳转。

+ 什么时候用转发和重定向？

  + 如果上一个servlet当中的request域当中绑定了数据，希望从下一个servlet当中的request域里面的数据取出来，使用转发机制
  + 剩下的均使用重定向(重定向使用较多)

+ 跳转到的下一个资源没有要求呢？必须是一个servlet吗？

  + 不一定，跳转的资源只要是一个合法的资源即可。包括html，jsp

+ 转发会存在浏览器的刷新问题。

  + 转发不需要知道项目名 但是sendRedirect需要知道项目名



+ 有一个问题是是的注意的是 实现如此简答的功能就要进行如此复杂的配置是非常不好的。

  这种配置信息能不能直接写道java类里面呢？

  servlet3.0之后提出了多种基于注解式开发 为什么不是直接标注上不再xml上面配置了

  + 优点：开发效率高

## 4.7 servlet的注解方式开发

### webservlet

```java
@WebServlet
public class UserAnnotationDevelopment extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        
    }
}
```

​	使用注解标注 此类就会变成一个servlet

Annotation源码

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface WebServlet {
    String name() default ""; //servlet-name

    String[] value() default {}; //和urlparttern要存储的数据是一样的 但是如果value只有一个值的时候可以省略value标签 //如果属性名是value的话 连value都可以省略

    String[] urlPatterns() default {}; //servlet-mapping--->urlpattern

    int loadOnStartup() default -1;  //启动阶段是非直接启动servlet 使用的话 置为1

    WebInitParam[] initParams() default {}; //注解的数组

    boolean asyncSupported() default false; 

    String smallIcon() default "";

    String largeIcon() default "";

    String description() default "";

    String displayName() default "";
}
```

request---------
	|----getServletPath()    获取urlpattern的内容
	|------getContextPath()	获取webapp的根目录名

## 模板方法设计模式解决类爆炸

```java
package com.sea;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;

@WebServlet("/emp/*")
public class TemplateForm extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        if("/emp/List".equals(req.getServletPath())){
            doList(req,resp);
        }
        if("/emp/Add".equals(req.getServletPath())){
            doAdd(req,resp);
        }
        if("/emp/Remove".equals(req.getServletPath())){
            doRemove(req,resp);
        }
        if("/emp/Update".equals(req.getServletPath())){
            doUpdate(req,resp);
        }
    }

    private void doUpdate(HttpServletRequest req, HttpServletResponse resp) {
        
    }

    private void doRemove(HttpServletRequest req, HttpServletResponse resp) {

    }

    private void doAdd(HttpServletRequest req, HttpServletResponse resp) {

    }

    private void doList(HttpServletRequest req, HttpServletResponse resp) {

    }

}

```

​	如果只是这样的话，可能会有用户恶意登录，就是说可能有人知道下一步的地址，从而不需要登录，直接访问内部信息。甚至可以做到直接通过url发送请求修改数据库的内容。

# 5 关于B/S架构的session机制 会话机制

## 5.1 什么是会话？

+ 用户打开浏览器。经行一系列的操作，然后最终i将浏览器关闭，这个过程叫一次会话

+ 是什么一次请求？
  + 用户在浏览器上一次点击，就会触发请求
+ 请求对应的服务端的对象是request 会话也有一个对象 叫做session
+ session是属于B/S架构的一部分
  + 如果使用php开发web项目，同样使用session这一机制
    + 他是一个规范，不同的语言对这样的会话方式都有实现
    + 它最重要的作用是保留登录状态
    + 这是一种登录成功的状态

## 5.2 Http是一种无状态协议

- 就是请求的时候B/S是连接的，但是请求结束之后连接就断开了。是为了减小服务器的压力

+ 为什么需要session来保存会话状态呢？

  + ```java
      HttpSession session = req.getSession();
            resp.setContentType("text/html");
            PrintWriter out = resp.getWriter();
            out.print(session);
      ```
    ```
  
    ```

证明session的特性:

+ 为什么不使用request和servletContext对象来保存会话状态？

  + request.setAttribute()和request.getAttribute()，servletContext也有这样的方法，request是请求域。servletContext是应用域
  + request是一次请求的对象
  + ServletContext对象是服务器启动的时候创建的，服务器关闭的时候销毁，这个servletContext对象只有一个。
  + servletContext域太大
  + request请求域，servletContext应用域，session会话域
  + request<session<application
  + session的意义是实现用户信息的独立

  

	# 5.3 Session的应用和原理	

+ 思考一下session的实现原理？

  + ```java
    HttpSession session = requset.getSession(); 
    session.setAttribute();  //使用session绑定资源
    session.getAttribute();	//使用session获取资源
    session.removeAttribute(); //使用session解绑资源
    ```

    

  + 这行代码很神奇。张三访问的时候获取的session对象是张三的，李四获取的session是李四的

  + 每个session会有一个记号 其实就是一个cookie

### 5.3.1 session的实现

+ 在web服务器中有一个session列表，类似于map集合，map集合中的key存储的的是session的is
+ 这个map的value是一个session的对象
+ 用户第一次发送请求的时候，服务器会创建一个session对象，并生成一个id，然后web服务器会将session的id发送给浏览器，浏览器将session的id保存在浏览器的缓存中。
+ 用户发送第二次请求的时候会自动将浏览器中的sessionid发送给服务器，服务器会根据sessionid找到对应的session对象。
+ 为什么关闭浏览器，会话结束？
  + 关闭浏览器之后，浏览器中的session消失，下次打开浏览器的时候浏览器的缓存中没有这个sessionid，自然就找不到对应的session对接。
    + 又有一个问题 此时的session在服务器中还存在吗？
      + 有可能session还存在
      + 网银中的安全退出可以告知服务器会话结束，session将被服务器销毁。
      + ![HiZ3Q.png](https://s1.328888.xyz/2022/05/10/HiZ3Q.png)
  + 如果cookie禁用了会发生什么呢？
    + cookie禁用是服务器继续给浏览器发送Sessionid但是浏览器不会收取该sessionid
    + 会导致每一次request都是产生新的session
    + 在url后面加上一个;JSESSIONID=sessionid 依然可以拿到session对象 但是url重写会提高开发的成本

### 5.3.2 session的销毁

+ session什么时候销毁？
  + 一种销毁是超时销毁
  + 一种销毁是手动销毁 session.invalid();
    

![HiBzM.png](https://s1.328888.xyz/2022/05/10/HiBzM.png)

```html
<session-config>
        <session-timeout>30</session-timeout> 可以涉资session的时效
    </session-config>
```

```
package com.sea;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

import java.io.IOException;

@WebServlet("/emp/*")
public class TemplateForm extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession();
        if(session != null && session.getAttribute("userinfo") != null) {
            if ("/emp/List".equals(req.getServletPath())) {
                doList(req, resp);
            }
            if ("/emp/Add".equals(req.getServletPath())) {
                doAdd(req, resp);
            }
            if ("/emp/Remove".equals(req.getServletPath())) {
                doRemove(req, resp);
            }
            if ("/emp/Update".equals(req.getServletPath())) {
                doUpdate(req, resp);
            }
        }else{
            resp.sendRedirect(req.getContextPath());//访问根目录即可 自动回到登录界面
        }
    }

    private void doUpdate(HttpServletRequest req, HttpServletResponse resp) {

    }

    private void doRemove(HttpServletRequest req, HttpServletResponse resp) {

    }

    private void doAdd(HttpServletRequest req, HttpServletResponse resp) {

    }

    private void doList(HttpServletRequest req, HttpServletResponse resp) {

    }

}

```





