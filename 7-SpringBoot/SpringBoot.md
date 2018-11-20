# Spring Boot

> 本文对应Spring Boot 官方文档 1.5.17版本



---

##  15.Spring Boot Configuration Classes(配置类)

> Spring Boot 倾向于基于java的配置。尽管可以使用XML源调用SpringApplication.run()，但我们通常建议您的主要源是@Configuration类。通常，定义main方法的类也可以作为主要的@Configuration类。 

### 15.1导入额外的配置classes

> 您不需要将所有的@Configuration放入单个类中。可以使用@Import注释导入其他配置类。或者，您可以使用@ComponentScan自动提取所有Spring组件，包括@Configuration类。 

### 15.2导入xml配置文件

> 若一定要使用xml配置,建议任然以一个`@Configuration class`开始.之后,你可以额外使用`@ImportResources`注解加载xml配置文件.

---

## 16.自动配置

> 1.Spring Boot自动配置尝试根据您添加的jar依赖项自动配置Spring应用程序。例如，如果 `HSQLDB`在您的类路径上，并且您尚未手动配置任何数据库连接bean，那么我们将自动配置内存数据库。 
>
> 2.您需要通过向其中一个类添加`@EnableAutoConfiguration`或 `@SpringBootApplication`注释来选择自动配置`@Configuration`。 
>
> > 您应该只添加一个`@SpringBootApplication`或`@EnableAutoConfiguration` 注释。我们通常建议您仅将一个或另一个添加到主 `@Configuration`类中。 



### 16.1逐步更换自动配置

> 自动配置是非侵入性的，您可以随时开始定义自己的配置以替换自动配置的特定部分。例如，如果添加自己的`DataSource`bean，则默认的嵌入式数据库支持将退回。
>
> 如果您需要了解当前正在应用的自动配置以及原因，请使用`--debug`交换机启动应用程序。这将为选择的核心记录器启用调试日志，并将自动配置报告记录到控制台。

### 16.2 禁用特定的自动配置

> 如果您发现正在应用您不需要的特定自动配置类，则可以使用exclude属性`@EnableAutoConfiguration`来禁用它们。

```java
import org.springframework.boot.autoconfigure。*;
import org.springframework.boot.autoconfigure.jdbc。*;
import org.springframework.context.annotation。*;

@Configuration 
@EnableAutoConfiguration（exclude = {DataSourceAutoConfiguration.class}）
 public  class MyConfiguration {
}
```

> 如果类不在类路径上，则可以使用`excludeName`注释的属性并指定完全限定名称。最后，您还可以控制要通过`spring.autoconfigure.exclude`属性排除的自动配置类列表  
>
> 您可以在注释级别和使用属性定义排除项。 

---

## 17.Spring Beans  and 依赖注入

- 您可以自由地使用任何标准的Spring Framework技术来定义bean及其注入的依赖项。为简单起见，我们经常发现使用`@ComponentScan` 找到你的bean，结合`@Autowired`构造函数注入效果很好。 

- 如果按照上面的建议构建代码(14节,在根包中定义main application class),可以直接添加`@ComponentScan`注解,并且注解中可以不添加任何参数。所有的应用程序组件（使用`@component`,`@Service`,`@Reponsitory`,`@Controller`etc,注解的class),都会被自动注册为Spring Beans。

- example：使用构造器注入

  ```java
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Service;
  
  @Service
  public class DatabaseAccountService implements AccountService {
  
      private final RiskAssessor riskAssessor;
  
      @Autowired
      public DatabaseAccountService(RiskAssessor riskAssessor) {
          this.riskAssessor = riskAssessor;
      }
  
      // ...
  
  }
  ```

  

  **如果一个bean只有一个构造器,则`@Autowired`可以省略:**

  ```java
  @Service
  public class DatabaseAccountService implements AccountService {
  
      private final RiskAssessor riskAssessor;
  
      public DatabaseAccountService(RiskAssessor riskAssessor) {
          this.riskAssessor = riskAssessor;
      }
  
      // ...
  
  }
  ```

  > **注意:使用构造函数注入允许将字段riskAssessor修饰为final,表示他随后无法修改**

  ## 18. Using the @SpringBootApplication annotation

  许多Spring Boot开发者喜欢他们的应用程序使用自动配置,组件扫描(component-scan)和在`application class`中定义额外的配置.

  一个单独的`@SpingBootApplication`注解可以实现以上三点

  :

  - `@EnableAutoConfiguration` :启用SringBoot的自动配置机制
  - `@ComponentScan`:在`main application`应用程序所在的包启用`@Component`Scan(扫描)
  - `@Configuration `:允许在上下文中注册额外的Beans或添加额外的配置类.

  > 该`@SpringBootApplication`注解相当于使用默认属性情况下的`@Configuration`， `@EnableAutoConfiguration`和`@ComponentScan`

  @SpringBootApplication还提供别名来定制@EnableAutoConfiguration和@ComponentScan的属性。 

  

  这些特性都不是强制性的，您可以选择用它所支持的任何特性来替换这个注释。例如，您可能不想在应用程序中使用组件扫描: 

  ```java
  @Configuration
  @EnableAutoConfiguration
  @Import({ MyConfig.class, MyAnotherConfig.class })
  public class Application {
  
      public static void main(String[] args) {
              SpringApplication.run(Application.class, args);
      }
  
  }
  ```

  在本例中，应用程序与任何其他Spring引导应用程序一样，只是没有自动检测到`@ component - annoated`类，并且显式地导入了用户定义的bean(请参阅`@Import`)。 

  

  





 

