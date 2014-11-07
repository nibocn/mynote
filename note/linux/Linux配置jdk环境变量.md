# Linux配置jdk环境变量

* 在`/etc/profile`文件中增加配置信息：
```
export JAVA_HOME=/usr/share/jdk1.6.0_14
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```
* 执行`source /etc/profile`使配置生效，**注：在profile文件里面的配置信息是需要重启系统或注销才生效的。**
