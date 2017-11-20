微信access_token刷新问题分析.md
========

[TOC]

我们应用中遇到微信access_token刷新不及时导致部分接口访问失败的问题。

[这篇文章](http://www.cnblogs.com/jiangtengteng/p/6905795.html)中提到

> access_token(即expire_in)的有效期为7200秒。 需要在有效期之前去刷新access_token。注意，不仅需要定时刷新access_token，也需要有被动刷新机制。

> 需要将access_token保存唯一的一个地方(非内存)，用于提供统一访问，否则可能引起access_token混乱

这两点给了我启示：我们的问题应该不是access_token没有及时刷新，而是没有保证刷新后生效。如何解决这个问题呢，思路如下

1. 打印当前的access_token，观察access_token及相关的值，比如refresh_token
2. 理解换票过程，即如何使用refresh_token去获取一个新的access_token
3. 通过强制换票复现问题场景

# 打印当前access_token

打印结果如下

```json
{"openId":"o5P-OjmfeRyrlZmSQQbhkUZyZ3sk","accessToken":"3_oYsCZ4TPFcK4rNiylg90dNA9dUYo3QLCjddxc9HlsmC0KqouLE1CWsMtI-WZVsaMhmFQfBGGExn-ffwGk-i8ZA","refreshToken":"3_mq5_0IkqNz0pT_WehSlGeakKQ1UytcbYkrspBHts3NmBdayjDnp6irBHUcAZNemplbwhjDdVvXQLtfgLeRY57Q","expiredTime":7200,"refreshTime":1510729661872}
```

算了下`new Date(1510729661872)`是`Wed Nov 15 2017 15:07:41 GMT+0800 (CST)`，即2017年11月15日下午15时07分41秒获得最新的`accessToken`，它将在7200秒也即两小时后后期。我可以使用`refreshToken`来更新`accessToken`

# 如何换票

从代码里面找到了如下协议

+ url - https://igame.qq.com/tipcomm/fn/refreshaccesstoken.php
+ ua - User-Agent: ' IGameApp/3.2.5 NetType/WIFI'
+ cookie -  Cookie: refresh_token=<refreshToken> 

使用Postman尝试换票，发现之前没有注意的一个小细节：单独使用postman构造请求时cookie和ua是受限制的，需要安装postman interceptor [postman interceptor](https://stackoverflow.com/questions/24472239/how-to-use-postman-interceptor)

安装postman interceptor后成功模拟换票了.

# 复现问题
复现有两个方法：一是等两个小时直到accessToken过期，二是强制刷新accessToken但同时保证访问接口时不要带上这个最新的accessToken。

实际验证时发现第二种方法不可行，短时间内请求accessToken时总是返回同一个值。


====

https://wohugb.gitbooks.io/wechat/content/qrconnent/refresh_token.html