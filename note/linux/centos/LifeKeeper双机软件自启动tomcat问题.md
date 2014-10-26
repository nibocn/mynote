# LifeKeeper双机软件自启动tomcat问题

## 具体现象

手动启动tomcat，应用程序运行完全正常。

但在使用双机软件LifeKeeper配置自启动tomcat时出现tomcat启动时应用程序报错。根据错误信息怀疑是由于使用的jdk和tomcat环境变量出错，导致tomcat启动时应用程序报错。（*注：如果只跑一个单独的tomcat不加载任何的应用程序可能就不会出现问题。*）

## 解决方法

在tomcat的bin目录`tomcat/bin`修改`catalina.sh`文件，在文件开头处增加`JAVA_HOME`和`CATALINA_HOME`的配置：
```
JAVA_HOME='/home/jdk1.7.0_25'
CATALINA_HOME='/home/user1/tomcat-7.0.42'
```

增加上面的配置信息后，再通过双机软件启动就正常了。
