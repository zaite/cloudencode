# 1. cloudencode
cloudencode是一个分布式并行转码服务

## 1.1. 如何实现转码分布式并行

### 1.1.1. 传统转码服务存在的问题
对于大视频文件转码，都是按照顺序进行。对于一个大视频文件，只能按照文件顺序在一台服务器上单进程进行转码。<br/>
即使有多台高性能服务器，也无法加快进度

### 1.1.2. 分布式并行转码服务的方法
主要利用文件切片技术，将大文件切成小片后，放入存储，多台服务器的服务进程都各自取分片文件转码，所有分片完成转码后，再组成完整视频文件。<br/>
业务架构组网图如下：<br/>

![分布式并行转码服务架构组网](https://github.com/runner365/cloudencode/blob/master/doc/fenbushi.jpg)
<br/>

主要步骤如下：
* 对大文件进行切片：切片后生成xxx.ts上传到oss存储，并lpush到redis消息队列中
* 所有转码进程读取redis消息队列中的消息，rpop出xxx.ts的文件名，对其进行转码
* 对xxx.ts转码完成后，上传消息到redis通知该分片转码完毕
* 如果所有的ts分片都转码完毕，就启动合并程序，将多个ts小文件合成完整mp4。

## 1.2. 如何编译和运行
编译方法: go build encoder.go <br/>
运行: ./encoder -c conf/encode.json -f logs/encode.log

## 1.3. 使用说明
使用说明简要文档: [使用说明](https://github.com/runner365/cloudencode/blob/master/doc/howtouse.md)

