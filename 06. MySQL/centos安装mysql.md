# centos安装mysql

## 更新包管理器
```bash
sudo yum update
```

## 下载安装mysql yum仓库
```bash
wget https://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm
yum localinstall mysql80-community-release-el7-1.noarch.rpm
```

```bash
[root@localhost zzy610]# wget https://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm
--2023-08-10 18:32:48--  https://repo.mysql.com/mysql80-community-release-el7-1.noarch.rpm
Resolving repo.mysql.com (repo.mysql.com)... 23.193.52.250, 2600:1417:4400:3bd::1d68, 2600:1417:4400:38d::1d68
Connecting to repo.mysql.com (repo.mysql.com)|23.193.52.250|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 25820 (25K) [application/x-redhat-package-manager]
Saving to: ‘mysql80-community-release-el7-1.noarch.rpm’

100%[==================================================================================================================================================>] 25,820      --.-K/s   in 0s      

2023-08-10 18:32:48 (329 MB/s) - ‘mysql80-community-release-el7-1.noarch.rpm’ saved [25820/25820]

[root@localhost zzy610]# yum localinstall mysql80-community-release-el7-1.noarch.rpm
Loaded plugins: fastestmirror, langpacks
Examining mysql80-community-release-el7-1.noarch.rpm: mysql80-community-release-el7-1.noarch
Marking mysql80-community-release-el7-1.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package mysql80-community-release.noarch 0:el7-1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

============================================================================================================================================================================================
 Package                                            Arch                            Version                          Repository                                                        Size
============================================================================================================================================================================================
Installing:
 mysql80-community-release                          noarch                          el7-1                            /mysql80-community-release-el7-1.noarch                           31 k

Transaction Summary
============================================================================================================================================================================================
Install  1 Package

Total size: 31 k
Installed size: 31 k
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : mysql80-community-release-el7-1.noarch                                                                                                                                   1/1 
  Verifying  : mysql80-community-release-el7-1.noarch                                                                                                                                   1/1 

Installed:
  mysql80-community-release.noarch 0:el7-1                                                                                                                                                  

Complete!
```

## 导入GPG密钥并下载

```bash
[root@localhost rpm-gpg]# rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
[root@localhost rpm-gpg]# yum install mysql-server
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.163.com
 * extras: mirrors.163.com
 * updates: mirrors.163.com
mysql-connectors-community                                                                                                                                  | 2.6 kB  00:00:00     
mysql-tools-community                                                                                                                                       | 2.6 kB  00:00:00     
mysql80-community                                                                                                                                           | 2.6 kB  00:00:00     
正在解决依赖关系
--> 正在检查事务
---> 软件包 mysql-community-server.x86_64.0.8.0.34-1.el7 将被 安装
--> 正在处理依赖关系 mysql-community-common(x86-64) = 8.0.34-1.el7，它被软件包 mysql-community-server-8.0.34-1.el7.x86_64 需要
--> 正在处理依赖关系 mysql-community-icu-data-files = 8.0.34-1.el7，它被软件包 mysql-community-server-8.0.34-1.el7.x86_64 需要
--> 正在处理依赖关系 mysql-community-client(x86-64) >= 8.0.11，它被软件包 mysql-community-server-8.0.34-1.el7.x86_64 需要
--> 正在检查事务
---> 软件包 mysql-community-client.x86_64.0.8.0.34-1.el7 将被 安装
--> 正在处理依赖关系 mysql-community-client-plugins = 8.0.34-1.el7，它被软件包 mysql-community-client-8.0.34-1.el7.x86_64 需要
--> 正在处理依赖关系 mysql-community-libs(x86-64) >= 8.0.11，它被软件包 mysql-community-client-8.0.34-1.el7.x86_64 需要
---> 软件包 mysql-community-common.x86_64.0.8.0.34-1.el7 将被 安装
---> 软件包 mysql-community-icu-data-files.x86_64.0.8.0.34-1.el7 将被 安装
--> 正在检查事务
---> 软件包 mariadb-libs.x86_64.1.5.5.68-1.el7 将被 取代
--> 正在处理依赖关系 libmysqlclient.so.18()(64bit)，它被软件包 2:postfix-2.10.1-9.el7.x86_64 需要
--> 正在处理依赖关系 libmysqlclient.so.18(libmysqlclient_18)(64bit)，它被软件包 2:postfix-2.10.1-9.el7.x86_64 需要
---> 软件包 mysql-community-client-plugins.x86_64.0.8.0.34-1.el7 将被 安装
---> 软件包 mysql-community-libs.x86_64.0.8.0.34-1.el7 将被 舍弃
--> 正在检查事务
---> 软件包 mysql-community-libs-compat.x86_64.0.8.0.34-1.el7 将被 舍弃
--> 解决依赖关系完成

依赖关系解决

===================================================================================================================================================================================
 Package                                                 架构                            版本                                     源                                          大小
===================================================================================================================================================================================
正在安装:
 mysql-community-libs                                    x86_64                          8.0.34-1.el7                             mysql80-community                          1.5 M
      替换  mariadb-libs.x86_64 1:5.5.68-1.el7
 mysql-community-libs-compat                             x86_64                          8.0.34-1.el7                             mysql80-community                          669 k
      替换  mariadb-libs.x86_64 1:5.5.68-1.el7
 mysql-community-server                                  x86_64                          8.0.34-1.el7                             mysql80-community                           64 M
为依赖而安装:
 mysql-community-client                                  x86_64                          8.0.34-1.el7                             mysql80-community                           16 M
 mysql-community-client-plugins                          x86_64                          8.0.34-1.el7                             mysql80-community                          3.6 M
 mysql-community-common                                  x86_64                          8.0.34-1.el7                             mysql80-community                          666 k
 mysql-community-icu-data-files                          x86_64                          8.0.34-1.el7                             mysql80-community                          2.2 M

事务概要
===================================================================================================================================================================================
安装  3 软件包 (+4 依赖软件包)

总计：89 M
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : mysql-community-common-8.0.34-1.el7.x86_64                                                                                                                     1/8 
  正在安装    : mysql-community-client-plugins-8.0.34-1.el7.x86_64                                                                                                             2/8 
  正在安装    : mysql-community-libs-8.0.34-1.el7.x86_64                                                                                                                       3/8 
  正在安装    : mysql-community-client-8.0.34-1.el7.x86_64                                                                                                                     4/8 
  正在安装    : mysql-community-icu-data-files-8.0.34-1.el7.x86_64                                                                                                             5/8 
  正在安装    : mysql-community-server-8.0.34-1.el7.x86_64                                                                                                                     6/8 
  正在安装    : mysql-community-libs-compat-8.0.34-1.el7.x86_64                                                                                                                7/8 
  正在删除    : 1:mariadb-libs-5.5.68-1.el7.x86_64                                                                                                                             8/8 
  验证中      : mysql-community-server-8.0.34-1.el7.x86_64                                                                                                                     1/8 
  验证中      : mysql-community-client-plugins-8.0.34-1.el7.x86_64                                                                                                             2/8 
  验证中      : mysql-community-icu-data-files-8.0.34-1.el7.x86_64                                                                                                             3/8 
  验证中      : mysql-community-common-8.0.34-1.el7.x86_64                                                                                                                     4/8 
  验证中      : mysql-community-libs-8.0.34-1.el7.x86_64                                                                                                                       5/8 
  验证中      : mysql-community-libs-compat-8.0.34-1.el7.x86_64                                                                                                                6/8 
  验证中      : mysql-community-client-8.0.34-1.el7.x86_64                                                                                                                     7/8 
  验证中      : 1:mariadb-libs-5.5.68-1.el7.x86_64                                                                                                                             8/8 

已安装:
  mysql-community-libs.x86_64 0:8.0.34-1.el7              mysql-community-libs-compat.x86_64 0:8.0.34-1.el7              mysql-community-server.x86_64 0:8.0.34-1.el7             

作为依赖被安装:
  mysql-community-client.x86_64 0:8.0.34-1.el7                  mysql-community-client-plugins.x86_64 0:8.0.34-1.el7          mysql-community-common.x86_64 0:8.0.34-1.el7         
  mysql-community-icu-data-files.x86_64 0:8.0.34-1.el7         

替代:
  mariadb-libs.x86_64 1:5.5.68-1.el7                                                                                                                                               

完毕！
```

## 更改临时密码

```bash
[root@localhost rpm-gpg]# grep "A temporary password" /var/log/mysqld.log 
2023-08-11T02:16:15.571735Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 9.fyp9dZ6lYr

[root@localhost /]# mysql_secure_installation 

Securing the MySQL server deployment.

Enter password for user root: 

The existing password for the user account root has expired. Please set a new password.

New password: 

Re-enter new password: 
 ... Failed! Error: Your password does not satisfy the current policy requirements

New password: 

Re-enter new password: 
The 'validate_password' component is installed on the server.
The subsequent steps will run with the existing configuration
of the component.
Using existing password for root.

Estimated strength of the password: 100 
Change the password for root ? ((Press y|Y for Yes, any other key for No) : y

New password: 

Re-enter new password: 

Estimated strength of the password: 100 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : n

 ... skipping.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : n

 ... skipping.
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done! 

```

## 登录

```bash
[root@localhost /]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 17
Server version: 8.0.34 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

## navicat连接虚拟机MySQL

```bash
mysql> use mysql
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select user from user;
+------------------+
| user             |
+------------------+
| mysql.infoschema |
| mysql.session    |
| mysql.sys        |
| root             |
+------------------+

```

```bash
mysql> select user,host from user;
+------------------+-----------+
| user             | host      |
+------------------+-----------+
| mysql.infoschema | localhost |
| mysql.session    | localhost |
| mysql.sys        | localhost |
| root             | localhost |
+------------------+-----------+
4 rows in set (0.03 sec)

mysql> grant all privileges on *.* to 'root'@'192.168.190.130' identified by '123456' with grant option;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'identified by '123456' with grant option' at line 1
mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'192.168.190.130' IDENTIFIED BY '123456' WITH GRANT OPTION;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'IDENTIFIED BY '123456' WITH GRANT OPTION' at line 1
mysql> grant all privileges on *.* to 'root'@'192.168.190.130' identified by 'zhuzhenyang123' with grant option;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'identified by 'zhuzhenyang123' with grant option' at line 1
mysql> grant all privileges on *.* to 'root'@'192.168.190.130' identified by 'Zhuzhenyang123!' with grant option;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'identified by 'Zhuzhenyang123!' with grant option' at line 1
mysql> grant all privileges on *.* to 'root'@'192.168.190.130' with grant option identified by '123456';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'identified by '123456'' at line 1
mysql> create user 'root'@'192.168.190.130' identified by '123456';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> create user 'root'@'192.168.190.130' identified by 'zhuzhenyang123';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> create user 'root'@'192.168.190.130' identified by 'Zhuzhenyang123!';
Query OK, 0 rows affected (0.14 sec)

mysql> grant all privileges on *.* to 'root'@'192.168.190.130' with grant option;
Query OK, 0 rows affected (0.06 sec)

mysql> flush privileges
    -> ;
Query OK, 0 rows affected (0.06 sec)

mysql> select user,host from user;
+------------------+-----------------+
| user             | host            |
+------------------+-----------------+
| root             | 192.168.190.130 |
| mysql.infoschema | localhost       |
| mysql.session    | localhost       |
| mysql.sys        | localhost       |
| root             | localhost       |
+------------------+-----------------+
5 rows in set (0.01 sec)

```