---
title: Spring DaoSupport
date: 2017-05-15 10:56:32
tags: Spring
---

## Dao Support
Spring provides abstract class DaoSupport to make an abstraction. It has subclasses such as JdbcDaoSupport to integrate with JDBC, HibernateDaoSupport to integrate with Hibernate, JpaDaoSupport to integrate with JPA.

<!-- more -->
Instead of Daosupport, Spring Dao class can also be defined as POJO that contains different template object. For JDBC, it contains SimpleJdbcTemplate; For Hibernate, it contains HibernateTemplate; For JPA, it contains EntityManager.

* assume "user" is target data object

|Item|JdbcDaoSupport|HibernateDaoSupport|
|:--:|:------------:|:-----------------:|
|template|SimpleJdbcTemplate/ NamedParameterJdbcTemplate|HibernateTemplate|
|Access Template|getJdbcTemplate()|getHibernateTemplate()|
|C(create)|JC**|save(user)|
|R(read)|JR*|find("from User")->table name|
|U(update)|JU*|update(user)|
|D(delete)|JD*|delete(user)|

* JC:

```java
Object[] params = {user.getName(), user.getAge()};
String sql = "insert into Sample values(?,?)";
this.getJdbcTemplate().update(sql, params);
```

* JR:

```java
String sql = "select * from Sample";
return this.getJdbcTemplate().query(sql, new RowMapper<User>() {
	@Override
	public User mapRow(ResultSet rs, int line) throws SQLException {
		User user = new User();
		user.setName(rs.getString("Name"));
		user.setAge(rs.getInt("Age"));
		return user;
	}
});
```
* JU:

```java
Object[] params = {user.getAge(), user.getName()};
String sql = "update Sample set Age=? where Name=?";
this.getJdbcTemplate().update(sql, params);
```

* JD:

```java
Object[] params = {user.getName(), user.getAge()};
String sql = "delete Sample where Name=? and Age=?";
this.getJdbcTemplate().update(sql, params);
```

## HibernateDaoSupport

Config.xml:

```xml
<bean id="hdb01" class="com.mercury.dao.impl.HelloDaoBean01">
	<property name="sessionFactory" ref="sessionFactory"/>
</bean>
<bean id="sessionFactory" class="org.springframework.orm.hibernate3.LocalSessionFactoryBean">
	<property name="dataSource" ref="dataSource"/>
	<property name="mappingResources">
		<list>
			<value>com/mercury/mappings/User.hbm.xml</value>
		</list>
	</property>
	<property name="hibernateProperties">
		<props>
			<prop key="hibernate.dialect">org.hibernate.dialect.OracleDialect</prop>
			<prop key="hibernate.show_sql">true</prop>
		</props>
	</property>
</bean>
<!-- Define DataSource -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName">
		<value>oracle.jdbc.driver.OracleDriver</value>
	</property>
	<property name="url">
		<value>jdbc:oracle:thin:@yourPCname:1521:XE</value>
	</property>
	<property name="username" value="yourusername"/>
	<property name="password" value="yourpassword"/>
</bean>
```

User.hbm.xml(map bean object and oracle table):

```xml
<!DOCTYPE hibernate-mapping PUBLIC 
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd"> 
<hibernate-mapping>     
    <class name="com.mercury.beans.User" table="Sample">
        <id name="name" column="Name"></id> 
        <property name="age" column="Age"/>
    </class> 
</hibernate-mapping>
```

## JdbcDaoSupport
config.xml:
```xml
<!-- Define DataSource -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName">
		<value>oracle.jdbc.driver.OracleDriver</value>
	</property>
	<property name="url">
		<value>jdbc:oracle:thin:@yourPCname:1521:XE</value>
	</property>
	<property name="username" value="yourusername"/>
	<property name="password" value="yourpassword"/>
</bean>
	
<!-- Define Daos -->
<bean id="hdb01" class="com.mercury.dao.impl.HelloDaoBean01">
	<property name="dataSource" ref="dataSource"/>
</bean>
<bean class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>
<bean id="hdb02" class="com.mercury.dao.impl.HelloDaoBean02"/>
```

## Spring Cache

> Spring Cache is to cache the result of functions

cacheconfig.xml:

```xml
<cache:annotation-driven/> <!--don't forget-->
<!-- Define Daos -->
<bean id="hdb04" class="com.mercury.dao.impl.HelloDaoBean04">
	<property name="dataSource" ref="dataSource"/>
</bean>
	
<!-- Define DataSource -->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
	<property name="driverClassName">
		<value>oracle.jdbc.driver.OracleDriver</value>
	</property>
	<property name="url">
		<value>jdbc:oracle:thin:@yourPCname:1521:XE</value>
	</property>
	<property name="username" value="yourusername"/>
	<property name="password" value="yourpassword"/>
</bean>
	
<bean id="cacheManager" class="org.springframework.cache.support.SimpleCacheManager">
	<property name="caches">
		<set>
			<bean class="org.springframework.cache.concurrent.ConcurrentMapCacheFactoryBean" p:name="default"/>
			<bean class="org.springframework.cache.concurrent.ConcurrentMapCacheFactoryBean" p:name="users"/>
		</set>
	</property>
</bean>
```

> In cache manager, p:name="users" should be related to cache annotation.

For examples:

```Java
@Cacheable("users") // correspond to p:name
@Override
public List<User> query() {
	String sql = "select * from Sample";
	return template.query(sql, new RowMapper<User>() {
		@Override
		public User mapRow(ResultSet rs, int line) throws SQLException {
			User user = new User();
			user.setName(rs.getString("Name"));
			user.setAge(rs.getInt("Age"));
			return user;
		}
	});
}

@CacheEvict(value="users", allEntries=true) // correspond to p:name
@Override
public void delete(User user) {
	Object[] params = {user.getName(), user.getAge()};
	String sql = "delete Sample where Name=? and Age=?";
	template.update(sql, params);
}
```


