#git权威指南学习笔记
* [第2章 爱上git的理由](#第2章-爱上git的理由)
* [第3章 git的安装和使用](#第3章-git的安装和使用)
* [第4章 git初始化](#第4章-git初始化)
* [第5章 git暂存区](#第5章-git暂存区)
* [第6章 git对象](#第6章-git对象)
* [第7章 git重置](#第7章-git重置)
* [第8章 git检出](#第8章-git检出)
* [第9章 恢复进度](#第9章-恢复进度)
* [第10章 git基本操作](#第10章-git基本操作)

## 第2章 爱上git的理由
* `git add`：将修改内容加入提交暂存区。
* `git add -u`：将所有修改过的文件加入暂存区。
* `git add -A`：将本地删除文件和新增文件都登记到提交暂存区。
* `git add -p`：将一个文件内的修改进行有选择性的添加。

## 第3章 git的安装和使用
**tips：**

* 通过命令：`git config --global core.quotepath false`，可解决在使用git命令查看具有中文的文件或路径时显示的字符串为八进制字符编码，如：`"git\346\235\203\345\250\201\346\214\207\345\215\227.md"`。
* Cygwin中设置忽略文件名大小定，编辑文件`~/.inputrc`，在其中添加`set completion-ignore-case on`。
* 取消git Windows下对文件权限的检测，防止因权限变更而需要重新提交文件。命令：`git config --system core.fileMode false`。

## 第4章 git初始化
* `git commit`：打开一个编辑器输入提交信息进行提交。
* `git commit -m`：直接在命令行输入提交信息进行提交。
* `git rev-parse --git-dir`：显示版本库.git目录所在的位置。
* `git rev-parse --show-toplevel`：显示工作区根目录。
* `git rev-parse --show-prefix`：显示当前目录相对于工作区根目录的相对目录路径。
* `git rev-parse --show-cdup`：显示从当前目录后退(`cd ..`)到工作区的根的深度。
* `git config -e`：设置版本库级别的配置文件(注：需要在有.git文件夹的目录下执行)。
* `git config -e --global`：设置全局配置文件，修改的文件为用户主目录下的`.gitconfig`文件，windows路径一般为：`C:\用户\XXX\.gitconfig`，Linux路径一般为：`/home/XXX/.gitconfig`，`XXX`指代用户名。
* `git config -e --system`：设置系统级配置文件，Windows路径一般为git安装根路径下的`etc/gitconfig`文件Linux路径为：`/etc/gitconfig`文件。
* 配置优先级为：`git config -e` > `git config -e --global` > `git config -e --system`。
* `git commit --allow-empty -m <msg>`：可执行一个空白提交，`msg`为具体的提交信息。
* `git log --pretty=fuller`：可显示完整的日志信息。
* `git commit --amend --reset-author`：修改最新的提交信息，`--amend`对刚刚的提交进行修补，不会产生另外新的提交，`--reset-author`将Author(作者)的ID同步修改，否则只会影响提交者(Commit)的ID。如果上次的提交是一个空的提交，需执行命令`git commit --amend --allow-empty --reset-author`。
* `git clone demo demo-step-1`：将本地库demo克隆(备份)。

### 第5章 git暂存区
* `git log --stat`：查看每次提交的文件变更统计。
* `git status -s`：查看git工作区精简的状态输出。
* `git diff`：显示工作区与提交暂存区中相比的差异。
* `git diff HEAD`：显示工作区与HEAD(当前工作分支)相比的差异。
* `git diff --cached`：显示提交暂存区和版本库中文件的差异。
* `git log --pretty=oneline`：精简的日志输出。
* `git rest HEAD`：暂存区的目录树会被重写，会被master分支指向的目录树所替换，但工作区不受影响。
* `git rm --cached <file>`：直接从暂存区删除文件，工作区则不做出改变。
* `git checkout .`或`git checkout -- <file>`：用暂存区的全部文件或指定的文件替换工作区的文件。
    注：此操作会清除工作区中未添加到暂存区的改动。
* `git checkout HEAD .`或`git checkout HEAD <file>`：用HEAD指向的master分支中的全部或部分文件替换暂存区和工作区中的文件。注：此命令极具危险性，因为它不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。
* `git ls-tree -l HEAD`：查看版本库中当前提交指向的目录树。
* `git clean -fd`：清除当前工作区中没有加入版本库的文件和目录(非跟踪文件和目录)，注：只有文件一次也没执行过
    `git add`才能将文件清除，执行过一次`git add`以后文件将不会被清除。
* `git ls-files -s`：显示暂存区的目录树。
* `git stash`：保存当前工作进度。


**tips：**

* 要针对暂存区执行`git ls-tree`命令，需要先将暂存区的目录树写入git对象库，命令：`git write-tree`，根据生成的40位SHA1哈希值ID执行命令：`git ls-tree -l <SHA1ID>`。
* 在使用SHA1哈希值ID可以不用写全，只要不与其他对象的SHA1 ID冲突就行了。
* 在使用`git ls-tree`命令时当要显示一个目录下所有的目录内容时可使用参数`-r`，要显示目录下所有字目录也显示出来可使用参数`-t`。如：`git ls-tree -l -r -t <SHA1ID>`
* 不要使用`git commit -a`命令，因为这虽然可以跳过`git add`命令直接提交，但也丢掉了git暂存区带给用户的最大好处：对提交内容进行控制的能力。

## 第6章 git对象
**tips：**

* 查看一个日志提交对象，命令：`git log -[N] --pretty=raw`，其中`N`为整数即查看最近一条或多条日志记录，如：`git log -1 --pretty=raw`

`commit 8d7a7092e6ec58b345f0f50ee627befb315bd026`：本次提交的唯一标识。

`tree ca30dbd1ded1aa35d849c574b4a4b8270c4f734b`：本次提交所对应的目录树。

`parent dd8590662601db4b8d2331632a92e2f49219facd`：本次提交的父提交(上一次提交)。

* 通过命令`git cat-file -t <SHA1>`可查看git对象ID类型。
* 通过命令`git cat-file -p <SHA1>`可查看git对象的内容。
* `git log --pretty=raw --graph <SHA1>`显示一个git对象ID与其他对象之间的关联。
* `git status -s -b`：以精简方式显示当前工作区状态，并显示当前工作区的分支名称。
* `git branch`：分支管理主要命令，可显示当前分支，**注：**星号（*）表示这个分支为当前的工作分支。
* `git rev-parse master|refs/heads/master|HEAD`：命令用于显示引用对应的提交ID。
* `HEAD`代表版本库中最近的一次提交。
* 使用符号`^`可以用于指代次提交。如：
    * `HEAD^`代表版本库中的上一次提交，即最近一次提交的父提交。
    * `HEAD^^`则代表HEAD^的父提交。
* 对于一个提交有多个父提交，可以在符号^后面用数字表示是第几个父提交。如：
    * `a573109^2`的含义是提交a573109的多个父提交中的第二个父提交。
    * `HEAD^1`相当于HEAD^，含义是HEAD的多个父提交中的第一个父提交。
    * `HEAD^^2`的含义是HEAD^（HEAD父提交）的多个父提交中的第二个父提交。
* 符号`~<n>`也可以用于指代祖先提交。如：
    * `a573109~5`相当于a573109^^^^^。
* 提交所对应的父对象，也可以用类似的语法。如：`a573109^{tree}`。
* 某一次提交对应的文件对象。如：`a573109:path/to/file`。
* 暂存区的文件对象。如：`:path/to/file`。

## 第7章 git重置
* `git log --graph --oneline`：显示提交日志内容和更短小的提交ID。
* `git rest --hard HEAD^`：将游标重置到当前提交的上一个提交（父提交）ID。**注：**`--hard`参数会破坏工作区未提交的改动，
并且会彻底丢弃提交的历史，慎用！
* 非裸版本库（带有工作区）提供了分支日志查询功能，这是因为带有工作区的版本库都有如下设置：`git config core.logallrefupdates`，值为true。
* master分支的日志文件路径：`.git/logs/refs/heads/master`。
* `git reflog`：命令可对分支日志文件进行操作，用来挽救错误的重置。如：`git reflog show master | head [-n]`，-n显示前n条日志。
* 根据显示的日志信息，通过命令`git rest --hard master@{2}`恢复。其中master@{2}是根据实际情况变化的。
* `git reset [-q] [<commit>] [--] <paths>`：第一种重置命令，此命令不会重置引用，更不会改变工作区，而是用指定提交状态
`(<commit>)`下的文件`(<paths>)`替换掉暂存区中的文件。
* `git reset [--soft | --mixed | --hard] [<commit>]`：
    * 使用参数`--hard`，如：`git reset --hard <commit>`。会执行1、替换引用的指向。引用指向新的提交ID。2、替换暂存区。
    替换后，暂存区的内容和引用指向的目录树一致。3、替换工作区。替换后，工作区的内容变得和暂存区一致。也和HEAD所指向的
    目录树内容相同。
    * 使用参数`--soft`，如：`git reset --soft <commit>`。只会更改引用的指向，不改变暂存区和工作区。
    * 使用参数`--mixed`或不使用参数（默认为`--mixed`），如：`git reset <commit>`。会更改引用的指向及重置暂存区，但是不改变工作区。
    * `git reset -- filename`：仅将文件filename的改动撤出暂存区，暂存区其它文件不改变。相当于对命令`git add filename`的反向操作。

## 第8章 git检出
* `git branch -v`：显示当前分支，最新的提交ID，提交日志内容。
* `git checkout 49230cd^`：通过`git checkout`命令检出该ID的父提交。
* “分离头指针”状态指的就是HEAD头指针指向了一个具体的提交ID，而不是一个引用（分支）。
* `git rev-parse HEAD master`：查看HEAD和master对应的提交ID。
* `git checkout master`：切换到master分支上。
* `git merge acc2f69`：将'acc2f69'提交合并到当前分支。
* `git checkout`命令用法：
    * 用法一：`git checkout [-q] [<commit>] [--] <paths>`
    * 用法二：`git checkout [<branch>]`
    * 用法三：`git checkout [-m] [[-b|--orphan] <new_branch>] [<start_point>]`
* 第一种用法的`<commit>`是可选项，如果省略则相当于从暂存区（index）进行检出。这与重置命令不同，重置的默认值是HEAD。因此
重置一般用于重置暂存区（除非使用`--hard`参数，否则不重置工作区），而检出命令主要是覆盖工作区（如果`<commit>`不省略，也会
替换暂存区中相应的文件）。
* 第二种用法（不使用路径`<paths>`）则会改变HEAD头指针。后面的参数`<branch>`是因为只有HEAD切换到一个分支才可以对提交
进行跟踪，否则仍然会进入“分离头指针”的状态。在“分离头指针”状态下的提交不能被引用关联到，从而可能丢失。故用法二的
主要作用就是切换分支。如果省略`<branch>`相当于对工作区进行状态检查。
* 第三种用法主要是创建和切换到新的分支（`<new_branch>`），新的分支从`<start_point>`指定的提交开始创建。

## 第9章 恢复进度
* `git log --stat`：显示被修改文件的修改统计信息。
* `git stash list`：查看保存的进度。
* `git stash pop`：恢复保存的进度，并从存储的工作进度中清除已经恢复的工作进度。
* `git clean -nd`：查看将要清理的目录和文件。
* `git clean -fd`：真正强制删除多余的目录和文件。
* `git stash pop [--index] [<stash>]`：
    * `<stash>`参数（来自于`git stash list`显示的列表）表示从该`<stash>`中恢复。完毕后也将从进度列表中删除`<stash>`。
    * `--index`参数除了恢复工作区的文件外，还尝试恢复暂存区。
* `git stash [save [--patch] [-k | --[no-] keep-index] [-q | --quiet] [<message>]]`：
    * `git stash`命令的完整版。需要在保存工作进度时使用指定的说明，必须使用如下格式：
        * `git stash save "message..."`。
    * 参数`--patch`会显示工作区和HEAD的差异，通过对差异文件的编辑决定在进度中最终要保存的工作区内容。
    * 使用`-k`或`--keep-index`参数，在保存进度后不会将暂存区重置。默认会将暂存区和工作区强制重置。
* `git stash apply [--index] [<stash>]`：除了不删除恢复的进度之外，其余和`git stash pop`命令一样。
* `git stash drop [<stash>]`：删除一个存储的进度。默认删除最新的进度。
* `git stash clear`：删除所有存储的进度。
* `git stash branch <branchname> <stash>`：基于进度创建分支。
* `git log --graph --pretty=raw refs/stash`：查看保存在暂存区和工作区的进度。
    * 其中在提交说明中有`WIP`字样（Work In Progess）的代表了工作区进度。
    * 其中在提交说明中带有`index on master`字样的代表着暂存区的进度。
* 查看具体某一次进度保存的信息命令`git log --graph --pretty=raw stash@{n}`，n表示第几次。

## 第10章 git基本操作
* `git tag -m "message..." tagname`：给当前版本库打标签。
* `git ls-files`：查看暂存区（版本库）的文件。
* `git ls-files --with-tree=HEAD^`：在历史提交中查看文件列表，其中`HEAD^`表示当前提交的父提交（上一次提交）。
* `git cat-file -p HEAD^:filename`：在历史版本中查看具体某个文件的内容。
* `git cat-file -p`命令相当于`git show`命令。
* `git mv sourcename newname`：文件重命名，`sourcename`表示原文件名，`newname`表示新文件名。
* `git describe`：显示版本号。
* `git log --decorate`：在显示的日志信息中显示该提交关联的引用（里程碑或分支）。
* `git add -i`：打开一个人机交互的命令界面。
* `git status --ignored [-s]`：查看被忽略的文件。
* `git add -f filename`：被忽略的文件只能通过`-f`参数并且必须写填写文件名，才能添加到版本控制中。
* 直接在版本库中设置的`.gitignore`忽略文件是一种共享式的，当提交到版本控制库时，其他人也会使用到。通过在目录
`.git/info/exclude`下可设置本地有效的独享式文件忽略。也可通过命令`git config --global core.excludesfile path`设置
全局独享的文件忽略列表。如`git config --global core.excludesfile /home/leo/.gitignore`。
* git文件忽略语法：
    * 以空行或以井号（#）开始的行会被忽略。
    * 可使用通配符，如：星号（*）代表任意多字符，问号（?）代表一个字符，方括号（[abc]）代表可选字符范围等。
    * 如果名称的最前面是一个路径分隔符（/），表明要忽略的文件在此目录下，而非子目录的文件。
    * 如果名称的最后面是一个路径分隔符（/），表明要忽略的是整个目录，同名文件不忽略，否则同名的文件和目录都忽略。
    * 通过在名称的最前面添加一个感叹号（!），代表不忽略。
* `git archive`：对任意提交对应的目录树建立归档。如：
    * `git archive -o lastest.zip HEAD`：基于最新提交建立归档文件lastest.zip。
    * `git archive -o partial.tar HEAD src doc`：只将目录src和doc建立到归档partial.tar中。
    * `git archive --format=tar --prefix=1.0/ v1.0 | gzip > foo-1.0.tar.gz`：基于里程碑v1.0建立归档，并且为归档中的文件添加目录前缀1.0。
