### Spring-事件处理

> 你已经看到了在所有章节中 Spring 的核心是 **ApplicationContext**，它负责管理 beans 的完整生命周期。当加载 beans 时，ApplicationContext 发布某些类型的事件。例如，当上下文启动时，ContextStartedEvent 发布，当上下文停止时，ContextStoppedEvent 发布。
>
> 通过 ApplicationEvent 类和 ApplicationListener 接口来提供在 ApplicationContext 中处理事件。如果一个 bean 实现 ApplicationListener，那么每次 ApplicationEvent 被发布到 ApplicationContext 上，那个 bean 会被通知。



1. Spring中的事件处理分为：**标准事件**、**自定义事件**

- 标准事件：`ContextStartedEvent`  `ContextStoppedEvent ` `ContextRefreshedEvent`  `ContextClosedEvent `

`RequestHandledEvent`

- 自定义事件：

  1. 定义事件，继承`ApplicationEvent`

     ```java
     public class MyEvent extends ApplicationEvent {
     
     	/**
     	 * @param source
     	 */
     	public MyEvent(Object source) {
     		super(source);
     	}
     	
     	/* (non-Javadoc)
     	 * @see java.util.EventObject#toString()
     	 */
     	@Override
     	public String toString() {
     		return "This is my event！！！";
     	}
     
     }
     ```

  2. 实现`ApplicationEventPublisherAware`

     ```java
     public class MyEventPublisher implements ApplicationEventPublisherAware {
     	
     	private ApplicationEventPublisher publisher;
     	
     	/* (non-Javadoc)
     	 * @see org.springframework.context.ApplicationEventPublisherAware#setApplicationEventPublisher(org.springframework.context.ApplicationEventPublisher)
     	 */
     	@Override
     	public void setApplicationEventPublisher(ApplicationEventPublisher publisher) {
     		this.publisher = publisher;
     	}
     	
     	public void publish(){
     		MyEvent mEvent = new MyEvent(this);
     		publisher.publishEvent(mEvent);
     	}
     	
     }
     ```

  3. 实现`ApplicationListener<MyEvent> `

     ```java
     public class MyEventHandler implements ApplicationListener<MyEvent> {
     
     	/* (non-Javadoc)
     	 * @see org.springframework.context.ApplicationListener#onApplicationEvent(org.springframework.context.ApplicationEvent)
     	 */
     	@Override
     	public void onApplicationEvent(MyEvent myEvent) {
     		System.out.println(myEvent.toString());
     	}
     
     }
     ```

  4. 在xml文件中注册

     ```xml
      <bean id="myEventPublisher" class="com.tong.event.MyEventPublisher"></bean>
      <bean id="myEventHandler" class="com.tong.event.MyEventHandler"></bean>
     ```

  5. 测试

     ```java
     ConfigurableApplicationContext context = new ClassPathXmlApplicationContext("tongbean.xml");
     
     //自定义事件处理
     MyEventPublisher myEventPublisher = (MyEventPublisher) context.getBean("myEventPublisher");
     myEventPublisher.publish()；
     ```

     

