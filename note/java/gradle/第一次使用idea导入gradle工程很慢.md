# 第一次使用idea导入gradle工程很慢

在idea中第一次导入一个gradle工程的项目时发现很慢，具体的原来是由于去下载了gradle软件包造成的。如图：

![](http://img.hb.aicdn.com/101719f39cef1961c61448b15de3b46dd1c9c11bea22-az9iWm)

> 这是因为gradle提供了一个叫`Gradle Wrapper`的东西，其作用就是当检测到我们没有gradle运行时环境时就会自动去gradle的官网下载`对应版本`的gradle软件包，**注：idea在下载gradle版本时是没有办法控制的至少目前我没有找到相关的配置，我使用idea13.1.5的版本默认下载的gradle版本为2.0**。
>
>但我们一般的使用习惯是先去下载gradle软件包，并在系统配置好环境，然后在命令行运行。*类似象maven的操作方式，但奇怪的就是当导入一个工程到idea的时候却不能使用我们系统配置的环境，这和maven就有一定的差别（也或许是我配置的有问题？）* **那么问题就来在我们使用idea导入gradle工程时（第一次）如何快速获取到gradle软件包（使用已经下载好的）？并能指定我们工程使用的gradle版本呢？**

## 指定工程使用的gradle版本

在工程根目录下的`build.gradle`文件中添加一个任务名叫`wrapper`的任务：
```
task wrapper(type: Wrapper) {
    // 声明gradle使用的版本
    gradleVersion = '2.1'
    // 默认的gradlew命令太长简化为'g'
    scriptFile = 'g'
}
```

在命令行执行`gradle wrapper`任务：
```
Leo@Leo-PC /e/workspace/gradle/gradle-learning/01-gradle-first-try
$ gradle wrapper
:wrapper

BUILD SUCCESSFUL

Total time: 4.168 secs
```

执行成功后会在工程根目录生成如下文件及目录：

![](http://img.hb.aicdn.com/cc431fcbd89227e6ffcb1d4b770d59fcd8d07746a945-xtYZZt_fw658)

**其作用就是：**

当我们和其他人协作开发时别人机器上却没有 Gradle 运行时，而别人需要执行相关gradle命令时可以在工程根目录下执行`./g xxx`来代替，而gradle这时就会检测如果没有下载gradle的软件包就会先去官网下载并保存到我们的`$USER_HOME/.gradle/wrapper/dists`目录下。我们可以查看下刚才执行`gradle wrapper`后生成的`gradle`目录下的`gradle-wrapper.properties`文件信息：
```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.1-bin.zip
```
`distributionUrl`属性就是去下载gradle软件包的地址。

下载成功后我们就可以使用`./g xxx`去执行gradle相关的命令:
```
Leo@Leo-PC /e/workspace/gradle/gradle-learning/01-gradle-first-try
$ gradle helloWorld
:helloWorld
Hello World!

BUILD SUCCESSFUL

Total time: 2.521 secs

Leo@Leo-PC /e/workspace/gradle/gradle-learning/01-gradle-first-try
$ ./g helloWorld
:helloWorld
Hello World!

BUILD SUCCESSFUL

Total time: 2.718 secs
```

由于我本机也配置了gradle的运行环境所以可以看到用`gradle helloWorld`和`./g helloWorld`执行的结果是一样的，后者就是当我们自己电脑上没有gradle运行环境时可以这样执行。

**我们在第一次导入gradle工程到idea时也执行了这样类似的操作，所以会导致我们第一次感觉特别慢！**

## 拷贝已下载的gradle包

> 通过上面的说明我们可以看到其实gradle这样的设计理念是好的，因为对于一个不了解gradle如何配置怎么运行的用户来说，只需要告诉他一个简单的命令就可以执行我们的gradle（对比我们使用maven的经验）。但是这还是免不了要去下载gradle包（只是gradle帮我们做了），而对于开发人员来说一般都会先去下载一个完整的包然后配置好环境。那么我们如何利用我们已经下载好的gradle包呢？

我们上面也说了gradle下载软件包时会将下载的包放到`$USER_HOME/.gradle/wrapper/dists`目录下，所以我们其实只要把对应的包拷贝到相关目录就可以了。如图：

![](http://img.hb.aicdn.com/32e94a8f6b69ff7c9ceecddbbd8a492939fbb3f2880c-AjeDdQ_fw658)

**注：**如果发现没有此目录可以先随便执行一次`./g xxx`任务，`xxx`为你自己实际的一个具体任务，这时gradle就会去下载包并生成对应目录，然后马上终止就好了（这时目录应该就存在了）。并且`2pk0g2l49n2sbne636fhtlet6a`这串字符请以你自己实际的为准。

## 在idea中重新导入gradle工程

再次使用idea重新导入gradle工程就不会再提示去下载gradle包了，因为我们已经手动拷贝到对应的目录下了。如图：

![](http://img.hb.aicdn.com/a255d6de4516aa8b8871c82ddd520168a726f3dece5d-WOy0av_fw658)

## 注意事项

* 我们在idea中编辑`build.gradle`文件时可能会出现，如下图所示的提示：
    ![](http://img.hb.aicdn.com/cee91cbfb7cce49adc13900eec86904d612ded0b90ca-CI3GyB)

    如果点击了**OK**又可能会去下载，如图：
    ![](http://img.hb.aicdn.com/1377bcc65ad33f1f9135b8021545a892a892a51d2bb4-KFjJ9t_fw658)

    **注：**可以看到包的名字变了，还记得我们上面说到的`gradle-wrapper.properties`文件吗？`gradle-wrapper.properties`文件里面配置去下载的包为`gradle-2.1-bin.zip`，而现在去下载的为`gradle-2.1-all.zip`，我们可以取消掉不下载（这样会比较慢，通过bt工具下载，完了拷贝到对应目录就好了）。

* 配置`Gradle Wrapper`并非是必须的，也可以不在工程里面配置，建议还是这样做。因为这样可以统一每个开发人员使用的gradle版本是一致的。如果不配置在已经有gradle环境的电脑的命令行执行gradle命令应该是没什么问题的。只是还是要注意在使用idea等开发工具导入gradle工程时gradle版本号就不是我们指定的`2.1`什么的呢，这时呢你还是可以在`$USER_HOME/.gradle/wrapper/dists`目录下去看下idea默认下载的gradle版本是多少，然后你就自己去下载对应的版本号拷贝到对应目录就好了。如：我上面说的我自己使用的idea13.1.5他默认下载的gradle版本为`2.0`

