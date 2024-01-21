# MySQL存储过程
MySQL 和 Oracle 在创建存储过程的语法上有一些区别。以下是两种数据库系统中创建存储过程的基本语法比较：

### MySQL 存储过程创建语法：

```sql
DELIMITER //

CREATE PROCEDURE procedure_name(parameters)
BEGIN
    -- 存储过程主体
END //

DELIMITER ;
```

在 MySQL 中，使用 `DELIMITER` 来定义语句的结束符，因为存储过程主体中可能包含分号。`CREATE PROCEDURE` 后面是存储过程的名字和参数列表，然后是 `BEGIN...END` 包裹的存储过程主体。

### Oracle 存储过程创建语法：

```sql
CREATE OR REPLACE PROCEDURE procedure_name(parameters)
IS
    -- 存储过程主体
BEGIN
    -- 存储过程主体
END procedure_name;
/
```

在 Oracle 中，使用 `IS` 和 `BEGIN...END` 来定义存储过程的主体，而且 Oracle 中一般使用 `CREATE OR REPLACE PROCEDURE` 来创建存储过程。语句的结束符可以是分号，也可以是斜杠 `/`。

需要注意的是，MySQL 和 Oracle 存储过程的语法差异可能还涉及到存储过程的特性、变量声明和异常处理等方面，具体语法细节还需要根据具体的需求和数据库版本进行调整。