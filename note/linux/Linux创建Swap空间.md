# Linux下创建Swap空间
@(Linux)[note|linux]

## 查看Swap空间
```
root@112505510c5aZ:/# swapon -s
Filename                Type        Size    Used    Priority
```

## 创建Swap空间
创建名称为swap.dat，大小为512MB的Swap文件

```
root@112505510c5aZ:/# dd if=/dev/zero of=/swap.dat bs=1024 count=512k
```

**启用Swap空间**

```
root@112505510c5aZ:/# mkswap /swap.dat
root@112505510c5aZ:/# swapon /swap.dat

```

查看启用后的Swap空间

```
root@112505510c5aZ:/# swapon -s
Filename                Type        Size    Used    Priority
/swap.dat               file      524284   126352    -1
```
