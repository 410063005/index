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



