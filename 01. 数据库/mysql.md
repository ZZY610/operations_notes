# mysql
mysql的分页查询：
select语句 limit 上页最后一条，每页条数

主键自增长：
在建表时，主键字段设置为Integer，再给一个auto_increment属性。
在insert语句中，该字段设置为NULL即可。

指定编码default charset="utf8"显示中文
```sql

```
mysql的日期和字符串的转换:
①字符串转日期----无需任何函数,只需要按照规范写入日期字符串即可
②日期转字符串----DATE_FORMAT(日期字段,%Y-%m-%d %H:%i:%s')

```



```

1. MD5加密：DigestUtils.md5Hex
2. SHA256加密：DigestUtils.sha256Hex

注册时明文加密存库 登录时将输入的密码加密与库中密文比对