# SpringFramework学习笔记

## Spring的主要作用：

spring作为一个桥梁连接其他框架，是一个整合工具

spring是一个轻量级框架，Java编写，易学，ioc，aop，事务，对安全对效率都有比较全面的考虑

IOC的优点：降低程序的耦合度

DI(Denpendency Injection)

IOC:Inverse of Control

## 一、hello spring

### 1）步骤 

1.导入jar包：

```Java
commons-logging-1.2.jar
spring-aop-4.1.6.RELEASE.jar
spring-aspects-4.1.6.RELEASE.jar
spring-beans-4.1.6.RELEASE.jar
spring-context-4.1.6.RELEASE.jar
spring-context-support-4.1.6.RELEASE.jar
spring-core-4.1.6.RELEASE.jar
spring-expression-4.1.6.RELEASE.jar
spring-jdbc-4.1.6.RELEASE.jar
spring-orm-4.1.6.RELEASE.jar
spring-tx-4.1.6.RELEASE.jar
spring-web-4.1.6.RELEASE.jar
spring-webmvc-4.1.6.RELEASE.jar
```
​			2. 编写spring配置文件(名称可以自定义)

​			3.编写bean类：

```java
package cn.sxt.bean;

public class Hello {
	private String name;
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
    public void show() {
        System.out.println("Hello! "+name);
    }
}
```

​		4.编写测试类：

```Java
package cn.sxt.bean;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Test {
    public static void main(String[] args) {
        ApplicationContext context= new ClassPathXmlApplicationContext("beans.xml");
        Hello hello=(Hello)context.getBean("hello");
        hello.show();
    }
}
```

## 二、IOC

### 1）控制反转：

Hello对象是由spring容器创建的

Hello对象的name属性是由spring容器创建的

这个过程就叫控制反转

### a) 控制的内容

指谁来控制对象的创建；传统的应用程序对象是程序本身控制的。使用spring后，是用spring容器创建的。

### b)反转

正转指程序来创建对象，反转指创建对象的权限由程序本身交到了spring容器，spring容器创建好对象后，程序变为被动接受对象。

### c)总结

以前对象是由程序本身来创建，使用spring后，程序变为被动接受spring创建好的对象。

控制反转也叫依赖注入（dependency injection）。

bean对象依赖的属性由spring容器来注入。

IOC是一种编程思想，由主动的编程变为被动的接收

IOC是通过IOC容器来实现的，IOC容器就是一个bean工厂(BeanFactory)

## 三、配置文件详解

## 四、依赖注入

### 1)Dependency Injection(DI)

依赖：指Bean对象的创建依赖于spring容器，Bean对象依赖的资源

注入：指Bean对象依赖的资源由容器来设置和装配

### 2)spring注入--构造器注入

```xml
<beans>
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
    </bean>

    <bean id="bar" class="x.y.Bar"/>

    <bean id="baz" class="x.y.Baz"/>
</beans>
```

### 3)spring注入--setter注入

要求被注入的属性必须有setter方法。如果属性是布尔类型，没有getter而是is方法。

a)常量注入

b)bean注入

c)数组注入

d)List注入

e)Map注入

f)Set注入

g)Null注入

h)properties注入

i)p-namespace(属性注入)命名空间注入(属性依然要设置setter)

j)c-namespace(构造方法)须在配置文件头加入

xmlns:c="http://www.springframework.org/schema/c"

且要求有对应的构造方法

### 4)Bean的作用域(bean scope)：默认为单例

singleton(单例)：整个容器中只有一个对象实例

prototype(原型)：每次获取bean都新产生一个新的对象

request(请求)：每次请求时创建一个新的对象实例

session(会话)：一个会话范围只有一个对象

global session：只在portlet下有用，表示的是application

application:在应用范围中只有一个对象

### 5)Bean自动装配(autowiring)：简化spring配置

byName根据名称查找相应的bean(setter的名称去掉set)

byType根据类型自动装配，不用管bean的id，但是同一种类型的bean只能有一个，多余一个会报错，慎用！

constructor通过构造器实例化bean时适用byType的方式装配构方法

## 五、AOP——Aspect orient programming(面向切面编程)

### 1）AOP在spring中的作用 

提供声明式事务（声明式事务）

允许用户自定义切面

### 2）简介

**传统的编程模式**

![传统编程模式](../resources/%E4%BC%A0%E7%BB%9F%E7%BC%96%E7%A8%8B%E6%A8%A1%E5%BC%8F.jpg)

**AOP：在不改变原有代码情况下，增加新的功能,横向编程**

![未命名文件-1565020610427](../resources/%E6%9C%AA%E5%91%BD%E5%90%8D%E6%96%87%E4%BB%B6-1565020610427-1572334152761.png)

### 3）优点

使得真实角色处理的业务更加纯粹，不再去关注一些公共的事情。

公共的业务由代理来完成--实现了业务的分工

公共业务扩展时变得更加集中和方便

### 4）名词解释

**关注点**：增加某个业务。如日志、安全、缓存、事务！

**切面**：一个关注点的模块化

**连接点(Aspect)**：在spring AOP中，一个连接点总是表示一个方法的执行。

**通知(Advance)**：在切面的某个特定的连接点上执行的动作。

**织入(Weaving)**：把切面连接到其他的应用程序类型或者对象上，并创建一个被通知的对象。

### 5）使用spring实现AOP

#### a)通过springAPI实现

Log.java--前置通知

```java
package cn.sxt.log;

import java.lang.reflect.Method;

import org.springframework.aop.MethodBeforeAdvice;


public class Log implements MethodBeforeAdvice {
	/**
	 *@param method 被调用的方法对象
	 *@param args 被调用方法的参数
	 *@param target被调用方法的目标对象
	 **/
	@Override
	public void before(Method method, Object[] args, Object target) throws Throwable {
		System.out.println(target.getClass().getName()+"的"+method.getName()+"方法");
	}
}

```

AfterLog.java--后置通知

```java
package cn.sxt.log;

import java.lang.reflect.Method;

import org.springframework.aop.AfterReturningAdvice;

public class AfterLog implements AfterReturningAdvice {
	/**
	 * 目标方法执行后的通知
	 * @param--returnValue--返回值
	 * @param--method 被调用的方法对象
	 * @param--args 被调用对象的参数
	 * @param--target 被调用的方法对象的目标对象
	 * */
	@Override
	public void afterReturning(Object returnValue, Method method, Object[] args, Object target) throws Throwable {
		System.out.println(target.getClass().getName()+"的"+method.getName()+"方法被执行，返回值是"+returnValue);
	}

}

```

target目标类

```java
package cn.sxt.service.impl;

import cn.sxt.service.UserService;

public class UserServiceImpl implements UserService {

	@Override
	public void add() {
		System.out.println("增加用户");
	}

	@Override
	public void update() {
		System.out.println("修改用户");
	}

	@Override
	public void delete() {
		System.out.println("删除用户");
	}

	@Override
	public void query() {
		System.out.println("查询用户");
	}
}

```

配置文件beans.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
 <bean id="userService" class="cn.sxt.service.impl.UserServiceImpl"/>
 <bean id="log" class="cn.sxt.log.Log"/>
 <bean id="afterLog" class="cn.sxt.log.AfterLog"/>
 <aop:config>
 	<aop:pointcut expression="execution(* cn.sxt.service.impl.*.*(..))" id="pointcut"/>
 	<aop:advisor advice-ref="log" pointcut-ref="pointcut"/>
 	<aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"/>
 </aop:config>
 </beans>       
```

测试类Tets.java

```java
package cn.sxt.test;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import cn.sxt.service.UserService;

public class Test {

	public static void main(String[] args) {
		ApplicationContext applicationContext=new ClassPathXmlApplicationContext("beans.xml");
		UserService userService=(UserService)applicationContext.getBean("userService");
		userService.add();
		userService.delete();
	}
}

```

#### b)自定义类来实现

Log.java

```java
package cn.sxt.log;

import java.lang.reflect.Method;

import org.springframework.aop.MethodBeforeAdvice;


public class Log{
	public void before() {
		System.out.println("方法执行前!");
	}
	public void after() {
		System.out.println("方法执行后!");
	}
}

```

领域业务类

```java
package cn.sxt.service.impl;

import cn.sxt.service.UserService;

public class UserServiceImpl implements UserService {

	@Override
	public void add() {
		System.out.println("增加用户");
	}

	@Override
	public void update() {
		System.out.println("修改用户");
	}

	@Override
	public void delete() {
		System.out.println("删除用户");
	}

	@Override
	public void query() {
		System.out.println("查询用户");
	}
}

```

配置

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
 <bean id="userService" class="cn.sxt.service.impl.UserServiceImpl"/>
 <bean id="log" class="cn.sxt.log.Log"/>
 <aop:config>
 	<aop:aspect ref="log">
 		<aop:pointcut expression="execution(* cn.sxt.service.impl.*.*(..))" id="pointcut"/>
 		<aop:before method="before" pointcut-ref="pointcut"/>
 		<aop:after method="after" pointcut-ref="pointcut"/>
 	</aop:aspect>
 </aop:config>
 </beans>       
```

#### c)AOP的重要性：

使得业务方法更加纯粹，spring将公共的业务(如日志、安全等)和领域业务结合。当执行领域业务的时候把公共业务加进来。实现公共业务的重复利用。领域业务更加纯粹。程序员更加专注与领域业务。其本质还是动态代理。

#### d)注解实现

配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
 <bean id="userService" class="cn.sxt.service.impl.UserServiceImpl"/>
 <bean id="log" class="cn.sxt.log.Log"/>
 <aop:aspectj-autoproxy/>
 </beans>       
```

公共业务类Log.java

```java
package cn.sxt.log;

import java.lang.reflect.Method;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.aop.MethodBeforeAdvice;

@Aspect
public class Log {

	@Before("execution(* cn.sxt.service.impl.*.*(..))")
	public void before() {

		System.out.println("方法执行前!");
	}

	@After("execution(* cn.sxt.service.impl.*.*(..))")
	public void after() {
		System.out.println("方法执行后!");
	}

	@Around("execution(* cn.sxt.service.impl.*.*(..))")
	public Object around(ProceedingJoinPoint jp) throws Throwable {
		System.out.println("环绕前");
		System.out.println("签名：" + jp.getSignature());
		// 执行目标方法 Object
		Object result=jp.proceed();
		System.out.println("环绕后");
		  return result;
	}

}

```

