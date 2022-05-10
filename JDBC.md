JDBC编程六部

第一步：注册驱动

第二步：获取连接

第三步：获取数据库操作对象

第四步：执行SQL语句

第五步：处理查询结果集

第六步：释放资源



```java
 String sql = "delete from emp where ename=?";
            ps = connection.prepareStatement(sql);
            Scanner scanner = new Scanner(System.in);
            String param = scanner.nextLine();
            ps.setString(1,param);
```

preparestatement使用方法

