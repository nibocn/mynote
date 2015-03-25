# git笔记

## 常用命令
```
git add .       # 添加修改过和添加的文件（change|add）到暂存区。
git add -A .    # 添加修改过和删除过的文件（change|delete）到暂存区。
git add --all . # 添加所有文件（change|delete|add）到暂存区。
```
## Git远程仓库管理
```
git push -u origin master       # 客户端首次push
git push -u origin master -f    # 强制提交本地的代码到远程仓库。(强制以本地代码覆盖远程仓库的，慎用)

# 设置跟踪远程库和本地库
git branch --set-upstream master origin/master
git branch --set-upstream develop origin/develop
```
## 查看文件diff
```
git diff <file>                 # 比较当前文件和暂存区文件差异
git diff
git diff <$id1> <$id2>          # 比较两次提交之间的差异
git diff <branch1>..<branch2>   # 在两个分支之间比较
git diff --staged               # 比较暂存区和本地版本库差异
git diff --cached               # 比较暂存区和本地版本库差异
git diff --stat                 # 仅仅比较统计信息
```

```
git cherry-pick <$id>           # 合并指定提交到当前分支
git cherry-pick -n <$id>        # 合并指定提交到当前分支，但是不直接提交，只先添加到缓冲区
# 上面两个命令挺有用的

git count-objects -v            # 查看使用了多少空间，size-pack 是以千字节为单位表示的 packfiles 的大小
```
