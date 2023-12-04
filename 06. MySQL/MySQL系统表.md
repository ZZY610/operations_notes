# MySQL系统表

- 查看当前用户
    ```sql
    select user();
    select current_user();
    ```

1. mysql.user：该表包含MySQL用户的信息，包括用户名、密码、权限等。
2. mysql.db：该表存储数据库的访问控制信息，包括哪些用户有哪些权限等。
3. mysql.host：该表存储MySQL服务器允许的主机名和IP地址。
4. mysql.tables_priv：该表存储用户对每个表的访问权限。
5. mysql.columns_priv：该表存储用户对每个表的列的访问权限。
6. mysql.procs_priv：该表存储用户对存储过程和函数的访问权限。
7. mysql.event：该表存储计划事件的信息，例如定期备份或清理任务。
8. mysql.func：该表存储MySQL系统函数的信息。
9. mysql.help_topic：该表存储MySQL官方文档的帮助主题。
10. mysql.plugin：该表存储MySQL插件的信息。