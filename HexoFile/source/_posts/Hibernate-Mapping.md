---
title: Hibernate Mapping
date: 2017-05-04 15:52:19
tags:
---
## Mapping for Composite Primary Key
Example: 
Table:Person( _FirstName_ , _LastName_ , Age)
Class: Person(firstName, lastName, age)

Class must implements Serial izable interface

Mapping File: ***.hbm.xml4

<!-- more -->

## Mapping for Class Hierachy
Example:
Parent Class: User(id, name)
Subclass: LocalUser(location), RemoteUser(country)

Three ways to map:

### Table per Class Hierachy
Table: 
All User( _ID_ , Name, Type, Location, Country)
Use <<span></span>Discriminator column="Type"/>in the mapping file
Use <<span></span>subclass discriminator-value="L"/> to identify LocalUser
Use <<span></span>subclass discriminator-value="R"/> to identify RemoteUser

* Pros: Good performance
* Cons: Null in Location or Country and results data redundancy

### Table per Subclass
Table:
Parent_User( _ID_ , Name)
LocalUser( _ID_ , Location)
RemoteUser( _ID_ , Country)

* Use joined-subclass tag in the mapping file
* Pros: No data redundancy
* Cons: Bad performance when using "join"

### Table per Concrete Class
Table:
Concrete_LocalUser( _ID_ , Name, Location)
Concrete_RemoteUser(_ID_ , Name, Country)

* Use union-subclass tag in the mapping file
* Tradeoff between performance and data redundancy.

## Mapping with Collection

Table: 
Customer( _ID_ , Name)
Email(ID, Address), ID as foreign key

Class:
Customer(id, name, emails)

* Lazy Fetching and LazyInitializationException

## Many-To-One Mapping

Table:
Game( _GID_ , Gname)
Player( _PID_ , Pname, GID as foreign key)

Class:
Game(gid, gname)
Player(pid, pname, game)

Cascade: cascade value can be: all, save-update, delete, delete-orphan, all-delete-orphan

## One-To-Many Mapping
Tbale:
House( _HID_ , Address)
Visitor( _VID_, Vname, HID as foreign key)

Class:
House(hid, address, visitors)
Visitor(vid, vname, hid)

Inverse: A boolean parameter. True means "many" controls "one"

## One-To-One Mapping

Table:
Trader( _TID_ , Name)
Login( _TID_ , Username, Password, TID as foreign key)

Class:
Trader(tid, name, login)
Login(tid, username, password, trader)

## Many-To-Many Mapping

Table: 
Student( _ID_ , Name)
Course( _CID_ , Course_name)
Enroll( _ID_ , _CID_ , ID and CID as foreign key)
Class:
Student(id, name, courses)
Course(cid, course_name, students)

## Transaction

A: Atomicity
C: Consistency
I: Isolation
D: Durability

## Isolation Levels

1. Read uncommitted
2. Read committed (default)
3. Repeatable Read
4. serializable

