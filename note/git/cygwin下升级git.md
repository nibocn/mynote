# cygwin下升级git

* 克隆最新的Git源代码
`git clone git@github.com:git/git.git`

* 以最新的release tag 创建分支

```
Leo@Leo-PC ~/workspace/git-src
$ git tag
gitgui-0.10.0
gitgui-0.10.1
gitgui-0.10.2
gitgui-0.11.0
gitgui-0.12.0
gitgui-0.13.0
....
v2.2.0
v2.2.0-rc0
v2.2.0-rc1
v2.2.0-rc2
v2.2.0-rc3

Leo@Leo-PC ~/workspace/git-src
$ git checkout -b build-v2.2.0-rc3 v2.2.0-rc3
Switched to a new branch 'build-v2.2.0-rc3'

Leo@Leo-PC ~/workspace/git-src
$ git status
On branch build-v2.2.0-rc3
nothing to commit, working directory clean

Leo@Leo-PC ~/workspace/git-src
$
```
* 编译源代码

`make configure`

`./configure --prefix=/usr/local`

`make`

`make install`

* 重启，验证安装是否正确

```
Leo@Leo-PC ~/workspace/git-src
$ git --version
git version 2.2.0.rc3

Leo@Leo-PC ~/workspace/git-src
$
```
