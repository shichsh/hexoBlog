---
title: JavaEE JSP配置流程
date: 2017-05-02
tags:
---
配置环境：
Tomcat: apache-tomcat-7.0.34
Java: 8.0
OS: Window10

配置步骤：
<!-- more -->

1. 解压缩apache-tomcat-7.0.34，进入bin文件夹。在bin文件夹当中找到context xml文件，重命名为context_old. 在bin目录下新建context xml文件, 内容如下：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>
	<Resource name="jdbc/myoracle" 
		auth="Container"
	  	type="javax.sql.DataSource" 
	  	driverClassName="oracle.jdbc.driver.OracleDriver"
	  	url="jdbc:oracle:thin:@**yourpcname**:1521:XE"
	  	username="yourdatabaseusername" 
	  	password="yourdatabasepassword" 
	  	maxActive="20"
	  	maxIdle="10"
	  	maxWait="-1"/>
</Context>
```
参数maxWait = -1 意味着当active connection 达到maxActive值后，再得到的request会等待，等待时间forever

2. 配置keystore
参考[mkyong文档](https://www.mkyong.com/tomcat/how-to-configure-tomcat-to-support-ssl-or-https/)配置，完全按照步骤进行，注意keystore和key password密码保持一致。可能会有很多问题，回答完就好。
在进行第二步Connector in server.xml之前，把bin中的server复制一份，并重命名为server_old。在粘贴过后，记得更改keystore。

3. 新建自己的JavaEE workplace，这里是“JavaEE”文件夹。打开Eclipse, 选择workplace为JavaEE（或切换workplace）。

4. build server: window -> preference -> server -> runtime envrionment -> add -> Tomcat v7.0 -> Tomcat installation directory(browser your Tomcat directory)

5. 新建JSP demo: 右键新建dynamic web project -> 输入project name -> next -> next -> check "generate web.xml deployment descriptor"

6. 新建空的css文件夹，添加standard bootstrap css files。right click webcontent -> create a .jsp file called "***.jsp"-> remove all contents before <!DOCTYPE> and some contenst after <!DOCTYPE html
7. 在底部的server栏中，新建new server, 选择Tomcat v7.0 Server。确定后选中Tomcat v7.0 Server at localhost右键选择Add and Remove，将jsp文件加入configured中, 然后finish
8. 点击start启动服务器，console显示无误后，打开浏览器输入 http://<span></span>localhost:8080/project name/jsp file name进行测试

