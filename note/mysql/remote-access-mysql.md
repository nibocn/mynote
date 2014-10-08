# remote-access-mysql

+ 初次安装mysql成功后由于权限问题一般从远程是无法直接访问mysql服务的；
+ 可通过在安装mysql服务的主机上登录mysql，然后在mysql库的user表中增加允许远程访问的配置；
+ 在user表中将已有的root用户信息的host字段(原来的值一般为localhost)更新为"%"，以一种通配符的方式重新进行设置；
+ 最后记得执行命令`flush privileges;`