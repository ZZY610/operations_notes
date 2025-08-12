# centos安装Oracle
```bash
[oracle@zzy database]$ ./runInstaller -silent -responseFile /home/oracle/database/response/db_install.rsp
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 120 MB.   Actual 28069 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 5921 MB    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2023-08-15_06-22-10PM. Please wait ...[oracle@zzy database]$ [WARNING] - 未指定 My Oracle Support 用户名/电子邮件地址
此会话的日志当前已保存为: /tmp/OraInstall2023-08-15_06-22-10PM/installActions2023-08-15_06-22-10PM.log。如果要保留此日志, Oracle 建议将它从临时位置移动到更持久的位置。

```

```bash
[oracle@zzy database]$ ./runInstaller -silent -responseFile /home/oracle/database/response/db_install.rsp
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 120 MB.   Actual 28069 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 5921 MB    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2023-08-15_07-13-08PM. Please wait ...[oracle@zzy database]$ [WARNING] [INS-30011] 输入的 ADMIN 口令不符合 Oracle 建议的标准。
   原因: Oracle 建议输入的口令应该至少长为 8 个字符, 至少包含 1 个大写字符, 1 个小写字符和 1 个数字 [0-9]。
   操作: 提供符合 Oracle 建议标准的口令。
[WARNING] [INS-13014] 目标环境不满足一些可选要求。
   原因: 不满足一些可选的先决条件。有关详细信息, 请查看日志。/tmp/OraInstall2023-08-15_07-13-08PM/installActions2023-08-15_07-13-08PM.log
   操作: 从日志 /tmp/OraInstall2023-08-15_07-13-08PM/installActions2023-08-15_07-13-08PM.log 中确定失败的先决条件检查列表。然后, 从日志文件或安装手册中查找满足这些先决条件的适当配置, 并手动进行修复。
可以在以下位置找到本次安装会话的日志:
 /u01/app/inventory/logs/installActions2023-08-15_07-13-08PM.log
Oracle Database 11g 的 安装 已成功。
请查看 '/u01/app/inventory/logs/silentInstall2023-08-15_07-13-08PM.log' 以获取详细资料。

以 root 用户的身份执行以下脚本:
	1. /u01/app/inventory/orainstRoot.sh
	2. /u01/app/oracle/product/11.2.0/db_1/root.sh


Successfully Setup Software.

```

```bash
[oracle@zzy ~]$ lsnrctl start

LSNRCTL for Linux: Version 11.2.0.4.0 - Production on 16-AUG-2023 09:00:51

Copyright (c) 1991, 2013, Oracle.  All rights reserved.

Starting /u01/app/oracle/product/11.2.0/db_1/bin/tnslsnr: please wait...

TNSLSNR for Linux: Version 11.2.0.4.0 - Production
System parameter file is /u01/app/oracle/product/11.2.0/db_1/network/admin/listener.ora
Log messages written to /u01/app/oracle/diag/tnslsnr/zzy/listener/alert/log.xml
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=zzy)(PORT=1521)))

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.4.0 - Production
Start Date                16-AUG-2023 09:00:53
Uptime                    0 days 0 hr. 0 min. 0 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/11.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/zzy/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=zzy)(PORT=1521)))
The listener supports no services
The command completed successfully

```

```bash
[oracle@zzy ~]$ lsnrctl status

LSNRCTL for Linux: Version 11.2.0.4.0 - Production on 16-AUG-2023 09:03:25

Copyright (c) 1991, 2013, Oracle.  All rights reserved.

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.4.0 - Production
Start Date                16-AUG-2023 09:00:53
Uptime                    0 days 0 hr. 2 min. 32 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/11.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/zzy/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=zzy)(PORT=1521)))
The listener supports no services
The command completed successfully

```

## 出现问题
### 1. 执行Oracle安装命令后出现乱码，系统无响应。
已尝试方法：
* 更新系统组件pdksh-5.2.14-21.x86_64.rpm为pdksh-5.2.14-30.x86_64.rpm
* 重新开始配置虚拟机。
* 按照过去版本（20211214、20220307）的文档配置内核参数和用户变量。

解决方法：将Xshell7的字符集更改为GBK后查看错误日志，经查询后将安装脚本中的参数DECLINE_SECURITY_UPDATES设置为true，成功安装了Oracle数据库。

### 2. oracle监听器报错
启动监听器后发现The listener supports no services。
```bash
[root@zzy admin]# lsnrctl start

LSNRCTL for Linux: Version 11.2.0.4.0 - Production on 16-AUG-2023 14:50:46

Copyright (c) 1991, 2013, Oracle.  All rights reserved.

Starting /u01/app/oracle/product/11.2.0/db_1/bin/tnslsnr: please wait...

TNSLSNR for Linux: Version 11.2.0.4.0 - Production
System parameter file is /u01/app/oracle/product/11.2.0/db_1/network/admin/listener.ora
Log messages written to /u01/app/oracle/diag/tnslsnr/zzy/listener/alert/log.xml
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=zzy)(PORT=1521)))

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.4.0 - Production
Start Date                16-AUG-2023 14:50:46
Uptime                    0 days 0 hr. 0 min. 3 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/11.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/zzy/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=zzy)(PORT=1521)))
The listener supports no services
The command completed successfully

```
解决方法：找到/u01/app/oracle/product/11.2.0/db_1/network/admin目录下的监听器配置文件listener.ora。
在其中加入

```bash
SID_LIST_LISTENER = 
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_NAME = PBDB01)
      (SID_NAME = pbdb01)
      (ORACLE_HOME = /u01/app/oracle/product/11.2.0/db_1)
      (SERVICE_NAME = pbdb01) 
    )
  )
```
就可以监听服务了。
```bash
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.4.0 - Production
Start Date                16-AUG-2023 14:55:17
Uptime                    0 days 0 hr. 0 min. 3 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/oracle/product/11.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/zzy/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=zzy)(PORT=1521)))
Services Summary...
Service "pbdb01" has 1 instance(s).
  Instance "pbdb01", status UNKNOWN, has 1 handler(s) for this service...
The command completed successfully
```

### 3. 导入数据错误

执行 imp 命令失败，原因是指定了多个导入模式。
将命令更改为：
```bash
imp trade_yy/trade_yy@pbdb01 file=allZGV20190204_基金.dmp full=y ignore=y
```
即可成功导入。

