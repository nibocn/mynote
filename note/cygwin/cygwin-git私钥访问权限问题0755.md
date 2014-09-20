# cygwin-git私钥访问权限问题0755

复制git私钥文件到cygwin用户.ssh目录下，访问远程仓库报私钥文件访问权限问题：
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

Permissions 0755 for ‘/home/Richard/.ssh/id_rsa’ are too open.

It is recommended that your private key files are NOT accessible by others.

This private key will be ignored.

bad permissions: ignore key: /home/Richard/.ssh/id_rsa

Enter passphrase for key ‘/home/Richard/.ssh/id_rsa’:
```

修改私钥文件的权限，即可解决：`chmod 600 /home/Richard/.ssh/id_rsa`