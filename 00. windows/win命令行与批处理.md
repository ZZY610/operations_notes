# win命令行与批处理
## DOS、CMD和PowerShell的关系
* **DOS**（`Disk Operating System`）：Microsoft在1981年推出的，早期个人电脑上广泛使用的操作系统。
    * 用户可以通过键入命令到DOS的命令行界面来执行操作。DOS的命令行语法与后来的Windows操作系统保持了很大的相似性。

* **CMD**（`Command Prompt`）：Windows中的命令行工具，用于在Windows中执行DOS命令和批处理文件。CMD支持的命令与DOS相似，但在Windows操作系统中提供了更多的功能。

* **PowerShell**：微软于2006年发布的一种新型的命令行工具和脚本语言，它在Windows Server 2008和Windows Vista中首次引入，可以用于自动化系统管理任务和批处理任务。cmd可认为是powershell的子集。

DOS是一个早期的操作系统，CMD是Windows中的命令行工具，而PowerShell是一个新型的命令行工具和脚本语言，它在Windows操作系统中提供了更多的功能和更加强大的脚本编写能力。
## 命令行

::: tip 斜杠与反斜杠
Windows使用反斜杠 `\` 作为文件路径分隔符，Linux使用斜杠 `/`。

* 在早期的操作系统中，斜杠字符（/）通常用作命令行选项或开关。因此，Microsoft决定使用反斜杠字符（\）作为文件路径的分隔符，以避免与命令行选项混淆。
* 而在Linux和其他Unix系统中，斜杠字符一直被用作文件路径分隔符，也没有命令行选项和开关的概念，因此保留了斜杠字符。

另外，反斜杠字符也被广泛用于转义字符序列，如换行符（\n）、制表符（\t）等，在Windows中，文件路径中如果需要使用反斜杠字符，需要使用两个反斜杠字符（\）表示一个反斜杠字符，以避免被误认为是转义字符。

:::

### 说明

- 不区分大小写
- 按上下键可以查看历史命令
- Tab键可以自动补全
- 使用`/?`或`/help`可以查看命令用法，用法中的`[]`代表可选
- 命令基本格式`命令 参数列表 `

### 基本命令

| 命令    | 功能 | 举例          |
| ------- | ---- | ------------- |
| `echo`  | 显示 |               |
| `pause` | 暂停 | `pause > nul` |
| `cls`   | 清屏 |               |
| `exit`  | 退出 |               |

### 常用命令

| 命令       | 功能          | 举例                 |
| ---------- | ------------- | -------------------- |
| `dir` | 查看当前目录内容 | `dir [路径]` |
| `type`     | 显示文件内容  | `type 1.txt`         |
| `find`     | 查找字符串    | `find "abc" 2.txt`   |
| `fc`       | 比较文件内容  | `fc 1.txt 2.txt`     |
| `date`     | 显示/修改日期 | `date /T`            |
| `time`     | 显示/修改时间 | `time /T`            |
| `ping`     | 网络连接测试  | `ping www.baidu.com` |
| `ipconfig` | TCP/IP信息    |                      |
| `set /a`   | 计算表达式    | `set /a 6%4`         |
| `shutdown` | 关机          | `shutdown /s /t 60`  |

### 多命令

```bash
&  顺序执行多条命令，不管命令是否执行成功
&& 顺序执行多条命令，前面的命令错误则不执行后面的命令
|| 顺序执行多条命令，碰到执行正确的命令后将不执行后面的命令

abc & dir & bcd
abc && dir && bcd
abc || dir || bcd
```

### 重定向

```bash
>  输出重定向
>> 追加重定向
<  输入重定向

date /t > 1.txt
time /t >> 1.txt
find "/" < 1.txt
```

### 管道

```bash
| 前一个命令的输出作为后一个命令的输入

dir | find "txt"
```

## 文件操作命令

**路径分隔符**：`\`

**当前目录**：`.`

**上一级目录**：`..`

| 命令            | 功能               | 举例                           |
| --------------- | ------------------ | ------------------------------ |
| `dir`           | 显示文件夹内容     | `dir /A`                       |
| `tree`          | 显示文件夹树形结构 | `tree /F`                      |
| `盘符:`         | 进入指定磁盘       | `c:` `d:`                      |
| `cd`            | 切换目录           | `cd test` `cd ..`              |
| `mkdir`或`md`   | 创建目录           | `mkdir test`                   |
| `rmdir`或`rd`   | 删除目录           | `rmdir test`                   |
| `del`           | 删除文件           | `del 1.txt` `del /q/a/f *.txt` |
| `rename`或`ren` | 重命名             | `rename a.txt b.txt`           |
| `copy`          | 复制文件           | `copy a\1.txt b\1.txt`         |
| `move`          | 移动文件/重命名    | `move a\1.txt 2.txt`           |

#  批处理

## 基本结构

```bash
@echo off		# 关闭回显
......
pause			# 暂停。或 pause > nul
```

## 运行方式

- 双击运行
- 在cmd中运行（可以传参）

## 变量

```bash
# 设置变量
set 变量名=变量值
set /a 变量名=表达式

# 取消变量
set 变量名=

# 打印变量
set 变量名

# 列出变量
set

# 引用变量
%变量名%

# 输入变量值
set /p 变量名=提示文字
```

### 预定义的变量

| 变量名             | 值                                |
| ------------------ | --------------------------------- |
| `%cd%`             | 当前目录                          |
| `%date%`           | 当前日期                          |
| `%time%`           | 当前时间                          |
| `%random%`         | 0~32767之间的随机数               |
| `%path%`           | 环境变量                          |
| `%errorlevel%`     | 上一个命令的返回值，0表示执行成功 |
| `%0`、`%1`、`%2`…… | 参数。`%0`表示命令名              |

## 调用

```bash
call 可执行文件 [参数列表]
```

## 条件

```bash
if [not] 条件 (
	语句块
) else (
	语句块
)

# 条件：
errorlevel number	# 上一个程序的返回值大于等于number则为true
string1==string2	# 两个字符串匹配则为true
exist filename		# 文件名存在则为true
[/I] string1 compare-op string2		# 比较数值或字符串
	# /I	忽略大小写
	# compare-op包括：
	#	equ	等于
	# 	neq	不等于
	#	lss	小于
	#	leq	小于等于
	#	gtr	大于
	#	geq	大于等于
defined variable	# 如果定义了该变量则为true
```

## 跳转

```bash
goto label		# 跳转到标签所在行
:label			# 标签以冒号开头
```

## 循环

**基本语法**

```bash
for %%i in (set) do (
	语句块
)

# 循环变量用 %% 指定，只能是单字母
# set表示文件集合（也可以是空文件），元素之间以逗号或空格分隔，支持通配符

for %%i in (a,b,c) do (echo %%i)
for %%i in (*.*) do (echo %%i)
```

**扩展语法**

- 只遍历文件夹

    ```bash
    for /d %%i in (set) do
    ```

- 遍历指定文件夹下的所有文件，不指定则表示当前目录。如果set为`.`表示枚举目录树

    ```bash
    for /r [[drive:]path] %%i in (set) do
    
    for /r %%i in (*) do (echo %%i)
    for /r d:\up %%i in (.) do (echo %%i)
    ```

- 遍历数字序列

    ```bash
    for /l %%i in (start,step,end) do
    ```

- 遍历文件内容

    ```bash
    for /f ["options"] %%i in (set) do
    ```

## 通配符

```bash
?		# 单个字符
*		# 任意字符
```

## 例子

按顺序为文件夹中的文件命名。

[批处理中for循环的自增自减问题_Dandelion_gong的博客-CSDN博客_bat for 自增](https://blog.csdn.net/Dandelion_gong/article/details/78067501)

```bash
@echo off
setlocal enabledelayedexpansion
set cnt=0
for %%i in (%1) do (
    rename %%i !cnt!.txt
    set /a cnt=!cnt! + 1
)
pause
```

