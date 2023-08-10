## 计算机网络

### 1. 知识背景

#### 1.1 进制

​	进制转换的本质是查表

##### 二进制转换

表格法

| 128  | 64   | 32   | 16   | 8    | 4    | 2    | 1    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|      |      |      |      |      |      |      |      |

#### 1.2 带宽

​	`带宽 ！= 网速；`

​	`吞吐量(下载速度) == 带宽 * 网速`

### 2. 网络

#### 2.1 报文的发报方式

###### 单播，组播，广播

#### 2.2 Mac，Ip地址

Mac地址：网卡生产出来的全球独一物理地址

==Mac地址类似人的长相，身份证（与生俱来的==

==Ip地址类似人的姓名，是后天赋予的逻辑名称。==



DHCP的作用：动态获取ip地址。

局域网的作用：隔离广播



==任何一个网络中，名全0和全1的都是不可用地址==

全0为网络号(网络地址)

全0位广播号(广播地址)

#### 2.3 子网掩码

合法的子网掩码一定是连续的0和1

#### 2.4 数据通信

##### 2.4.1 TCP/IP协议

TCP/IP协议族  （TCP/IP不是协议，是一个庞大的协议族，美国军方创建）

##### 2.4.2 OSI的7层结构

​     **OSI 7层结构**                  **TCP/IP**

| 应用层（信件） | 应用层（应用软件） | 你好  |       |
| -------------- | ------------------ | ----- | ----- |
| 表示层（信封） |                    |       |       |
| 会话层（信封） |                    |       |       |
| 传输层（信封） | 传输层（操作系统） | Dport | Sport |
| 网络层（信封） | 网络层（操作系统） | Dip   | Sip   |
| 链路层（信封） | 链路层（网卡）     | Dmac  | Smac  |
| 物理层（信封） |                    |       |       |

##### 2.4.3 数据通信的过程

![image-20230304141048836](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230304141048836.png)

DNS：（域名翻译服务器），将ip和域名互相映射

一个路由器包含多个网卡

当pc1想要访问百度时，首先要先获取所在局域网的mac地址，dmac的获取需要进行一次arp协议（广播包），通过目标IP为路由器获取路由器的mac地址，然后再组建邮件包，smac为自身，dmac为所在局域网路由器物理地址，dip为百度ip，端口为意图（想要百度提供的服务）。路由器1收到邮件后，修改包的mac地址，dmac地址为下一个路由器的mac地址（进行一次arp协议），smac地址为自身mac地址。

 

PC1通过[www.baidu.com](http://www.baidu.com)域名访问百度时一共发三个包，默认PC不知道局域网网关mac地址，首先通过DHCP获取的ip发送一个arp广播包获取网关mac地址，路由器3发送一次dns包，信件内容为域名字符串，获取目标端口，和域名的ip地址，域名收到dns包向操作系统发送报文，分配端口提供服务作为源端口。Pc收到域名ip地址再组建https/http包。不在一个局域网内，dip为域名的ip地址，dmac为所在局域网mac地址。



#### 2.5 几个重要协议的作用

**ARP协议** 获取mac地址

**DHCP协议** 自动分配ip地址

**DNS协议** 翻译域名对应的ip地址

**HTTP协议**

**ICMP协议**（没有传输层，没有端口，操作系统处理）ping协议

**IGMP协议** 组播协议



#### 2.6 虚拟机抓包

##### 2.6.1 缓存清除

(1) 清除ARP:`ARP -d`

(2) Dns 两处，内存:`ipconfig /flushdns`

​             浏览器:关闭浏览器

#####  2.6.2 抓包步骤

(1)先启动抓包，再清除ARP和dns缓存

(2)打开网页访问百度,网页打开成功后，停止抓包。

(3)在抓包软件的过滤栏中输入，arp/dns/http/icmp,在命令行中执行ipconfig /all 和捕获的报文进行对比。

 

~~由于windows2003的ie浏览器版本太低，所以浏览器不支持java script~~

~~换句话说，这个浏览器就算得到souhu的html源码也无法渲染~~



### 面试问题

==(1) 为什么32位操作系统最多识别4g内存==

cpu 最多识别 2^32 次方的地址



2^32 byte=1024 * 1024 * 1024 * 4 byte

==(2) ipv4和物理地址地址==

ipv4地址占 4 byte, 物理地址占 6 byte [0C-DD-24-40-E2-B9]

物理地址的两个16进制位占一个字节。

==(3)== 

什么是软件？    程序+配置文件
什么是程序？    简单的指令或有序的操作的集合
什么叫进程？    资源分配和拥有的基本单位
什么叫线程？    程序执行的基本单位
什么叫协程？    用户态的轻量级线程，线程内部调度的基本单位

==(4) 1024*768分辨率的24位真彩位图多少M？==

24位真彩：RGB颜色，一个颜色8位（1 byte），三个颜色一共24位 即3个字节
1024*768*3 byte=2.25 M

==(5) 子网的划分==

开发一部的网络号是192.168.10.0/255.255.255.0

部门中共有200人，现在需要四个项目组，其中：

A组需要100人

B组需要50人

C和D组需要各25人

请设计出他们呢个字的网络号/掩码

```
192.168.10.0/255.255.255.128
192.168.10.128/255.255.255.192
192.168.10.192/255.255.255.224
192.168.10.224/255.255.255.224
11111111 11111111 111111111 10000000
```





## Linux

### 1. 虚拟机

#### 1.1 虚拟机配置网络

1.桥接

第一步选项卡设置网络连接方式为桥接

第二步将虚拟机网络ip地址配置成与主机使用的网卡相同的IP地址。 

2.NAT连接

 第一步选项卡设置网络连接方式为NAT连接

 第二步编辑虚拟网络编辑器查看子网ip

 第三步将虚拟机ip设置成相同村庄的

 

**实验**：主机ping百度获取百度的ip地址，将虚拟机Ip设置为与百度ip地址相同村庄的邻居关系。虚拟机再去ping百度会ping不通，因为此时虚拟机和百度ip为邻居关系，虚拟机访问百度不需要再通过网关，只是一直在发送ARP包呼喊百度。

结论：局域网内最好不要配公网地址，可能有的网址访问不了。

 

==端口映射：外网访问私网需要进行端口映射。==

### 2. Linux指令

#### 2.1 系统管理：获取帮助，查看历史，开关机，登录注销。

注销：`logout` 登录：`login` 关机：`shutdown` 重启：`reboot`（关机和重启需要管理员权限）

`Init 0`关机（常用）

`init 6` 重启 

`init 5` 启动Windows界面

Windows获取帮助的命令：`[ ] /?` 例：`ipconfig/flush /?`

Linux获取帮助的命令：`man [ ]` 例：`man ping`

​                  `Info [ ]` 例：`info ping`

#### 2.2 文件系统

| windows            | Linux        | 功能描述                         |
| ------------------ | ------------ | -------------------------------- |
| \--                | pwd          | 了解当前命令行所在的路径         |
| dir                | ls           | 查阅指定路径下有哪些子目录或文件 |
| cd                 | cd           | 进入指定路径                     |
| mkdir              | mkdir        | 新建目录                         |
| rmdir              | rmdir        | 删除空目录                       |
| --                 | touch        | 新建文件                         |
| del                | rm           | 删除文件                         |
| del--              | rm -f/rm -fr | 删除非空目录                     |
| copy               | cp           | 拷贝文件或者目录                 |
| move(也可用于更名) | mv           | 剪切                             |
|                    | find         | 查找                             |
|                    | more/less    |                                  |
|                    | head/tail    | 阅读文件内容                     |
|                    | cat/tac/nl   |                                  |
|                    | vi           | 编辑文件（linux中最核心的指令）  |
|                    | ln           | 新建链接                         |

### 3. Shell编程

#### 3.1 前景知识

`语法 语句 -> 代码微逻辑 -> 算法和数据结构 -> 数学模型 -> 支撑代码 -> 平台 -> 应用程序`

#### 3.2 概念

##### 3.2.1 Java数据类型

###### 八种基本数据类型（系统预定义好的数据类型）

`byte、short、int、long、float、double、char、boolean`

###### 引用型数据类型

##### 3.2.2 变量

```java
int i = 5;
int j;
short k;
```

java和c中的变量分为==基本数据类型==和==引用型类型==

```java
char c = 'a'; 
boolean b = true;
```

java中:

==基本数据类型定义的变量存放的是真实的数据。==

==引用型数据类型定义的变量存放的是地址。==

python中:

变量存放的都是地址

shell中:

变量存放的都是字符串



##### 3.2.3 符号 

###### (1) $ 变量引导符

```shell
i=100
echo i #打印i
echo $i #打印100
```



###### (2) / 转义符

==1：将一个普通符号神话==

***e.g***

`\a 响铃 \b 回退 \t 制表 \n 换行 `

==2: 将任何一个神话字符还原成普通字符==

***e.g***

`\$   \\`

```shell
i=100 
#打印100
echo "$i"
#打印$i
echo "\$i"
#打印\$i
echo "\\\$i"
```

字符串里面的转义符能不能生效取决于 `echo -e`

`printf`自动识别转义字符

***e.g***

```shell
echo -e "a\x20b" #结果为a b 
echo "a\x20b" #结果为a\x20b

printf "a\x20b" #结果为a b
```

==字符串的合法性==

```shell
i=100
echo a$ibc  #输出a
echo a$i-bc  #输出a  
echo a$i_bc	  #输出a100_bc
echo a${i}_bc  #输出a100_bc
```



==shell语句的本质：三步走==

1:将一条语句根据空格分解成多个单词。

2:从左向右依次单词取出每个单词的每个字母，判断是否有特殊的可替换符号($,\,'')

如果存在可替换元素，则立即替换。

3:将替换完毕后的整个字符串当成指令进行执行。

***e.g***

```shell
c=ch
o=o
h=hello
w=world

e$c$o $w$h   #打印 helloworld
e$c$o $w\ $h  #打印 hello world
```



###### (3) `` 倒引号

作用:函数或指令替换

***e.g***

```shell
his=`history`
echo $his

ret=`ping -c 3 www.baidu.com`
echo $echo
```



###### (4) (())

作用:将c语言代码转换成shell

==注:在转换的过程中只能识别整型数据==

***e.g***

```shell
i=3;j=4
echo $((i+j))	#输出7
# k=((i+j))
# echo $k	输出7 

i=3.0;j=4
echo $((i+j))	#报错
```



==练习==

请打印出3+4=7  3*4=12

```shell
i=3;j=4
echo $i+$j=$((i+j))   #	输出3+4=7

i=3;j=4
echo $i*$j=$((i*j))   # 输出3*4=12
```

#### 3.3 shell语句

##### 3.3.1 控制语句

##### 3.3.2 分支语句

###### (1) if 语句

```shell
# 语法
if [条件];then
 elif [条件];then
else
fi
```

```shell
read -p "请输入年龄" age
if ((age<30);then
  if ((age<16));then
     echo "少年人"
  else 
     echo "青年人"
  fi
else
	if ((age<40));then
	   echo "中年人"
	else
	   echo "老年人"
	fi
fi
```



##### 3.3.3 循环语句

###### (1) while语句

```shell
s=0;i=1
while((i<=100));do
	((s+=i));((i++))
done
echo $s
```

###### (2) for语句

```shell
s=0
for ((i=1;i<=100;i++));do
	((s+=i))
done
echo $s
```

***e.g*** 求Fibonacci数列前十位

```shell
a=1;b=1
s="1 1 "
for ((i=0;i<8;i++));do
  ((c=a+b));s=$s$c" "
  a=$b; b=$c
done
echo $s
```

**题：将人机交互输入的任何十进制整数转换成2进制，并打印**

```shell
while((num>0));do
  rem=$((num%2))$re
read -p "请输入要转换的十进制数" num

rem=""
while((num>0));do
  rem=$((num%2))$rem
  ((num/=2))
done
  echo $rem
```



#### 3.4 代码逻辑

##### 3.4.2 循环的第二个重要逻辑：

扫描，我不清楚哪个数据是我所求，但是我知道这些数据的特点以及他们的范围



题1：求三位数的水仙花数

```shell
for((abc=100;abc<1000;abc++));do
  a=abc/100; b=abc/10%10; c=abc%10
  if((abc==a**3+b**3+c**3));then
    echo $abc
  fi
done
```

题2：求最大公约数

```shell
read -p "请输入第一个数字" num1
read -p "请输入第二个数字" num2

for((i=num1;i>0;i--));do
  if((num1%i==0 && num2%i==0));then
    echo $i;break
  fi
done
```

题3：求最小公倍数

```shell
read -p "请输入第一个数" fir
read -p "请输入第二个数" sec

for((i=fir;i<=fir*sec;i++));do
    if((i%fir==0 && i%sec==0));then
      echo $i
    fi
done
```



##### 3.4.3 循环的第三个能力：推论

推论的步骤3步：

1：先假设

2：循环诊断

3：鉴别结论

题1：求100-200之间的质数

```shell
for((i=100;i<=200;i++));do
  flag=1
  for((cnt=2;cnt<i;cnt++));do
    if((i%cnt==0));then
      flag=0;break
    fi
  done
  if((flag==1));then
    echo $i
  fi
done
```



##### 3.4.4 随机数问题

系统中shell中随机数：RANDOM(取的是计算机的时间最后两位字节数值0~65535)

返回0-10之间的数:

`echo $((RANDOM%11))`

返回mn-mx之间的数:

`echo $((RANDOM%(mx-mn+1)+mn))`

1：初始化随机数

2：循环猜测

3：结论评判

e.g 猜数字大小

```shell
echo "Welcom to my guessnum game"
read -p "please enter the minnum" mn
read -p "please enter the maxnum" mx
((r=RANDOM%(mx-mn+1)+mn))
   cnt=0
while((1==1));do
   ((cnt++))
   read -p "please guess the num" guess
   if((guess<r));then
     echo "guess is small"
   elif((guess>r));then
     echo "guess is big"
   else
     echo "you guessed it";break
   fi
done

echo $cnt
```



##### 3.4.5 画图问题

###### 3.4.5.1 常规枚举打印

(1) 打印 *****************

```shell
line='';ch='\x2a'   # *不能正常打印，并且不能用转义符
for((i=0;i<5;i++));do
  line=$line$ch
done
  echo -e $line
```

(2) 打印![image-20230306232458351](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230306232458351.png)

```shell
for((j=0;j<5;j++));do
line='';ch='\x2a'
  for((i=0;i<5;i++));do
     line=$line$ch
  done
  echo -e $line
#  line=''
done
```

(3) 打印![image-20230306233428973](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230306233428973.png)

```shell
for((y=0;y<5;y++));do
  line='';ch='\x2a'
  for((x=0;x<=y;x++));do
     line=$line$ch
  done
  echo -e $line
done
```

(4) 打印![image-20230306234006957](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230306234006957.png)

```shell
for((y=0;y<5;y++));do
  line='';ch='\x2a'
  for((x=0;x+y<5;x++));do
     line=$line$ch
  done
  echo -e $line
done
```

(5) 打印![image-20230306234525262](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230306234525262.png)

```shell
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x<5;x++));do
     if((x==y));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
done
```

(6) 打印![image-20230306235119147](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230306235119147.png)

```shell
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x<5;x++));do
     if((x+y==4));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
done
```

(7) 打印![image-20230306235459284](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230306235459284.png)

```shell
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x-y<5;x++));do
     if((x+y==4));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
done
```

###### 3.4.5.2 设置左右起点

(8) 将(7)平移

```shell
left=30;right=30
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
  ((left--));((right++))
done
```

(9) 打印梯形

```shell
left=30;right=40
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
  ((left--));((right++))
done
```

(10) 打印平行四边形

```shell
left=30;right=40
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
  ((left++));((right++))
done
```

###### 3.4.5.3 设置左右增量

(10) 设置左增量后右增量，打印为不等腰三角形

```shell
left=30;right=30;ldlt=-1;rdlt=3
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
  ((left+=ldlt));((right+=rdlt))
done
```

(11) 打印矩形

 ```shell
 left=30;right=50;ldlt=0;rdlt=0
 for((y=0;y<5;y++));do
   line='';ch='\x20'
   for((x=0;x<=right;x++));do
      if((x==left));then
        ch='\x2a'
      fi
      line=$line$ch
   done
   echo -e $line
   ((left+=ldlt));((right+=rdlt))
 done
 ```

(12) 打印钝角三角形

```shell
left=30;right=30;ldlt=1;rdlt=4
for((y=0;y<5;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
  ((left+=ldlt));((right+=rdlt))
done
```

(13) 打印![image-20230307002737536](C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230307002737536.png)

```shell
left=30;right=30;ldlt=0;rdlt=2;height=11
for((y=0;y<height;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
  if((y==height/2));then
     ((rdlt*=-1))
  fi
  ((left+=ldlt));((right+=rdlt))
done
```

(14) 将(13)的左边补全(菱形)

```shell
left=30;right=30;ldlt=-2;rdlt=2;height=11
for((y=0;y<height;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left));then
       ch='\x2a'
     fi
     line=$line$ch
  done
  echo -e $line
  if((y==height/2));then
     ((rdlt*=-1));((ldlt*=-1))
  fi
  ((left+=ldlt));((right+=rdlt))
done
```

(15) 菱形中间镂空

```shell
left=30;right=30;ldlt=-2;rdlt=2;height=11
for((y=0;y<height;y++));do
  line='';ch='\x20'
  for((x=0;x<=right;x++));do
     if((x==left||x==right));then
       ch='\x2a'
     else
       ch='\x20'
     fi
     line=$line$ch
  done
  echo -e $line
  if((y==height/2));then
     ((rdlt*=-1));((ldlt*=-1))
  fi
  ((left+=ldlt));((right+=rdlt))
done
```

(16) 打印乘法口诀表

```shell
for((y=1;y<10;y++));do
  line=''
  for((x=1;x<=y;x++));do
     line=$line"$x\x2a$y="$((x*y))"\t"
  done
  echo -e $line
done
```



## 数据库

### 1. 数据库和文件的区别

(1)

文件需要和特定软件进行耦合，大多是本地访问，无法在互联网进行分享

数据库不需要特定的耦合，可以有多个客服端访问，可以在互联网信息分享

(2)

文件访问是直接访问

数据库访问是间接访问  ==数据库构成 DBS=DB+DBMS==



数据库设计 三大范式

### 2. 数据库

#### 2.1 三大范式

(1) 第一范式：一张表中的每个属性必须是最小的不可再分解的原子

**例如 一张表中出现了长相属性(万能属性)是不允许的**

(2) 第二范式：同一张表中，所有的非主属性必须==完全函数依赖==于主属性

**例如 学生表中出现了书籍，成绩等属性时不允许的**

(3) 第三范式：同一张表中，非主属性对主属性的依赖禁止出现传导

**依赖传导会导致数据冗余和数据的不一致性**

#### 2.2 数据库设计

一对一 一张表

一对多 两张表直接相切

多对多 两张表中间还要加一张关系表



**练习：XX图书馆管理系统**

1：能够支持图书的录入，删除，信息修改，图书信息至少包括：
  图书名称，价格，所属出版社名称，出版社地址，出版社联系方式
  图书的==折旧率== 

图书和出版社是==一对多==关系

图书表

| bid  | bname    | bprice | pid  | bsort  |
| ---- | -------- | ------ | ---- | ------ |
| b01  | 高等数学 | 55     | p01  | 数学类 |
| b02  | 高等数学 | 48     | p02  | 数学类 |
| b03  | 英语四级 | 50     | p01  | 语言类 |

出版社表

| pid  | pname      | paddress | ptel |
| ---- | ---------- | -------- | ---- |
| p01  | 人民出版社 |          |      |
| p02  | 工业出版社 |          |      |

书籍表

| txm  | bid  | per  |
| ---- | ---- | ---- |
| tx01 | b01  | 0    |
| tx02 | b01  | 0    |
| tx03 | b02  | 0    |
| tx04 | b03  | 0.2  |

2：图书信息中还包含图书的分类，一种书籍可以包含多个分类，例如  
   《电子机械》这种书籍属于‘机械类’和‘电子类’

读者表

| rid  | rname | rage | vipid |
| ---- | ----- | ---- | ----- |
| r01  | 张三  | 18   | v01   |
| r02  | 李四  | 20   | v02   |

vip表

| vipid | rvip | vnum | vtime | vprice |
| ----- | ---- | ---- | ----- | ------ |
| v01   | 白银 | 1    | 1周   | 0      |
| v02   | 白金 | 2    | 1月   | 5      |

类别表

| sortid | sortname | sortdes                    | sortprice |
| ------ | -------- | -------------------------- | --------- |
| s01    | 基础类   | 可以借阅大部分基础类别图书 | 0         |
| s02    | 数学类   | 可以借阅数学类别图书       | 50        |
| s03    | 语言类   | 可以借阅语言类别图书       | 30        |

读者类别表

| rid  | sortid | sortname |
| ---- | ------ | -------- |
| r01  | s01    | 基础类   |
| r01  | s02    | 数学类   |
| r02  | s01    | 基础类   |
| r02  | s03    | 语言类   |

书籍类别表

| bid  | sortid | bname    | sortname |
| ---- | ------ | -------- | -------- |
| b01  | s02    | 高等数学 | 数学类   |
| b03  | s03    | 英语四级 | 语言类   |



3：系统支持读者的注册，注销，修改，信息至少包括：读者姓名，读者年龄，读者vip，读者分类
其中，读者只能有一个vip，如‘白银’，‘白金’等，vip等级决定了读者一次能够借阅多少本书籍，一次能借阅的时长，借阅过期后的滞纳金折扣
而读者分类表示读者可以关注的类别，比如，读者可以关注‘机械类’，‘军事类’，‘政治类’等等

4，读者可以借阅图书和归还图书，读者一次能借阅的时长，借阅过期后的滞纳金折扣取决于vip。
而读者能否借阅某本书取决于读者的分类和图书的分类，例如
读者“王伟”具备的分类包括：‘基础类’，‘电子类’，‘军事类’。因此，他可以借阅所有具有这三类之一的图书，如：《电子机械》这本书，有电子类，而王伟也具有电子类，因此可以借阅
而“李梅”只有‘基础类’，因此无法借阅《电子机械》

5、可以查阅读者或者书籍的借阅历史

| ==借阅编号== | 读者编号 | 条形码 | 借阅日期 | 实际归还时间 | 实际缴纳滞纳金 |
| ------------ | -------- | ------ | -------- | ------------ | -------------- |
|              | r01      |        |          |              |                |

#### 2.3 查询

##### 2.3.1 单表查询

sql注释 --  快捷键 Ctrl+?

**查询的第一核心：书写和执行顺序**

select       5

from		 1

where        2

group by     3

having       4

order by     6

**第二核心: from子句和group by子句都会通过循环扫描**

###### (1) from子句

1: 将表格从磁盘中载入内存

2: 将多张表格进行笛卡尔相乘

3: 通过循环语句每次取出一条记录，将之交给后续执行的子句

###### (2) select子句

1: 可以通过属性名投影相关的值

2: 同时投影多个属性，属性名之间通过英文逗号进行间隔 

`select sname,sage from student`;

3: *表示所有属性的组合

`select * from student`;

4: 可以在属性名后面添加介词as再加别名(==别名是变量名，不要加引号==)，as可以省略  

`select sname as 姓名 from student`;

5: distinct可以对右侧的表达式进行去重 

`select distinct sage 年龄 from student`;

6: select语句中可以嵌入字符串连接符(数据库字符串用单引号)

`select sname ||'今年'||sage||'岁' from studnet`;

7: 数据分数值类和字符类，字符类必须通过单引号括起来

###### (3) where子句

1: 用于过滤每一行，后面跟的是一个布尔表达式(真/假)

`select * from student where sage>19;`

2: 支持比大小 > >= < <= =

`select * from student where sid=1003;`

3: 字符串也可以比大小

`select * from student where 'abc'>'ab';`

4: 支持逻辑运算 与或非 ==and or not==

`select * from student where snatliveplace='福建' and ssex='女'`;

5: 运算存在优先级 ==()>not>and>or==

找出来自福建和上海的女生信息

`select * from student where (snativeplace='福建' or snativeplace='上海') and ssex='女';`

6: 支持连续的数值空间比对 ==between==

`select * from student where sage between 19 and 22;`

7: 支持非连续数值区间比对 ==in==

`select * from student where sage in(19,21,23);`

8: 支持字符串的模糊匹配 ==like== **%表示任意多个任意字母**

​                           **_表示任意一个字母**

`select * from student where sname like '张%';`

`select * from student where sname like '张__'`

9: 可以对空(==Null==)进行匹配 ==用到比大小的是找不到Null的==

`select * from student where ssex is null`

###### (4) group by分组子句

1: 根据分组特征进行聚合，分组特征值相同的记录聚合为一个组

2: 分组完成之后解释器的眼中再无个体记录，只有组

3: 分组完毕之后group by语句通过循环扫描每一个组并且将之交给后面的子句处理

4: 可以有多个分组特征

5：组所继承的属性全部来自分组特征，因此不能投影分组以外的任何显示属性

6: 默认分组就是没有书写group by语句而进行了统计，默认分组中组没有继承任何显示特征(**count是隐式特征**)

7: count(*)

`select ssex,max(),min(),sum(),avg(),count(*)`

查询每个学生每门课的均分

`select sid,avg(cmark) from mark group by sid;`、

查询每门课程的均分

`select cid,avg(cmark) from mark group by cid;`

**练习1: 找出各个班级的男生人数，要求显示班级，男生人数，两列信息**

`select sclass,count(*) from student where ssex='男' group by sclass;`

**练习2: 找出各个班级的男生分别来自多少个不同的地区**

`select sclass,count(distinct snativeplace) from student where ssex='男' group by sclass;`

###### (5) Having 组过滤子句

1: 对组进行过滤

2: where中不可出现任何统计函数，但是having可以

3: 某些场合having可以和where互换

**找出各个班级的男生人数**

`select sclass,count(*) from student where ssex='男' group by sclass;`

`select sclass,count(*) from student group by sclass,ssex having ssex='男';`

**练习:**

**1: 找出除了2001号课程以外，各科成绩都高于75分的学生sid**

==各科成绩都高于75分即最小值大于75== 

```sql
SELECT sid from mark GROUP BY sid HAVING MIN(cmark)>75
```

**2: 找出没有选修2001号课程的学生sid**

```sql
step 1:SELECT sid FROM mark GROUP BY sid,cid HAVING cid=2001

step 2:SELECT sid FROM mark GROUP BY sid HAVING sid NOT in (step1)
```



###### (6) order by 排序子句

1: 根据某个特征进行排序

2：可以升序**asc**或者降序**desc**，**asc**可以省略

`select * from student order by sage asc`

3: 排序特征可以是多个，甚至可以对统计数据进行排序

4: 排序中可以进行排序

```sql
select sid,avg(cmark) amk
from mark
group by sid
order by amk desc;
```

##### 2.3.2 多表查询

1: 多表组合，将多张单表组合成为一张混合单表，然后进行单表查询

2: 子查询，将第一个查询的结果代入到下一个单表查询中。

***e.g* 找出每个学生的2001号课程成绩，投影sid,sname,cid,cmark**

```sql
select s.sid,sname,cid,cmark from student s,mark m --给student,mark表起别名
where s.sid=m.sid and cid=2001;
```

**练习:** 

**找出各个班级的2001号课程均分，要求显示班级，均分**

```sql
SELECT sclass,avg(cmark) from mark m,student s
WHERE m.sid=s.sid and cid=2001
GROUP BY sclass
```

**找出每个学生的均分，要求显示姓名和均分**

```sql
select sname,avg(cmark)
from mark m,student s
where s.sid=m.sid
group by s.sid,sname;
```

**哪些学生的2001号课程成绩比2002号课程成绩高？**

==数据库只允许同行比较，所以将两张mark表组合进行多表查询==

```sql
select a.sid from mark a,mark b
where a.sid=b.sid and a.cid=2001 and b.cid=2002 and a.cmark>b.cmark;
```

**1: 1002号学的哪些课程成绩比1003号高？找出这些课程的cid**

```sql
select a.cid from mark a,mark b
where a.cid=b.cid and a.sid=1002 and b.sid=1003 and a.cmark>b.cmark
```

**2: 1002号学的哪些课程成绩比1003号高？找出这些课程的课程名**

```sql
select c.cname from mark a,mark b,course c
where a.cid=b.cid and b.cid=c.cid and a.sid=1002 and b.sid=1003 and a.cmark>b.cmark
```

**3：陈杰的哪些课程成绩比卢云阳高？找出这些课程的课程名**

```sql
select c.cname from mark a,mark b,course c,student s1,student s2
where a.cid=b.cid and b.cid=c.cid and a.sid=s1.sid and b.sid=s2.sid and a.cmark>b.cmark
and s1.sname='陈杰' and s2.sname='卢云阳';
```



##### 2.3.3 子查询

###### 1: 条件代入  where,having

**找出陈杰的2001号课程的成绩**

```sql
step1: select sid from student where sname='陈杰'
step2: select cmark from mark where sid=(step1) and cid=2001;
```

1: 找出陈杰的均分

```sql
select avg(cmark) from mark where sid=(select sid from student where sname='陈杰');
```

2: 找出年龄最大的学生信息

```sql
step1: select max(sage) from student
step2: select * from student where sage=(step1);
```

3: 找出年龄最大的女生姓名

```sql
step1:select max(sage) from student where ssex='女'
step2:select * from student where sage=(step1) and ssex='女';
```

4: 找出陈杰的最高分课程名称

```sql
step1:select sid from student where sname='陈杰'
step2:select max(cmark) from mark where sid=(step1)
step3:select cid from mark where cmark=(select max(cmark) from mark where sid=(step2)
step4:select cname from course 
where cid in (step3)
```



5: 找出高等数学成绩最高的女生姓名

```sql
step1:找出所有女生的sid
select sid from student where ssex='女'
step2:找出高等数学的cid
select cid from course where cname='高等数学'
step3:找出女生的高等数学最高分
select max(cmark) from mark where cid=(step2) and sid in (step3)
step4:找出高等数学成绩等于step3的学生sid
select sid from mark where cmark=(select max(cmark) from mark where cid=(step2) and sid in (step3))
step5:在学生表中将这些sid转换成sname,并且过滤出性别为'女'
select sname from student where sid=(step4)
```

**找出没有选高等数学课程的学生姓名**

```sql
step1:找出高等数学的cid
step2:找出没有选高等数学学生的sid
step3:在学生表中将sid转换成sname

step1: select cid from course where cname='高等数学'
step2: select sid from mark where cid in (step1)
step3: select sname from student where sid not in(step2)
```

**如果我的年龄大于等于包括自己在内的所有人，那么我就是最大的**

```sql
step1:select sage from student where sage is not null
step2: select * from student where sage>=(step1) 
select * from student where sage>=all(step1)  --all代表与
select * from student where not sage<any(step1)  --
```

**找出均分最高的姓名**

```sql
step1: 找出每个学生的均分
step2: 找出均分大于等于step1的学生sid
step3: 换成sname

SELECT max(AVG(cmark)) FROM mark 
GROUP BY sid 


SELECT sid from mark GROUP BY sid 
HAVING avg(cmark)>=(SELECT max(AVG(cmark)) FROM mark 
GROUP BY sid )

SELECT sname FROM student where sid=(SELECT sid from mark GROUP BY sid 
HAVING avg(cmark)>=(SELECT max(AVG(cmark)) FROM mark 
GROUP BY sid ))

```

###### 伪列 rownum

`select rownum,s.* from student s`

找出年龄最大的信息

```sql
select * from student order by sage desc
select * from (
select * from student order by sage desc
) where rownum=1
```

==找出年龄由大到小排序第4到第6名学生信息(最重要的两个笔试题之一)==

```
step1 select * from student order by sage desc
step2 select * from (step1) where rownum
```

```sql
select * from student order by sage desc

select * from (select * from student order by sage desc) where rownum<=6

select * from (
    select * from (select * from student order by sage desc) where rownum<=6
) order by sage

select * from (
   select * from (
      select * from (
          select * from student order by sage desc
      ) where rownum<=6
   ) order by sage
) where rownum<=3 

select * from (
   select * from (
     select * from (
         select * from (
             select * from student order by sage desc
         ) where rownum<=6
     ) order by sage
   ) where rownum<=3 
) order by sage desc
```

###### 2: 表格代入  from

给两张表组合查询得到的新表起别名

```sql
select sname,amk
from student s, (select sid,avg(cmark) amk from mark group by sid) m
where s.sid=m.sid
```

```sql
SELECT * FROM(
SELECT s.*,m.sid mid FROM student s,mark m WHERE s.sid=m.sid
)
```



练习

1: 找出选课内容和陈杰完全相同的学生，显示姓名

```sql
step 1:找出陈杰的sid
SELECT sid from student WHERE sname='陈杰'
step 2:找出陈杰选课的cid
SELECT cid from mark WHERE sid=(SELECT sid from student WHERE sname='陈杰')
step 3:找出陈杰没有选的课的cid
SELECT cid from course WHERE cid not in (step2)
step 4:找出选了step3的人
SELECT sid from mark WHERE cid in (step3)

step 5:获得陈杰选课的总数
SELECT COUNT(*) from mark where sid = (SELECT sid from student WHERE sname='陈杰')

step 6:排出step 4的人
SELECT sid from mark where sid not in (step4)

SELECT sid from mark where sid in (step6)

SELECT sname FROM student WHERE sid in(SELECT sid from mark where sid in (SELECT sid from mark where sid not in (step7)
```





###### 3: oracle分析函数

(1) row_number()

```sql
select m.*,row_number() over (partition by sid ORDER BY cmark desc) p,
      row_number() over (partition by sid ORDER BY cmark) q
from mark m
```

**找出每个同学成绩前三名的课程，显示姓名，课程号，成绩**

```sql
SELECT sname,cid,cmark FROM student s,(select m.*,row_number() over (partition by sid ORDER BY cmark desc) p,
row_number() over (partition by sid ORDER BY cmark) q
from mark m) m2
WHERE s.sid=m2.sid and m2.p<=3
```

**找出各个班级中高等数学最高分的学生姓名，班级，成绩**

```sql
SELECT sname,s.sclass,cmark from student s, (SELECT sclass,m.*,row_number() over (partition by sclass ORDER BY cmark desc) p FROM student s,mark m WHERE s.sid=m.sid and cid=(SELECT cid FROM course WHERE cname='高等数学') ) m2

 where s.sid=m2.sid and m2.p=1
```

(2) rank()

(3) dense_rank()

(4) sum

```sql
SELECT m.*,row_number() over (partition by sid ORDER BY cmark desc) p,
					rank() over (partition by sid ORDER BY cmark desc) q,
					dense_rank() over (partition BY sid ORDER BY cmark desc) r,
					sum(cmark) over (partition BY sid ORDER BY cmark desc) s
		FROM mark m

```

###### 4: 投影子查询

```sql
SELECT sname, (SELECT cmark from mark where sid=s.sid and cid=2001) 高等数学成绩,
         (SELECT cmark from mark where sid=s.sid and cid=2002) 英语成绩
	 FROM student s
```

请用上述的范例实现:找出每个学生的姓名和其均分

```sql
 SELECT sname, (SELECT AVG(cmark) FROM mark where sid=s.sid) 均分
 FROM student s
```

###### 5: 相关子查询

**exists(表达式) 返回真和假**

​		**如果表达式结果非空则返回真，否则返回假**

***e.g*** 找出至少一门课成绩高于80的学生姓名

```sql
SELECT sname 
from student s
WHERE EXISTS(SELECT * FROM mark WHERE sid=s.sid and cmark>80)
```

**not exists(表示)**

​        **如果表达式结果非空则返回假，否则返回真**

***e.g*** 找出年龄最大的学生姓名(五种解法)

```sql
--5.如果我的年龄是最大的，那么一定不存在有人年龄比我大
SELECT sname 
FROM student s
WHERE NOT EXISTS(SELECT * FROM student WHERE sage>s.sage)

--找出最大年龄，问谁的年龄等于最大年龄
--
```

找出每门课成绩均高于均分的同学

```sql
step1: SELECT m.*,(SELECT AVG(cmark) from mark WHERE cid=m.cid) avgmk FROM mark m

step2: SELECT sid from (step1) x WHERE NOT EXISTS(
   SELECT * FROM (SELECT m.*,(step1) WHERE cmark<avgmk and sid=x.sid
)
```

**双重否定**

找出选了所有课程的学生信息

```sql
SELECT * FROM student s WHERE NOT EXISTS(
	SELECT * FROM course c WHERE NOT EXISTS(
		SELECT * FROM mark WHERE sid=s.sid and cid=c.cid
	)
)
```

###### 6: 集合

交并差

union intersect minus

```sql
select sid,sname,sage from student where sage in (19,21)
union
select sid,sname,sage from student where sage in (19,20)
```

###### 7: 连接查询

```sql
select * from student s, mark m
where s.sid=m.sid          --先扫描student表存到内存里，再和mark表组合

SELECT * FROM student s JOIN mark m on s.sid=m.sid   --直接判断是否合法，合法才组合
```

(1) 内连接

```sql
SELECT * FROM student s INNER JOIN mark m on s.sid=m.sid
```

(2) 外连接

```sql
SELECT * FROM student s LEFT JOIN mark m on s.sid=m.sid
```

(3) on和where的区别(on是连接条件，where是过滤条件)

```sql
SELECT * FROM student s LEFT JOIN mark m on s.sid=m.sid and ssex='男'


SELECT * FROM student s LEFT JOIN mark m on s.sid=m.sid 
WHERE ssex='男'
```

查找每个人的选课数(==用连接查询获取张颖和郭志伟的选课的0值==)

```sql
SELECT sname,COUNT(cid) FROM student s LEFT JOIN mark m on s.sid=m.sid
GROUP BY s.sid,sname
```



#### 2.4 sql(ddl,dml,dcl)

**ddl=datadefinelanguage create drop   alter**

**dml                    insert delete update**

##### 2.4.1 创建表格

```sql
CREATE TABLE studentback 
as SELECT * FROM STUDENT WHERE 1=2  --只给结构不给数据
```

##### 2.4.2 插入数据

```sql
INSERT INTO MARK(cid,cmark,sid) VALUES (2008,80,1001)
```

```sql
INSERT into  studentback (sid,sname,sage,ssex,SNATIVEPLACE,SMAJOR,SCLASS,SNATIVE)
SELECT * from STUDENT WHERE SSEX='男'
```

**练习：请创建均分表，sid和amk，将均分高于80的学生学号和其均分写入均分表**

```sql
--创建均分表
CREATE TABLE tamk
as SELECT sid,avg(cmark) amk FROM MARK WHERE 1=2 GROUP BY sid 
--写入数据
INSERT INTO tamk(sid,amk)
SELECT sid,AVG(cmark) FROM MARK HAVING AVG(cmark)>80 GROUP BY sid
```

##### 2.4.3 更新数据

**练习：**

**1. 将所有非汉族学生中均分低于75的学生及其高等数学成绩+2分**

```sql
UPDATE MARK
SET cmark=CMARK+2
WHERE SID in (SELECT sid FROM STUDENT WHERE SNATIVE!='汉族' AND
sid in (SELECT SID FROM MARK HAVING AVG(cmark)<75 GROUP BY SID))
and cid in (SELECT cid FROM COURSE WHERE CNAME='高等数学')
```

**2. 用一条update语句将陈杰的年龄+1岁，同时将其改动到3班**

```sql
UPDATE STUDENT
SET SAGE=SAGE+1,SCLASS='3班'
WHERE sname='陈杰'
```

##### 2.4.4 删除

###### 1: 删除数据

```sql
--删除张泽澄的数据
DELETE FROM STUDENT WHERE SNAME='张泽澄'

--删除学生表的所有数据
delete from student where 1=1

--截断数据表(速度非常快，但无法回退)
truncate table student
```

###### ==2: 去重删除==

==rownum== from子句构造的伪列，每次调用from都会创建新的rownum

==rowid== 是每一条记录的Hashcode,近似于相对地址

```sql
DELETE FROM STUDENT
WHERE ROWID not in (SELECT MIN(ROWID) FROM STUDENT GROUP BY sid)
```

##### 2.4.5 更新表格

```sql
ALTER TABLE stu11 add ssex CHAR(2) 

ALTER TABLE stu11 DROP COLUMN ssex --drop后面要跟属性名
```

#### 2.5 约束 contraint

##### 2.5.1 主键约束

==primary key==

主键特征：==唯一==且==非空==

注：非空唯一的属性未必是主键

```sql
CREATE TABLE stu1 (
		sid CHAR(4) primary key,
		sname VARCHAR2(50)
)

CREATE TABLE mark1 (
		sid char(4),
		cid char(4),
		score number,
		constraint pk_sidcid primary key(sid,cid)  --联合主键
)
```



##### 2.5.2 外键约束

==foreign key==

```sql
CREATE TABLE stu1 (
		sid CHAR(4) primary key,
		sname VARCHAR2(50)
)

CREATE TABLE course1 (
	  cid char(4),
		cname varchar(20),
		constraint pk_cid primary key(cid)
)

CREATE TABLE mark1 (
		sid char(4),
		cid char(4),
		score number,
		constraint pk_sidcid primary key(sid,cid),
        --删除某条记录时连带删除和他相关联的数据，两种方式
        --(1) on delete cascade
        --(2) on delete set null
		constraint fk_sid foreign key(sid) references stu1(sid) on DELETE cascade,
		constraint fk_cid foreign key(cid) references course1(cid) on DELETE set null
)
```



##### 2.5.3 唯一约束

==unique==

==唯一约束可为空==

```sql
CREATE TABLE stu2 (
    sid char(4) primary key,
		sname varchar(50),
		sfz char(4) UNIQUE
)
```

##### 2.5.4 非空约束

```sql
CREATE TABLE stu2 (
    sid char(4) primary key,
		sname varchar(50) not null,
		sfz char(4) UNIQUE not null
)	
```

##### 2.5.5 check校验约束

```sql
CREATE TABLE stu3 (
    sid char(4) primary key,
		sname varchar(50) not null,
		ssex char(2) DEFAULT '男' CHECK(SSEX in ('男','女')),
 		sfz char(4) UNIQUE
)
```

##### 2.5.6 正则表达式

正则表达式
有精确度的模糊匹配方式
对字符进行分类，并且用某个符号表征一个类别
比如 \d 表示任意一个阿拉伯数字
   . 表示任意一个字母
   \s 表示空格或者或者或者横向制表符
  [465]  字符集合
  [a-f] [A-f gh]
   [\^a-f] 倒集合

  \^(..)以..为首字母开头

  (苹果|香蕉|橘子)
量词，数量关系，用于修饰其左侧的元素的个数
\*  表示任意多个，可以是0个
\+  至少一个
？ 0个或者1个
{m,n} 最少m个，最多n个
{n} 就是n个
{,n}最多n个
{m,} 最少m个

```sql
create table x(
phone char(11) check(regexp_like(trim(phone),'^\d[11]$'))
)

```



#### 2.6 序列 sequence

```sql
CREATE SEQUENCE sequence_name
[START WITH num]
[INCREMENT BY increment]
[MAXVALUE num|NOMAXVALUE]
[MINVALUE num|NOMINVALUE]
[CYCLE|NOCYCLE]
[CACHE num|NOCACHE]

--
① START WITH：从某一个整数开始，升序默认值是 1，降序默认值是-1。
② INCREMENT BY：增长数。如果是正数则升序生成，如果是负数则降序生成。升序默
认值是 1，降序默认值是-1。
③ MAXVALUE：指最大值。
④ NOMAXVALUE：这是最大值的默认选项，升序的最大值是： 1027，降序默认值是-1。
⑤ MINVALUE：指最小值。
⑥ NOMINVALUE：这是默认值选项，升序默认值是 1，降序默认值是-1026。
⑦ CYCLE：表示如果升序达到最大值后，从最小值重新开始；如果是降序序列，达到最
小值后，从最大值重新开始。
⑧ NOCYCLE：表示不重新开始，序列升序达到最大值、降序达到最小值后就报错。默
认 NOCYCLE。
⑨ CACHE：使用 CACHE 选项时，该序列会根据序列规则预生成一组序列号。保留在内
存中，当使用下一个序列号时，可以更快的响应。当内存中的序列号用完时，系统
再生成一组新的序列号，并保存在缓存中，这样可以提高生成序列号的效率。 Oracle
默认会生产 20 个序列号。
⑩ NOCACHE：不预先在内存中生成序列号
```

#### 2.7 视图

视图有哪些特征？

```sql
create view v_stu as 
select sid,sname from student
```

创建视图，显示 姓名，最高分，最低分，平均分

```sql
CREATE view stumark as
SELECT (SELECT sname FROM student WHERE sid=m.sid) nm, MAX(cmark) mxs,
MIN(cmark) mns,TRUNC(AVG(cmark), 2) amk FROM mark m GROUP BY sid
```



#### 2.8 索引（空间换时间）

频繁使用的东西才创建索引

主键自带索引且为unique索引

==mysql索引的底层算法是B+树==

```sql
CREATE  INDEX idx_sage on student (sage)
```

#### 2.9 数据库编程

##### 2.9.1 plsql代码块

###### (1) 查询赋值打印

DBMS收到发送过来的语句，会首先诊断这个字符串是指令还是代码

`selete * from student`这种被诊断为指令

但是用plsql代码块包括起来的字符串被诊断为代码。指令和代码块分别用不同的解释器运行

```sql
set serveroutput on --在终端打印

declare
--定义区 不允许出现语句
begin
--代码区
exception
--异常区
end;
```

```sql
set serveroutput on 
declare
  age int;
begin
   select sage into age from student where sname='陈杰';
   dbms_output.put_line(age);
end;
```

###### (2) 取址符

```sql
--两种写法
set serveroutput on
declare
  nm varchar(20):='&姓名';
  sno int:=&学号;
begin
  nm:='张三';
  select sname into nm from student where sid=sno;
  dbms_output.put_line(nm);
end;

set serveroutput on
declare
  nm varchar(20):='&姓名';
begin
  nm:='张三';
  select sname into nm from student where sid=&学号;
  dbms_output.put_line(nm);
end;
```

###### (3) 同步数据类型

```sql
set serveroutput on
declare
  nm student.sname%type;
begin
  nm:='张三';
  select sname into nm from student where sid=&学号;
  dbms_output.put_line(nm);
end;
```

###### (4) 多个赋值

```sql
set serveroutput on
declare
    rcd student%rowtype;
begin
    select * into rcd from student where sid=&学号;
    dbms_output.put_line(rcd.sname||'的性别是'||rcd.ssex);
end;
```

练习：

人机交互获取课程名，编写代码找出此课程的最高分以及对应的学生姓名，并列第一的话也只显示一个人

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


set serveroutput on 
declare
    nm student.sname%type;
    cn course.cname%type:='&课程名';
    rcd mark%rowtype;
begin
	select * into rcd from (
       select * from mark where cid=(select cid from course where cname=cn)
        order by cmark desc
    ) where rownum=1;
    select sname into nm from student where sid=rcd.sid;
    dbms_output.put_line(nm||'取得'||cn||'的最高分'||rcd.cmark);
end;
```

人机交互获取课程名，打印这门课前5名学生的姓名和对应成绩

```sql
set serveroutput on
declare
    nm student.sname%type;
    rcd mark%rowtype;
begin
    for rcd in (
      select *  from (
         select * from mark where cid=(select cid from course where cname='&课程名')
         order by cmark desc
      ) where rownum<=5
    ) loop
       select sname into nm from student where sid=rcd.sid;
       dbms_output.put_line(nm||':'||rcd.cmark);
      end loop;
end;
```



打印均分前5名的学生姓名和其均分，精确到小数点后两位即可

```sql
create view v_avg
as select sid,trunc(avg(cmark),2) amk from mark group by sid order by amk desc

set serveroutput on
declare
    rcd v_avg%rowtype;
    nm student.sname%type;
begin
    for rcd in (
       select * from v_avg where rownum<=5 
    ) loop
        select sname into nm from student where sid=rcd.sid;
        dbms_output.put_line(nm||'的均分'||rcd.amk);
      end loop;
end;
```

###### (5) 循环

```sql
set serveroutput on 
declare
   s int:=0;
   i int:=0;
begin
   while i<=100 loop
   s:=s+i;
   i:=i+1;
   end loop;
   dbms_output.put_line(s);
   
   s:=0;
   for i in 1..100 loop
     s:=s+i;
   end loop;
   dbms_output.put_line(s);
end;
```

###### (6) if

```sql
if  then

elsif

end if;
```

```sql
set serveroutput on 
declare
    rcd student%rowtype;
    sno int:=1001;
begin
    loop
      select * into rcd from student where sid=sno;
--      if rcd.sname='陈天乐' then
--        exit;
--      end if;
      exit when rcd.sname='陈天乐';
      dbms_output.put_line(rcd.sname);
      sno:=sno+1;
    end loop;
end;
```

###### (7) 异常

**有三种类型的异常错误** **:**
 **预定义 ( Predefined )错误****
 ORACLE预定义的异常情况大约有24个。对这种异常情况的处理，无需在程序中定义，由ORACLE自动将其引发。


 **非预定义 ( Predefined )错误**
 即其他标准的ORACLE错误。对这种异常情况的处理，需要用户在程序中定义，然后由ORACLE自动将其引发。

 **用户定义(User_define) 错误**
 程序执行过程中，出现编程人员认为的非正常情况。对这种异常情况的处理，需要用户在程序中定义，然后显式地在程序中将其引发。

**预定义的异常处理**

![IMG_256](file:///C:/Users/86151/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)



对于==预定义异常==情况的处理，只需在PL/SQL块的异常处理部分，直接引用相应的异常情况名，并对其完成相应的异常错误处理即可。

==非预定义的异常处理== 
 对于这类异常情况的处理，首先必须对非定义的ORACLE错误进行定义 
 步骤如下：
 在PL/SQL 块的声明部分定义异常情况：
 <异常情况> EXCEPTION; 
 将其定义好的异常情况，与标准的ORACLE错误联系起来，使用EXCEPTION_INIT语句 
 PRAGMA EXCEPTION_INIT(<异常情况>, <错误代码>)； 
 在PL/SQL 块的异常情况处理部分对异常情况做出相应的处理。
 deptno_remaining EXCEPTION;
 PRAGMA EXCEPTION_INIT(deptno_remaining, -2292);
 /* -2292 是违反一致性约束的错误代码 */

 ==用户自定义的异常处理==
 用户定义的异常错误是通过显式使用 RAISE 语句来触发。当引发一个异常错误时，控制就转向到 EXCEPTION块异常错误部分，执行错误处理代码。
 对于这类异常情况的处理，步骤如下 ：

 ```sql
 在PL/SQL 块的声明部分定义异常情况 ：
  <异常情况> EXCEPTION; 
  RAISE <异常情况> 
  在PL/SQL 块的异常情况处理部分对异常情况做出相应的处理
 ```

 ==RAISE_APPLICATION_ERROR函数==
 ```sql
 exception
  when others then
  raise_application_error(-20001 , 'department '||v_deptid||' does not exists');
 end;
 ```

```sql
set serveroutput on 
declare
    nm student.sname%type:='&学生姓名';
    rcd student%rowtype;
    cnt int:=1;
begin
    select * into rcd from student where sname=nm;
    dbms_output.put_line(nm||'的学号是'||rcd.sid);
exception
    when no_data_found then
      dbms_output.put_line('查无此人');
    when too_many_rows then
      dbms_output.put_line('姓名为'||nm||'的人不止一人');
      for rcd in (select * from student where sname=nm) loop
        dbms_output.put_line('第'||cnt||'个'||nm||'的学号是'||rcd.sid);
        cnt:=cnt+1;
      end loop;
    when others then
      dbms_output.put_line('未知异常');
end;
```

***练习：***

*人机交互输入姓名，课程名，成绩*
*请编写数据库代码向Mark表插入此记录*
*但是若指定姓名的学生不存在，则提示“没有XX学生”*
*若指定名称的课程不存在，则提示“没有XX课程”*
*若成绩不在0~100之间，则提示“非法成绩，请输入正确的成绩”*

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

##### 2.9.2 事务

数据库事务的四个特点：

==原子性==：

一致性：

隔离性：

持久性：

<img src="C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230317162146356.png" alt="image-20230317162146356" style="zoom: 50%;" />

##### 2.9.3 存储过程

存储过程只编译一次,存储过程没有返回值。

```sql
create or replace procedure showmaxage(sx char) as
  age student.sage%type;
begin
  select max(sage) into age from student where ssex=sx;
  dbms_output.put_line(age);
end;

set serveroutput on;
begin
  showmaxage('女');
end;
```

```sql
create or replace procedure showmaxage(sx char,age out int) as
begin
  select max(sage) into age from student where ssex=sx;
end;

set serveroutput on;
declare
  ag int;
begin
  showmaxage('女',ag);
  dbms_output.put_line(ag);
end;
```

编写存储过程，输入班级，课程名，返回这个班级这门课的最高分，最低分

```sql
drop procedure showcmark;
create or replace procedure showcmark(sna varchar,cna varchar,mxmark out number,mimark out number)
as
begin
  select max(cmark),min(cmark) into mxmark,mimark from mark 
    where cid in (select cid from course where cname=cna) 
    and sid in (select sid from student where sclass=sna); 
end;

set serveroutput on 
declare
  sna student.sclass%type:='&班级';
  cna course.cname%type:='&课程';
  mxmark mark.cmark%type;
  mimark mark.cmark%type;
begin
  showcmark(sna,cna,mxmark,mimark);
  dbms_output.put_line(mxmark||mimark);
end;
```

##### 2.9.4 函数

函数要有返回值

```sql
create or replace function () as
begin
return
end;
```

##### 2.9.5 触发器

触发器的触发条件：

1.触发的时间: before after

2.触发的对象: on 表名

3.触发的行为(action): update,insert,delete (select不可以)

4.触发的范围: 行级还是表级触发器

##### 2.9.6 游标

==游标是控制循环处理表格数据的一种方式方法==



## Java

### 1. Java入门

#### 1.1 Java运行过程

<img src="C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230323104519864.png" alt="image-20230323104519864" style="zoom:50%;" />编写代码

1.创建java文件

2.编译

将java文件编译成字节码，编译后的文件叫class文件

3.类装载ClassLoader

类装载的功能是为执行程序寻找和装在所需要的类

4.字节码校验

对class文件代码进行校验，JVM将代码输入一个字节校验码校验器测试

校验器对程序代码进行四遍校验

5.解释

解释器解释class文件

6.运行

由运行环境(jre)中的Runtime对代码进行运行

#### 2.1 Java技术三大特性

##### 2.1.1 虚拟机

<img src="C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230323113135882.png" alt="image-20230323113135882" style="zoom:50%;" />



##### 2.1.2 垃圾回收

##### 2.1.3 代码安全

<img src="C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230323113213141.png" alt="image-20230323113213141" style="zoom:50%;" />

### 2. 基础语法

#### 2.1 关键字

<img src="C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230323113915682.png" alt="image-20230323113915682" style="zoom:50%;" />

#### 2.2 标识符

标识符命名规则

<img src="C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230323114050158.png" alt="image-20230323114050158" style="zoom:50%;" />

#### 2.3 数据类型

##### 2.3.1 Java中的数据类型

<img src="C:\Users\86151\AppData\Roaming\Typora\typora-user-images\image-20230323114152044.png" alt="image-20230323114152044" style="zoom:50%;" />

==基本数据类型存的是数值==

==引用数据类型存的是地址==

涉及到常量，编译器就在内存里创建

涉及到变量，编译器不做，解释器做

### 面试问题

1.java虚拟机(jvm),java运行环境(jre),java工具开发包(jdk)三者关系

jdk>jre>jvm

2.垃圾回收
