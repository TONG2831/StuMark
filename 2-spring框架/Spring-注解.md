### Spring--注解



1. **spring的注解**：@Autowried 、@Qualifier、@Resource

   @Autowried和@Resource两者区别：

   - @Autowried默认**类型匹配**（byType），如果想使用通过名称（byName）匹配，需要使用**@Qualifier("name")指定名称**，***注：@Qualifier注解不能单独使用***
   - @Resource默认通过name匹配Bean，找不到，通过type匹配，都找不到，报错。该注解可以指定name和type去匹配bean
   - **@Autowried注解为Spring的注解，@Resource为JavaEE的注解，*在实际使用过程中，尽量使用@Resource注解，以减少代码和Spring之间的耦合***

2. **使用注解时，以下两者配置的区别**

   [CSDN解析两者的区别]: https://www.cnblogs.com/leiOOlei/p/3713989.html

   

   ```xml
   1. <context:annotation-config/>
   2. <context:component-scan base-package="com.xxx"/>
   ```

   - 第一种配置，**用于激活已经在xml文件中配置过的Bean**，如果是xml文件中没有配置任何bean，即使代码中使用了注解，也不会起作用。
   - 第二种配置，除了拥有第一种配置相同的作用，**还具有自动将带有@component,@service,@Repository等注解的对象注册到spring容器中的功能 **，*在使用时，注意扫描的包是否完整。*

**注：如果两中配置同时出现，不会对程序产生影响，因为@Autowried和@Resource只会注入一次，不会出现重复注入的情况**

