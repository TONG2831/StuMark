## Spring IOC

> Spring中Beans属性的配置
>
> 在使用这些配置时,需要在xml中添加命名空间
>
>  xmlns:p="http://www.springframework.org/schema/p"
>  xmlns:c="http://www.springframework.org/schema/c"

```xml


<!-- 构造方法 1 -->
<bean id="stu5" class="com.wanho.java99.po.Stu" c:_0="8" c:_1="WH-9908" c:_2-ref="name"/>

 <!-- 构造方法 2 索引-->
 <bean id="s1" class="com.wanho.java99.po.Stu">
       <constructor-arg index="0" value="2"/>
       <constructor-arg index="1" value="WH-9902"/>
 </bean>
       
  <!-- 构造方法 3 类型-->
  <bean id="s2" class="com.wanho.java99.po.Stu">
      <constructor-arg type="java.lang.Integer" value="3"/>
      <constructor-arg type="java.lang.String" value="WH-9903"/>
  </bean>


<!-- 属性,使用p标签 1 -->
<bean id="stu4" class="com.wanho.java99.po.Stu" p:sid="7" p:code="WH-9907" p:name-ref="name"/>
   
   <!-- 属性 2 -->
   <bean id="name" class="com.wanho.java99.po.Name">
        <property name="firstName" value="zhang"/>
        <property name="lastName" value="san"/>
    </bean>
    <!-- 属性 3 -->
    <bean id="stu1" class="com.wanho.java99.po.Stu">
        <property name="sid" value="4"/>
        <property name="code" value="WH-9904"/>
        <property name="name" ref="name"/>
    </bean>

    <bean id="stu2" class="com.wanho.java99.po.Stu">
        <constructor-arg index="0" value="5"/>
        <constructor-arg index="1" value="WH-9905"/>
        <constructor-arg index="2" ref="name"/>
    </bean>

    <bean id="stu3" class="com.wanho.java99.po.Stu">
        <property name="sid" value="6"/>
        <property name="code" value="WH-9906"/>
        <property name="name">
            <bean class="com.wanho.java99.po.Name">
                <property name="firstName" value="li"/>
                <property name="lastName" value="4"/>
            </bean>
        </property>
    </bean>
```

