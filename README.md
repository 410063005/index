这里记录了一些不错的资源
# [A](#)
## Adb
[解决5037端口被占用adb无法连接的问题](http://www.tuicool.com/articles/VJFZn2M) 
[adb server version (31) doesn't match this client (36); killing...](http://blog.csdn.net/rodulf/article/details/51939974)

## Android Studio

## aapt
`aapt dump badging <apk>` 导出apk的信息，包括包名、版本、权限等等。

./bin/aapt dump badging Downloads/smoba.apk
package: name='com.tencent.gamehelper.smoba' versionCode='170101801' versionName='2.3.2.1018' platformBuildVersionName='7.1.1'
install-location:'auto'
sdkVersion:'16'


插件

[有什么好用的Android Studio的插件值得推荐](http://www.zhihu.com/question/28026027)

# [G](#)

## Genymotion
[Access host from Genymotion emulator](http://stackoverflow.com/questions/18463319/access-host-from-genymotion-emulator): 通过`10.0.3.2`访问host 
genymotion emulator(Android 6.0)上禁止SD卡权限后app直接crash

## Git
[git pull自动输入密码](http://www.cnblogs.com/dudu/archive/2011/07/06/git_save_username_password.html) 
`git log --all --grep='Build 0051'` 根据git log搜索

msygit安装或卸载右键菜单 

```
D:\king\Git\git-cheetah>regsvr32 /i git_shell_ext64.dll
D:\king\Git\git-cheetah>regsvr32 /u git_shell_ext64.dll
```

[git或cmd代理](http://jerrychia.com/2015/03/30/hexo-how-to-set-http-proxy/)  
[git远程仓库](https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E8%BF%9C%E7%A8%8B%E4%BB%93%E5%BA%93%E7%9A%84%E4%BD%BF%E7%94%A8)
[git子模块](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)
[git子模块](https://erttyy8821.gitbooks.io/android_memo/content/chapter1/SubmoduleAndModules.html)

git pull失败时可用以下命令强制使用远程代码

```
git fetch --all
git reset --hard origin/master
```

git rebase

[简介](http://blog.csdn.net/hudashi/article/details/7664631/)
[用法](http://blog.csdn.net/wangjia55/article/details/8490679)

## Gradle

插件

[Gradle Retrolambda Plugin](https://github.com/evant/gradle-retrolambda)

[打开Gradle工程慢的问题](http://blog.csdn.net/fuchaosz/article/details/51567808)

[Speeding Up Your Android Gradle Builds (Google I/O ‘17)](https://www.youtube.com/watch?v=7ll-rkLCtyk&list=PLWz5rJ2EKKc-odHd6XEaf7ykfsosYyCKp&index=31)

## Maven
Maven国内镜像 [maven阿里云中央仓库](http://maven.aliyun.com/nexus/content/groups/public/)

# [O](#)
## OkHttp
[OkHttp使用教程](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0106/2275.html)

# [P](#)

# [R](#)
## RxJava
[我们为什么要在Android中使用RxJava](http://www.imooc.com/article/3936) 
[RxJava](https://medium.com/swlh/party-tricks-with-rxjava-rxandroid-retrolambda-1b06ed7cd29c#.tqop78uai) 
[深入浅出RxJava](http://blog.csdn.net/lzyzsd/article/details/41833541) 
[RxJava from gitbook](https://asce1885.gitbooks.io/android-rd-senior-advanced/content/che_di_le_jie_rxjava_ff08_yi_ff09_ji_chu_zhi_shi.html)

## Regre正则表达式
'.'可以用于匹配换行符之外的所有字符，Java中可使用`Pattern.DOTALL`以匹配所有字符

[java.util.regex.Pattern](http://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html)

# [V](#)
## Vim
