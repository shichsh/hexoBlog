---
title: Spring AOP
date: 2017-05-14 23:11:12
tags: Spring
---
	
## Concepts
Spring AOP (Aspect-oriented programming) framework is used to modularize cross-cutting concerns in aspects. Put it simple, it’s just an interceptor to intercept some processes, for example, when a method is execute, Spring AOP can hijack the executing method, and add extra functionality before or after the method execution. (mkyong)

<!-- more -->
Purpose: Build dependency between two different classes

POJO: Plain-Old-Java-Object 

In Spring AOP, 4 type of advices are supported:

* Before advice – Run before the method execution
* After returning advice – Run after the method returns a result
* After throwing advice – Run after the method throws an exception
* Around advice – Run around the method execution, combine all three advices above.

## ProxyFactoryBean

```xml
<bean id="logProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
<property name="proxyInterfaces"></property>
<property name="target" ref=""/>
<property name="interceptorNames"><list></list></property>
```
## AspectJ

### Common AspectJ annotations:
1.	@Before – Run before the method execution
2.	@After – Run after the method returned a result
3.	@AfterReturning – Run after the method returned a result, intercept the returned result as well.
4.	@AfterThrowing – Run after the method throws an exception
5.	@Around – Run around the method execution, combine all three advices above.

> In Spring configuration file, put "<span><</span>aop:aspectj-autoproxy />", and define your Aspect (interceptor) and normal bean.

add @Aspect before advice class.


