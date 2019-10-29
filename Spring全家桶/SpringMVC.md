# SpringMVC学习笔记

## 一、Hello SpringMVC

### 1)mvc要做哪些事情

a)将url映射到Java类或Java方法

b)封装用户提交的数据

c)处理请求--调用相关的业务处理--封装响应的数据

d)将响应的数据进行渲染：jsp、html、freemaker

### 2)springMVC要做哪些事情

springMVC是一个轻量级的，基于请求响应的mvc框架。

### 3)为什么要学习springMVC

a)性能较struts2好

b)简单，便捷，易学。

c)天生和spring无缝集成(使用sping ioc，aop)

d)使用规范优于配置

e)能够进行简单junit测试

f)支持Restful风格

g)本地化，国家化

h)数据验证，类型转换

i)拦截器

......

### 4)简单了解结构

![springMVC结构图](../resources/springMVC%E7%BB%93%E6%9E%84%E5%9B%BE.jpg)

### 5)spring hello实现

#### a)导包

```java
commons-logging-1.2.jar
spring-beans-4.1.6.RELEASE.jar
spring-context-4.1.6.RELEASE.jar
spring-context-support-4.1.6.RELEASE.jar
spring-expression-4.1.6.RELEASE.jar
spring-core-4.1.6.RELEASE.jar
spring-web-4.1.6.RELEASE.jar
```

#### b)配置web.xml文件--配置分发器

```xml
<servlet>
  	<servlet-name>springmvc</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
  	<servlet-name>springmvc</servlet-name>
  	<url-pattern>*.do</url-pattern>
  </servlet-mapping>
```

#### c)添加springmvc配置文件

默认在WEB-INF下添加[servlet-name]-servlet.xml 。此处为springmvc-servlet.xml

#### d)编写HelloController.java

```java
package cn.sxt.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

public class HelloController implements Controller{
	public ModelAndView handleRequest(HttpServletRequest req, HttpServletResponse resp) throws Exception {
		ModelAndView modelAndView=new ModelAndView();
		//封装要显示的数据
		modelAndView.addObject("msg","hello springmvc!");
		modelAndView.setViewName("hello");
		return modelAndView;
	}
}

```

####  e)编写sprigmvc配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd">
	<!-- 配置handlerMapping -->
	<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
	<!-- 配置handlerAdapter -->
	<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
	<!-- 配置渲染器 -->
	<bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
		<property name="prefix" value="/WEB-INF/jsp/"/>
		<property name="suffix" value=".jsp"/>
	</bean>
	<!-- 配置请求和处理器  -->
	<bean name="/hello.do" class="cn.sxt.controller.HelloController"></bean>
</beans>
 
```

#### f)测试

启动Tomcat并发布后：打开浏览器：<http://localhost:8080/hello/hello.do>

注意修改项目名称

## 二、使用注解开发springMVC

### 1)导入jar包

```
commons-logging-1.2.jar
spring-beans-4.1.6.RELEASE.jar
spring-context-4.1.6.RELEASE.jar
spring-context-support-4.1.6.RELEASE.jar
spring-expression-4.1.6.RELEASE.jar
spring-core-4.1.6.RELEASE.jar
spring-web-4.1.6.RELEASE.jar
spring-aop-4.1.6.RELEASE.jar
spring-webmvc-4.1.6.RELEASE.jar
```

### 2)配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>02springmvc_annotation</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <!--classpath指src目录下-->
      <param-value>classpath:mvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <url-pattern>*.do</url-pattern>
  </servlet-mapping>
</web-app>
```

### 3)Controller

```java
package cn.sxt.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
@Controller
public class HelloController {
	@RequestMapping("/hello")
	public ModelAndView hello(HttpServletRequest req,HttpServletResponse res){
		ModelAndView modelAndView=new ModelAndView();
		//封装要显示的数据
		modelAndView.addObject("msg","Hello world ,this is a springmvc and annotation test! Sucessful!");
		modelAndView.setViewName("hello");
		System.out.println(" --------------");
		return modelAndView;
	}
}
```

### 4)springmvc配置

mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd">
	<bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
	<property name="prefix" value="/WEB-INF/jsp/"/>
	<property name="suffix" value=".jsp"/>
	</bean>
	<!--扫描该包下的注解-->
	<context:component-scan base-package="cn.sxt.controller"/>
</beans>
```

## 三、Controller配置汇总

### 1)URL对应Bean

```xml
<!-- 配置handlerMapping -->
	<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
<!-- 配置请求和处理器  -->
	<bean name="/hello.do" class="cn.sxt.controller.HelloController"></bean>
```

以上配置，访问/hello.do就会寻找ID为/hello.do的Bean，此类方式仅适用于小型的应用系统

### 2)为URL分配Bean

```xml
	<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
		<property name="mappings">
			<props>
				<!-- key为请求路径，value为对应处理器的id -->
				<prop key="/hello.do">helloController</prop>
			</props>
		</property>
	</bean>
	<bean id="helloController" class="cn.sxt.controller.HelloController"/>
```

此类配置还可以使用通配符，访问/hello.do，spring会把请求分给helloController处理

### 3)URL匹配Bean 

```xml
<!-- 将hello*.do交给cn.sxt.controller.HelloController处理 -->
<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping"/>
	<bean name="helloController" class="cn.sxt.controller.HelloController"/>
```

### 4)注解

```xml
<context:component-scan base-package="cn.sxt.controller"/>
```

controller的代码中，要写相应的注解

```java
package cn.sxt.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
@Controller
public class HelloController {
	@RequestMapping("/hello")
	public ModelAndView hello(HttpServletRequest req,HttpServletResponse res){
		ModelAndView modelAndView=new ModelAndView();
		//封装要显示的数据
		modelAndView.addObject("msg","Hello world ,this is a springmvc and annotation test! Sucessful!");
		modelAndView.setViewName("hello");
		System.out.println(" --------------");
		return modelAndView;
	}
}

```

## 四、结果跳转方式

### 1)设置ModelAndView对象

根据View的名称，和视图解析器跳转到指定的页面。

页面：视图解析器前缀+view name+视图解析器的后缀 

```java
ModelAndView modelAndView=new ModelAndView();
		//封装要显示的数据
		modelAndView.addObject("msg","Hello world ,this is a springmvc and annotation test! Sucessful!");
		modelAndView.setViewName("hello");
		return modelAndView;
```

### 2)根据servlet API对象,不需要视图解析器

#### a)通过HttpServletResponse来进行输出。

```java
package cn.sxt.controller;

import java.io.IOException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class HelloController {
	@RequestMapping("/hello")
	public void hello(HttpServletRequest req,HttpServletResponse resp) throws IOException{
		System.out.println("=============");
		resp.getWriter().println("Hello spring mvc! Using servlet API!");
	}
}

```

#### b)通过HttpServletResponse实现重定向。

```java
package cn.sxt.controller;

import java.io.IOException;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class HelloController {
	@RequestMapping("/hello")
	public void hello(HttpServletRequest req,HttpServletResponse resp) throws IOException{
		System.out.println("=============");
		resp.sendRedirect("index.jsp");
	}
}

```

#### c)通过HttpServletRequest是实现请求转发

```java
package cn.sxt.controller;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
@Controller
public class HelloController {
	@RequestMapping("/hello")
	public void hello(HttpServletRequest req,HttpServletResponse resp) throws IOException, ServletException{
		System.out.println("=============");
		req.setAttribute("msg","请求转发！");
		req.getRequestDispatcher("index.jsp").forward(req, resp);
	}
}
```

### 3)通过springMVC实现重定向和请求转发--没有视图解析器

#### 转发的实现

##### a)

```java
@RequestMapping("/hello1")
	public String hello1(){
        //转发
		return "index.jsp";
	}
```

##### b)

```java
@RequestMapping("/hello1")
	public String hello1(){
		return "forward:index.jsp";
	}
```

#### 重定向

```java
@RequestMapping("/hello1")
	public String hello1(){
		return "redirect:index.jsp";
	}
```

### 4)通过springMVC实现重定向和请求转发--有视图解析器

转发

```java
@RequestMapping("/hello2")
	public String hello2(){
		return "hello";
	}
```

重定向

```java
@RequestMapping("/hello2")
	public String hello2(){
		return "hello.do";
	}
```

注意：重定向“rediirect:index.jsp”不需要视图解析器

## 五、数据的处理

### 1)提交数据的处理

#### a)提交的域名称和处理方法的参数名一致即可。

提交的数据

![1565347799413](D:/Typora/img/1565347799413.png)

处理方法

```java
public class HelloController {
	@RequestMapping("/hello")
	public String hello(@RequestParam("uname")String name){
		System.out.println(name);
		return "index.jsp";
	}
```

#### b)如果域名和处理方法的参数名不一致，则要通过注解实现

![1565348074220](D:/Typora/img/1565348074220.png)

处理方法

```java
public class HelloController {
	@RequestMapping("/hello")
	public String hello(@RequestParam("uname")String name){
		System.out.println(name);
		return "index.jsp";
	}
```

#### c)如果要提交一个对象

![1565349102442](D:/Typora/img/1565349102442.png)

要求提交的表单域名和对象的属性名一致，参数使用对象作为参数即可

处理方法

```Java
	@RequestMapping("/user")
	public String user(User user){
		System.out.println(user);
		return "index.jsp";
	}
```

实体类

```java
package cn.sxt.vo;

public class User {
	private String name;
	private int id;
	private String pw;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	@Override
	public String toString() {
		return "User [name=" + name + ", id=" + id + ", pw=" + pw + "]";
	}
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getPw() {
		return pw;
	}
	public void setPw(String pw) {
		this.pw = pw;
	}
}

```

### 2)将数据显示到UI层

#### a)通过ModelAndView--需要视图解析器

```java
package cn.sxt.controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;
@Controller
public class HelloController {
	@RequestMapping("/hello")
	public ModelAndView hello(HttpServletRequest req,HttpServletResponse res){
		ModelAndView modelAndView=new ModelAndView();
		//封装要显示的数据
		//相当于req.setAttribute("msg", "Hello world ,this is a springmvc test! Sucessful!");
		modelAndView.addObject("msg","Hello world ,this is a springmvc test! Sucessful!");
		modelAndView.setViewName("hello");//web-inf/jsp/hello.jsp
		return modelAndView;
	}
}
```

#### d)通过ModelMap

modleMap需要作为处理方法的参数

```java
	@RequestMapping("/hello")
	public String hello(@RequestParam("uname")String name,ModelMap model){
		model.addAttribute("name", name);
		System.out.println(name);
		return "index.jsp";
	}
```

## 六、乱码及restful

### 1)乱码的解决

##### a)解决post方式的乱码

springmvc中提供CharacterEncodingFilter解决，在web.xml文件中添加：

```xml
<filter>
  	<filter-name>encoding</filter-name>
  	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  	<init-param>
  		<param-name>encoding</param-name>
  		<param-value>utf-8</param-value>
  	</init-param>
  </filter>
  <filter-mapping>
  	<filter-name>encoding</filter-name>
  	<url-pattern>*.do</url-pattern>
  </filter-mapping>
```

##### b)解决get方式的乱码

修改tomcat的配置解决

自定义乱码解决的过滤器，如jsp页面的编码设置  

### 2)restful风格的url

优点：轻量级，安全，效率高

```java
	//http://localhost:8080/restful/delete/1/2.do
	@RequestMapping("/delete/{id}/{uid}")
	public String hello(@PathVariable int id,@PathVariable int uid){
		System.out.println("id="+id+",uid="+uid);
		return "/index.jsp";
	}
```

### 3)同一个controller通过参数来达到不同的处理方法--不常用

提交：localhost:8080/restful/hello2.do?metod=add

```java
package cn.sxt.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
@RequestMapping("/hello2")
public class Hello2Controller {
	@RequestMapping(params="method=add",method=RequestMethod.GET)
	public String add(){
		System.out.println("add");
		return "redirect:/index.jsp";
	}
	@RequestMapping(params="method=update",method=RequestMethod.GET)
	public String update() {
		System.out.println("update");
		return "redirect:/index.jsp";
	}
}

```

## 七、springmvc实现文件上传

### 1.通过commons-fileupload来实现。

#### a)步骤：

- 导入相关jar包commons-fileupload, commons-io

- 配置springMVC配置解析器

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
  	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
  	xmlns:context="http://www.springframework.org/schema/context"
  	xsi:schemaLocation="
  		http://www.springframework.org/schema/beans
  		http://www.springframework.org/schema/beans/spring-beans.xsd
  		http://www.springframework.org/schema/context
  		http://www.springframework.org/schema/context/spring-context.xsd">
  	<bean id="multipartResolver"
  		class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
  		
  		<property name="defaultEncoding" value="utf-8"/>
  		<!-- one of the properties available; the maximum file size in bytes -->
  		<property name="maxUploadSize" value="1000000000" />
  		<property name="maxInMemorySize" value="40960"/>
  	</bean>
  	<!-- 扫描该包下的注释 -->
  	<context:component-scan base-package="cn.sxt.controller"/>
  </beans>
  ```

- Controller代码 

```java
package cn.sxt.controller;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.commons.CommonsMultipartFile;

@Controller
public class FileuploadController {
	@RequestMapping("/upload")
	public String fileupload(@RequestParam("file")CommonsMultipartFile file,HttpServletRequest req) throws IOException{
		//获取文件名
//		file.getOriginalFilename();
		//获取上传文件路径
		System.out.println("-----------");
		String path= req.getRealPath("/fileupload");
		InputStream istream=file.getInputStream();
		OutputStream ostream=new FileOutputStream(new File(path,file.getOriginalFilename()));
		int len=0;
		byte[] buffer= new byte[400];
		while((len=istream.read(buffer))!=-1)
			ostream.write(buffer,0,len);
		ostream.close();
		istream.close();
		return "index.jsp"; 
	}
}

```

- JSP

```jsp
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <base href="<%=basePath%>">
    
    <title>上传文件</title>
	<meta http-equiv="pragma" content="no-cache">
	<meta http-equiv="cache-control" content="no-cache">
	<meta http-equiv="expires" content="0">    
	<meta http-equiv="keywords" content="keyword1,keyword2,keyword3">
	<meta http-equiv="description" content="This is my page">
	<!--
	<link rel="stylesheet" type="text/css" href="styles.css">
	-->
  </head>
  
  <body>
     <h1>请上传一个文件</h1>
     <form action="upload.do" method="post" enctype="multipart/form-data">
     	file:<input type="file" name="file"/>
     	<input type="submit" value="上传"/>
     </form>
  </body>
</html>

```

## 八、spring中Ajax的使用

### 1.使用HttpServletResponse来处理

