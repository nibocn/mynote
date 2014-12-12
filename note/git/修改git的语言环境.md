# 修改git的语言环境

cygwin升级git完成后可能会由于中文的系统环境导致git的默认语言环境会变成中文（使用git命令时相关提示信息是中文）。这时可以修改`/usr/share/locale/zh_CN/LC_MESSAGES`或者`/usr/local/share/locale/zh_CN/LC_MESSAGES`目录下的`git.mo`文件，将文件重命名为`git.mo.backup`就可以了。
