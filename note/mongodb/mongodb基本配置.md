# mongodb基本配置

## 参数配置

在mongodb安装根目录，如：`E:\Software\mongodb`创建`mongodb.cfg`配置文件，内容如下：
```
# 日志文件保存路径
logpath=E:\Software\mongodb\data\log\mongodb.log
# 数据库文件保存路径
dbpath=E:\Software\mongodb\data\db
# 以追加的方式写日志文件，false:每次重新启动都会创建新的日志文件，true:在已有的日志文件中追加内容
logappend=true
```

## 创建windows服务

以`系统管理员身份`执行命令行，输入命令：
`mongod --config "E:\Software\mongodb\mongodb.cfg" --install`

**注：mongod安装时安装路径最好不要存在空格，以免造成windows服务无法启动！**

## 启动&停止windows服务

**注：以`系统管理员身份`执行以下命令**

* 启动：`net start MongoDB`
* 停止：`net stop MongoDB`