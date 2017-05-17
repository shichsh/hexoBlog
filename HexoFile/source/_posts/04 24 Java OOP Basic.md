---
title: Java OOP Basic
date: 2017-05-16
tags: Core Java
---

## Basic 
```java
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Hello world!");
  }
}
```

* If the class name is same to the file name, then the class is public by default. 
* public static void: if no public, then would be runtime exception(can't not access method)
* If no static: then runtime exception(main can't be called)
* Constructor can not have return type, but other methods must have return value.

```java
class A {
  void A() {
    // It is ok to do that, but not suggested 
  }
}
```

* final method: method can not be overrided 

* the position of public and static can be changed.

```java
public static void main() {} // it is allowed to create a new method call main() 
```

## OOP
### P:Polymorphism(override and overload)
Both: Return type should be the same 

```java
void foo(int x);		// 1
void foo(Integer x);	// 2
int foo(int x);			// 3
```
1 & 2 overload
2 & 3 not overload or override
1 & 3 not overload or override

```java
void setName(String fir, String la);
void setName(String la, String fir); 
//In polymorphism, different params mean order, or type, or numbers of params, but not parameter's name
```

#### Rule of Override

![Screen Shot 2017-05-16 at 15.12.12](/media/Screen%20Shot%202017-05-16%20at%2015.12.12.png)


```java
class One {
	One foo() {}
}

class Two extends One {
	// leagl: public, protected, One, Two
	protected Tow foo() {}
}
```

### I:Inheritance("is a" relation)
* B inherits A, B has A's private fields, but cannot access those fields

### E:Encapsulation("has a" relation)

### A:Abstraction

## OOP Rules
* First parent, then child 
* If an objected is created, its non-static fields are initialized first and finally its constructor is called.
* The static fields are initialized when the class is loaded.
* Polymorphism can only be applied to non-static, non-private methods.

> Summary: In a class hierarchy, an object stands at some level. It looks up and will know everything; it looks down and will know nothing.

## Reference and value

* For primitive type, Java is passed by the value
* For any object, Java is passed by the references or the value of the references.

There are four types of references in Java
1. Strong reference
2. java.lang.ref.SoftReference
3. java.lang.ref.WeakReference
4. java.lang.ref.PhantomReference

## Heap

* All the Java classes and any class-level things are loaded on Stack. Stack is small but faster.
* Any new objects in a Java program are created on the Heap. Heap is big but slower.

1. Young Generation
2. Old(Tenure) Generation
3. Permanent Generation(can not be garage collection): String Pool

## String Pool
* JVM maintains a String Pool on the Stack that stores all constant strings.
* The "new" keyword always create a new object on the Heap.
* String class has a method intern() that returns the canonical copy of the string, which means the reference of the constant string in the String Pool.

## == and equals
* == is to compare references, while equals method is to compare values
* In class Object, the value actually is the value of the references. Thus the original definition of equals method is to compare the values of the references. However, we can override equals method to compare the real values.

> * ==: same exact object
> * .equals(): same meaning (Defined by class: default is ==)

||2|3|
|:--:|:------------:|:-----------------:|

## Final and Immutable
### Final
* A final class cannot be inherited
* A final method cannot be overridden
* A final object's reference cannot be changed, not value

### Immutable
* An immutable object's value cannot be changed.
* As a "Read-Only" object, an immutable object is popular used in a multi-threading environment.


```java
class A {
  int x = 1;
}
final A a = new A();
a.x = 2; // accepted
a = null // not accepted
```
### How to initialize a final object

* Give an value directly in the class definition 

```java
class A { // ok
  final int x = 0;
}
```
* Initialize the final object in all constructors

```java
class A { // ok
  final int x;
  A(int x) { // called only once 
    this.x = x; // and you have to init x in constructor or "final int x = 10"
  }
}
```

```java
class A { // not ok 
  final int x;
  A(int x) {foo(x)}
  // can be called many times, so not allowed
  void foo(int x) {
    this.x = x;
  }
}
```
```java
//If a variable was defined by static and final, then it must be initialized through this way:
static final int x = 10; 
```

### How to create an immutable class
* The class is final
* All the fields are private and final
* The class only provides getter methods and does not provide setter methods.
* A constructor is usually defined by user to initialize all the fields.

## String, StringBuffer, StringBuilder
String: An immutatble class
StringBuffer: mutable and thread-safe
StringBuilder: mutable but not thread-safe, but performance is better than StringBuffer

> A thread-safe class means that at one time only one thread can access the object of the thread-safe class.

```java
int a = 10;
Integer x = new Integer(x); // Boxing
Integer y = a; // autoBoxing
```

## Upcasting and Downcasting
* For primitive types, shorter-byte types can be upcasting to longer-byte types.
* For objects, "is a" relationship can be used for upcasting.

```java
float a = 1.5F; // don't lost F
short x = 10;
short y = 11;
short z = (short)(x + y); // if x + y, then it is integer, cast it to short
```

## Clone and Cloneable

.clone() is one of the method in class Object

```java
protected Object clone() throws CloneNotSuppoertedException
```
* If you want to make a copy of an object, we need to implement the Cloneable interface and override clone method.
* Cloneable: marker interface
* clone is a shallow copy
* Prototype(GoF) Design Pattern

## Shallow Copy and Deep Copy

It is not recommended to use clone. pass object by reference. clone too much object make gc a lot 

```java
public class A {
  char[] a;
  public A(char[] a) {
    this.a = a.clone()
  }
}
```

Diamond Paradigm

no abstract before enum
no abstract and final together

Class Loader
Extensive
Bootstrap
system

### Final

final, finally, finalize

finally: try check

finalize() -> gc

### Reflection

```js
function exec(obj, method) {
  obj[method]();
}
```

SecurityManager -> control reflection

### Garbage Collection

gc can be invoked(not run), but not to be forced

cant never be garbage collected
static final field in a class 

minimize the usage of garbage collection
Object Pooling-> Factory

make memory overflow, how to deal with
jcvz -Xms512M, -Xmx 4096M Hello (eclipse.ini)

### Inner Class

local inner class:

```java
class A {
  class B {}
}w
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

HashCode()
hash value of address
(fn + " " + ln).hashcode()

instanceof vs getClass()







