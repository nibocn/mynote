# git笔记

## 添加文件

* `git add .` | `git add -A .` | `git add --all .`：添加文件到暂存区。

> 区别：
>
> `-A`参数，添加修改过和删除过的文件（change|delete）
>
>`--all`参数，添加所有文件（change|delete|add）
>
>不加参数，添加修改过和添加的文件（change|add）

## 远程提交

* 在已有的本地仓库添加一个远程仓库（并且仓库为空时）第一次push推送变更时需要加上`-u`参数，
即：`git push -u origin master`

> 由于远程库是空的，我们第一次推送master分支时，加上-u参数，git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

* `git push -u origin master -f`强制提交本地的代码到远程仓库。

**注：**加了`-f`参数后远程仓库之前的修改就会被删掉（强制以本地代码覆盖远程仓库的），谨慎用！
