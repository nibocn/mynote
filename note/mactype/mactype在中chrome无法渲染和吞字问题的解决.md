# MacType在中chrome无法渲染和吞字问题的解决
@(MacType)[note|MacType]

## 无法渲染的问题
* 修改`chrome://flags`中的`停用DirectWrite Windows`改为停用。
* 在chrome的快捷方式`属性-->目标`中添加`--disable-directwrite-for-ui`参数。

## 吞字的问题
在`chrome://flags`中修改`光栅线程数 Mac, Windows, Linux, Chrome OS, Android`参数的值为1。
