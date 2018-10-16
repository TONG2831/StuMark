## Spring AOP

#### 一.基于xml的AOP

> 废话不多说,直接上代码

```xml
<!-- 注册切面 -->
	<bean id="studentLogAspect" class="com.spring.aop.StudentLogAspect" />
	<aop:config>
		指定切面
		<aop:aspect id="logAop" ref="studentLogAspect">
			指定切入点
			<aop:pointcut id="logPointCut" expression="execution(* com.spring.action.StudentAction.*(..))" />
			<aop:before method="myBeforeAdvice" pointcut-ref="logPointCut"/>
			<aop:after method="myAfterReturningAdvice" pointcut-ref="logPointCut"/>
			<aop:after-throwing method="myAfterThrowingAdvice" pointcut-ref="logPointCut" throwing="exception"/>
		</aop:aspect>
	</aop:config> 
```

**注:**

`pointcut` 和`before` 都是放在`aspect` 中

#### 二.使用注解配置的AOP

> 使用注解的AOP,定义的(切入点)`pointcut` ,和`advice` (通知),都在是切面中

1. ##### **切入点的定义**

   `pointcut` 使用空的方法定义, 方法的名称即为切入点名称;

   该方法需要满足两点要求:

   - 方法体为空
   - 返回值为void

   ```java
   @Pointcut("execution(* com.spring.action.StudentAction.*(..))")
   private void myPointcut(){}
   ```

2. **定义通知，以前置通知为例**

   *方法名+括号*

   ```java
   /**
   	 * 前置通知
   	 */
   	@Before("myPointcut()")
   	public void myBeforeAdvice(JoinPoint joinpoint) {
   		String classAndMethodName = joinpoint.getTarget().getClass().getName() + "类的:"
   				+ joinpoint.getSignature().getName() + "方法";
   		logger.info("前置通知"+classAndMethodName);
   	}
   ```

   

3. **在IOC容器中注册切面**

   *两种方式：*

   **方式一：在xml文件中注册bean**

   ```xml
   <bean id="studentLogAspect" class="com.spring.aop.StudentLogAspect" />
   ```

   **方式二：加注解`@Component`**

   ``` java
   @Aspect
   @Component
   public class LoggAspect {
       
   }
   ```

4. **需要在xml配置，开启代理**

   ```xml
   <context:component-scan base-package="xxx.xxx"/>
   <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
   ```

   

   **注意：**使用注解的配置方式，**切入点**可以直接在**通知的注解中**添加

   例：

   ```java
   @Before("execution(* com.spring.action.StudentAction.*(..))")
   public void myBeforeAdvice(JoinPoint joinpoint) {
   	String classAndMethodName = joinpoint.getTarget().getClass().getName() + "类的:"
   			+ joinpoint.getSignature().getName() + "方法";
   	logger.info("前置通知"+classAndMethodName);
   }
   ```



**环绕通知**：

```java
	/**
	* ProceedingJoinPoint	该参数可以决定是否执行目标方法
	* 可以存在返回值，返回值是目标方法的返回值，但不是必须的
	*/
	@Around("mypointcut()")
	public void doAround(ProceedingJoinPoint pJoinPoint){
		System.out.println("环绕通知：=======开始");
		try {
			pJoinPoint.proceed();
		} catch (Throwable e) {
			e.printStackTrace();
		}
		System.out.println("环绕通知：========结束");
	}
```

环绕通知的几点用处：

1. 可以获取参数，校验参数
2. 可以计算方法执行的时间，由于前置通知和后置通知无法实现参数共享，所以只有通过环绕通知可以实现

#### 切面的优先级

当一个方法有不止一个的前置通知时，先执行哪个就成了问题，这个时候需要指定优先级

使用`@Order（2）` 声明切面的优先级，值越小优先级越高