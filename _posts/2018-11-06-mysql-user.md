--- 
layout: post
title: 'Hello Mysql'
date: 2018-11-06
author: GorgeousNi
color: RGB(107,167,66)
cover: '/assets/doc_img/mysql.jpg'
tags: mysql
---



### MySQL添加新用户、为用户创建数据库、为新用户分配权限
 <br/> 
####一. 创建用户
 <br/> 
```
CREATE USER 'username'@'host' IDENTIFIED BY 'password’;
```

> #####说明：
username：你将创建的用户名
host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

> #####例子：
CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
CREATE USER 'pig'@'%' IDENTIFIED BY '';
CREATE USER 'pig'@'%';

 <br/> 
####二. 授权:
 <br/> 
 
```
GRANT privileges ON databasename.tablename TO 'username'@'host'
``` 
> #####说明:
privileges：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL
databasename：数据库名
tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*

> #####例子:
GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
GRANT ALL ON *.* TO 'pig'@'%';
GRANT ALL ON maindataplus.* TO 'pig'@'%';

> #####注意:
用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;

 <br/> 
####三.设置与更改用户密码
 <br/> 
 
``` 
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
``` 
>如果是当前登陆用户用: SET PASSWORD = PASSWORD("newpassword");
例子：
 ``` SET PASSWORD FOR 'pig'@'%' = PASSWORD("123456"); ``` 

 <br/> 
####四. 撤销用户权限
 <br/> 
``` 
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
``` 
> #####说明:
privilege, databasename, tablename：同授权部分

> #####例子:
REVOKE SELECT ON *.* FROM 'pig'@'%';

> #####注意:
假如你在给用户'pig'@'%'授权的时候是这样的（或类似的）：GRANT SELECT ON test.user TO 'pig'@'%'，则在使用REVOKE SELECT ON *.* FROM 'pig'@'%';命令并不能撤销该用户对test数据库中user表的SELECT 操作。相反，如果授权使用的是GRANT SELECT ON *.* TO 'pig'@'%';则REVOKE SELECT ON test.user FROM 'pig'@'%';命令也不能撤销该用户对test数据库中user表的Select权限。
具体信息可以用命令SHOW GRANTS FOR 'pig'@'%'; 查看。

 <br/> 
####五.删除用户
 <br/> 
``` 
DROP USER 'username'@'host';
``` 
或者
``` 
drop user "gerrit"@"localhost";
``` 
> ####`注：我的使用的mysql版本：5.7.21`


 <br/> 
####六.实际操作
 <br/> 
  
``` 
mysql -u root -p
``` 
> #####5.1 新建用户
``` 
create user 'yd2018'@'localhost' identified by 'ss&sss#2';
``` 
``` 
create user 'yd2018'@'%' identified by 'ss&sss#2';
```
```  
flush privileges;
``` 


> #####5.2 创建数据库

```  
create database test01 DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```  

> #####5.3 授权
说明: privileges - 用户的操作权限,如SELECT , INSERT , UPDATE 等。如果要授予所的权限则使用 ALL;
databasename - 数据库名,tablename-表名,如果要授予该用户对所有数据库和表的相应操作权限则可用*表示, 如*.*

> -  **给该用户授予操作该数据库的全部权限 **
>``` grant all privileges on `test01`.* to 'yd2018'@'%' identified by 'Jiasajskas#2’;``` 
> -  **可查询 MySQL 中所有数据库中的表 **
>``` grant select on *.* to yd2018@localhost ...; ``` 
> -  **可管理 MySQL 中的所有数据库 **
>```grant all on *.* to yd2018@localhost …;``` 
>```GRANT ALL PRIVILEGES ON *.* TO yd2018@localhost IDENTIFIED BY PASSWORD 'jiasasjs18’;``` 
>```GRANT ALL PRIVILEGES ON *.* TO yd2018@'%' IDENTIFIED BY PASSWORD 'jiasasjs18’;``` 

> #####5.4 查看其他 MySQL 用户权限：
```  
show grants for yd2018 @localhost;
```

> #####5.5 查看MYSQL数据库中所有用户
```  
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
```  
<br/>
#####七.阿里云默认安装mysql root密码更改或者root密码忘记
<br/>
>参考 https://www.cnblogs.com/hgj123/p/5279471.html 我就不整理了

<br/>
其中修改root密码命令如下：
```  
update mysql.user set authentication_string=password('Jx47My2018RO#1') where user='root' and Host = 'localhost’;
```  

<br/>

##### 八.MySQL表名不区分大小写的设置方法
<br/>
>在用centox安装mysql后，把项目的数据库移植了过去，发现一些表的数据查不到，排查了一下问题，最后发现是表名的大小写不一致造成的。
mysql在windows系统下安装好后，默认是对表名大小写不敏感的，但是在linux下，一些系统需要手动设置。
用root登录，打开并修改 /etc/my.cnf；在[mysqld]节点下，加入一行：
```  
lower_case_table_names=1
```  
>重启mysql服务
```  
service mysqld restart
```  

<br/>
>参考链接：
https://blog.csdn.net/wushuchu/article/details/80529254