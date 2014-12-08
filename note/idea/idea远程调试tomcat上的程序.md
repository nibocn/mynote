# idea远程调试Tomcat上的程序

## 通过Remote配置

* 选择菜单`Run-->Edit Configurations...`，点击`+`按钮添加一个`Remote`。

![](http://img.hb.aicdn.com/e37b226b30725f802e6297c65464b192e56f9826889b-fcqmqd_fw658)

* 配置远程服务器地址及监听端口和本地的模块。

![](http://img.hb.aicdn.com/00f6773707ffede3ddd79128ae17c40105d00f08b795-pC8GMR)

**Host：**远程Tomcat服务器地址。

**Port：**远程Tomcat服务器监听端口。（注：这里的端口不是Tomcat的Http访问端口）

**module's classpath：**本地对应的服务器端模块。

## 通过Remote Tomcat Server配置

* 选择菜单`Run-->Edit Configurations...`，点击`+`按钮添加`Tomcat Server-->Remote`。

![](http://img.hb.aicdn.com/1565874dbe030d11f0ec38639d1732b88bb49d5580bc-kj5rTS_fw658)

* 配置远程Tomcat的访问地址及端口。

![](http://img.hb.aicdn.com/3e6fe9cb9477402a6292e1f5e56d7d97016554458032-QIHFFj_fw658)

**Host：**远程Tomcat服务器地址。

**Port：**远程Tomcat的Http访问端口。（注：这里的端口指的是在浏览器访问的端口）

**注：**这里不用配置具体的模块。

* 配置监听端口。

![](http://img.hb.aicdn.com/a4bb64dc7eff92c7d76b6cec7e586d92f3739b3d714a-l3IAgp_fw658)

## 配置服务器端Tomcat

* 在Tomcat的`bin/catalina.sh`文件中，找到`JAVA_OPTS`变量，并增加配置参数：`-Xdebug -Xrunjdwp:transport=dt_socket,address=9999,server=y,suspend=n"`

**注：**

如果JDK版本较低（1.3.x或更低）可能还需要追加参数`-Xnoagent -Djava.compiler=NONE`。

其中`address`参数一定要和前面在IDEA里面配置的一致。

最后配置结果：
```
JAVA_OPTS="$JAVA_OPTS -Xdebug -Xrunjdwp:transport=dt_socket,address=9999,server=y,suspend=n"
```

**各参数说明：**

```
-Xdebug
启用调试特性
-Xrunjdwp
启用JDWP实现，它包含若干子选项：
transport=dt_socket
JPDA front-end和back-end之间的传输方法。dt_socket表示使用套接字传输。
address=9999
JVM在9999端口上监听请求。
server=y
y表示启动的JVM是被调试者。如果为n，则表示启动的JVM是调试器。
suspend=y
y表示启动的JVM会暂停等待，直到调试器连接上。

suspend=y这个选项很重要。如果你想从Tomcat启动的一开始就进行调试，那么就必须设置suspend=y。
```

* 配置完成重启Tomcat

重新启动Tomcat应该就能在日志信息中看到打印的日志信息，`Listening for transport dt_socket at address: 9999`表示正在监听9999端口。

## 在本地Debug启动Remote或者Tomcat Server Remote

通过上面配置的Remote和Tomcat Server Remote可以选择其中的一种方式进行启动（**Debug启动**），并打断点看是否能正常跟踪。

![](http://img.hb.aicdn.com/995edf3828a84af203d98228415f65da2c17536b4e8c-ArCFZD_fw658)

启动后的效果：

![](http://img.hb.aicdn.com/e8f3346998c62979fd458f56bc0ac1af877e5b093343-mEbwZ2_fw658)
