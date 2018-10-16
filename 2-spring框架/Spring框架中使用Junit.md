## JUnit 4

#### 一、 @BeforeClass

1. **问题描述**

> 在使用JUnit时，使用了@BeforeClass注解，却发现，无法初始化`context` 
>
> - 当时的方法使用的是`private ` 修饰，且没有加`static`

```java
static ApplicationContext context;
static StudentAction studentAction;
@BeforeClass
public static void doBefore() {
	context = new 	 					  	     ClassPathXmlApplicationContext("applicationContext2.xml");
	studentAction = (StudentAction) 					context.getBean("studentAction");
	System.out.println("------" + studentAction);
	}
```

2. **解决方法：**

- 在使用Junit测试时，如果想使用**@BeforeClass**，则：
- （1）.该方法必须是**static**
- （2）.该方法必须是**public**



二、

