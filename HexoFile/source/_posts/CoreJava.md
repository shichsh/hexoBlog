---
title: CoreJava(2)
date: 2017-04-25 22:15:06
tags:
---
## Clone in Java

* clone() 方法
clone() is one of the method of class Object.

<!-- more -->

```java
public class Employee implements Cloneable{

	private String name;
	
	public Employee() {}
	public Employee(String name) {
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	
	@Override
	public Employee clone() throws CloneNotSupportedException {
		// return new Employee(name);
		return (Employee)super.clone();  // shallow copy
	}
}
```
If we want to make a copy of an object, we need to implement the Cloneable interface and override clone method.
Clonable: marker interface
Clone() is a shallow copy

## Shallow Copy and Deep Copy

Relation chain: Prototype - clone - shallow copy
It is not recommended to use clone. pass object by reference. clone too much object make garbage collection a lot 

```java
public class A {
  char[] a;
  public A(char[] a) {
    this.a = a.clone()
  }
}
```

Diamond Paradigm

* no abstract before enum
* no abstract and final together

Class Loader
* Extensive
* Bootstrap
* system

## Final keyword

final, finally, finalize

finally: try check

finalize() -> gc

## Reflection

```js
function exec(obj, method) {
  obj[method]();
}
```

SecurityManager -> control reflection

## Garbage Collection

gc can be invoked(not run), but not to be forced

cant never be garbage collected
static final field in a class 

minimize the usage of garbage collection
Object Pooling-> Factory

make memory overflow, how to deal with
jcvz -Xms512M, -Xmx 4096M Hello (eclipse.ini)

## Inner Class

local inner class:

```java
class A {
  class B {}
}
A a = new A();
A.B b = a.new B();
```

Anonymous Inner class:

```java
class A { 
  void foo() {
    class B{}
  }
  Interface a = new I() {
    @Override
    public void foo() {}
  }
}

interface I {
  void foo();
}

Object a = new Object();
Object b = new Object() {};
```

An interface can contain an static inner class

## hashcode() 和 equals()
* 定义

hashCode() : returns a hash code value of object
equals(): 
hash value of address
(fn + " " + ln).hashcode()

instanceof vs getClass()


