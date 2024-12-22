# Ubuntu安装mysql数据库


```shell
sudo apt-get install mysql-server mysql-client
```

更改root密码
```shell
alter user 'root'@'localhost' identified with mysql_native_password by '12345678';
update user set plugin="mysql_native_password";
flush privileges;
exit
```

重启mysql
```shell
systemctl restart mysql
```
