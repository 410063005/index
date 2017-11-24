# [Interlacing (bitmaps) - Wikipedia](https://en.wikipedia.org/wiki/Interlacing_(bitmaps))

交错(也称为interleaving)是一种位图图像编码方式。当收到该图像的部分数据时，可以先看到整个图片的一个模糊版本。当在低速网络上查看图片时，这种方式通常好过看到完整且清晰的图片，因为查看图片的用户可以快速判断是要继续等待还是中止图片的传输过程。

以下格式支持interlacing，interlacing是可选的：

+ [GIF](https://en.wikipedia.org/wiki/GIF)的交错方式是按0, 8, 16, ...(8n), 4, 12, ...(8n+4), 2, 6, 10, 14, ...(4n+2), 1, 3, 5, 7, 9, ...(2n+1)
+ [PNG](https://en.wikipedia.org/wiki/Portable_Network_Graphics)使用[Adam7 algorithm](https://en.wikipedia.org/wiki/Adam7_algorithm)，该算法对水平和垂直方向都进行交错
+ [TGA](https://en.wikipedia.org/wiki/Truevision_TGA)使用两种可选的交错算法，两路交错:0,2,4,...(2n), 1, 3, ...(2n+1) 以及四路交错:0,4,8,...(4n),2,6,...(4n+2),3,7,...(4n+3)
+ [JPEG](https://en.wikipedia.org/wiki/JPEG), [JPEG 2000](https://en.wikipedia.org/wiki/JPEG_2000)以及[JPEG XR](https://en.wikipedia.org/wiki/JPEG_XR) (实际上是使用frequency decomposition hierarchy 而非像素值交错)
+ [PGF](https://en.wikipedia.org/wiki/Progressive_Graphics_File) (也是使用frequency decomposition hierarchy)

Interlacing是一种增加解码方式，因为图片可以被增量地加载。另一种形式的增加解码是[progressive scan](https://en.wikipedia.org/wiki/Progressive_scan)。在progressive scan中，被加载的图片是一行一行地被解码，所以它是一行行地变大而不是逐渐变清晰。图片和视频中交错概念的差异主要在于progressive bitmaps也可由多个帧加载。

比如，交错GIF像是从一个慢慢打开的[Venetian blind](https://en.wikipedia.org/wiki/Venetian_blind)逐渐到达你的显示器。七个连续的位流慢慢充满缺少的线条，图片模糊的轮廓逐渐被取代，直到图片被完全解析。

交错图片曾经使用相当广泛。但这种技巧今天已经越来越不常见了，因为更宽的带宽允许多数图片可以快速下载并马上展示到用户屏幕上，而交错是一种不高效的图片编码方式。

交错图片的缺点是渲染完成后可能不像非交错图片那样清晰，加载过程明显(未加载完的数据是空白的)。另外，交错图片的压缩效果通常不好，它在慢速网络上的优点可能被不得不下载更大(因为压缩效果不好)图片而抵消。

# [PNG Specification: Data Representation](http://www.libpng.org/pub/png/spec/1.2/PNG-DataRep.html#DR.Interlaced-data-order)
PNG图片的数据可以交错存储以达到渐进显示的效果。这个特性的目的在于允许在线图片"渐入"加载效果。交错方式可轻微地增加文件大小，但可以给用户更快更符合直觉的加载速度。注意，不管是否需要渐入显示，解码器都应当能读取交错图片。

当interlace method是0时，像素是从左到右显示的，而扫描线相应地从上往下(即，没有交错)。

当interlace method是1时，即Adam7(以它的作者Adam M. Costello而得名)，则对图片进行七次不同的pass。每个pass会对图片像素的一个子集进行转换。每个pass对整个图片重复如下8*8模式，从左上角开始

```
   1 6 4 6 2 6 4 6
   7 7 7 7 7 7 7 7
   5 6 5 6 5 6 5 6
   7 7 7 7 7 7 7 7
   3 6 4 6 3 6 4 6
   7 7 7 7 7 7 7 7
   5 6 5 6 5 6 5 6
   7 7 7 7 7 7 7 7
```

在每个pass中，被选中的像素会被从左到右转换到一条扫描线中，而扫描线则是从上到下。比如，在第2个pass中包含第0,8,16...扫描线会包含4,12,20...像素。(从左上角0,0开始算起)。最后一个pass包含整个1,3,5,...扫描线。

每个pass的数据以一幅完整图像的方式排列。比如，如果一整个图片是16x16像素，则pass 3会包含两个扫描线，每个包含4个像素。如果像素少于8位，则扫描线会补齐到一个完整的byte。filter操作以通常的方式应用于这个"小"图片，每个扫描线的filter-type位会添加到扫描线的前面。

