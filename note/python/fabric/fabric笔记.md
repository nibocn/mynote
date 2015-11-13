
执行命令后`failed`属性判断是否成功，执行时必须设置`warn_only`参数为`True`才能通过代码来进行判断，否则如果执行失败会自动退出。**注：**设置了`warn_only`参数为`True`后如果命令执行失败也不会自动停止！

```
result = run('mvn clean install', warn_only=True)
if result.failed:
    print red(u'打包失败')
    abort("中止！")
else:
    print green(u'打包成功')
```

`confirm`方法以命令行交互的方式让用户选择`yes/no`进行继续操作。

```
if result.failed and not confirm("Tests failed. Continue anyway?"):
    abort("Aborting at user request.")
```

隐藏过多的输出信息，可在执行`fab xxxx`命令时添加`fab --hide=output xxxx`的参数。

fabric常用参数：

```
env.host           -- 主机ip，当然也可以-H参数指定
env.password       -- 密码，打好通道的请无视
env.roledefs       -- 角色分组，比如：{'web': ['x', 'y'], 'db': ['z']}

fab -l             -- 显示可用的task（命令）
fab -H             -- 指定host，支持多host逗号分开
fab -R             -- 指定role，支持多个
fab -P             -- 并发数，默认是串行
fab -w             -- warn_only，默认是碰到异常直接abort退出
fab -f             -- 指定入口文件，fab默认入口文件是：fabfile/fabfile.py
更多请参考：fab --help
```
