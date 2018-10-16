# Spring MVC

> Spring为展现层提供的基于MVC的设计理念的优秀的Web框架.



#### 1.DispatcherServlet

##### 1.1在web.xml中的配置

```xml
<servlet>
	<servlet-name>DispatcherServlet</servlet-name>
	<servlet-class>
    	org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:springmvc.xml</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
	<servlet-name>DispatcherServlet</servlet-name>
	<url-pattern>/</url-pattern>
</servlet-mapping>
```

1).init-param: 配置DispatcherServlet的初始化参数,配置springMVC配置文件的位置和名称；

该参数名称必须是:"contextConfigLocation",否则在项目启动的时候报错 

实际上,可以使用默认的配置文件 ：

默认，位置：/WEB—INF/  名称:<servlet-name>-servlet.xml 

即:/WEB-INF/<servlet-name>-servlet.xml -->

2). load-on-startup: Servlet的加载顺序:默认或小于0,第一请求时初始化;大于或等于0时,服务器启动时初始化,正值越小,优先级越高 。

3).映射请求路径,当"/"时,映射所有的请求,不可以为空

#### 2.@RequestMapping

该注解有三个参数

- value ,该值即映射的路径
- method,指定处理的请求的方法,常用:GET/POST,所有:GET/POST/DELETE/PUT
- params,指定请求中的参数,指定参数是否存在,指定参数的值

@RequestMapping 可以修饰方法,也可以修饰类;
 * 1).在类上,提供初步的请求映射信息.相对于WEB项目的根路径
 * 2).在方法上,提供进一步的细分映射信息.相对于类定义出标记的的路径
  * 如果类定义处未标注@RequestMapping,则在方法处标记的URL,相对于WEB应用的根路径.

#### 3.@RequestParam

三个参数

- value,指定参数名称
- required,默认true,表明指定的参数是必要的,如果不存在报错,**注意基本数据类型**
- defaultValue,给参数一个默认值,如果该参数是必要的,但是没有给它赋值,启用默认值,

该注解用来修改方法中的参数

public void testRequest(@RequestParam(value="age",required=false,defaultValue=1)Integer age){}

#### 4.@RequestHeader

> 参数同`3.@RequestParam`

> 主要用来获取请求头中的参数,例如:@RequestHeader("Accept-Language")String val

#### 5.@CookieValue

> 用来获取JSESSIONID的值,例:@CookieValue("JSESSIONID") String session

#### 6.@PathVariable

> 主要用来处理带占位符的URL

```java
/springtest/test1/{id}

@PathVariable("id")Integer id

```

#### 7.POJO

> 使用POJO对象绑定请求参数值,支持级联属性,但是属性的名称严格一致

```java
@RequestMapping("/test")
public String test(Student student) {
	System.out.println(student);	
	return "success";
}
```

#### 8.Servlet原生API

> 可以使用Servlet原生API作为参数

```java
HttpServletRequest
HttpServletResponse
HttpSession
​```
@RequestMapping("/test")
public String test(Student student,HttpServletRequest request) {
	System.out.println(student);	
	return "success";
}
```

#### 9.ModelAndView

> 处理方法返回值类型为 ModelAndView 时, 方法体即可通过该对象添加模型数据

```java
@RequestMapping("/test")
	public ModelAndView test() {
		ModelAndView model = new ModelAndView();
		model.setViewName("success");
		model.addObject("time", new Date());
		
		return model;
	}
```

#### 10.Map

> 在方法参数中放入map参数,SpringMVC会把map放到请求域中

```java
@RequestMapping("/test")
public String test(Map<String, Object> map) {
	map.put("name", Arrays.asList(new String[]{"zhansan","lisi","王五"}) );
	return "success";
}
```



#### 11.@SessionAttributes

> 将模型中的某个属性暂存到HTTPSession中,以便多个请求之间可以共享这个属性

只能在`Controller`类上标注该注解

该注解参数:

- value,通过对象名,指定放到session中的对象
- types,通过类,指定放到session中的对象

```java
@SessionAttributes(value = { "stu" }, types = { String.class })
public class SSessionAttributes {
	@RequestMapping("/test")
	public String test(Map<String, Object> map) {
		Student student = new Student(10001L, "和离子");
		Student student1 = new Student(10002L, "马到理想");
		map.put("stu", student);
		map.put("s", student1);

		map.put("str", "张三");
		return "success";
	}
```

#### 12.@ModelAttribute







