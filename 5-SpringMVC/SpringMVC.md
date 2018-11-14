#  Spring MVC

> Spring为展现层提供的基于MVC的设计理念的优秀的Web框架.



#### 1.DispatcherServlet

##### 1.2DispatcherServlet的作用

主要用于控制流程，主要的职责如下：

- 文件上传解析，如果请求类型是multipart将通过MultipartResolver进行文件的上传解析;
- 通过HandlerMapping,将请求映射到处理器(返回一个HandlerExecutionChain,它包括一个处理器,多个HandlerInterceptor拦截器);
- 通过HandlerAdapter支持多种类型的的处理器(HandleExecutionChain中的处理器);
- 本地化解析
- 通过视图解析,渲染具体的视图等;
- 如果执行的过程中遇到异常将交给HandlerExceptionResolver来解析.

##### 1.2 DispatcherServlet 的配置

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

> ModelAttribute标注的方法会在SpringMVC执行目标方法之前被调用。

```java
@ModelAttribute
public void doBefore(@RequestParam(value="userId")String userId, Map<String, User> map) {
		User user = new User();
		user.setUserId("13");
		user.setUsername("张三");
		user.setPassword("this is password");
		map.put("user1", user);
		System.out.println("userID"+userId);
	}
	
	@RequestMapping("/test")
	public String test(User user){
		System.out.println("修改user的info user:"+user);
		return "success";
	}
```



运行流程：

- 1.执行@ModelAttribute 注解修饰的方法，将对象放到map中，键为:user
- 2.SpringMVC从Map中取出User对象,并把表单请求的参数赋给改User对象的对应属性'.
- 3.SpringMVC 上述的对象传入目标方法入参.

**注意:**

1. 放入到Map中键的名称需要和目标方法入参类型的第一个首字母小写的字符串一致!

   ex:User --> user

2. 如果在步骤一中放入的键名称特殊,可以在目标方法的参数中加@ModelAttribute("users111") 

   ```java
   @RequestMapping("/test")
   	public String test(@ModelAttribute("user111")User user){
   		System.out.println("修改user的info user:"+user);
   		return "success";
   	}
   ```

   

3. @ModelAttribute在修饰目标方法POJO类型的入参,其value属性有如下的作用:

   - SpringMVC会使用value属性值在implicitModel中查找对应的对象,若存在则会直接传入到目标方法的入参中
   - SpringMVC会议value为key,POJO类型的对象为value,存入到request中.

4. 若同时使用了@SessionAttribute注解,有可能会引发异常,

   ```java
   @Controller
   @RequestMapping("/modelattribute")
   @SessionAttribute("user")
   public class SModelAttributes {
   	@ModelAttribute
   	public void doBefore(@RequestParam(value="userId")String userId, Map<String, User> map) {
   		User user = new User();
   		user.setUserId("13");
   		user.setUsername("张三");
   		user.setPassword("this is password");
   		map.put("user1", user);
   		System.out.println("userID"+userId);
   	}
   	
   	@RequestMapping("/test")
   	public String test(User user){
   		System.out.println("修改user的info user:"+user);
   		return "success";
   	}
   }
   ```

   此时,如果没有@ModelAttribute修饰的方法,程序实际运行时会引发异常

   解决办法:

   1.将目标方法的入参标识,@ModelAttribute("user111")User user

   2.添加@ModelAttribute标识的方法,如以上代码

#### 13.mvc:view-controller

```xml
!--该配置文件可以直接转发界面,无需再经过handler的方法 -->

	<mvc:view-controller path="/success" view-name="success" />

	<!-- 该注解,可以消除mvc:view-controller带来的其他页面无法响应的问题 -->

	<mvc:annotation-driven><mvc:annotation-driven/>

```

#### 14.自定义视图

1. 创建一个普通的Java类,实现View接口,实现其中的方法,使用@Component注解标注
2. 在SpringMVC配置文件中,配置视图解析器
3. 在具体的方法中,返回的视图名称,为该类首字母小写

代码:

视图类:

```java
@Component
public class HelloWorldView implements View{

	@Override
	public String getContentType() {
		// TODO Auto-generated method stub
		return "text/html";
	}

	@Override
	public void render(Map<String, ?> arg0, HttpServletRequest arg1, HttpServletResponse response) throws Exception {
		response.getWriter().print("this is Heloeworld "+ new Date());
	}
}
```

配置文件

```xml
<!--自定义的视图的视图解析器,使用视图的名称解析视图  -->
	<bean class="org.springframework.web.servlet.view.BeanNameViewResolver">
		<property name="order" value="100"></property>
	</bean>
```

实现:

```java

@Controller
@RequestMapping("/views")
public class SViews {
	@RequestMapping("/test")
	public String test() {
		System.out.println("viewname");
		return "helloWorldView";
	}
}
```

#### 15.重定向

> forward为转发，redirect为重定向。在SpingMVC中直接返回视图名称，为请求转发；如果需要重定向，则返回值 "redirect:/index.jsp"

**需要注意的是:重定向的路径相对于项目的根路径**

#### 16.静态资源的加载

> 将DispatcherServlet的请求映射配置为"/",服务器中所有的请求将会被SpringMVC处理,对于静态资源的请求,SpringMVC找不到对应的`handler` ,出错

方法一:

`<mvc:default-servlet-handler/>`

在Tomcat服务器的配置文件web.xml中,有一个DefaultServlet,处理静态资源.

```xml
 <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
```

在springMVC-servlet.xml中配置<mvc:default-servlet-handler />后，会在Spring MVC上下文中定义一个org.springframework.web.servlet.resource.DefaultServletHttpRequestHandler，它会像一个检查员，对进入DispatcherServlet的URL进行筛查，如果发现是静态资源的请求，就将该请求转由Web应用服务器默认的Servlet处理，如果不是静态资源的请求，才由DispatcherServlet继续处理。 

一般Web应用服务器默认的Servlet名称是"default"，因此DefaultServletHttpRequestHandler可以找到它。如果你所有的Web应用服务器的默认Servlet名称不是"default"，则需要通过default-servlet-name属性显示指定：<mvc:default-servlet-handler default-servlet-name="所使用的Web服务器默认使用的Servlet名称" />

方法二:

`<mvc:resources /> `

<mvc:default-servlet-handler />将静态资源的处理经由Spring MVC框架交回Web应用服务器处理。而<mvc:resources />更进一步，由Spring MVC框架自己处理静态资源，并添加一些有用的附加值功能 .

首先，<mvc:resources />允许静态资源放在任何地方，如WEB-INF目录下、类路径下等，你甚至可以将JavaScript等静态文件打到JAR包中。通过location属性指定静态资源的位置，由于location属性是Resources类型，因此可以使用诸如"classpath:"等的资源前缀指定资源位置。传统Web容器的静态资源只能放在Web容器的根路径下，<mvc:resources />完全打破了这个限制。

其次，<mvc:resources />依据当前著名的Page Speed、YSlow等浏览器优化原则对静态资源提供优化。你可以通过cacheSeconds属性指定静态资源在浏览器端的缓存时间，一般可将该时间设置为一年，以充分利用浏览器端的缓存。在输出静态资源时，会根据配置设置好响应报文头的Expires 和 Cache-Control值。

在接收到静态资源的获取请求时，会检查请求头的Last-Modified值，如果静态资源没有发生变化，则直接返回303相应状态码，提示客户端使用浏览器缓存的数据，而非将静态资源的内容输出到客户端，以充分节省带宽，提高程序性能。

在springMVC-servlet中添加如下配置：

```
<mvc:resources location="/,classpath:/META-INF/publicResources/" mapping="/resources/**"/>
```

> 以上摘自https://www.cnblogs.com/dflmg/p/6393416.html

#### 17.<mvc:annotation-driven/>

> 会自动注册
> RequestMappingHandlerMapping
> RequestMappingHandlerAdapter 与
> ExceptionHandlerExceptionResolver 三个bean。

同时,使用该配置文件可以使

- 当前SpringMVC支持使用ConversionService对表单参数进行类型转换
- @NumberFormat annotation、@DateTimeFormat对数据进行格式化
- @Valid 注解对参数校验
- 支持使用@RequestBody 和@ResponseBody

#### 18.返回JSON

通过AJAX返回JSON数据

@ResponseBody 

@RequestBody

#### 19.文件上传

> SpringMVC通过即插即用的MultipartResolver实现的.Spring用Jakarta CommonsFileUpload技术实现了一个MultipartResolver实现类:CommonsMultipartResolver
>
> 默认情况下,没有装配该插件,,需要在上下文中配置MultipartResolver

1.在SpringMVC配置文件

```xml
<bean id="commonsMultipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<property name="defaultEncoding" value="utf-8"></property>
		<property name="maxUploadSize" value="1024000"></property>
	</bean>
```

2.前端页面

```html
<form action="file/testFileUpload" method="post" enctype="multipart/form-data">
		文件: <input type="file" name="file" >
		desc:<input type="text" name="desc">
		    <input type="submit" value="Submit">
	</form>
```

3.后端代码

```java
@RequestMapping("/testFileUpload")
	public String fileUpload(@RequestParam("desc")String desc,@RequestParam("file")MultipartFile file) throws IOException {
		System.out.println("desc:"+desc);
		System.out.println("文件名称:" + file.getOriginalFilename());
		System.out.println("輸入流:" + file.getInputStream());
		return "success";
	}
```

#### 20.自定义拦截器

> SpringMVC可以使用拦截器对请求进行拦截处理,用户可以自定义拦截实现特定的功能,自定义拦截器必须实现HandlerInterceptor借口.

- **preHandle()**：这个方法在业务处理器处理请求之前被调用，在– 该
  方法中对用户请求 request 进行处理。如果程序员决定该拦截器对
  请求进行拦截处理后还要调用其他的拦截器，或者是业务处理器去
  进行处理，则返回true；如果程序员决定不需要再调用其他的组件
  去处理请求，则返回false
- **postHandle()**：这个方法在业务处理器处理完请求后，但 –
  是DispatcherServlet 向客户端返回响应前被调用，在该方法中对
  用户请求request进行处理。
- **afterCompletion()**：这个方法在 DispatcherServlet 完全处理完– 请
  求后被调用，可以在该方法中进行一些资源清理的操作

拦截器的配置:

```xml
<mvc:interceptors>
		<!-- 配置自定义的拦截器 -->
		<bean class="com.atguigu.springmvc.interceptors.FirstInterceptor"></bean>
		
		<!-- 配置拦截器(不)作用的路径 -->
		<mvc:interceptor>
			<mvc:mapping path="/emps"/>
			<bean class="com.atguigu.springmvc.interceptors.SecondInterceptor"></bean>
		</mvc:interceptor>
		
		<!-- 配置 LocaleChanceInterceptor -->
		<bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor"></bean>
	</mvc:interceptors>
```

