# SQL编程练习

## 数据类型
### `%TYPE`类型
当声明一个变量名称后便声明其具体的数据类型，
而使用%TYPE类型可以在声明变量名称后再指定一个字段，声明该变量的数据类型与指定字段的数据类型一致。

```sql
DECLARE
 --声明变量INAME，其类型与EMP表中的ENAME字段一致
IEMPNO EMP.EMPNO%TYPE;
--定义一个变量承载异常
ERR   VARCHAR2(200);
BEGIN
  SELECT EMPNO INTO IEMPNO FROM EMP WHERE ENAME='SMITH';
/*  SELECT ENAME INTO INAME FROM EMP;*/   --测试异常
  DBMS_OUTPUT.PUT_LINE(IEMPNO);
  EXCEPTION
    WHEN OTHERS THEN
      ERR:=SQLERRM;
      DBMS_OUTPUT.PUT_LINE(ERR);
END;
```

### RECORD类型
RECORD类型又称记录类型，该类型的变量可以存储由多个列组成的一行数据。
因为变量中的每一列需要有明确的数据类型，且每个变量中的列成员是不固定的，所以在声明记录类型的变量之前，
需要先定义一个记录类型，再声明变量 。
--语法
DECLARE
  TYPE RECOED_TYPE IS RECORD
  (
  VAR_NUMBER1 TYPE ,
  V_NUMBER2 TYPE,
  V_NUMBER3 TYPE,
  .
  .
  V_NUMBERN TYPE
  );
  VAR_NAME RECORD_TYPE;  --将该记录给变量，作为变量的类型
BEGIN
  SELECT ENAME,SAL,HIREDATE INTO VAR_NAME FROM EMP WHERE DEPTNO=7369;
  DBMS_OUTPUT.PUT_LINE(VAR_NAME);
END;
--SELECT * FROM DEPT;
DECLARE
--声明一个记录类型
  TYPE DEPT_TYPE IS RECORD
  (
    DE DEPT.DEPTNO%TYPE,
    DN DEPT.DNAME%TYPE,
    LO DEPT.LOC%TYPE
   );
   --用该记录类型来声明一个变量
   VAR_DEPT DEPT_TYPE;
BEGIN
  --查询DEPT表，并将查询数据放入变量VAR_DEPT中
  SELECT DEPTNO,DNAME,LOC INTO VAR_DEPT FROM DEPT WHERE DEPTNO=10;
  --将变量VAR_DEPT打印(打印时需要表明要打印的字段，字段间用||连接)
  DBMS_OUTPUT.PUT_LINE(VAR_DEPT.DE||' '||VAR_DEPT.DN||' '||VAR_DEPT.LO);
END;
### %ROWTYPE类型
`%ROWTYPE`类型是结合了%TYPE类型与RECORD类型的特点，%TYPE类型是效仿一个字段设置数据类型，
RECORD类型是设置一行变量的数据类型，而%ROWTYPE是效仿一行字段设置变量的数据类型。
%ROWTYPE类型与RECORD类型一样本身没有确切的列数和属性，但是%ROWTYPE类型不需要单独定义，直接使用即可。
--例子（DEPT表）
DECLARE
--%ROWTYPE类型不需要声明定义，可以直接使用
VAR_DEPT DEPT%ROWTYPE;
BEGIN
  --查询数据，并将查询的数据放入变量VAR_DEPT
  SELECT DEPTNO,DNAME,LOC INTO VAR_DEPT FROM DEPT WHERE DEPTNO=10;
  --打印数据
  DBMS_OUTPUT.PUT_LINE(VAR_DEPT.DEPTNO||' '||VAR_DEPT.DNAME||' '||VAR_DEPT.LOC);
END;


人机交互获取课程名，编写代码打印此课程的最高分以及对应的学生姓名，若并列第一也只显示一个人。

```sql
set serveroutput on
declare
    nm student.sname%type;
    cn course.cname%type:='&课程名';
    rcd mark%rowtype;
    cno mark.cid%type;
begin
    select cid into cno from course where cname= cn;
    select * into rcd from ( 
        select * from mark where cid =cno order by cmark desc
    )where rownum = 1;
    select sname into nm from student where sid  =rcd.sid;
    dbms_output.put_line(nm||'取得'||cn||'的最高分'||rcd.cmark);
end;
```

打印学生姓名年龄
```sql
set serveroutput on 
declare 
  rcd student%rowtype;
begin 
  for rcd in(select * from student) loop
    dbms_output.put_line(rcd.sname||'今年'||rcd.sage||'岁');
  end loop;
end;
```

人机交互获取课程名，编写代码打印此课程前五名学生的姓名成绩

```sql
set serveroutput on
declare
    -- 学生姓名
    nm student.sname%type;
    rcd mark%rowtype;
begin
    for rcd in(select * from (
        select * from mark where cid =(
            select cid from course where cname='&课程名'
            )order by cmark desc 
        )where rownum <=5
    ) loop
    select sname into nm from student where sid  =rcd.sid;
    dbms_output.put_line(nm||':'||rcd.cmark);
    end loop;
end;
```


## 异常
```sql
set serveroutput on
declare
  nm student.sname%type:='&学生姓名';
  rcd student%rowtype;
begin
  select *into rcd from student where sname=nm;
  dbms_output. put_line(nm||'的学号是'||rcd.sid);
exception
  when no_data_found then
    dbms_output.put_line('查无此人');
end;
```

```sql
set serveroutput on 
declare
    nm student.sname%type:='&学生姓名';
    sx student.ssex%type:='&性别';
    sxErr exception;
begin
    if sx not in ('男','女') then
      raise sxErr;
    end if;
    insert into student(sid,sname,ssex) values (1098,nm,sx);
    commit;
    dbms_output.put_line('成功插入一条记录');
exception
    when no_data_found then
      dbms_output.put_line('查无此人');
    when sxErr then
      dbms_output.put_line('未知性别，请输入合法的性别');
    when others then
      dbms_output.put_line('未知异常');
end;
```

```sql
set serveroutput on
declare
  nm  student.sname%type:='&学生姓名';sno student.sid%type;
  cn  course.cname%type:='&课程名称'; cno course.cid%type;
  score mark.cmark%type:='&课程成绩';
  scErr  exception;cnt int:=0;
begin
  select sid into sno from student where sname=nm;
  cnt:=cnt+1;
  select cid into cno from course where cname=cn;
  if score not between 0 and 100 then
    raise scErr;
  end if;
  insert into mark(sid,cid,cmark) values(sno,cno,score);
  commit;
  dbms_output.put_line('成功插入选课记录');
exception
  when no_data_found then
    if cnt=0 then
      dbms_output.put_line('没有姓名为'||nm||'的学生');
    else
      dbms_output.put_line('没有课程名为'||cn||'的课程');
    end if;
  when scErr then
    dbms_output.put_line('非法成绩，请输入正确的成绩');
  when others then
    dbms_output.put_line('未知异常');
end;

```

编写存储过程，输入班级，课程名，返回这个班级这门课的最高分、最低分
```sql
create or replace procedure getScore(
  scls studnet.sclass%type,cn course.cnam%type,
  mx out number,mn out number
) as
  cno course,cid%type;
begin
  select cid into cno from course where cname=cn;
  select max(cmark),min(cmark) into mx,mn
  from mark where sid in(select sid from student where sclass=scls) and
  cid=cno;
end;
```
