## Spring Transaction(事务)

### 一.事务介绍:



![](图片\事务管理器.png)

![](图片\事务介绍.dib)

------



> Spring将事务分为两类:
>
> 一,编程式事务(手动编写代码实现事务管理);
>
> 二,声明式事务.
>
> 主要介绍声明式事务.

------



### 二.3种声明式事务

------



#### 1. 基于TransactionProxyFactoryBean的方式（很少使用） 

> 需要为每个事务管理的类配置一个TransactionProxyFactoryBean进行管理。使用时还需要在类中注入该代理类。 

#### 2.基于AspectJ的事务声明

> 基于AOP的事务声明,配置好之后,按照方法的名称进行管理,无需在代码中添加任何东西

```
<!-- 声明一个事务通知,配置事物的属性,例如传播行为,隔离级别,只读,事务超时,回滚规则 -->
 	<!-- 指定事务由谁来管理,由txManager -->
 	<tx:advice id="txAdvice" transaction-manager="txManager">
 		<!-- 配置事务属性 -->
 		<tx:attribute>
 			<!-- 指定所有get开头的方法,只读事务 -->
 			<tx:method name="get*" read-only="true"/>
 			<!-- 其余方法在读写事务中 -->
 			<tx:method name="*"/>
 		</tx:attribute>
 	</tx:advice>
 	
 	<!-- 由AOP实现指定事务的执行方法 -->
 	<aop:config>
 		<!-- 定义一个切面 -->
 		<aop:pointcut id="mypointcut" expression="execution(* com.jdbc.template.*(..))"/>
 		<!-- 使用aop的通知器,将切面AspectJ和txadvice绑定在一起,在切面中实现事务的逻辑 -->
 		<aop:advisor advice-ref="txAdvice" pointcut-ref="mypointcut"/>
 	</aop:config>
	
	<!-- 管理事物的类 myDataSource3为数据源-->
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="myDataSource3"></property>
	</bean>
```



#### 3.基于注解的事务声明

```
<!-- 开启事务 -->
<tx:annotation-driven transaction-manager="txManager"  proxy-target-class="true" />
	
	<!-- 管理事物的类 -->
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="myDataSource3"></property>
	</bean>
```

**注意：proxy-target-class属性值决定是基于接口的还是基于类的代理被创建。如果proxy-target-class 属性值被设置为true，那么基于类的代理将起作用（这时需要cglib库）。如果proxy-target-class属值被设置为false或者这个属性被省略，那么标准的JDK 基于接口的代理理** 

**问题:**:实际上是在类中添加的@Transactional注解,但是在开启事务时,属性proxy-target-class属性被省略,这个时候,通过context.getBean("").来获取bean,对象能获取到,但是强转报错

`studentJDBCTempalte = (StudentJDBCTempalte) context.getBean("studentJDBCTempalte");` 

**解决:** 设置proxy-target-class属性基于类的代理

1. @Transactional 注解应该只被应用到 public 可见度的方法上。 如果你在 protected、private 或者 package-visible 的方法上使用 @Transactional 注解，它也不会报错， 但是这个被注解的方法将不会展示已配置的事务设置。 
2. @Transactional 注解可以被应用于接口定义和接口方法、类定义和类的 public 方法上。然而，请注意仅仅 @Transactional 注解的出现不足于开启事务行为，它仅仅 是一种元数据，能够被可以识别 @Transactional 注解和上述的配置适当的具有事务行为的beans所使用。上面的例子中，其实正是 <tx:annotation-driven/>元素的出现 开启 了事务行为。 
3. Spring团队的建议是你在具体的类（或类的方法）上使用 @Transactional 注解，而不要使用在类所要实现的任何接口上。你当然可以在接口上使用 @Transactional 注解，但是这将只能当你设置了基于接口的代理时它才生效。因为注解是 不能继承 的，这就意味着如果你正在使用基于类的代理时，那么事务的设置将不能被基于类的代理所识别，而且对象也将不会被事务代理所包装（将被确认为严重的）。因 此，请接受Spring团队的建议并且在具体的类上使用 @Transactional 注解 