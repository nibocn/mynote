# centos-mysql配置步骤

## 常用命令

* 查看是否已安装某软件命令`rpm -qa | grep -i xxxx`。
* 获取某软件包的版本列表`yum list xxxx`或`yum list xxxx --showduplicates`。
* 安装某软件包`yum install xxxx`。
* 移除软件包`yum -y remove xxxx`。

## 替换yum源

> 替换默认的yum源换用国内的镜像源，提高软件下载速度。

* 访问163网易提供的centos镜像源，根据帮助文档替换现有源。URL：
http://mirrors.163.com/.help/centos.html
* 更新系统插件包`yum update`。

## 创建用户

执行命令`useradd cafuc`创建`cafuc`用户。

## 安装mysql

**通过rpm包安装**

* 查看是否已存在mysql相关软件包`rpm -qa | grep mysql`：
```
[root@centos-6 mysql-5.7.5]# rpm -qa | grep mysql
mysql-libs-5.1.73-3.el6_5.x86_64
```
如果存在相关软件包，先移除避免安装时出现相关包冲突：
```
[root@centos-6 mysql-5.7.5]# yum -y remove mysql-libs-5.1.73-3.el6_5.x86_64
```

* 通过mysql官网下载相关包，URL：
http://dev.mysql.com/get/Downloads/MySQL-5.7/MySQL-5.7.5-m15-0.6.m15.el6.x86_64.rpm-bundle.tar

* 解压下载的包，执行命令`rpm -ivh mysql-community-server-5.7.5-0.6.m15.el6.x86_64.rpm`，如果出现
错误，则是缺少依赖，需要先安装相关依赖，如：
```
[root@centos-6 mysql-5.7.5]# rpm -ivh mysql-community-server-5.7.5-0.6.m15.el6.x86_64.rpm
error: Failed dependencies:
    /usr/bin/perl is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    libaio.so.1()(64bit) is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    libaio.so.1(LIBAIO_0.1)(64bit) is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    libaio.so.1(LIBAIO_0.4)(64bit) is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    mysql-community-client(x86-64) = 5.7.5-0.6.m15.el6 is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    mysql-community-common(x86-64) = 5.7.5-0.6.m15.el6 is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    perl(File::Path) is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    perl(Getopt::Long) is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    perl(POSIX) is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
    perl(strict) is needed by mysql-community-server-5.7.5-0.6.m15.el6.x86_64
[root@centos-6 mysql-5.7.5]#
```

* 安装相关依赖库：
```
[root@centos-6 mysql-5.7.5]# yum install libaio
[root@centos-6 mysql-5.7.5]#
[root@centos-6 mysql-5.7.5]# rpm -ivh mysql-community-common-5.7.5-0.6.m15.el6.x86_64.rpm
Preparing...                ########################################### [100%]
   1:mysql-community-common ########################################### [100%]
[root@centos-6 mysql-5.7.5]#
[root@centos-6 mysql-5.7.5]# rpm -iv mysql-community-libs-5.7.5-0.6.m15.el6.x86_64.rpm
Preparing packages for installation...
mysql-community-libs-5.7.5-0.6.m15.el6
[root@centos-6 mysql-5.7.5]#
[root@centos-6 mysql-5.7.5]# rpm -ivh mysql-community-client-5.7.5-0.6.m15.el6.x86_64.rpm
Preparing...                ########################################### [100%]
   1:mysql-community-client ########################################### [100%]
[root@centos-6 mysql-5.7.5]#
[root@centos-6 mysql-5.7.5]# yum install perl
[root@centos-6 mysql-5.7.5]#
```

* 再次执行`rpm -ivh mysql-community-server-5.7.5-0.6.m15.el6.x86_64.rpm`安装mysql server：
```
[root@centos-6 mysql-5.7.5]# rpm -ivh mysql-community-server-5.7.5-0.6.m15.el6.x86_64.rpm
Preparing...                ########################################### [100%]
   1:mysql-community-server ########################################### [100%]
[root@centos-6 mysql-5.7.5]#
```

* 启动服务`service mysqld start`：
```
[root@centos-6 mysql-5.7.5]# service mysqld start
初始化 MySQL 数据库：                                      [确定]
正在启动 mysqld：                                          [确定]
Securing the MySQL server deployment.
.............
```

* 通过进程查看mysql是否成功启动`ps -ef|grep mysql`：
```
[root@centos-6 mysql-5.7.5]# ps -ef|grep mysql
root     10478     1  0 21:49 pts/1    00:00:00 /bin/sh /usr/bin/mysqld_safe --datadir=/var/lib/mysql --socket=/var/lib/mysql/mysql.sock --pid-file=/var/run/mysqld/mysqld.pid --basedir=/usr --user=mysql
mysql    10686 10478  0 21:49 pts/1    00:00:00 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --user=mysql --log-error=/var/log/mysqld.log --pid-file=/var/run/mysqld/mysqld.pid --socket=/var/lib/mysql/mysql.sock
root     10733  1116  0 21:51 pts/1    00:00:00 grep mysql
[root@centos-6 mysql-5.7.5]#
```

* 登录mysql：
```
[root@centos-6 ~]# mysql -u root -p
Enter password:
```
输入root用户对应的密码，在第一次登录时输入的密码可从root用户目录下查看到：
```
[root@centos-6 ~]# cat /root/.mysql_secret
# Password set for user 'root@localhost' at 2014-10-15 21:49:00
k:zWHSeFJF0s
[root@centos-6 ~]#
```

* 修改密码

    第一次登录后都要求修改密码才能继续进行其它操作，具体命令：
    ```
    mysql> SET PASSWORD = PASSWORD('BP&FFhGK!107ou');
    ```
*注：*从MySQL5.6.6增加了密码强度验证插件validate_password，故密码的验证较严格太简单的密码
不会允许通过。

* 远程访问mysql

    通过远程访问mysql时可能出现不能访问的情况：

    * 修改默认数据库`mysql`的`user`用户表信息：
    ```
    mysql> use mysql;
    Reading table information for completion of table and column names
    You can turn off this feature to get a quicker startup with -A
    Database changed
    mysql> update user set host = '%' where user = 'root';
    Query OK, 0 rows affected (0.04 sec)
    Rows matched: 1  Changed: 0  Warnings: 0
    mysql>
    ```

    * 查看防火墙配置是否允许访问`3306`端口：
    ```
    [root@centos-6 ~]# cat /etc/sysconfig/iptables
    # Firewall configuration written by system-config-firewall
    # Manual customization of this file is not recommended.
    *filter
    :INPUT ACCEPT [0:0]
    :FORWARD ACCEPT [0:0]
    :OUTPUT ACCEPT [0:0]
    -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    -A INPUT -p icmp -j ACCEPT
    -A INPUT -i lo -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
    -A INPUT -j REJECT --reject-with icmp-host-prohibited
    -A FORWARD -j REJECT --reject-with icmp-host-prohibited
    COMMIT
    [root@centos-6 ~]#
    ```

    象上面的信息防火墙就只开了`22`端口，所以需要添加打开`3306`端口的信息，修改文件
    `/etc/sysconfig/iptables`，添加如下信息：
    ```
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
    ```

    **最后重启防火墙`service iptables restart`**
