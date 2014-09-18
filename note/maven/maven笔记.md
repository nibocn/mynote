# maven笔记

## 常用参数

+ `-U`：该参数能强制让Maven检查所有SNAPSHOT依赖更新，确保集成基于最新的状态，如果没有该参数，Maven默认以天为单位检查更新，而持续集成的频率应该比这高很多。
+ `-e`：如果构建出现异常，该参数能让Maven打印完整的stacktrace，以方便分析错误原因。
+ `-C`：如果校验码不匹配的话，构建失败。
> Maven仓库为每个存储在仓库里的构件维护一个MD5 和 SHA1 校验码。如果构件的校验码不匹配下载的构件，Maven默认被配置成告警终端用户。如果传递-C 选项，当遇到带着错误校验码的构件，会引起Maven构建失败
+ `-DskipTests`：不执行测试用例，但编译测试用例类生成相应的class文件至target/test-classes下。
+ `-Dmaven.test.skip=true`：不执行测试用例，也不编译测试用例类。
+ `updatePlocy`：用来配置maven从远程仓库检查更新的频率，默认的值为daily,表示每天检查一次。具体配置：
```
<pluginRepository>
    <id>oschinaRepository</id>
    <name>local private nexus</name>
    <url>http://maven.oschina.net/content/groups/public/</url>
    <releases>
        <enabled>true</enabled>
        <updatePolicy>daily</updatePolicy>
    </releases>
    <snapshots>
        <enabled>true</enabled>
    </snapshots>
</pluginRepository>
```