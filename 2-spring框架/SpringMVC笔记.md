## SpringMVC

#### 一.URL模板

```
@RequestMapping(path="/owners/{ownerId}",	method=RequestMethod.GET) 
public	String	findOwner(@PathVariable	String	ownerId,	Model	model)	{					Owner	owner	=	ownerService.findOwner(ownerId);									model.addAttribute("owner",	owner);
	return	"displayOwner"; 
}

```

> URI模板"	/owners/{ownerId}	"指定了一个变量，名为	ownerId	。当控制器处理这个请求的时 候，	ownerId	的值就会被URI模板中对应部分的值所填充。比如说，如果请求的URI 是	/owners/fred	，此时变量	ownerId	的值就是	fred

> 为了处理`@PthVariable` 注解，SpringMVC必须通过变量名找到URL中的变量。
>
> `@PathVariable（“ownerId”） String Id` 
>
> 注：如果变量名相同，不必再指定一次

#### 二.Web.xml-<context-param>

```xml
<servlet>
  	<servlet-name>helloweb</servlet-name>
  	<servlet-class>
  		org.springframework.web.servlet.DispatcherServlet
  	</servlet-class>
    <!-- 这里的配置不可少-->
  	<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:applicationContext.xml</param-value>
	</init-param>
	<load-on-startup>2</load-on-startup>
  </servlet>

<context-param>
	<param-name>contextConfigLocation</param-name>
	<param-value>classpath:applicationContext.xml</param-value>
</context-param>

```

Spring配置文件的配置，在`context-param` 和`init-param` 中都要配置

1. context-param和init-param的区别

   context-param是放在web-app标签下，属于上下文参数，整个的环境中都可以使用，存放在getServletContext对像中，因此使用方法是：getServletContext().getInitParameter(“user”) ；

   init-param是在servlet标签下，属于某一个servlet，存放在getServletConfig中，因此使用方法是：getServletConfig().getInitParameter(“user1”); 由于它属于当前的servlet类，所以用this替代getServletConfig(), 使用this.getInitParmeter(“user1”) 。

   init-param是在servlet标签下，两者的区别在于，context-param可以在任何的servlet中使用，而init-param只能在当前的sevlet中使用。

2. classpath 路径的介绍

   classpath，指WEB-INF文件夹下的classes目录。

   classpath*，不仅指classes目录还包括jar文件的lib目录。

   如果是将spring配置文件放在了src目录下，则使用`classpath:applicationContext.xml` 等同于`WEB-INF/classes/cpplicationContext.xml` ；

   如果配置文件放在WEB-INF下面，使用`WEB-INF/applicationContext.xml` 

**总结**：从上面的介绍，我们不难发现。spring框架在加载web配置文件的时候。首先加载的是context-param配置的内容，而并不会去初始化servlet。只有进行了网站的跳转，经过了DispatcherServlet的导航的时候，才会初始化servlet，从而加载init-param中的内容。 