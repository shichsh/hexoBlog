---
title: Spring MVC
date: 2017-05-09
tags: Spring
---
## Spring Features
Spring MVC is different from other MVC frameworks. It is a method driven, not mapping driven framework. Mapping is determined by annotation, instead of explicitly defined in configuration file.

<!-- more -->

## Spring Workflow
A request goes through DispatcherServlet and uses @Controller to find a controller class. And then it uses @RequestMapping to find the corresponding method to handle the request. Then the method returns a ModelAndView object, in which model contains data and view contains a string that will be converted to a real url by ViewResolve.

> DispatcherServlet -> find @Controller -> map @RequestMapping -> use ViewResolve to concatenate a real url -> find target jsp file.

In old Spring2 version, we use 

* SimpleUrlHandlerMapping
* BeanNameUrlHandlerMapping

to handle our mapping.
## A Spring MVC Demo

1. File -> new -> Dynamic Web Project -> project name: SpringMVCDemo -> Next -> Next -> check "Generate web.xml deployment descriptor" -> Finish

2. Here is the content of web.xml:

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
   xmlns="http://java.sun.com/xml/ns/javaee" 
	 xsi:schemaLocation="
		http://java.sun.com/xml/ns/javaee 
		http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>SpringMVCDemo</display-name>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  <!-- Spring MVC -->
  <servlet>
  <servlet-name>spring</servlet-name>
  <servlet-class>
  		org.springframework.web.servlet.DispatcherServlet <!-- auto find "****"-servlet -->
  </servlet-class>
  <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
  	<servlet-name>spring</servlet-name>
  	<url-pattern>*.html</url-pattern>
  </servlet-mapping>
  </web-app> 
  ```
3. In lib folder, import jar files: 
![7B6741661725AD913E010FBDB208E02D](/media/7B6741661725AD913E010FBDB208E02D.jpg)

4. In WEB-INF folder, create a xml file called spring-servlet.xml. Here is the content of it:

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xmlns:context="http://www.springframework.org/schema/context" 
	xmlns:mvc="http://www.springframework.org/schema/mvc" 
	xsi:schemaLocation="
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context-3.0.xsd
		http://www.springframework.org/schema/mvc
		http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">
	<context:component-scan base-package="com.mercury" />
	<mvc:annotation-driven />
	
	<bean id="viewResolver"
		class="org.springframework.web.servlet.view.UrlBasedViewResolver">
		<property name="viewClass"
			value="org.springframework.web.servlet.view.JstlView" />
		<property name="prefix" value="/WEB-INF/pages/" />
		<property name="suffix" value=".jsp" />
	</bean>
</beans>
```
	> You can define your package name on base-package attribute. You should use format "****-servlet.xml" to name your file. **** is your <span><</span>servlet-name> in web.xml file.

5. In Java Resources, src folder, create a package called "com.mercury.controller"(You should create it based on your package name). In this package, create a class called "HelloController".
6. Here is the content of HelloController.java:

	```java
	package com.mercury.controller;
	
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpSession;
	
	import org.springframework.stereotype.Controller;
	import org.springframework.ui.ModelMap;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.ResponseBody;
	import org.springframework.web.servlet.ModelAndView;
	
	@Controller
	public class HelloController {
		@RequestMapping("/hello")  // or ("hello.html")
		public ModelAndView sayHello(HttpServletRequest request) {
			String name = request.getParameter("name"); // http://localhost:8080/SpringMVCDemo/hello.html?name=bob
			ModelAndView mav = new ModelAndView();
			mav.setViewName("hello");
			mav.addObject("title", "Hello " + name + ", welcom to Spring MVC!");
			return mav;
		}
		
		@RequestMapping("/main")
		public String goMain(ModelMap model, HttpSession session) {
			model.addAttribute("msg", "This is a message from ModelMap");
			session.setAttribute("welcome", "Welcom from session");
			return "main";
		}
		
		@RequestMapping("/data")
		@ResponseBody // return data
		public String getData() {
			return "This is data, not a url";
		}
	}
```
> @ResponseBody means it return the data, not going to viewResolver

7. Create a folder called "pages" in WebContent, create two jsp files:
	hello.jsp:
	```jsp
	<!DOCTYPE html>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<link rel="stylesheet" href="css/bootstrap.min.css" type="text/css">
	<title>Hello Page</title>
	</head>
	<body>
	<div class="container">
		<h1 style="color: blue">${title}</h1>
	</div>
	</body>
	</html>
	```
	main.jsp:
	```jsp
	<!DOCTYPE html>
	<html>
	<head>
	<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
	<link rel="stylesheet" href="css/bootstrap.min.css" type="text/css">
	<script src="js/jquery-3.1.0.min.js"></script>
	<title>Main Page</title>
	<script>
		$(document).ready(function() {
			$("#btn").click(function() {
				$.get("data.html").then(function(resp) {
					$("#data").text(resp);
				});
			});
		});
	</script>
	</head>
	<body>
	<div class="container">
		<h1 style="color: green">This is a main page</h1>
		<hr>
		<p>Message: ${msg}</p>
		<p>Session: ${sessionScope.welcome}</p>
		<hr>
		<h3>Data: <span id="data"></span></h3>
		<button id="btn" class="btn btn-primary">Get Data</button>
	</div>
	</body>
	</html>
	```
8. Create css and js folder in WebContent. In css folder, import bootstrap.css and bootstrap.min.css files. In js folder, import jquery-3.1.0.js and jquery-3.1.0.min.js files
9. 在底部的server栏中，新建new server, 选择Tomcat v7.0 Server。确定后选中Tomcat v7.0 Server at localhost右键选择Add and Remove，将jsp文件加入configured中, 然后Finish
10. 点击start启动服务器，console显示无误后，打开浏览器输入 http://<span></span>localhost:8080/project name/hello.html(or main.html)进行测试
	




