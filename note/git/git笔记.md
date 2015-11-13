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
git diff [--cached|--staged]    # 比较暂存区和本地版本库差异
git diff --stat                 # 仅仅比较统计信息
```

```
git cherry-pick <$id>           # 合并指定提交到当前分支
git cherry-pick -n <$id>        # 合并指定提交到当前分支，但是不直接提交，只先添加到缓冲区
# 上面两个命令挺有用的

git count-objects -v            # 查看使用了多少空间，size-pack 是以kb为单位表示的 packfiles 的大小
```

## 查看配置
* `git config [--global|--system] --list` 查看用户配置信息。


## .gitignore匹配模式说明
> 星号（\*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（\*) 表示匹配任意中间目录，比如a/**/z 可以匹配 a/z, a/b/z 或 a/b/c/z等
>
>针对不同语言的.gitignore文件列表：https://github.com/github/gitignore

## git log 日志
`git log`常用选项

| 选项 | 说明 |
| ---- | ---- |
| -p  | 按补丁格式显示每个更新之间的差异。 |
| --stat | 显示每次更新的文件修改统计信息。 |
| --shortstat | 只显示 --stat 中最后的行数修改添加移除统计。 |
| --name-only | 仅在提交信息后显示已修改的文件清单。 |
| --name-status | 显示新增、修改、删除的文件清单。 |
| --abbrev-commit | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。 |
| --relative-date | 使用较短的相对时间显示（比如，“2 weeks ago”）。 |
| --graph | 显示 ASCII 图形表示的分支合并历史。 |
| --pretty | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式）。 |

**限制 git log 输出的选项(条件筛选)**

| 选项 | 说明 |
| ---- | ---- |
| -(n) | 仅显示最近的 n 条提交 |
| --since, --after | 仅显示指定时间之后的提交。 |
| --until, --before | 仅显示指定时间之前的提交。 |
| --author | 仅显示指定作者相关的提交。 |
| --committer | 仅显示指定提交者相关的提交。 |
| --grep | 仅显示含指定关键字的提交 |
| -S | 仅显示添加或移除了某个关键字的提交 |

`git log --pretty=format`常用选项

| 选项 | 说明 |
| ---- | ---- |
| %H | 提交对象（commit）的完整哈希字串 |
| %h | 提交对象的简短哈希字串 |
| %T | 树对象（tree）的完整哈希字串 |
| %t | 树对象的简短哈希字串 |
| %P | 父对象（parent）的完整哈希字串 |
| %p | 父对象的简短哈希字串 |
| %an | 作者（author）的名字 |
| %ae | 作者的电子邮件地址 |
| %ad | 作者修订日期（可以用 --date= 选项定制格式） |
| %ar | 作者修订日期，按多久以前的方式显示 |
| %cn | 提交者(committer)的名字 |
| %ce | 提交者的电子邮件地址 |
| %cd | 提交日期 |
| %cr | 提交日期，按多久以前的方式显示 |
| %s | 提交说明 |
