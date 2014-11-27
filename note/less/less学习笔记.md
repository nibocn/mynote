# less 学习笔记

## 注释

使用`//`注释内容时less在编译时不会将注释信息编译到对应的css文件中。使用`/**/`注释则会将注释的信息编译到css文件中。

## 变量

变量声明`@变量名: 值`。使用变量`@变量名`，如：
```
@box-width: 300px;
.box {
    width: @box-width;
    height: @box-width;
    background-color: #374e0c;
}
```
编译生成的css：
```
.box {
    width: 300px;
    height: 300px;
    background-color: #374e0c;
}
```

## 混合

> 减少重复的代码，将多处使用到的代码提取出来，提高代码的复用。

* 混合（不带参数）：
```
// 声明，不带参数的时候与类样式一样。
.box {
    width: 300px;
    height: 300px;
    background-color: #374e0c;
}
.box2 {
    .box; // 引用
    margin: 10px;
}
```
编译生成的css：
```
.box {
    width: 300px;
    height: 300px;
    background-color: #374e0c;
}
.box2 {
    width: 300px;
    height: 300px;
    background-color: #374e0c;
    margin: 10px;
}
```
* 混合（带参数）：
```
.box-args(@box-width, @box-height) {
    width: @box-width;
    height: @box-height;
    background-color: #374e0c;
}
.box3 {
    .box-args(600px, 400px);
    margin: 10px;
}
```
编译生成的css：
```
.box3 {
    width: 600px;
    height: 400px;
    background-color: #374e0c;
    margin: 10px;
}
```
* 混合（带默认参数值）
```
.box-args2(@box-width: 500px, @box-height: 300px) {
    width: @box-width;
    height: @box-height;
    background-color: #374e0c;
}
.box4 {
    .box-args2;
    margin: 10px;
}
```
编译生成的css：
```
.box4 {
  width: 500px;
  height: 300px;
  background-color: #374e0c;
  margin: 10px;
}
```

**注：**

1. 在引用时默认参数的值也可覆盖，如：`.box-args2(400px)`表示`width`的值为400px，`height`的值仍为300px。
2. 当有多个参数时，无默认值的参数放前面，有默认值的参数放后面。


