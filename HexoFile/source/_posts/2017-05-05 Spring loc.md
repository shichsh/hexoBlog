---
title: Spring loc
date: 2017-05-12
tags: Spring
---

xmlns: xml naming space

> 解剖 iocconfig.xml

## Containers
Spring provides the following two distinct types of containers: 

<!-- more -->
1. Spring BeanFactory Container
	
	* The most commonly used BeanFactory implementation is the XmlBeanFactory class.
	* It is usually preferred where the resources are limited like mobile devices or applet-based applications.
	* Has lazy-loading feature for all beans. Its key subclass XmlBeanFactory is deprecated.
	* Do not support annotation and has lazy-loading feature.
	* e.g 

	```java
	XmlBeanFactory factory = new XmlBeanFactory(new ClassPathResource("Beans.xml"));
	HelloWorld obj = (HelloWorld) factory.getBean("helloWorld");
	obj.getMessage();
	```
	In **Beans.xml** file: 	
	```xml
	<bean id = "helloWorld" class = "com.mercury.HelloWorld">
	<property name = "message" value = "Hello World!"/>
	</bean> 
   ```
	
2. Spring ApplicationContext Container
	
	* The most commonly used ApplicationContext implementations are **FileSystemXmlApplicationContext**,**ClassPathXmlApplicationContext** and **WebXmlApplicationContext**
	* Support annotation
	* Sub-interface of BeanFactory with more functionalities.
	
## Dependency Injection

|Injection|Setter Methods Injection|Constructor Injection|
|:---:|:------------------:|:----------------:|
|Tag|<span><</span>property>|<span><</span>constructor-arg>|
|Format|<span><</span>property name="" value=""/>|<span><</span>constructor-arg index=""(begin from 0) value=""/>
|Partial Dependency|Yse|No|
|Flexibility|high|low|

For reference type property injection, use "ref"

|Scope|Description|
|:--:|:------------------:|
|singleton|A single instance per Spring IoC container (default) |
|prototype|Have any number of object instances|
|request|Only valid in the context of a web-aware Spring ApplicationContext|
|session|Only valid in the context of a web-aware Spring ApplicationContext|
|global-session|A global HTTP session. Only valid in the context of a web-aware Spring ApplicationContext|

### Autowiring

Autowiring inject <span><</span>bean> without using <constructor-arg> and <span><</span>property>.

|Mode|Description|Match what|
|:--:|:------------------:|:--:|
|no|Default setting, no autowiring|via 'ref' to match|
|byName|If the name of a bean is same as the name of other bean property, auto wire it|properties|
|byType|If data type of a bean is compatible with the data type of other bean property, auto wire it|propetites|
|constructor|Ff data type of a bean is same as the data type of other bean constructor argument, auto wire it|constructor|
|autodetect|If a default constructor is found, uses “constructor”; Otherwise, uses “byType”|constructor and properties

> default priority: byType > byName

* Usually we use constructor arguments for mandatory dependencies and setters for optional dependencies.
* You have to use constructor injection, not setter injection to inject immutable class

Scope Relation
```java
public class Wrapper {
	private User user;
	private Person p;
}
```
(s: singleton | p: prototype)

|Wrapper(W) refer User(U)|Wrapper|Wrapper's User|User|
|:--:|:------------------:|:--:|:--:|
|W:s & U:s|Same|Same|Same|
|W:p & U:s|Not same|Same|Same|
|W:s & U:p|Same|Same|Not same|
|W:p & U:p|Not same|Not same|Not same|

### Singleton Pattern
```java
public class MySingle {
  private static MySingle instance = null
  public private MySingle() {
    }
  }
  public static MySingle getInstance() {
    if (instance == null) {
      instance = new MySingle();
    }
    return instance;
  }
}
```

## Annotation:
@Required: The @required annotation applies to bean property setter methods.

@Autowired

1. The @Autowired annotation can apply to bean property setter methods, non-setter methods, constructor and properties. 
2. It is auto wire the bean by matching data type.

To enable @Autowired, you have to register "AutowiredAnnotationBeanPostProcessor"

* Include <span><</span>context:annotation-config/> in bean configuration file.
* Include "<span><</span>bean
	class="org.springframework.beans.factory.
annotation.AutowiredAnnotationBeanPostProcessor">" 

@Qualifier: The @Qualifier annotation along with @Autowired can be used to tell Spring about which bean should autowired.

* e.g. 

  ```java
  @Qualifier("person1") // person1 is bean id
  @Autowired
  private Person person;
  ```

## Two special Injection

### Collection Injction

* List

```xml
<property name="list"> 
 	<list>
		<value>Item1</value>
		<value>Item2</value>
	</list>
</property>
```
* Set

```xml
<property name="set">
	<set>
		<value>100</value>
		<value>200</value>
	</set>
</property>
```

* Map

```xml
<property name="map">
	<map>
		<entry key="key1" value="100"/>
		<entry key="key2" value="200"/>
	</map>
</property>
```

* Properties

```xml
<property name="properties">
	<props>
		<prop key="key3">value3</prop>
		<prop key="key4">value4</prop>
	</props>
</property>
```

### Singleton Injection

* First Example:

```xml
<bean id="ms" class="com.mercury.beans.MySingle" scope="prototype" factory-method="getInstance"/>
```
> spring use reflection to call constructor to make many objects, but use getInstance factory-method to force getInstance() constructor called.

* Second Example:

```xml
<bean id="ms" class="com.mercury.beans.MySingle" factory-method="getInstance"/>
```
> use getInstance() to create object no matter inside or outside.

## Stereotype
> Put "context:component" in bean configuration file, it means, enable auto scanning feature in Spring. The base-package is indicate where are your components stored, Spring will scan this folder and find out the bean (annotated with @Component) and register it in Spring container.
> e.g. <span><</span>context:component-scan base-package="com.mercury.stereotype.beans"/>

@Component: Indicate this is class is an auto scan component.

@Controller(controller package): Indicates a controller component in the presentation layer.

@Service(Service package): Indicate Service component in the business layer.

@Repository(DAO package): Indicates DAO component in the persistence layer.

@Value
@Resource
@Autowired

* @Service("AAA") -> getBean("AAA")
* lazy-init: set it "true" means the constructor of the bean won't called until "getBean".


