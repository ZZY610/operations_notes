# centos7.8 安装 oracle11g
## 一、安装过程
### 1. oracle 用户、安装目录配置

#### 1. **创建 Oracle 用户和组**

```bash
groupadd -g 1001 dba
groupadd -g 1002 oinstall
useradd -g oinstall -G dba -d /home/oracle -m -s /bin/bash oracle
passwd oracle
```

#### 2. **创建 Oracle 安装目录**

```bash
mkdir -p /u01/app/oracle/product/11.2.0/db_1
mkdir -p /u01/app/oracle/oradata
mkdir -p /u01/app/inventory
mkdir -p /u01/app/oracle/fast_recovery_area
chown -R oracle:oinstall /u01
chmod -R 775 /u01
```

#### 3. **设置 Oracle 环境变量**

切换 Oracle 用户，编辑 `.bash_profile` 文件：

```bash
vim /home/oracle/.bash_profile
```

添加以下内容：

```bash
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1
export ORACLE_SID=orcl
export PATH=$ORACLE_HOME/bin:$PATH

export LANG=en_US
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK
export LC_CTYPE=zh_CN.GBK

export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib:/usr/local/lib:/usr/X11R6/lib
export PATH=$PATH:$ORACLE_HOME/bin:/bin:/usr/bin:/usr/local/bin:/usr/X11R6/bin:/sbin

CLASSPATH=$ORACLE_HOME/JREORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
CLASSPATH=$CLASSPATH:$ORACLE_HOME/network/jlib
export  CLASSPATH
export  PATH
```

使更改生效：

```bash
source /home/oracle/.bash_profile
```

-   TMP 和 TMPDIR：这两个变量用于指定 Oracle 进程在执行过程中使用的临时目录。通常设置为 /tmp。这些临时文件在 Oracle 数据库运行过程中会被频繁访问。
-   ORACLE_SID：这个变量定义了当前 Oracle 实例的名称。在一台服务器上可以有多个 Oracle 实例,每个实例都有一个唯一的 SID 标识。
-   ORACLE_BASE：这个变量定义了 Oracle 软件的根安装目录。所有 Oracle 相关的文件和目录都会放在这个根目录下。
-   ORACLE_HOME：这个变量指定了当前 Oracle 版本的安装目录。通常是 ORACLE_BASE/product/[版本号]/db_1。
-   LANG、NLS_LANG、LC_CTYPE：这些变量定义了 Oracle 数据库的语言和字符集设置。LANG 指定了操作系统的语言环境,NLS_LANG 定义了 Oracle 数据库的语言和字符集,LC_CTYPE 设置了字符类型。这些变量确保了 Oracle 数据库能够正确处理和显示各种语言的数据。

如果出现以下报错：

```bash
[oracle@hecs-65539 ~]$ source ~/.bash_profile
-sh: warning: setlocale: LC_CTYPE: cannot change locale (zh_CN.GBK): No such file or directory
-sh: warning: setlocale: LC_CTYPE: cannot change locale (zh_CN.GBK)
```

出现这个问题的原因可能是:

CentOS/RHEL 8 默认不包含 zh_CN.GBK 字符集,需要手动安装。
Oracle 数据库安装时设置的 LC_CTYPE=zh_CN.GBK 与系统实际支持的字符集不匹配。

```bash
yum install glibc-langpack-zh
```

### 2. 服务器环境配置

#### 1. **安装必要的软件包**

安装 Oracle 10g 安装所需的软件包：

```bash
yum install binutils compat-libstdc++-33 elfutils-libelf elfutils-libelf-devel gcc gcc-c++ glibc glibc-common glibc-devel glibc-headers kernel-headers ksh libaio libaio-devel libgcc libgomp libstdc++ libstdc++-devel make numactl-devel sysstat unixODBC unixODBC-devel
```

注意：安装 oracle 需要 pkdsh 包，此包在 yum 源中没有包含，需要自己下载。

```bash
[root@hecs-65539 ~]# rpm -ivh pdksh-5.2.14-37.el5.x86_64.rpm
warning: pdksh-5.2.14-37.el5.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID e8562897: NOKEY
error: Failed dependencies:
	pdksh conflicts with (installed) ksh-20120801-144.el7_9.x86_64
[root@hecs-65539 ~]# rpm -e ksh-20120801-144.el7_9.x86_64
[root@hecs-65539 ~]# rpm -ivh pdksh-5.2.14-37.el5.x86_64.rpm
warning: pdksh-5.2.14-37.el5.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID e8562897: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:pdksh-5.2.14-37.el5              ################################# [100%]
```

#### 2. **设置内核参数**

编辑 `/etc/sysctl.conf` 文件，添加以下参数：

```bash
sudo nano /etc/sysctl.conf
```

在文件末尾添加：

```plaintext
fs.aio-max-nr = 1048576
fs.file-max = 6815744
kernel.shmall = 2097152
kernel.shmmax = 2147483648
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 1024 65000
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
```

应用更改：

```bash
sudo sysctl -p
```

#### 3. 修改/etc/profile

```bash
# 添加以下内容：
if [ $USER = "oracle" ]; then
if [ $SHELL = "/bin/ksh" ]; then
ulimit -p 16384
ulimit -n 65536
else
ulimit -u 16384 -n 65536
fi
fi

# 执行生效
source /etc/profile
```

#### 4. 设置用户限制

编辑 `/etc/security/limits.conf` 文件：

```bash
sudo nano /etc/security/limits.conf
```

添加以下行：

```plaintext
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
oracle soft stack 10240
```

#### 5. 修改登录验证

```bash
vim /etc/pam.d/login
```

添加以下内容：

```
session required	/lib64/security/pam_limits.so
session required	pam_limits.so
```

#### 6. 增加虚拟内存

在 Linux 中，创建虚拟内存通常是通过调整交换空间（swap space）来实现的。交换空间是硬盘上的一块区域，用于暂时存储不常用的内存数据，以便为需要更多内存的进程释放内存空间。

- 创建交换文件：
    -  `mkdir /swap; cd /swap/;`
    创建swap目录。
   - 使用 dd 命令创建一个指定大小的交换文件。
        ```bash
        dd if=/dev/zero of=/swapfile bs=1G count=1  # 创建一个1GB大小的交换文件
        ```

- 使交换文件可执行：
```bash
chmod 600 /swapfile
```
- 启用交换文件：
```
mkswap /swapfile
```
- 启用交换：
```
swapon /swapfile
```
- 使交换文件永久化：
为了让交换设置在系统重启后依然有效，开机自动挂载虚拟内存，需要将它添加到/etc/fstab 文件中。
```bash
/swap/swapfile   swap  swap  defaults  0 0
```


```bash
[root@zzy610 ~]# mkdir /swap
[root@zzy610 ~]# cd /swap/
[root@zzy610 swap]# dd if=/dev/zero of=/swapfile bs=1G count=1
dd: unrecognized operand ?𻱣ou?
Try 'dd --help' for more information.
[root@zzy610 swap]# dd if=/dev/zero of=swapfile bs=1G count=1
1+0 records in
1+0 records out
1073741824 bytes (1.1 GB) copied, 8.95213 s, 120 MB/s
[root@zzy610 swap]# mkswap swapfile
Setting up swapspace version 1, size = 1048572 KiB
no label, UUID=efb78ea8-fd60-4bf2-8fd9-e0f915b0c0b0
[root@zzy610 swap]# swapon swapfile
swapon: /swap/swapfile: insecure permissions 0644, 0600 suggested.
[root@zzy610 swap]# chmod 600 swapfile

```

### 3. 下载和安装 Oracle 10g
1. 官网下载以下两个安装文件
    ```
    p13390677_112040_Linux-x86-64_1of7
    p13390677_112040_Linux-x86-64_2of7
    ```
2. **解压缩安装文件**
   将下载的文件解压缩到安装目录中，解压后出现database目录。
    ```bash
    unzip oracle10g.zip -d /u01/app/oracle/
    ```

3. **以 oracle 用户登录并运行安装程序**
   切换到 oracle 用户：

    ```bash
    su - oracle
    ```

    运行安装程序：

    ```bash
    cd ~/database
    
    ./runInstaller -silent -responseFile /home/oracle/database/response/db_install.rsp
    ```

#### 安装成功

```bash
[oracle@zzy610 database]$ ./runInstaller -silent -responseFile /home/oracle/database/response/db_install.rsp
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 120 MB.   Actual 29663 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 1023 MB    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2024-06-08_10-52-49PM. Please wait ...[oracle@zzy610 database]$ [WARNING] [INS-30011] 输入的 ADMIN 口令不符合 Oracle 建议的标准。
   原因: Oracle 建议输入的口令应该至少长为 8 个字符, 至少包含 1 个大写字符, 1 个小写字符和 1 个数字 [0-9]。
   操作: 提供符合 Oracle 建议标准的口令。
[WARNING] [INS-13014] 目标环境不满足一些可选要求。
   原因: 不满足一些可选的先决条件。有关详细信息, 请查看日志。/tmp/OraInstall2024-06-08_10-52-49PM/installActions2024-06-08_10-52-49PM.log
   操作: 从日志 /tmp/OraInstall2024-06-08_10-52-49PM/installActions2024-06-08_10-52-49PM.log 中确定失败的先决条件检查列表。然后, 从日志文件或安装手册中查找满足这些先决条件的适当配置, 并手动进行修复。
可以在以下位置找到本次安装会话的日志:
 /u01/app/inventory/logs/installActions2024-06-08_10-52-49PM.log
Oracle Database 11g 的 安装 已成功。
请查看 '/u01/app/inventory/logs/silentInstall2024-06-08_10-52-49PM.log' 以获取详细资料。
以 root 用户的身份执行以下脚本:
	1. /u01/app/inventory/orainstRoot.sh
	2. /u01/app/oracle/product/11.2.0/db_1/root.sh


Successfully Setup Software.

```

### 4. 安装后配置

1. **运行 root 脚本**
   安装过程中，系统会提示以 root 用户运行两个脚本。切换到 root 用户并运行这些脚本：

    ```bash
    sudo /u01/app/oracle/oraInventory/orainstRoot.sh
    sudo /u01/app/oracle/product/10.2.0/db_1/root.sh
    ```

2. **配置监听器**
   使用`netca`工具配置监听器：

    ```bash
    netca
    ```

3. **创建数据库**
   使用`dbca`工具创建数据库：
    ```bash
    dbca
    ```

### 远程连接


## 二、安装后处理

### 创建表空间、用户
#### 创建表空间
```sql
CREATE TABLESPACE my_tablespace  DATAFILE '/u01/app/oracle/oradata/orcl/mytable.dbf' SIZE 100m 
extent management LOCAL
segment SPACE management auto;
```

#### 创建用户
```sql
CREATE USER zzy
IDENTIFIED BY zzy DEFAULT TABLESPACE my_tablespace
TEMPORARY TABLESPACE TEMP;
```
##### 1. TEMPORARY TABLESPACE

在 Oracle 数据库中，`TEMPORARY TABLESPACE` 指的是用于存储用户在执行某些 SQL 操作时产生的临时数据的表空间。比如排序操作、大型查询和全表扫描等，都会产生临时数据。`TEMPORARY TABLESPACE` 主要用于：

- **排序操作**：如 `ORDER BY`、`GROUP BY`、`UNION` 等
- **临时表**：存储会话级别的临时表数据
- **哈希连接**：在连接两个大表时，哈希连接操作可能会产生临时数据

`TEMPORARY TABLESPACE` 只在会话期间存储临时数据，一旦会话结束，这些数据就会被清除。因此，临时表空间对于保持数据库性能和稳定性是至关重要的。

创建用户时指定 `TEMPORARY TABLESPACE` 是为了确保用户在执行需要临时存储的操作时，有一个指定的临时存储空间。例如：

```sql
CREATE USER TRADE_YY
IDENTIFIED BY trade_yy 
DEFAULT TABLESPACE TBS_TRADE_YY
TEMPORARY TABLESPACE TEMP;
```

以上语句创建了一个名为 `TRADE_YY` 的用户，其默认表空间为 `TBS_TRADE_YY`，临时表空间为 `TEMP`。

## 报错处理

### jdk 版本不适配

```bash
[oracle@zzy610 database]$ ./runInstaller -silent -responseFile /home/oracle/database/response/db_install.rsp
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 120 MB.   Actual 24189 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 1808 MB    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2024-06-03_09-29-30AM. Please wait ...[oracle@zzy610 database]$ There was an error trying to initialize the HPI library.
Please check your installation, HotSpot does not work correctly
when installed in the JDK 1.2 Linux Production Release, or
with any JDK 1.1.x release.
Could not create the Java virtual machine.

```

```bash
dnf install libnsl
```

### 没有指定有效的用户名或电子邮件地址

```bash
[oracle@zzy610 database]$ ./runInstaller -silent -responseFile /home/oracle/database/response/db_install.rsp
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 120 MB.   Actual 23975 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 1759 MB    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2024-06-03_09-33-54AM. Please wait ...[oracle@zzy610 database]$ [WARNING] - My Oracle Support Username/Email Address Not Specified
A log of this session is currently saved as: /tmp/OraInstall2024-06-03_09-33-54AM/installActions2024-06-03_09-33-54AM.log. Oracle recommends that if you want to keep this log, you should move it from the temporary location to a more permanent location.
```

修改静默文件中 DECLINE_SECURITY_UPDATES=true

### 共享内存不足

```bash
[oracle@zzy610 database]$ ./runInstaller -silent -responseFile /home/oracle/database/response/db_install.rsp
Starting Oracle Universal Installer...

Checking Temp space: must be greater than 120 MB.   Actual 23975 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 1733 MB    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2024-06-03_09-42-19AM. Please wait ...[oracle@zzy610 database]$ [FATAL] [INS-35172] Target database memory (1024MB) exceeds the systems available shared memory ({0}MB).
   CAUSE: The total available shared memory on the system (908 MB) was less than the chosen target database memory (1024 MB).
   ACTION: Enter a value for target database memory that is less than908 MB.
A log of this session is currently saved as: /tmp/OraInstall2024-06-03_09-42-19AM/installActions2024-06-03_09-42-19AM.log. Oracle recommends that if you want to keep this log, you should move it from the temporary location to a more permanent location.
```

增加/dev/shm, /dev/shm 通常用做共享内存。

```bash

[root@orcl ~]# umount tmpfs

[root@orcl ~]# mount -t tmpfs shmfs -o size=2G /dev/shm/

##如果umount的时候报错如下：
[root@host157 ~]# umount tmpfs
umount: /dev/shm: device is busy.
        (In some cases useful info about processes that use
         the device is found by lsof(8) or fuser(1))

可以通过如下操作处理：
fuser -km /dev/shm
```
