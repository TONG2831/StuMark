## Spring--Xml 配置数据库

1. 

```
 <!--配置数据库连接池 dbcp,c3p0,durid-->
    <!-- <bean id="myDataSource1" class="org.apache.commons.dbcp.BasicDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/springtest"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </bean> -->
```

2. 

   ```
    <!--使用 p-namespace-->
      <!--  <bean id="myDataSource2" class="org.apache.commons.dbcp.BasicDataSource"
             p:driverClassName="com.mysql.jdbc.Driver"
             p:url="jdbc:mysql://localhost:3306/java99"
             p:username="root"
             p:password=""/> -->
   ```

   

3. 使用properties文件

   ```
   <bean id="myDataSource3" class="org.apache.commons.dbcp.BasicDataSource"
             p:driverClassName="${jdbc.driver}"
             p:url="${jdbc.url}"
             p:username="${jdbc.username}"
             p:password="${jdbc.password}"/>
   ```

   