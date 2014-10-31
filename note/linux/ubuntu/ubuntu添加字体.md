# Ubuntu添加字体

@(Ubuntu)[字体添加]

## 创建字体文件目录

在`/usr/share/fonts/`目录下新建`myfonts`目录：
```
sudo mkdir /usr/share/fonts/myfonts
```
复制字体到上面的目录中。

## 修改字体权限

```
cd /usr/share/fonts/myfonts
sudo chmod 774 *
```

## 生成字体信息

```
sudo mkfontscale
sudo mkfontdir
sudo fc-cache -f -v
```

**至此就可选择刚才增加的字体了，*注：如果发现不能选择可注销或重启系统***
