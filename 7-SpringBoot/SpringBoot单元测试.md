# SpringBoot（41） Testing（测试）

> Spring Boot在测试应用程序时提供了许多实用程序和注释。测试支持由两个模块提供;spring-boot-test包含核心项，spring-boot-test- autoconfiguration支持测试的自动配置。
>
> 大多数开发人员只会使用Spring - Boot - Starter -test“Starter”，它导入了两个Spring引导测试模块以及JUnit、AssertJ、Hamcrest和许多其他有用的库。

## 41.1.Test scope dependencies

> jar包的版本在指定父类依赖版本的时候就已经确定,指定的spring.boot .stater版本是1.5.17,添加test依赖之后,junit版本为`4.1.2`
>
> 添加spring-boot-starter-test,会提供以下库:
>
> - [JUnit](http://junit.org/) — The de-facto standard for unit testing Java applications.
> - [Spring Test](https://docs.spring.io/spring/docs/4.3.20.RELEASE/spring-framework-reference/htmlsingle/#integration-testing) & Spring Boot Test — Utilities and integration test support for Spring Boot applications.
> - [AssertJ](https://joel-costigliola.github.io/assertj/) — A fluent assertion library.
> - [Hamcrest](http://hamcrest.org/JavaHamcrest/) — A library of matcher objects (also known as constraints or predicates).
> - [Mockito](http://mockito.org/) — A Java mocking framework.
> - [JSONassert](https://github.com/skyscreamer/JSONassert) — An assertion library for JSON.
> - [JsonPath](https://github.com/jayway/JsonPath) — XPath for JSON.
>
>  

```xml
<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
    		<scope>test</scope>
		</dependency>
```

## 41.2 Test Spring Application

> 依赖注入的一个主要优点是，它应该使您的代码更容易进行单元测试。您可以简单地使用新操作符实例化对象，甚至不需要涉及Spring。您还可以使用模拟对象而不是真正的依赖项。 
>
> 通常，您需要超越“单元测试”，开始“集成测试”(流程中实际涉及Spring ApplicationContext)。能够在不需要部署应用程序或不需要连接到其他基础设施的情况下执行集成测试是非常有用的。 
>
> Spring框架包含一个专门的测试模块来进行集成测试。您可以直接向org声明依赖关系。springframework:spring-test或使用spring-boot-starter-test“Starter”将其临时拉入。 
>
> 如果您之前没有使用过Spring -test模块，那么您应该首先阅读Spring Framework参考文档的相关部分。 

代码中主要的配置

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = Application.class)
public class ConfigurationPorpertiesTest {

	@Autowired
	Person person;

	@Test
	public void test() {
		System.out.println(person.toString());
	}
}
```

> @SpringBootTest注解,指定SpringBoot项目的`main application`启动类

