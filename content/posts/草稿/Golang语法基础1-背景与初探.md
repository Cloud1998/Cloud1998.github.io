---
title: Golang语法基础1-背景与初探
date: 2023-02-02
tags: [Golang语法基础]
categories: [编程技术]
slug: golang basic grammar 1 background and preliminary exploration
draft: true
---

## 1. 起源与发展

Go/Golang 起源于2007年，并于 2009 年正式对外发布，是一个完全开源的项目，背后的支持者是谷歌公司，它的主要目标是「兼具 Python 等动态语言的开发速度和 C/C++ 等编译型语言的性能与安全性」。它不但能让你访问底层操作系统，还提供了强大的网络编程和并发编程支持。

Go 的核心设计者是三位著名IT工程师：Ken Thompson，Rob Pike，Robert Griesemer。其中 Ken Thompson 是 Unix 操作系统的设计者，并因此获得图灵奖，也是 C 语言前身 B 语言的设计者，UTF-8 编码设计者之一，计算机史的重要人物，2006 年加入谷歌，和另外两人一起设计了 Go 语言。 Rob Pike 是 Ken 的老搭档。

随后又有 lan Lance Taylor 和 Russ Cox 两人加入团队，前者是 gccgo 编译器的作者和 cgo 工具链的维护者，后者加入团队后着手 Go 语言标准库的开发。

Go 语言以囊地鼠(Gopher)为图标和吉祥物，这是才华横溢的插画家 Renee French 设计的，她也是 Go 设计者之一 Rob Pike 的妻子。囊地鼠是一种原产于加拿大的啮齿类动物，Go 语言开发者也一般自称为 Gopher。

Go 语言相比于其它语言的最大优势在于它的执行性能与开发效率，这得益于 Go 在并发编程、内存回收等许多方面的良好设计，并因此大规模用于服务器编程、网络编程、数据库和云平台领域。

Go 是一种编译型的语言。它使用编译器来编译代码。编译器将源代码编译成二进制（或字节码）格式；在编译代码时，编译器检查错误、优化性能并输出可在不同平台上运行的二进制文件。要创建并运行 Go 程序，程序员必须执行如下步骤。

1. 使用文本编辑器创建 Go 程序；
2. 保存文件；
3. 编译程序；
4. 运行编译得到的可执行文件。

这不同于 Python、Ruby 和 JavaScript 等语言，它们不包含编译步骤。Go 自带了编译器，因此无须单独安装编译器。

比较出名的 Go 语言项目有(不限于这些)

- Go语言本身： https://github.com/golang/go
- Docker： https://www.docker.com/
- kubernetes： https://github.com/kubernetes/kubernetes
- Ethereum： https://github.com/ethereum/go-ethereum
- fabric： https://github.com/hyperledger/fabric
-  Hugo： https://github.com/gohugoio/hugo
- TiDB： https://github.com/pingcap/tidb
- InfluxDB： https://github.com/influxdata/influxdb
- ETCD： https://github.com/etcd-io/etcd

使用 Go 的国外公司有：Google、Docker、NetFlix、CloudFlare、Dropbox、MongoDB、Uber等。

使用 Go 的国内公司有：七牛、字节跳动、bilibili、京东、[百度](https://github.com/baidu/bfe)、小米、腾讯、阿里等。

## 2. 跟踪最新动态

最直接的方式是跟踪Go语言的源码库，关注提交历史和[issue](https://github.com/golang/go/issues)

1. 原始代码库： https://go.googlesource.com/go
2. github镜像： https://github.com/golang/go

其它活跃论坛或动态

- [golang-dev](https://groups.google.com/forum/#!forum/golang-dev)：Google邮件列表的Go开发组讨论区
- [golang-nuts](https://groups.google.com/forum/#!forum/golang-nuts)：Google邮件列表的Go讨论社区
- [golang-announce](https://groups.google.com/forum/#!forum/golang-announce)：发布Go版本或Go开发的最新状态
- [go.dev](https://go.dev/)：2019.11.14上线的Go开发人员中心
- [gotime](https://changelog.com/gotime)：Go的一个播客，每周一更，内容有干货
- [@golang](https://twitter.com/golang)：Go 语言在 Twitter 的官方帐号

此外还有每年举办的几个大会

- [Gopher Con](https://www.gophercon.com/)，举办地在美国，时间不定，今年在7月，2020年会在6月份举行。[会议总结](https://github.com/gophercon)
- [GopherChina](https://gopherchina.org/)，举办地在中国，每年4月份。[会议总结](https://github.com/gopherchina/conference)
- [dotGo](https://www.dotgo.eu/)，举办地在欧洲，每年3月份
- 详细的会议列表可查看 https://github.com/golang/go/wiki/Conferences

Go 下载地址和相关的文档、标准库等访问地址为

- 官网 https://golang.org/
- 国内的镜像网站 https://golang.google.cn/

[Go语言中文网](https://studygolang.com/) 是国内最活跃的Go社区，每周会发行一份[Go语言爱好者周刊](https://studygolang.com/go/weekly)

Go相关资料聚集最多的还是[go wiki](https://github.com/golang/go/wiki/Articles)

## 3. 下载安装

MacOS下快速安装可以使用 Homebrew ，执行如下命令即可

```bash
$ brew hugo
```

自动配置环境变量，安装完重启终端即可使用。下面开始介绍常规的安装方法，以win10和Ubuntu为例。

### 3.1 下载

Golang中国官网下载页面为 [golang.google.cn/dl](https://golang.google.cn/dl/)，为windows，macOS和Linux三种环境都提供了安装包。



## 4. 第一个程序

编辑并运行我们的第一个程序，按照编程界的惯例，输出`hello, world!`。

首先创建项目文件夹并初始化模块，模块路径自行选择

## 5. 编辑器/IDE

Golang 开发最流行的两个工具是 Goland 和 VScode，我自己是 VScode 的使用者。除了这两个工具外，官方还提供了一份[IDE和插件列表](https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins)。

VScode 中的 Go 扩展提供了大量的特性，如自动补全、悬停信息显示、括号匹配等，原本属于第三方开发者维护，现在交给了 Go 团队。详细的特性说明查看[官网](https://code.visualstudio.com/docs/languages/go)，下面进行一些简单介绍。

### 5.1 Go工具链

微软在开发 VS Code 过程中, 定义了一种协议： [语言服务器协议](https://link.zhihu.com/?target=https%3A//microsoft.github.io/language-server-protocol/) ，用来为每种语言提供诸如自动完成，代码提示等功能。gopls 就是Go语言的服务器, 安装命令为

```zsh

```

事实上，编辑 Go 代码时如果没有安装，VScode 会在右下角弹出提示，只要直接点击 `Install` 即可，不需要自己输入命令。同时，因为没有设置go.toolsGopath，默认使用了 GOPATH 作为安装路径