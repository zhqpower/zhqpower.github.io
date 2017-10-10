---
title: java IO 学习
date: 2017-02-13 19:34:04
tags: java
categories: 后端
---

### 一. 基础概念
1. 流： 有序、有始有终的传输字节数据的总称和抽像。

      有数据传输的源，就一定有数据传输的目的地。

2. 流分类:

   1> 根据处理类型，字节流和字符流

   2> 根据数据 流向不同，输入流和输出流

    核心点: 输入与输出的参考对象是内存

3. 常有流对象:

    InputStream 和 OutputStream

    Reader 和 Writer

    FileReader 和 FileWriter（字符流，只能针对文本IO,而字节流可以针对任何对象）

    InputStreamReader 和 OutputStreamWriter（转换器，字节流向字符流转换的通道，该类也只能针对文本IO）

    BufferedReader 和 BufferedWriter (缓冲流, 内部默认有8192byte array的缓冲数组，故能够提高读写效率)

    ByteArrayInputStream 和 ByteArrayOutputStream (字节数组流,方便做对象的序列化。字节数组可以保存到任意的存储位置)

    ObjectInputStream 和 ObjectOutputStream (对象流，将对象IO 到相关位置)

    特殊的 RandomAccessFile (文件随机读取对象，它不是流对象，但它内部封装了流。)

    ---
     #### 子节到字符是解码，字符到子节到编码。
    ---
