# Gradle 2 用户指南

## Java Quickstart 快速开始 Java
### A basic Java project 基本的 Java 项目
**添加java插件**

`apply plugin: java`

> 这个就是 定义一个 Java 项目的全部。它会将 Java 插件应用到项目中，并且添加很多 task。

>Gradle 会在 src/main/java 目录下寻找产品代码，在 src/test/java 寻找测试代码 。 另外在 src/main/resources 包含了资源的 JAR 文件, src/test/resources 包含了运行测试。所有的输出都在 build 目录下，JAR包 在 build/libs 目录下

### Building the project 构建项目
**常用task**

`clean`：删除`build`目录，移除所有构建的文件。

`assemble`：编译打包代码，但不运行单元测试。其他插件带给这个 task 更多特性，比如如果你使用 War 插件，task 将给 project 构建 WAR 文件。

`check`：编译测试你的代码。其他插件带给这个 task 提供更多检查类型。比如，你使用 checkstyle 插件, 这个 task 会在你的代码中 执行 Checkstyle。

### External dependencies 外部依赖
**添加Maven仓库地址**
```
repositories {
    // Maven中央仓库
    mavenCentral()
    // 本地仓库
    mavenLocal()
}

```
**添加依赖**
```
dependencies {
    // 依赖的写法有多种，这里的写法格式为：
    // xxx 'group:name:version'
    // group与maven的groupId对应，name与mave的
    compile 'commons-collections:commons-collections:3.2'
    testCompile 'junit:junit:4.9'
}
```
`compile`：编译项目的生产源所需的依赖。

`runtime`：生产类在运行时所需的依赖。默认情况下，还包括编译时的依赖。

`testCompile`：编译项目的测试源所需的依赖。默认情况下，还包括产品编译类和编译时的依赖。

`testRuntime`：运行测试所需的依赖。默认情况下，还包括 编译，运行时和测试编译的依赖。

### Publishing the JAR file 发布JAR文件
发布jar包到本地：
```
uploadArchives {
    repositories {
       flatDir {
           dirs 'repos'
       }
    }
}
```

## Using the Gradle Command-Line 使用 Gradle 命令行
### Excluding tasks 排除 task
可以通过 `-x` 命令行来排除 task 被执行，如：`gradle dist -x test`

### Continuing the build when a failure occurs 发生故障时继续构建
通过执行 `--continue` ，Gralde 会执行每一个 task。

### Task name abbreviation 任务名称缩写
> 当你试图执行某个 task 的时候,无需输入 task 的全名.只需提供足够的可以唯一区分出该 task 的字符即可。例如,上面的例子你也可以这么写, 用 gradle di 来直接调用 dist 。同时也可以应用在驼峰的 task 名称。如，执行 compileTest 时，运行 gradle compTest 或者 gradle cT 都可以。

### Selecting which build to execute 选择要执行的构建
> 调用 gradle 命令时,默认情况下总是会在当前目录下寻找构建文件（译者注：首先会寻找当前目录下的 build.gradle 文件,以及根据settings.gradle 中的配置寻找子项目的 build.gradle ）。

可以使用`-b`参数选择其他的构建文件,并且当你使用此参数时 settings.gradle 将不会被使用。如：`gradle -q -b subdir/myproject.gradle hello`

或者，您可以使用`-p`选项来指定要使用的项目目录。多project的构建时应使用 -p 选项来代替 -b 选项。如：`gradle -q -p subdir hello`

### Listing project dependencies 依赖列表
执行`gradle dependencies`会列出项目的依赖列表,所有依赖会根据任务区分,以树型结构展示出来。

可以通过`--configuration`可选参数来实现查看`compile`、`runtime`、`testCompile`等阶段的不同依赖情况。如：`gradle dependencies --configuration compile`

执行`gradle -q webapp:dependencyInsight --dependency`可以查看指定的依赖情况，也可增加`--configuration`参数。如：`gradle dependencyInsight --dependency junit --configuration testCompile`

## Writing Build Scripts 编写构建脚本
### Local variables 局部变量
局部变量是用`def`关键字声明的。如：
```
def dest = "dest"

task copy(type: Copy) {
    from "source"
    into dest
}
```

### Extra properties 额外属性
额外的属性可以通过所属对象的`ext`属性进行添加，读取和设置。如：
```
ext {
    springVersion = "3.1.0.RELEASE"
    emailNotification = "build@master.org"
}

sourceSets.all { ext.purpose = null }

sourceSets {
    main {
        purpose = "production"
    }
    test {
        purpose = "test"
    }
    plugin {
        purpose = "production"
    }
}

task printProperties << {
    println springVersion
    println emailNotification
    sourceSets.matching { it.purpose == "production" }.each { println it.name }
}
```
> 额外属性在任何能够访问它们所属的对象的地方都可以被访问，这使它们有着比局部变量更广泛的作用域。父项目上的额外属性，在子项目中也可以访问。
