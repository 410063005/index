如何启动王者荣誉并创建房间呢

只拿到了少量不完整的信息，`tencent1104466820://?gamedata=GameHelper_1-1-20015_8_2`。使用这个字符串构建Intent来启动王者，发现可以启动，但进不了房间。

Logcat看日志，搜关键信息。什么是关键信息呢？王者启动时，总能看到包名吧，所以果断搜`com.tencent.tmgp.sgame`。观察发现王者打出了很规律的日志，以`WeGame`开头。于是使用`WeGame`过滤日志，立马发现WeGame打印出来的全部是王者启动的信息，甚至可以看到我们通过Intent传给王者的数据。

```
11-09 19:42:02.405 21324-21324/? D/WeGame Logger.d:  ********************** INTENT START **************************
11-09 19:42:02.405 21324-21324/? D/WeGame WeGame.handleCallback: Action: android.intent.action.MAIN
11-09 19:42:02.405 21324-21324/? D/WeGame WeGame.handleCallback: Component: ComponentInfo{com.tencent.tmgp.sgame/com.tencent.tmgp.sgame.SGameActivity}
11-09 19:42:02.405 21324-21324/? D/WeGame WeGame.handleCallback: Flags: 272629760
11-09 19:42:02.405 21324-21324/? D/WeGame WeGame.handleCallback: Scheme: null
11-09 19:42:02.405 21324-21324/? D/WeGame WeGame.handleCallback: gamedata:GameHelper_1-3-20011-5_11_1
11-09 19:42:02.405 21324-21324/? D/WeGame Logger.d:  ********************** INTENT END **************************
```

凭直觉，感觉这里的参数过少，应该不足以进入王者房间。那王者荣誉助手是怎么做的呢？

接下来的步骤按步就班了。下载王者荣誉助手APK，反编译得到Java源文件。当然，代码很多，仍然需要搜索关键信息。

首先当然是`find . -type file  | xargs grep "gamedata"`, 搜索结果显示用到gamedata字符串的地方只有`OpenBlackChatFragment.a`方法。

然后搜索`find . -type file  | xargs grep "OpenBlackChatFragment.a("`

![]()

以上代码重新整理下大概是这样:

```java
    private static void wx(Context context, String packageName) {
        Intent intent = context.getPackageManager().getLaunchIntentForPackage(packageName);
        Bundle bundle = new Bundle();
        bundle.putString("gamedata", "GameHelper_1-3-20011-5_11_1");
        bundle.putString("messageExt", "王者荣耀助手");
        bundle.putString("wx_callback", "onReq");
        bundle.putString("wx_openId", "owanlsllOoB8_jDsHVO5Xtk0ugc");
        bundle.putString("platformId", "wechat");
        intent.putExtras(bundle);
        context.startActivity(intent);
    }
```

我们知道王者荣誉助手是可以成功启动王者荣誉并且创建房间的。用助手客户端创建，并观察WeGame日志

```
11-09 19:38:51.328 21324-21324/? D/WeGame Logger.d:  ********************** INTENT START **************************
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: Action: android.intent.action.MAIN
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: Component: ComponentInfo{com.tencent.tmgp.sgame/com.tencent.tmgp.sgame.SGameActivity}
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: Flags: 272629760
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: Scheme: null
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: gamedata:GameHelper_1-3-20011-5_11_1
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: messageExt:王者荣耀助手
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: wx_callback:onReq
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: wx_openId:owanlsllOoB8_jDsHVO5Xtk08ugc
11-09 19:38:51.329 21324-21324/? D/WeGame WeGame.handleCallback: platformId:wechat
11-09 19:38:51.329 21324-21324/? D/WeGame Logger.d:  ********************** INTENT END **************************
```

对比我们的日志以及源码，不难发现王者荣誉助手比我们传入的参数多出了`messageExt`, `wx_callback`, `wx_openId`, `platformId`。如果我们将这些参数全部拷贝到自己的App中用来启动王者，能否成功呢。尝试了下，果然可以启动并创建房间。现在可以确定需要哪些参数了。

问题来了，`wx_openId`是怎么生成的呢？推测是用户在王者荣耀中的id。
