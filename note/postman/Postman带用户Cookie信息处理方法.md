# Postman带用户Cookie信息处理方法
> 当我们使用Postman发送一个请求时，可能会要求发送这个请求前先做用户认证（登录）。并且在具体请求某个URL时还会验证当前用户是否有权限请求这个URL。在Postman中如何达到这一要求呢？

## 安装Postman Interceptor插件
> Interceptor主要就是用来做请求的拦截。

* 访问chrome webstore https://chrome.google.com/webstore/category/apps 搜索`Postman Interceptor`然后进行安装。

  ![](http://img.hb.aicdn.com/620b3505b7e02ebd551109c3412753e5e8d20fef515a-Wpczxp_fw658)

* 配置Interceptor

  ![](http://img.hb.aicdn.com/5ec04c25160ce9c2e80ff52df77fb113cc3f54de6b89-ujIym3_fw658)

  **注：**其中的`Filter requests`参数可以自己配置，如：`localhost:8081`表示只拦截本地的8081端口的请求。默认的配置`.*`配置拦截所有请求，可以使用默认配置不用修改。

## 登录系统
> Interceptor配置完后，在浏览器中正常登录被测试的系统(**这很重要**)。只需要按正常的流程登录即可。

## Postman配置
在Postman中启用Interceptor，如下图：

![](http://img.hb.aicdn.com/2d6e7d481d39bf39fb30c23deee7f683e620ae788397-Yld50h_fw658)

至此通过Postman发送需要登录或授权的URL请求时已配置完成。
