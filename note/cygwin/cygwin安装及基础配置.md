# cygwin基础配置

@(Cygwin)[note|cygwin|cygwin配置]

## 安装Cygwin
* 下载地址：https://cygwin.com/install.html **根据自己的操作系统情况选择x86(32位)或x86_64(64位)。**
* 默认从网络安装，点击下一步
![](http://img.hb.aicdn.com/0b0b54d8deeb306a37a15baec7cff5b524cd2e8136d5-w0j8LN_fw658)
* 选择Cygwin在本地的安装路径
![](http://img.hb.aicdn.com/9de47274a16a37a453480b073a5a4250ac15e63187fd-7cWcO9_fw658)
* 选择下载的软件包存放的路径
![](http://img.hb.aicdn.com/1ad1a0e18c6bd4cd79ed43d5b127995cccb753fe6143-MqBa7b_fw658)

  *注：* **软件包的存放路径要与Cygwin的安装路径区分开**。软件包比较有用的一个作用是当我们想再次重新安装Cygwin时，如果已经存在已经下载过的软件包的话，那我们在第一步选择安装方式时就可以选择从本地安装，即`Install from Local Directory`。这样速度上就会比从网络下载包要快得多，并且原来你已经安装过的软件包也不用再一个一个去找了。

* 选择网络连接，使用默认即可
![](http://img.hb.aicdn.com/58b52fc7f69f533d56b232ecca9551eefaf5e33e5d1b-cj42yw_fw658)
* **选择软件包的下载站点**，因为某些不可抗的原因(都懂得)，建议选择一个国内的镜像源下载，速度较快
> 阿里源：http://mirrors.aliyun.com/cygwin/
>
> 163源：http://mirrors.163.com/cygwin/

  选择`Add`按钮，并在已添加的列表中选择添加的下载源，然后`下一步`

  ![](http://img.hb.aicdn.com/a96f211639c5e8fe16b064cd12803222e091bec4823f-dMRDgV_fw658)
* **第一次安装，可不选择具体的软件包进行安装**，直接`下一步`即先安装一个基本的Cygwin环境，**以后根据需要再选择安装具体的软件包**，如下图
![](http://img.hb.aicdn.com/2de61bc39196113ae72b1b374c0ab6c4431e55f11a885-cucCwZ)
* 最后就可以开始安装Cygwin了

## 配置Cygwin
### 配置Git环境
> 完成了Cygwin的安装后，就可以开始安装Git了。

* 再次点击运行下载的Cygwin安装程序，不要诧异。进入到软件包选择列表。输入关键字`git`搜索相关软件包。
![](http://img.hb.aicdn.com/c4a499750881b8c7a22791c9016cfb6cfcce3cc89c5d-PiUaOT)

  *注：*一定要选择`Install`，然后`下一步`。

* 安装完成后在Cygwin中输入命令：`git --version`确认git是否安装成功。
![](http://img.hb.aicdn.com/06313bd194903b60e2efb299a9d911ba441411c94f82-hUMt7x_fw658)

### 配置Cygwin基础环境
> 这里主要是替换默认的bash改用zsh，bash与zsh的差别可网上搜索。

#### 安装zsh
* 首先安装zsh，安装方式和安装git一致
![](http://img.hb.aicdn.com/959518437936e5a3065844f4373d39d955cedde162c7-D11mL3_fw658)
* 修改Cygwin默认启动的shell为zsh，选中Cygwin的快捷图标`右键-->属性`，在`目标`中增加参数`-e /bin/zsh`
![](http://img.hb.aicdn.com/b10495d7b9f8e25a4b2b17a9d09ec2d94777b8b0a41d-j9cFwp_fw658)
* 完成重新打开Cygwin，应该就会发现显示的信息会有一些变化了。
* 感受下zsh的cd命令，是不是感觉比bash酸爽多了。这还没完下一个要配置的神器是oh-my-zsh。它俩搭配起来就根本停不下来。
![](http://img.hb.aicdn.com/cb9604344b6a15c294c419b223c6d2024713698e11825-f6dmGv)

#### 配置oh-my-zsh
> 安装完了zsh后，再配合oh-my-zsh使用，那酸爽，用过就停不下来。

* 打开Cygwin从github上clone oh-my-zsh（前提请先在Cygwin中安装git）
  ```
    ➜ /home/nibo » cd  #切换到home目录
    ➜ /home/nibo » git clone git://github.com/robbyrussell/oh-my-zsh.git .oh-my-zsh   # clone oh-my-zsh到.oh-my-zsh
  ```

* 配置oh-my-zsh
  ```
    ➜ /home/nibo » cp .oh-my-zsh/templates/zshrc.zsh-template .zshrc    # 拷贝默认的配置文件到home目录名称为.zshrc
  ```

* .zshrc文件和我们通常使用的.bashrc文件作用是一样的。一个是zsh shell的配置，一个是bash shell的配置。oh-my-zsh默认的zshrc配置文件基本不用修改就可以正常使用。其中几个重要的属性说明下：
  ```
    export ZSH=$HOME/.oh-my-zsh    # oh-my-zsh的根目录，如果是按上面的操作步骤，可不用修改。
    ZSH_THEME="robbyrussell"       # oh-my-zsh主题，在$HOME/.oh-my-zsh/themes下可查看更多主题。
    plugins=(git)                  # oh-my-zsh插件，默认只有git。在$HOME/.oh-my-zsh/plugins下可查看更多已存在的插件。查看$HOME/.oh-my-zsh/plugins/git/git.plugin.zsh文件可查看已经定义的很多git快捷操作
  ```

* 关闭Cygwin并重新打开，测试oh-my-zsh配置是否成功。如果象刚才所说在oh-my-zsh中启用了git插件，则在一个git目录下执行`gst`命令能查看到工作空间状态，这是一个缩写的命令实际为`git status`
  ```
    ➜ /cygdrive/e/Workspace/AndroidStudioProjects/Big-Nerd-Ranch-Android (master) » gst
    On branch master
    Your branch is up-to-date with 'origin/master'.
    nothing to commit, working directory clean
  ```

#### 安装autojump
> autojump会记住已经访问过的目录，再次访问时可通过命令`j xxx`快速跳转。下面来安装autojump

* 安装autojump，从github上clone
  ```
    # clone源代码
    ➜ /home/nibo » git clone git clone git://github.com/joelthelion/autojump.git .autojump-src
    ➜ /home/nibo » cd .autojump-src
    ➜ /home/nibo/.autojump-src (master) » python ./install.py       # 安装autojump
  ```

* 配置autojump，在$HOME/.zshrc文件中添加如下配置
  ```
    # autojump配置
    [[ -s ~/.autojump/etc/profile.d/autojump.sh ]] && source ~/.autojump/etc/profile.d/autojump.sh
  ```

* 配置完成后关闭Cygwin重新打开，执行`j -s`命令看否成功。或者随便访问一个目录，然后通过`j xxx`看是否能直接跳转。
![](http://img.hb.aicdn.com/46317f0ec70285dba5843890cc5df21cf75b99e59e592-C8R8lF)

#### 在Cygwin中直接打开文件资源管理器
> 我们可能会碰到当我在Cygwin的命令行操作时，偶尔会需要快速打开当前目录的文件资源管理器。下面通过两个小脚本来实现这一功能。

* 下载脚本
  ```
    ➜ /home/nibo » git clone git@github.com:nibome/cygwin-script.git script
  ```

* 在.zshrc文件中配置
  ```
    # 在cygwin中打开当前所有的目录
    alias xpl='~/script/xpl.sh'
    alias xpf='~/script/xpf.sh'
  ```

* 关闭Cygwin重新打开

#### 注册右键菜单
> 通过将Cygwin注册到右键菜单中，我们可以在浏览文件时更快速的在当前位置启动Cygwin。

* 新建文件`cygwin.reg`，并复制如下代码，并保存，**注意，Cygwin的安装路径请以自己的系统为准**
  ```
    Windows Registry Editor Version 5.00

    [HKEY_CLASSES_ROOT\Directory\Background\shell\Open Cygwin]
    "Icon"="D:\\Software\\cygwin64\\Cygwin.ico"

    [HKEY_CLASSES_ROOT\Directory\Background\shell\Open Cygwin\Command]
    @="D:\\Software\\cygwin64\\bin\\mintty.exe -i /Cygwin-Terminal.ico /bin/env _T=%V /bin/zsh -l"
  ```

* 双击文件执行，添加注册表。
* 在Cygwin的`$HOME/.zshrc`文件中添加如下代码：
  ```
    # window 右键菜单时用cygwin打开目录所在的路径位置
    _T=${_T//\\//} #将所有的'\'替换为'/'
    if [[ $_T == "" ]]; then
        _T=${HOME}
    fi
    cd "${_T}"
  ```

* 完成后即可通过鼠标右键在当前目录快速打开Cygwin。

**至此Cygwin的一些常用配置就基本完成了！**
