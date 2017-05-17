---
title: Hibernate环境配置 (Demo)
date: 2017-05-03 22:20:21
tags:
---
Hibernate配置环境：
Java SE8
Eclipse Mars
Hibernate 3.2
Oracle 10g Express

<!-- more -->
步骤：

1. create a maven project, in pom.xml file, add hibernate dependencies. The maven project will automatically create jar files.

2. Create table Samle(name, age) in Oracle database

3. Create Java Bean class User
4. create a package in src folder. In this package, we create a file called "User.hbm.xml", write the following code:

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
5. Import hibernate 3.2 jar files in `WebContent/Web-INF/lib`

6. create a hibernate.cfg.xml file, write following code:
```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://hibernate.sourceforge.net/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory>
	    <property name="show_sql">true</property> 
	    <property name="format_sql">false</property>
        <property name="dialect">org.hibernate.dialect.OracleDialect</property>
		<property name="connection.datasource">java:/comp/env/jdbc/myoracle</property>
		<mapping resource="com/*****/mappings/User.hbm.xml"/>
    </session-factory>
</hibernate-configuration>
```
 com/*****/mappings is your package name

7. In util package, create a hibernateUtil class,
```java
public class HibernateUtil {
	private static final SessionFactory FACTORY;
	static {
		try {
			FACTORY = new Configuration().configure().buildSessionFactory();
		} catch (Exception e) {
			throw new RuntimeException(e.getMessage(), e);
		}
	}
	private static final ThreadLocal<Session> SESSION = new ThreadLocal<Session>() {
		@Override
		protected Session initialValue() {
			return FACTORY.openSession();
		}
	};
	
	public static SessionFactory getSessionFactory() {
		return FACTORY;
	}
	
	public static Session currentSession() throws HibernateException {
		Session s = SESSION.get();
		if (s == null) {
			s = FACTORY.openSession();
			SESSION.set(s);
		}
		return s;
	}
	public static void closeSession() throws HibernateException {
		Session s = SESSION.get();
		SESSION.set(null);
		if (s!=null) s.close();
	}
```

8. In dao.Impl package, create a daoimpl class which implements dao interface.

	```java
	public class HelloDaoImpl2 implements HelloDao {
	
		@Override
		public void save(User user) {
			Session session = HibernateUtil.currentSession();
			Transaction tx = session.beginTransaction();
			session.save(user);
			tx.commit();
		}
	
		@Override
		public void update(User user) {
			Session session = HibernateUtil.currentSession();
			Transaction tx = session.beginTransaction();
			session.update(user);
			tx.commit();
		}
	
		@Override
		public void delete(User user) {
			Session session = HibernateUtil.currentSession();
			Transaction tx = session.beginTransaction();
			session.delete(user);
			tx.commit();
		}
	
		@Override
		public List<User> queryAll() {
			Session session = HibernateUtil.currentSession();
			String hql = "from User";
			Query query = session.createQuery(hql);
			List<User> list = query.list();
			return list;
		}
	
		@Override
		public User findByName(String name) {
			Session session = HibernateUtil.currentSession();
			User user = (User)session.load(User.class, name);
			return user;
		}
	}
	```


