## JDK1.8新特性

#### 1.Lambda表达式

> Lambda表达式的基础语法：Java8引入了一个新的操作符**“->”**，该操作符成为箭头操作符或者Lambda操作符，箭头操作符将Lambda表达式拆分成两部分 :
>
> 左侧：Lambda表达式的参数列表
>
> 右侧：Lambda表达式中所需要执行的语句
>
> Lambda表达式的本质只是一个"[**语法糖**](http://zh.wikipedia.org/wiki/%E8%AF%AD%E6%B3%95%E7%B3%96)",由编译器推断并帮你转换包装为常规的代码,因此你可以使用更少的代码来实现同样的功能 



**1.1语法格式:**

**1). 无参数,无返回值**

```java
Runnable r = () ->System.out.println("hello");
r.run();
```

**2). 有一个参数,无返回值**

```java
(x) -> System.out.println(x);
```

***注:只有一个参数时,小括号可以不写***

```java
x -> System.out.print(x);
```



**3).两个以上的参数,有返回值,执行语句多条**

```java
 Comparator<Integer> c1 = (x, y) -> {
            System.out.print(Integer.compare(x, y)+"函数式接口");
            return Integer.compare(x, y);
        }  ;
nnnnnn
```

**4).Lambda体中只有一条语句,return和大括号都可以省略**

```java
Compartor<Integer> c1 = (x,y) ->Integer.compare(x,y);
```

**5.)Lambda表达式的参数列表的数据类型可以省略不写**

```java
(Integer x,Integer y)->Integer.compare(x,y);
```



------



#### 2.函数式接口

**1).Lambda表达式的使用**

排序:

```java
new Employee("张三", "上海", 5000, 22),
            new Employee("李四", "北京", 4000, 23),
            new Employee("c五", "日本", 6000, 50),
            new Employee("b七", "香港", 7000, 50),
            new Employee("赵六", "纽约", 1000, 8)
    );

    /**
     * 调用COllections.sort方法，通过定制排序比较两个Employee（先按年龄比较，年龄相同按姓名比），使用
     * Lambda作为参数传递。
     */
    @Test
    public void test1(){
        Collections.sort(list,(x,y)->{
            if(x.getAge()!=y.getAge())
                return Integer.compare(x.getAge(),y.getAge());
            else
                return x.getName().compareTo(y.getName());

        });

        for (Employee employee : list) {
            System.out.println(employee);
        }
    }
```



**2).声明函数式接口**

一个参数:

```java

@FunctionalInterface
public interface MyFunction {
	String getValue(String str);
}

private String getValue(String str, MyFunction function) {
		return function.getValue(str);
	}
	@Test
	public void testFunction() {
		// 声明函数式接口
		String value = getValue("hello world", x -> x.toUpperCase());
		System.out.println(value);
	}
```

两个参数

```java
public interface MyFunction2<T, R> {
	R apply(T t1,T t2);
}
// 带两个参数的函数式接口
	public <T, R> R apply(T t1, T t2, MyFunction2<T, R> myFunction2) {
		return myFunction2.apply(t1, t2);
	}

	@Test
	public void testFunction2() {
		Integer apply = apply(1, 2, (x, y) -> x + y);
		System.out.println(apply);
	}
```

三个参数

```java
@FunctionalInterface
public interface MyFunction3<T,V,R> {
	R apply(T t, V v);
}
// 三个参数的函数式接口
	public <T, V, R> R apply3(T t, V v, MyFunction3<T, V, R> function3) {
		return function3.apply(t, v);
	}

	@Test
	public void testFunction3() {
		String apply = apply3(1, "this is a string", (x, y) -> y.substring(x));
		System.out.println(apply);
	}

```



**2).四大函数式接口**

```java
Consumer<T> :消费型接口
    void accept(T t);

Supplier<T> :供给型接口
    T get();

Function<T,R> :函数型接口
    R apply(T t);

Predicate<T> :断言型接口
    boolean test(T t);
```





------



#### 3.表达式中的引用

**1).方法的引用**

若Lambda体中的内容有方法已经实现了，我们可以使用“方法引用”（可以理解为方法引用是Lambda表达式的另外一种表现形式）

主要语法：

对象：：实例方法名

类：：静态方法名

类：：实例方法名

注意： 
①Lambda体中调用方法的参数列表与返回值类型，要与函数式接口中抽象方法的函数列表和返回值类型一致。 

②若Lambda参数列表中的第一参数是实例方法的调用者，而第二个参数是实例方法的参数时，可以使用ClassName：：method进行调用。

**2).构造器的引用**

格式：

**ClassName：：new**

注意：需要引用的构造器的参数列表要与函数式接口中抽象方法的参数列表保持一致。

**3).数组引用**

格式：

**Type[]：：new**





------



#### 4.Stream流

> Stream是对集合的包装,通常和lambda一起使用。 使用lambdas可以支持许多操作,如 map, filter, limit, sorted, count, min, max, sum, collect 等等。 同样,Stream使用**懒运算**,他们并不会真正地读取所有数据,遇到像getFirst() 这样的方法就会结束链式语法。 

注意：  

①Stream自己不会存储元素。  

②Stream不会改变源对象。相反，会返回持有新结果的新Stream。  

③Stream操作是延迟执行的。这意味着他们会等到需要结果的时候才执行 



##### 4.1 forEach

> 遍历

```java
list.forEach(System.out::println);
//等同于
list.stream().forEach(System.out::println);
```



