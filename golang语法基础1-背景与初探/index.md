# Golang语法基础1-背景与初探


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

最直接的方式是跟踪 Go 语言的源码库，关注提交历史和 [issue](https://github.com/golang/go/issues)

1. 原始代码库： https://go.googlesource.com/go
2. github镜像： https://github.com/golang/go

其它活跃论坛或动态

- [golang-dev](https://groups.google.com/forum/#!forum/golang-dev)：Google邮件列表的Go开发组讨论区
- [golang-nuts](https://groups.google.com/forum/#!forum/golang-nuts)：Google邮件列表的Go讨论社区
- [golang-announce](https://groups.google.com/forum/#!forum/golang-announce)：发布Go版本或Go开发的最新状态
- [go.dev](https://go.dev/)：2019.11.14上线的Go开发人员中心
- [gotime](https://changelog.com/gotime)：Go的一个播客，每周一更，内容有干货
- [@golang](https://twitter.com/golang)：Go 语言在 Twitter 的官方帐号

Go 下载地址和相关的文档、标准库等访问地址为

- 官网 https://golang.org/
- 国内的镜像网站 https://golang.google.cn/

[Go语言中文网](https://studygolang.com/) 是国内最活跃的Go社区，每周会发行一份 [Go语言爱好者周刊](https://studygolang.com/go/weekly)

Go 相关资料聚集最多的还是 [go wiki](https://github.com/golang/go/wiki/Articles)

## 3. 下载安装

MacOS 下快速安装可以使用 Homebrew ，执行如下命令即可

```bash
$ brew install go
```

自动配置环境变量，安装完重启终端即可使用。

安装 Go 完成后，通过 `brew list` 查看，是否已安装成功。 用 `go version` 查看当前go的版本号。 需要升级 Go 语言版本的，输入以下命令：

```bash
$ brew upgrade go
```

下面开始介绍常规的安装方法。

### 3.1 下载安装

Golang 中国官网下载页面为 [golang.google.cn/dl](https://golang.google.cn/dl/)，为 Windows、MacOS 和 Linux 三种环境都提供了安装包。

Windows 默认下载文件为 `go1.20.windows-amd64.msi`，双击启动即可安装，默认安装到`Program Files` 或`Program Files (x86)`。也可以根据需要更改位置。安装后，需要关闭并重新打开所有打开的命令提示符，以便安装程序对环境所做的更改反映在命令提示符中。环境变量将自动设置。但如果下载了以.zip为后缀的版本，则需要自己解压到合适的路径，并自己设置环境变量。

MacOS 默认下载文件为 `go1.20.darwin-amd64.pkg`，该软件包将 Go 发行版安装到 `/usr/local/go`。该包应将 `/usr/local/go/bin` 目录放入您的 `PATH`环境变量中。安装后，需要重新启动所有打开的终端会话才能使更改生效。

### 3.2 配置

打开终端配置代理
```bash
go env -w GOPROXY=https://goproxy.cn,direct
```

## 4. 编辑器/IDE

Golang 开发最流行的两个工具是 Goland 和 VScode，我自己是 VScode 的使用者。除了这两个工具外，官方还提供了一份[IDE和插件列表](https://github.com/golang/go/wiki/IDEsAndTextEditorPlugins)。

VScode 中的 Go 扩展提供了大量的特性，如自动补全、悬停信息显示、括号匹配等，原本属于第三方开发者维护，现在交给了 Go 团队。详细的特性说明查看[官网](https://code.visualstudio.com/docs/languages/go)或是[VS Code 中的 Go 扩展 Github 项目](https://github.com/golang/vscode-go)，下面进行一些简单介绍。

### 4.1 Go工具链

微软在开发 VS Code 过程中, 定义了一种协议： [语言服务器协议](https://microsoft.github.io/language-server-protocol/) ，用来为每种语言提供诸如自动完成，代码提示等功能。gopls 就是Go语言的服务器。

当在 VS Code 中编辑 Go 代码时如果没有安装，VScode 会在右下角弹出提示，只要直接点击 `Install` 即可，不需要自己输入命令。默认使用了 GOPATH 作为安装路径。

如果没有弹出，则在 VS Code 中安装 Go 扩展插件

1. `shift+command+p` 搜索 `>Go: Insatall/Update Tools`
2. 全选后确定

分别包括了以下 7 种工具：

```bash
gotests	# 测试工具，根据函数签名生成测试用例。
gomodifytags # 一个修改 Go 源代码文件中结构体字段标签的 Go 工具。
impl # 一个根据其使用情况生成接口方法存根的 Go 工具。
goplay # 一个在线实验 Go 代码的游乐场。
dlv	# Go 语言的调试器，它允许开发人员暂停程序执行，检查变量的值，设置断点，以及执行其他调试操作。
staticcheck # 一个 Go 静态检查工具，对 Go 源代码执行各种检查，包括检测潜在错误、找到代码缓慢、不安全或过于复杂的代码。
gopls # Go 语言服务器是一个为各种文本编辑器和集成开发环境（IDE）提供语言特定功能的工具，例如代码导航、代码完成和诊断等。
```

### 4.2 用户和工作区设置

使用 VS Code 需要关心的一个重要部分是用户和工作区设置，几乎所有的事情都和它们有关。

这是两种不同的设置范围

- 用户设置：是一个全局的设置，适用于打开的任何VScode窗口
- 工作区设置：是指项目工作区的设置，只适用于对应的工作区（譬如某个文件夹）

工作区的设置会覆盖掉用户设置，它针对具体的项目，配置文件位于项目根目录`.vscode`文件夹，可与其它开发者共享。`.vscode`文件夹还用于存放调试配置和任务配置。

点击左下角的齿轮，选择`设置`，默认的设置界面是一个可视化的界面，不过也可以使用`settings.json`配置文件

- 用户设置文件在 Windows 中位于 `%APPDATA%\Code\User\settings.json`
- 用户设置文件在 MacOS 中位于 `~/Library/Application Support/Code/User/settings.json`
- 工作区设置文件位于根目录的 `.vscode` 文件夹中

> 请注意，`~` 表示用户主目录。因此，如果您的用户名是 "Cloud"，则该文件位于：
>
> /Users/Cloud/Library/Application Support/Code/User/settings.json
>
> 如果文件不存在，您可以打开 VSCode，单击文件菜单，然后单击首选项 > 设置。您可以在此处编辑设置，VSCode 将自动创建该文件。

最后，VScode大量的操作都可以通过命令完成，使用快捷键`Ctrl+Shift+P`可以打开命令输入框。

### 4.3 特性说明

在用户或工作区设置中，将 `go.autocompleteUnimportedPackages` 设为 `true` ，可以在代码中点击包名跳转查看包的具体内容。

鼠标悬停在变量、函数和结构体的名称上方可以查看它们的签名等信息，这一功能需要 `godoc` 或 `gogetdoc` 实现，通过在用户或工作区设置中调整 `go.docsTool` 来切换工具。

代码导航功能无需设置默认实现。

对源码的保存操作会自动触发格式化、编译和代码质量检查。格式化工具可以通过调整`go.formatTool`来设置。

编译的过程使用`go build`命令。

代码质量检查的工具为`golint`，也可以使用`gometalinter`，用来检查代码的规范性，检查得到的`errors`和`warning`会在编辑器里以红色/绿色波浪线标出来，下面的输出窗口也会显示详细信息。

### 4.4 调试

**调试**使用的是前面安装的`delve`工具。在 VScode 中，按`F5`启动调试，一般情况下使用默认的调试配置即可，不过还是应当对调试配置选项有一定的了解。

具体调试方法可查看 [Debugging Go code using VS Code](https://github.com/golang/vscode-go/blob/master/docs/debugging.md)，更多关于 VS Code 中 Go 调试的相关信息都可查看该文档。

**运行**程序使用快捷键`Ctrl+F5`，和调试使用的是同一个配置文件。
