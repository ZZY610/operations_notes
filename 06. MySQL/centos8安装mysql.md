# centos8安装mysql

## 1. 下载与安装 

### 下载MySQL 8.0社区版本的存储库配置文件

```bash
wget https://repo.mysql.com/mysql80-community-release-el8-1.noarch.rpm
yum localinstall mysql80-community-release-el8-1.noarch.rpm
```

1. `wget https://repo.mysql.com/mysql80-community-release-el8-1.noarch.rpm`
   - `wget` 是一个用于从Web下载文件的命令行工具。
   - `https://repo.mysql.com/mysql80-community-release-el8-1.noarch.rpm` 是一个URL，指向MySQL社区版本的仓库中的一个特定RPM文件（RPM是一种Linux软件包格式）。
   - 这个命令的作用是从指定的URL下载名为 `mysql80-community-release-el8-1.noarch.rpm` 的文件到当前目录。

2. `yum localinstall mysql80-community-release-el8-1.noarch.rpm`
   - `yum` 是一个在许多Linux发行版中用于包管理的工具，它可以帮助你安装、升级和管理软件包。
   - `localinstall` 是`yum`命令的一个选项，用于本地安装一个本地RPM包。
   - `mysql80-community-release-el8-1.noarch.rpm` 是我们在第一步中下载的文件的名称。
   - 这个命令的作用是使用`yum`将 `mysql80-community-release-el8-1.noarch.rpm` 安装到系统中。这个特定的RPM包是用于配置系统，以便能够从MySQL社区版本的仓库中安装MySQL 8.0。

这两个命令的组合用于下载MySQL 8.0社区版本的仓库配置文件（`mysql80-community-release-el8-1.noarch.rpm`），然后使用`yum`安装这个配置文件，以便用`yum`来安装MySQL 8.0及其相关组件。安装这个配置文件后，你可以使用`yum`命令轻松安装和管理MySQL 8.0。

```bash
[root@hecs-65539 ~]# wget https://repo.mysql.com/mysql80-community-release-el8-1.noarch.rpm
--2023-09-07 21:28:37--  https://repo.mysql.com/mysql80-community-release-el8-1.noarch.rpm
Resolving repo.mysql.com (repo.mysql.com)... 23.51.5.5, 2600:1417:4400:3bd::1d68, 2600:1417:4400:38d::1d68
Connecting to repo.mysql.com (repo.mysql.com)|23.51.5.5|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 30388 (30K) [application/x-redhat-package-manager]
Saving to: ‘mysql80-community-release-el8-1.noarch.rpm’

mysql80-community-release-el8-1.no 100%[================================================================>]  29.68K   176KB/s    in 0.2s    

2023-09-07 21:28:40 (176 KB/s) - ‘mysql80-community-release-el8-1.noarch.rpm’ saved [30388/30388]

[root@hecs-65539 ~]# yum localinstall mysql80-community-release-el8-1.noarch.rpm
Last metadata expiration check: 4:05:24 ago on Thu 07 Sep 2023 05:23:35 PM CST.
Dependencies resolved.
============================================================================================================================================
 Package                                       Architecture               Version                    Repository                        Size
============================================================================================================================================
Installing:
 mysql80-community-release                     noarch                     el8-1                      @commandline                      30 k

Transaction Summary
============================================================================================================================================
Install  1 Package

Total size: 30 k
Installed size: 29 k
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                    1/1 
  Installing       : mysql80-community-release-el8-1.noarch                                                                             1/1 
  Verifying        : mysql80-community-release-el8-1.noarch                                                                             1/1 

Installed:
  mysql80-community-release-el8-1.noarch                                                                                                    

Complete!
```

### 安装mysql服务器
执行了这两条命令后，再执行 `yum install mysql-server` 会有以下效果：

1. **MySQL存储库配置：** 前两条命令分别用于下载名为 `mysql80-community-release-el8-1.noarch.rpm` 的MySQL社区存储库配置文件，并将其安装到系统中。这意味着你的系统现在知道从哪里获取MySQL软件包。

2. **MySQL服务器安装：** 当你执行 `yum install mysql-server` 时，它会尝试从已配置的MySQL社区存储库中安装MySQL服务器。由于之前已经配置了存储库，该命令应该能够找到并安装MySQL服务器的相应版本。

总之，执行了这两条命令后，再执行 `yum install mysql-server` 应该会成功安装MySQL服务器，因为你已经配置了正确的存储库以获取MySQL软件包。这种配置和安装过程有助于确保系统上的MySQL版本是最新的，并且可以通过`yum`来轻松管理。
```bash
yum install mysql-server
```

```bash
[root@hecs-65539 ~]# yum install mysql-server
Last metadata expiration check: 0:02:21 ago on Thu 07 Sep 2023 09:32:42 PM CST.
Dependencies resolved.
============================================================================================================================================
 Package                                  Architecture       Version                                            Repository             Size
============================================================================================================================================
Installing:
 mysql-server                             x86_64             8.0.26-1.module_el8.4.0+915+de215114               AppStream              25 M
Installing dependencies:
 checkpolicy                              x86_64             2.9-1.el8                                          BaseOS                348 k
 libaio                                   x86_64             0.3.112-1.el8                                      BaseOS                 33 k
 mariadb-connector-c-config               noarch             3.1.11-2.el8_3                                     AppStream              15 k
 mecab                                    x86_64             0.996-1.module_el8.4.0+589+11e12751.9              AppStream             393 k
 mysql                                    x86_64             8.0.26-1.module_el8.4.0+915+de215114               AppStream              12 M
 mysql-common                             x86_64             8.0.26-1.module_el8.4.0+915+de215114               AppStream             134 k
 mysql-errmsg                             x86_64             8.0.26-1.module_el8.4.0+915+de215114               AppStream             598 k
 policycoreutils-python-utils             noarch             2.9-16.el8                                         BaseOS                252 k
 protobuf-lite                            x86_64             3.5.0-13.el8                                       AppStream             149 k
 python3-audit                            x86_64             3.0-0.17.20191104git1c2f876.el8                    BaseOS                 86 k
 python3-libsemanage                      x86_64             2.9-6.el8                                          BaseOS                127 k
 python3-policycoreutils                  noarch             2.9-16.el8                                         BaseOS                2.2 M
 python3-setools                          x86_64             4.3.0-2.el8                                        BaseOS                626 k

Transaction Summary
============================================================================================================================================
Install  14 Packages

Total download size: 42 M
Installed size: 206 M
Is this ok [y/N]: y
Downloading Packages:
(1/14): mariadb-connector-c-config-3.1.11-2.el8_3.noarch.rpm                                                221 kB/s |  15 kB     00:00    
(2/14): mecab-0.996-1.module_el8.4.0+589+11e12751.9.x86_64.rpm                                              4.8 MB/s | 393 kB     00:00    
(3/14): mysql-common-8.0.26-1.module_el8.4.0+915+de215114.x86_64.rpm                                        4.4 MB/s | 134 kB     00:00    
(4/14): mysql-errmsg-8.0.26-1.module_el8.4.0+915+de215114.x86_64.rpm                                         19 MB/s | 598 kB     00:00    
(5/14): protobuf-lite-3.5.0-13.el8.x86_64.rpm                                                               6.9 MB/s | 149 kB     00:00    
(6/14): checkpolicy-2.9-1.el8.x86_64.rpm                                                                    8.5 MB/s | 348 kB     00:00    
(7/14): libaio-0.3.112-1.el8.x86_64.rpm                                                                     1.0 MB/s |  33 kB     00:00    
(8/14): policycoreutils-python-utils-2.9-16.el8.noarch.rpm                                                  4.6 MB/s | 252 kB     00:00    
(9/14): mysql-8.0.26-1.module_el8.4.0+915+de215114.x86_64.rpm                                                34 MB/s |  12 MB     00:00    
(10/14): python3-audit-3.0-0.17.20191104git1c2f876.el8.x86_64.rpm                                           929 kB/s |  86 kB     00:00    
(11/14): python3-libsemanage-2.9-6.el8.x86_64.rpm                                                           3.8 MB/s | 127 kB     00:00    
(12/14): python3-policycoreutils-2.9-16.el8.noarch.rpm                                                       33 MB/s | 2.2 MB     00:00    
(13/14): mysql-server-8.0.26-1.module_el8.4.0+915+de215114.x86_64.rpm                                        48 MB/s |  25 MB     00:00    
(14/14): python3-setools-4.3.0-2.el8.x86_64.rpm                                                             2.7 MB/s | 626 kB     00:00    
--------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                        67 MB/s |  42 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                    1/1 
  Installing       : mariadb-connector-c-config-3.1.11-2.el8_3.noarch                                                                  1/14 
  Installing       : mysql-common-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                          2/14 
  Installing       : mysql-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                                 3/14 
  Installing       : mysql-errmsg-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                          4/14 
  Installing       : python3-setools-4.3.0-2.el8.x86_64                                                                                5/14 
  Installing       : python3-libsemanage-2.9-6.el8.x86_64                                                                              6/14 
  Installing       : python3-audit-3.0-0.17.20191104git1c2f876.el8.x86_64                                                              7/14 
  Installing       : libaio-0.3.112-1.el8.x86_64                                                                                       8/14 
  Installing       : checkpolicy-2.9-1.el8.x86_64                                                                                      9/14 
  Installing       : python3-policycoreutils-2.9-16.el8.noarch                                                                        10/14 
  Installing       : policycoreutils-python-utils-2.9-16.el8.noarch                                                                   11/14 
  Installing       : protobuf-lite-3.5.0-13.el8.x86_64                                                                                12/14 
  Installing       : mecab-0.996-1.module_el8.4.0+589+11e12751.9.x86_64                                                               13/14 
  Running scriptlet: mecab-0.996-1.module_el8.4.0+589+11e12751.9.x86_64                                                               13/14 
  Running scriptlet: mysql-server-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                         14/14 
  Installing       : mysql-server-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                         14/14 
  Running scriptlet: mysql-server-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                         14/14 
ValueError: File context for /var/log/mysql(/.*)? already defined

  Verifying        : mariadb-connector-c-config-3.1.11-2.el8_3.noarch                                                                  1/14 
  Verifying        : mecab-0.996-1.module_el8.4.0+589+11e12751.9.x86_64                                                                2/14 
  Verifying        : mysql-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                                 3/14 
  Verifying        : mysql-common-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                          4/14 
  Verifying        : mysql-errmsg-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                          5/14 
  Verifying        : mysql-server-8.0.26-1.module_el8.4.0+915+de215114.x86_64                                                          6/14 
  Verifying        : protobuf-lite-3.5.0-13.el8.x86_64                                                                                 7/14 
  Verifying        : checkpolicy-2.9-1.el8.x86_64                                                                                      8/14 
  Verifying        : libaio-0.3.112-1.el8.x86_64                                                                                       9/14 
  Verifying        : policycoreutils-python-utils-2.9-16.el8.noarch                                                                   10/14 
  Verifying        : python3-audit-3.0-0.17.20191104git1c2f876.el8.x86_64                                                             11/14 
  Verifying        : python3-libsemanage-2.9-6.el8.x86_64                                                                             12/14 
  Verifying        : python3-policycoreutils-2.9-16.el8.noarch                                                                        13/14 
  Verifying        : python3-setools-4.3.0-2.el8.x86_64                                                                               14/14 

Installed:
  checkpolicy-2.9-1.el8.x86_64                                         libaio-0.3.112-1.el8.x86_64                                         
  mariadb-connector-c-config-3.1.11-2.el8_3.noarch                     mecab-0.996-1.module_el8.4.0+589+11e12751.9.x86_64                  
  mysql-8.0.26-1.module_el8.4.0+915+de215114.x86_64                    mysql-common-8.0.26-1.module_el8.4.0+915+de215114.x86_64            
  mysql-errmsg-8.0.26-1.module_el8.4.0+915+de215114.x86_64             mysql-server-8.0.26-1.module_el8.4.0+915+de215114.x86_64            
  policycoreutils-python-utils-2.9-16.el8.noarch                       protobuf-lite-3.5.0-13.el8.x86_64                                   
  python3-audit-3.0-0.17.20191104git1c2f876.el8.x86_64                 python3-libsemanage-2.9-6.el8.x86_64                                
  python3-policycoreutils-2.9-16.el8.noarch                            python3-setools-4.3.0-2.el8.x86_64                                  

Complete!

```

## 2. MySQL配置

### 2.1 启动mysql
执行命令
```bash
systemctl start mysqld
#或
systemctl start mysqld.service
```
查看mysql运行状态

```bash
systemctl status mysqld
```

### 2.2 设置密码

首先查看mysql的初始密码。

```bash
grep "password" /var/log/mysql/mysqld.log
```

如果出现以下信息：

```bash
[Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
```

说明mysql root用户没有设置初始密码。需要设置密码。

```bash
alter user 'root'@'localhost' identified by 'new_passwd';
```

```bash
# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.26 Source distribution

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> alter user 'root'@'localhost' identified by 'new_passwd';
Query OK, 0 rows affected (0.01 sec)
```
