# 创建自定义maven项目模板

## 创建项目模板

1. 新建一个普通的maven工程。
2. 在工程根目录下(`pom.xml`文件所在位置)，执行：`mvn archetype:create-from-project`。
3. 在目录：`target\generated-sources\archetype`执行：`mvn install`，将项目模板安装到本地maven仓库。

## 使用创建的项目模板

1. 在任意目录位置新建一个目录，执行命令：`mvn archetype:generate -DarchetypeCatalog=local`，列出本地的项目模板选择相应的`模板编号`即可创建新的项目。

## 使用maven自带的项目模板创建项目

**方法1：**

执行命令`mvn archetype:create`列出已存在的maven项目模板，根据具体提示进行操作即可。

**方法2：**

执行命令`mvn archetype:create -DgroupId=com.demo -DartifactId=simple -DpackageName=com.demo.simple -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false`

> 参数说明：

> `-DgroupId`：groupId（项目或者组织的唯一标志）。

> `-DartifactId`：artifactId（项目的通用名称）。

> `-DpackageName`：项目的包名。

> `-DarchetypeArtifactId`：模板名称，如：`maven-archetype-webapp`表示创建一个标准的maven web项目。如果要创建一个普通java项目可不填写此参数。

> `-DinteractiveMode`：是否已交互模式进行，如果是false的话就会采用默认设置建立项目。
