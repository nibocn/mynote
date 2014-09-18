# git笔记

+ 在已有的本地仓库添加一个远程仓库（并且仓库为空时）第一次push推送变更时需要加上`-u`参数，即：`git push -u origin master`

> 由于远程库是空的，我们第一次推送master分支时，加上-u参数，git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。




