gradle依赖

```
compile 'com.squareup.okio:okio:1.6.0'
```

Sink, Source, Closeable

![okio](http://img.blog.csdn.net/20160115144102367)

图片来源:  http://blog.csdn.net/sbsujjbcy/article/details/50523623

关键类 Buffer, Segment, SegmentPool


一图胜千言 

![okio整体架构](http://img.blog.csdn.net/20170123170254601?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc29jcmF0ZXNfbGVl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

图片来源: http://blog.csdn.net/socrates_lee/article/details/54893752

![okio整体架构2](http://img.blog.csdn.net/20160424112132772)

图片来源: http://blog.csdn.net/zoudifei/article/details/51232711

---

使用okio改造TCPSocket

如何度量性能上的改进呢

---
Buffer的设计原理 [ref](http://blog.csdn.net/zoudifei/article/details/51232711)

+ 通过Segment循环双向链表来实现Buffer
+ 使用SegmentPool对当前未使用的Segment进行管理，缺省的池子大小是64KB

优化后的池子大小是多少

http://blog.csdn.net/yhaolpz/article/details/54948521
---

> 纸上得来终觉浅，绝知此事要恭行

okio号称能提高io性能，但是按这个帖子中的代码验证，发现okio的性能相对于BufferedInputStream/BufferedOutputStream没有明显提升。

为什么okio没有明显性能提升  https://stackoverflow.com/questions/44904083/why-is-okio-more-efficient-than-bufferedinputstream-and-bufferedoutputstream


====
# Okio
okio是对`java.io`和`java.nio`的补充，让它们可以更简单地访问、存储以及处理数据。

## ByteStrings和Buffers
Okio基于这两个类来构建，它们以直观的API提供了非常多的功能：

+ [ByteString](http://square.github.io/okio/1.x/okio/okio/ByteString.html)是一个不可变的字节序列。`String`是字符串数据的基石，而`ByteString`跟String类似，是字节数组的基石，使长久以来我们缺少这样一个类。用它可以很方便地将二进制数据视为一个值。这个类改造过，它知道如何将自己编解码为hex, base64以及UTF-8
+ [Buffer](http://square.github.io/okio/1.x/okio/okio/Buffer.html)是一个可变的字节序列。就跟`ArrayList`一样，你不用事先改变buffer的大小。将buffer作为队列来读写，向尾部写数据，向头部读数据，不用再去管理position, limit和capacity了。

`ByteString`和`Buffer`在内部使用一些聪明的策略来降低CPU和内存占用。比如你将UTF-8字符串编码为`ByteString`时，它会缓存这个字符串的引用，当你下次解码时其实不必再做任何工作。

`Buffer`使用segment双向链表实现。当你将数据从一个buffer移动到另一个buffer，实际上只是改变segment的所有权而并不是真的在buffer间拷贝数据。这种方式对多线程程序特别有好处：网络线程可以直接跟工作线程交换数据而不必拷贝。

## Sources和Sinks
`java.io`设计上的优雅之处在于可以将stream层层叠加来实现加密或压缩之类的数据变换。Okio也有自己的stream类型，分别是[Source](http://square.github.io/okio/1.x/okio/okio/Source.html)和[Sink](http://square.github.io/okio/1.x/okio/okio/Sink.html)，它们的工作方式跟`InputStream`和`OutputStream`类似，但也有一些重要的不同：

+ **超时** Source和Sink提供访问底层IO超时机制的方法。不像`java.io`的socket stream, `read()`和`write()`方法都支持超时。
+ **易于实现** `Source`声明了三个方法：`read()`, `close()`, `timeout()`。没有像`available()`这样的奇怪方法，也没有易于引入bug和性能问题的读取单字节方法。
+ **易于使用** 尽管`Source`和`Sink`只有三个方法需要实现，但[BufferedSource](http://square.github.io/okio/1.x/okio/okio/BufferedSource.html)和[BufferedSink](http://square.github.io/okio/1.x/okio/okio/BufferedSink.html)接口向调用方提供了丰富的API。这些接口向你统一的位置向你提供了各种需要的方法。

