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

=======
# cloudencode

#### 介绍
视频转码

#### 软件架构
软件架构说明


#### 安装教程

1. xxxx
2. xxxx
3. xxxx

#### 使用说明

1. xxxx
2. xxxx
3. xxxx

#### 参与贡献

1. Fork 本仓库
2. 新建 Feat_xxx 分支
3. 提交代码
4. 新建 Pull Request


#### 码云特技

1. 使用 Readme\_XXX.md 来支持不同的语言，例如 Readme\_en.md, Readme\_zh.md
2. 码云官方博客 [blog.gitee.com](https://blog.gitee.com)
3. 你可以 [https://gitee.com/explore](https://gitee.com/explore) 这个地址来了解码云上的优秀开源项目
4. [GVP](https://gitee.com/gvp) 全称是码云最有价值开源项目，是码云综合评定出的优秀开源项目
5. 码云官方提供的使用手册 [https://gitee.com/help](https://gitee.com/help)
6. 码云封面人物是一档用来展示码云会员风采的栏目 [https://gitee.com/gitee-stars/](https://gitee.com/gitee-stars/)
