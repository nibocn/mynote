# MySql Can't connect to MySQL server on 'localhost' (10038) 错误解决方法
@(MySQL)[note|mysql]

在`/etc/mysql/my.cnf`文件中注释掉`bind-address=127.0.0.1`的参数配置，并重启服务。
