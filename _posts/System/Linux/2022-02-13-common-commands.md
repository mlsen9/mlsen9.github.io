---
title: Linux常用命令整理
date: 2022-02-13 11:53:33 +0800
categories: [System, Linux]
tags: [linux-commands]
---

### tar 用来压缩和解压文件
```bash
tar [选项]... 目标文件名 源文件/目录
-z 识别.gz 格式
-j 识别.bz2 格式
-c 打包
-v 显示打包过程
-f 指定产生后的文件名
-x 解打包

打包 tar -cvf 打包文件名  源文件
解包 tar -xvf 打包文件名
压缩同时打包     tar -zcvf 压缩文件名  源文件
解压缩时同时解打包  tar -zxvf 压缩文件名
```

### ps 查看进程
```sh
ps [-aAcdefHjlmNVwy][acefghLnrsSTuvxX]
-A  显示所有进程。
-e  此参数的效果和指定"A"参数相同。
-f  显示UID,PPIP,C与STIME栏位。
-u<用户识别码>  此参数的效果和指定"-U"参数相同。
-U<用户识别码>  列出属于该用户的进程的状况，也可使用用户名称来指定。
 a  显示现行终端机下的所有进程，包括其他用户的进程。
 u  以用户为主的格式来显示进程状况。
 x  显示所有进程，不以终端机来区分。

ps aux   #不区分终端，显示所有用户的所有进程
ps -e    #显示所有进程
ps -ef   #显示所有进程的UID,PPIP,C与STIME栏位
ps -u zhangy   #显示zhangy用户的所有进程
```

### netstat 显示网络连接，路由表，接口状态，伪装连接，网络链路信息和组播成员组。
```sh
-c, --continuous 将使  **netstat**  不断地每秒输出所选的信息。
-e, --extend     显示附加信息。使用这个选项两次来获得所有细节。
-o, --timers     包含与网络定时器有关的信息。
-p, --programs   显示套接字所属进程的PID和名称。
-l, --listening  只显示正在侦听的套接字(这是默认的选项)
-a, --all        显示所有正在或不在侦听的套接字。

netstat -a | more  #列出所有端口 (包括监听和未监听的)
netstat -at        #列出所有TCP端口
netstat -au        #列出所有UDP端口
netstat -r         #显示核心路由信息
netstat -i         #显示网络接口列表
netstat -tpnl      #显看已连接的TCP端口，以及PID
netstat -anp | grep 3306 -c   #查看3306 端口(mysql)的链接数
netstat -alp | grep 8080      #找出运行在指定端口的进程
```

### chown 更改每个文件的所有者和/或所属组
```sh
chown [选项]... [所有者][:[组]] 文件...
chown [选项]... --reference=参考文件 文件...
-R, --recursive  递归处理所有的文件及子目录
-v, --verbose    为处理的所有文件显示诊断信息

chown -R gy:gy www      #将www目录，所属用户和组改为gy,gy
chown gy:gy nginx.conf  #将nginx.conf所属用户和组改为gy,gy
chown root nginx.conf   #将nginx.conf，所属用户改为root
```

### chmod 更改每个文件的模式更改为指定值
```sh
chmod [选项]... 模式[,模式]... 文件...
chmod [选项]... 八进制模式 文件...
chmod [选项]... --reference=参考文件 文件...
-R, --recursive  递归处理所有的文件及子目录
-v, --verbose    为处理的所有文件显示诊断信息

每种 MODE 都应属于这类形式"[ugoa]*([-+=]([rwxXst]*|[ugo]))+"。
操作对像
   u 文件属主权限
   g 同组用户权限
   o 其它用户权限
   a 所有用户（包括以上三种）
权限设定
   + 增加权限
   - 取消权限
   =  唯一设定权限
权限类别
   r 读权限
   w 写权限
   x 执行权限
   X 表示只有当该档案是个子目录或者该档案已经被设定过为可执行。
   s 文件属主和组id
   l 给文件加锁，使其它用户无法访问
   r-->4
   w-->2
   x-->1

chmod ugo+r nginx_bak.conf        #所有人皆可读取
chmod a+r nginx_bak.conf          #所有人皆可读取
chmod ug+w,o-w nginx_bak.conf     #设为该档案拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入
chmod u+x nginx_bak.conf          #创建者拥有执行权限
chmod -R a+r ./www/               #将www下的所有档案与子目录皆设为任何人可读取
chmod a-x nginx_bak.conf          #收回所有用户的对nginx_bak.conf的执行权限
chmod 777 nginx_bak.conf          #所有人可读，写，执行
chmod +x test                     #所有用户加执行权限

锁定关键文件
chattr +i /etc/passwd /etc/shadow /etc/group /etc/gshadow /etc/inittab
锁定后无法删除
rm -f /etc/passwd
rm: cannot remove ‘/etc/passwd’: Operation not permitted
解锁
chattr -i /etc/passwd /etc/shadow /etc/group

```

### 参考文献

> - [tar 命令详解](http://linux.51yip.com/search/tar){:target="_blank"}
> - [ps 命令详解](http://linux.51yip.com/search/ps){:target="_blank"}
> - [netstat 命令详解](http://linux.51yip.com/search/netstat){:target="_blank"}
> - [awk 命令详解](http://linux.51yip.com/search/awk){:target="_blank"}
> - [chown 命令详解](http://linux.51yip.com/search/chown){:target="_blank"}
> - [chmod 命令详解](http://linux.51yip.com/search/chmod){:target="_blank"}
