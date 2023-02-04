# hugo搭建个人博客1-基础建站


[Hugo](https://gohugo.io/) 是由 Go 语言实现的静态网站生成器，可以快速建立一个静态网站，虽然多数情况下用来搭建个人博客，但也可以用作展示在线书籍、个人简历等。

<!--more-->

最早我也是使用 Hugo 搭建的个人博客，由于之前才疏学浅，我所使用的博客出现了一些 bug，我无法修复，因此也很久没有更新过文章。借着 2023 年的春节，我重新搭建了这个博客。其中很多细节已经忘记了，因此耗费了我不少时间，为了节约精力，因而有了这篇文章小记，方便自己以及后人。

本文用来记录 Hugo 使用种遇到的问题和积累的经验。本文是第一篇（也许也是最后一篇），介绍博客网站搭建的过程和一些基础配置。

## 1. 安装Hugo

**安装Hugo前请确保您的电脑上已安装了 [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) 和 [Go](https://go.dev/doc/install) 。**

详细的安装说明参见[官方文档](https://gohugo.io/getting-started/quick-start/)，这里简单介绍 MacOS 的快速安装。

MacOS 下可自行从官网下载软件包安装，也可以使用 [Homebrew](https://brew.sh/) 包管理工具快速安装

```zsh
# 更新homebrew到最新
% brew update

# 安装hugo（用homebrew默认安装扩展版本）
% brew install hugo

# 检查安装
% brew list
```

注意要安装 extended 版本，主要是因为很多主题都需要扩展版的功能，如果确认自己的主题不需要(阅读主题说明)，可以按照正常的版本。

## 2. 生成博客网站

执行下面的命令在本地生成博客网站项目文件夹，该文件夹是这一系列文章之后所有操作执行的根目录(简称为项目根目录)，我建立的项目文件夹名为 cloud1998.github.io （因为之后要使用 [Github Pages](https://pages.github.com/) 托管博客）。

```zsh
% hugo new site cloud1998.github.io
% cd cloud1998.github.io
```

blog 文件夹的目录结构如下所示：

```bash
% ls
config.toml	layouts		static
archetypes	content		themes
assets		data		resources
```

其中：

- config.toml 是博客的配置文件
- content 是博客文章存放的地方
- themes 是博客主题目录

## 3. 安装主题

Hugo没有默认主题，需要自己从官方的[主题列表](https://themes.gohugo.io/)下载安装。其中 [LoveIt](https://themes.gohugo.io/themes/loveit/) 是我喜欢的主题。因为主题通常是单独的 Github 仓库，因此将其作为博客项目的子模块进行管理。

```bash
# 初始化项目目录为 git 仓库
git init
# 将主题项目作为子模块添加
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

复制主题提供的站点配置文件 `config.toml` 到项目根目录，覆盖 Hugo 本身的站点配置文件。

请打开下面的代码块查看完整的配置⬇️：

```toml
baseURL = "http://example.org/"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

# 网站标题
title = "我的全新 Hugo 网站"

# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"
# 是否包括中日韩文字
hasCJKLanguage = true

# 默认每页列表显示的文章数目
paginate = 12
# 谷歌分析代号 [UA-XXXXXXXX-X]
googleAnalytics = ""
# 版权描述，仅仅用于 SEO
copyright = ""

# 是否使用 robots.txt
enableRobotsTXT = true
# 是否使用 git 信息
enableGitInfo = true
# 是否使用 emoji 代码
enableEmoji = true

# 忽略一些构建错误
ignoreErrors = ["error-remote-getjson", "error-missing-instagram-accesstoken"]

# 作者配置
[author]
  name = "xxxx"
  email = ""
  link = ""

# 菜单配置
[menu]
  [[menu.main]]
    weight = 1
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
  [[menu.main]]
    weight = 2
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
  [[menu.main]]
    weight = 3
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""

[params]
  # 网站默认主题样式 ["auto", "light", "dark"]
  defaultTheme = "auto"
  # 公共 git 仓库路径，仅在 enableGitInfo 设为 true 时有效
  gitRepo = ""
  #  哪种哈希函数用来 SRI, 为空时表示不使用 SRI
  # ["sha256", "sha384", "sha512", "md5"]
  fingerprint = ""
  #  日期格式
  dateFormat = "2006-01-02"
  # 网站标题, 用于 Open Graph 和 Twitter Cards
  title = "我的网站"
  # 网站描述, 用于 RSS, SEO, Open Graph 和 Twitter Cards
  description = "这是我的全新 Hugo 网站"
  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = ["/logo.png"]

  # 页面头部导航栏配置
  [params.header]
    # 桌面端导航栏模式 ["fixed", "normal", "auto"]
    desktopMode = "fixed"
    # 移动端导航栏模式 ["fixed", "normal", "auto"]
    mobileMode = "auto"
    #  页面头部导航栏标题配置
    [params.header.title]
      # LOGO 的 URL
      logo = ""
      # 标题名称
      name = ""
      # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
      pre = ""
      # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
      post = ""
      #  是否为标题显示打字机动画
      typeit = false

  # 页面底部信息配置
  [params.footer]
    enable = true
    #  自定义内容 (支持 HTML 格式)
    custom = ''
    #  是否显示 Hugo 和主题信息
    hugo = true
    #  是否显示版权信息
    copyright = true
    #  是否显示作者
    author = true
    # 网站创立年份
    since = 2019
    # ICP 备案信息，仅在中国使用 (支持 HTML 格式)
    icp = ""
    # 许可协议信息 (支持 HTML 格式)
    license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

  #  Section (所有文章) 页面配置
  [params.section]
    # section 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  #  List (目录或标签) 页面配置
  [params.list]
    # list 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  #  应用图标配置
  [params.app]
    # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
    title = "我的网站"
    # 是否隐藏网站图标资源链接
    noFavicon = false
    # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
    svgFavicon = ""
    # Android 浏览器主题色
    themeColor = "#ffffff"
    # Safari 图标颜色
    iconColor = "#5bbad5"
    # Windows v8-10磁贴颜色
    tileColor = "#da532c"

  #  搜索配置
  [params.search]
    enable = true
    # 搜索引擎的类型 ["lunr", "algolia"]
    type = "lunr"
    # 文章内容最长索引长度
    contentLength = 4000
    # 搜索框的占位提示语
    placeholder = ""
    #  最大结果数目
    maxResultLength = 10
    #  结果内容片段长度
    snippetLength = 50
    #  搜索结果中高亮部分的 HTML 标签
    highlightTag = "em"
    #  是否在搜索索引中使用基于 baseURL 的绝对路径
    absoluteURL = false
    [params.search.algolia]
      index = ""
      appID = ""
      searchKey = ""

  # 主页配置
  [params.home]
    #  RSS 文章数目
    rss = 10
    # 主页个人信息
    [params.home.profile]
      enable = true
      # Gravatar 邮箱，用于优先在主页显示的头像
      gravatarEmail = ""
      # 主页显示头像的 URL
      avatarURL = "/images/avatar.png"
      #  主页显示的网站标题 (支持 HTML 格式)
      title = ""
      # 主页显示的网站副标题 (允许 HTML 格式)
      subtitle = "这是我的全新 Hugo 网站"
      # 是否为副标题显示打字机动画
      typeit = true
      # 是否显示社交账号
      social = true
      #  免责声明 (支持 HTML 格式)
      disclaimer = ""
    # 主页文章列表
    [params.home.posts]
      enable = true
      # 主页每页显示文章数量
      paginate = 6
      #  被 params.page 中的 hiddenFromHomePage 替代
      # 当你没有在文章前置参数中设置 "hiddenFromHomePage" 时的默认行为
      defaultHiddenFromHomePage = false

  # 作者的社交信息设置
  [params.social]
    GitHub = "xxxx"
    Linkedin = ""
    Twitter = "xxxx"
    Instagram = "xxxx"
    Facebook = "xxxx"
    Telegram = "xxxx"
    Medium = ""
    Gitlab = ""
    Youtubelegacy = ""
    Youtubecustom = ""
    Youtubechannel = ""
    Tumblr = ""
    Quora = ""
    Keybase = ""
    Pinterest = ""
    Reddit = ""
    Codepen = ""
    FreeCodeCamp = ""
    Bitbucket = ""
    Stackoverflow = ""
    Weibo = ""
    Odnoklassniki = ""
    VK = ""
    Flickr = ""
    Xing = ""
    Snapchat = ""
    Soundcloud = ""
    Spotify = ""
    Bandcamp = ""
    Paypal = ""
    Fivehundredpx = ""
    Mix = ""
    Goodreads = ""
    Lastfm = ""
    Foursquare = ""
    Hackernews = ""
    Kickstarter = ""
    Patreon = ""
    Steam = ""
    Twitch = ""
    Strava = ""
    Skype = ""
    Whatsapp = ""
    Zhihu = ""
    Douban = ""
    Angellist = ""
    Slidershare = ""
    Jsfiddle = ""
    Deviantart = ""
    Behance = ""
    Dribbble = ""
    Wordpress = ""
    Vine = ""
    Googlescholar = ""
    Researchgate = ""
    Mastodon = ""
    Thingiverse = ""
    Devto = ""
    Gitea = ""
    XMPP = ""
    Matrix = ""
    Bilibili = ""
    Discord = ""
    DiscordInvite = ""
    Lichess = ""
    ORCID = ""
    Pleroma = ""
    Kaggle = ""
    MediaWiki= ""
    Plume = ""
    HackTheBox = ""
    RootMe= ""
    Phone = ""
    Email = "xxxx@xxxx.com"
    RSS = true # 

  #  文章页面全局配置
  [params.page]
    #  是否在主页隐藏一篇文章
    hiddenFromHomePage = false
    #  是否在搜索结果中隐藏一篇文章
    hiddenFromSearch = false
    #  是否使用 twemoji
    twemoji = false
    # 是否使用 lightgallery
    lightgallery = false
    #  是否使用 ruby 扩展语法
    ruby = true
    #  是否使用 fraction 扩展语法
    fraction = true
    #  是否使用 fontawesome 扩展语法
    fontawesome = true
    # 是否在文章页面显示原始 Markdown 文档链接
    linkToMarkdown = true
    #  是否在 RSS 中显示全文内容
    rssFullText = false
    #  目录配置
    [params.page.toc]
      # 是否使用目录
      enable = true
      #  是否保持使用文章前面的静态目录
      keepStatic = true
      # 是否使侧边目录自动折叠展开
      auto = true
    #  代码配置
    [params.page.code]
      # 是否显示代码块的复制按钮
      copy = true
      # 默认展开显示的代码行数
      maxShownLines = 50
    #  KaTeX 数学公式
    [params.page.math]
      enable = true
      #  默认行内定界符是 $ ... $ 和 \( ... \)
      inlineLeftDelimiter = ""
      inlineRightDelimiter = ""
      #  默认块定界符是 $$ ... $$, \[ ... \],  \begin{equation} ... \end{equation} 和一些其它的函数
      blockLeftDelimiter = ""
      blockRightDelimiter = ""
      # KaTeX 插件 copy_tex
      copyTex = true
      # KaTeX 插件 mhchem
      mhchem = true
    #  Mapbox GL JS 配置
    [params.page.mapbox]
      # Mapbox GL JS 的 access token
      accessToken = ""
      # 浅色主题的地图样式
      lightStyle = "mapbox://styles/mapbox/light-v10?optimize=true"
      # 深色主题的地图样式
      darkStyle = "mapbox://styles/mapbox/dark-v10?optimize=true"
      # 是否添加 NavigationControl
      navigation = true
      # 是否添加 GeolocateControl
      geolocate = true
      # 是否添加 ScaleControl
      scale = true
      # 是否添加 FullscreenControl
      fullscreen = true
    #  文章页面的分享信息设置
    [params.page.share]
      enable = true
      Twitter = true
      Facebook = true
      Linkedin = false
      Whatsapp = false
      Pinterest = false
      Tumblr = false
      HackerNews = true
      Reddit = false
      VK = false
      Buffer = false
      Xing = false
      Line = true
      Instapaper = false
      Pocket = false
      Flipboard = false
      Weibo = true
      Blogger = false
      Baidu = false
      Odnoklassniki = false
      Evernote = false
      Skype = false
      Trello = false
      Mix = false
    #  评论系统设置
    [params.page.comment]
      enable = false
      # Disqus 评论系统设置
      [params.page.comment.disqus]
        # 
        enable = false
        # Disqus 的 shortname，用来在文章中启用 Disqus 评论系统
        shortname = ""
      # Gitalk 评论系统设置
      [params.page.comment.gitalk]
        # 
        enable = false
        owner = ""
        repo = ""
        clientId = ""
        clientSecret = ""
      # Valine 评论系统设置
      [params.page.comment.valine]
        enable = false
        appId = ""
        appKey = ""
        placeholder = ""
        avatar = "mp"
        meta= ""
        pageSize = 10
        # 为空时自动适配当前主题 i18n 配置
        lang = ""
        visitor = true
        recordIP = true
        highlight = true
        enableQQ = false
        serverURLs = ""
        #  emoji 数据文件名称, 默认是 "google.yml"
        # ["apple.yml", "google.yml", "facebook.yml", "twitter.yml"]
        # 位于 "themes/LoveIt/assets/lib/valine/emoji/" 目录
        # 可以在你的项目下相同路径存放你自己的数据文件:
        # "assets/lib/valine/emoji/"
        emoji = ""
      # Facebook 评论系统设置
      [params.page.comment.facebook]
        enable = false
        width = "100%"
        numPosts = 10
        appId = ""
        # 为空时自动适配当前主题 i18n 配置
        languageCode = "zh_CN"
      #  Telegram Comments 评论系统设置
      [params.page.comment.telegram]
        enable = false
        siteID = ""
        limit = 5
        height = ""
        color = ""
        colorful = true
        dislikes = false
        outlined = false
      #  Commento 评论系统设置
      [params.page.comment.commento]
        enable = false
      #  utterances 评论系统设置
      [params.page.comment.utterances]
        enable = false
        # owner/repo
        repo = ""
        issueTerm = "pathname"
        label = ""
        lightTheme = "github-light"
        darkTheme = "github-dark"
      # giscus comment 评论系统设置 (https://giscus.app/zh-CN)
      [params.page.comment.giscus]
        # 你可以参考官方文档来使用下列配置
        enable = false
        repo = ""
        repoId = ""
        category = "Announcements"
        categoryId = ""
        # 为空时自动适配当前主题 i18n 配置
        lang = ""
        mapping = "pathname"
        reactionsEnabled = "1"
        emitMetadata = "0"
        inputPosition = "bottom"
        lazyLoading = false
        lightTheme = "light"
        darkTheme = "dark"
    #  第三方库配置
    [params.page.library]
      [params.page.library.css]
        # someCSS = "some.css"
        # 位于 "assets/"
        # 或者
        # someCSS = "https://cdn.example.com/some.css"
      [params.page.library.js]
        # someJavascript = "some.js"
        # 位于 "assets/"
        # 或者
        # someJavascript = "https://cdn.example.com/some.js"
    #  页面 SEO 配置
    [params.page.seo]
      # 图片 URL
      images = []
      # 出版者信息
      [params.page.seo.publisher]
        name = ""
        logoUrl = ""

  #  TypeIt 配置
  [params.typeit]
    # 每一步的打字速度 (单位是毫秒)
    speed = 100
    # 光标的闪烁速度 (单位是毫秒)
    cursorSpeed = 1000
    # 光标的字符 (支持 HTML 格式)
    cursorChar = "|"
    # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
    duration = -1

  # 网站验证代码，用于 Google/Bing/Yandex/Pinterest/Baidu
  [params.verification]
    google = ""
    bing = ""
    yandex = ""
    pinterest = ""
    baidu = ""

  #  网站 SEO 配置
  [params.seo]
    # 图片 URL
    image = ""
    # 缩略图 URL
    thumbnailUrl = ""

  #  网站分析配置
  [params.analytics]
    enable = false
    # Google Analytics
    [params.analytics.google]
      id = ""
      # 是否匿名化用户 IP
      anonymizeIP = true
    # Fathom Analytics
    [params.analytics.fathom]
      id = ""
      # 自行托管追踪器时的主机路径
      server = ""
    # Plausible Analytics
    [params.analytics.plausible]
      dataDomain = ""
    # Yandex Metrica
    [params.analytics.yandexMetrica]
      id = ""

  #  Cookie 许可配置
  [params.cookieconsent]
    enable = true
    # 用于 Cookie 许可横幅的文本字符串
    [params.cookieconsent.content]
      message = ""
      dismiss = ""
      link = ""

  #  第三方库文件的 CDN 设置
  [params.cdn]
    # CDN 数据文件名称, 默认不启用
    # ["jsdelivr.yml"]
    # 位于 "themes/LoveIt/assets/data/cdn/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/data/cdn/"
    data = ""

  #  兼容性设置
  [params.compatibility]
    # 是否使用 Polyfill.io 来兼容旧式浏览器
    polyfill = false
    # 是否使用 object-fit-images 来兼容旧式浏览器
    objectFit = false

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # 目录设置
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6

# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# Permalinks 配置
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"

# 隐私信息配置
[privacy]
  #  Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
    # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# 用于 Hugo 输出文档的设置
[outputs]
  # 
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]
```

运行`hugo serve`命令，在浏览器键入网址 [http://localhost:1313](http://localhost:1313) 预览主题效果（首页图片未加载是因为还没有放置头像文件）。

## 4. 网站配置

正式使用前，我们需要编辑站点配置文件从而设置网站的一些内容，上面的配置文件已经进行了详细的说明，如有更多疑问请查阅LoveIt官方[主题文档](https://hugoloveit.com/zh-cn/theme-documentation-basics/)。

### 4.1 头像

新建`static/images`文件夹，将头像文件 `avatar.png` 存放在这里。

### 4.2 网站图标

使用 [favicon generator](https://www.google.com/search?q=favicon+generator) 生成配套的网站图标，放到 `/static` 目录下，可以设置网站在各平台的显示图标，包括如下内容

- android-chrome-192x192.png
- android-chrome-512x512.png
- apple-touch-icon.png
- browserconfig.xml
- cover.png
- favicon.ico
- favicon-16x16.png
- favicon-32x32.png
- logo.png
- mstile-150x150.png
- safari-pinned-tab.svg
- site.webmanifest

然后修改站点配置文件中的配置项即可。

更多配置可以参考[Mogeko的个人博客](https://github.com/Mogeko/Blog)

## 5. 托管到Github

将本地的所有项目文件提交到本地仓库中。

```zsh
% git add .
% git commit -m "Initial commit"
```

浏览器打开 Github 网站，创建和项目文件夹同名的仓库，该仓库用于存储项目文件夹下所有内容。创建完成后，在本地项目根目录，执行下列命令，将项目文件推送到远程仓库。

```zsh
% git remote add origin https://github.com/cloud1998/cloud1998.github.io.git
% git push -u origin main
```

关于网页如何托管在Github的详细说明可以参考[Host on Github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

### 5.1 源码备份

按照 Hugo 的生成规则，执行 `hugo` 命令后，网站静态文件将会生成在 `public` 文件夹。但由于我们使用 Github Pages 托管博客网站，该功能启用后 Github 仓库只会从 `main branch` 或 `main branch` 中的 `/docs` 目录下读取网站源码。

我们解决这一问题的方法是新建 `blog` 分支将博客源码放在该分支下，利用 Github Action 自动根据 blog 分支的博客源码执行 hugo 命令，并将生成的结果推送到 `main` 分支。首先在本地项目根目录下执行下列命令新建并切换到 `blog` 分支。

注：Github Action 的说明见附录I

```bash
% git checkout -b blog
% git branch
* blog
  main
  
# 将新分支推送到远程仓库 并建立跟踪关系
% git push -u origin blog
```

> 在使用 "git push" 时，第一次推送新分支时，需要使用 "--set-upstream" 或 "-u" 选项，以便在远程仓库中设置跟踪关系。

将本地 `blog` 分支的内容推送到远程仓库后，在网页端进入`cloud1998.github.io`仓库的设置页面，将默认分支设置为 `blog` 分支。

### 5.2 推送到main分支

首先生成公私钥供 Github Action 使用

```bash
ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f blog -N ""
# You will get 2 files in current file:
#   blog.pub (public key)
#   blog     (private key)
```

然后进入 `cloud1998.github.io` 仓库设置页面，在 `Deploy Keys` 中添加公钥，在 `Secrets` 中添加私钥，私钥名设置为 `ACTIONS_DEPLOY_KEY`

接着新建 YAML 配置文件，Github Action 要求配置文件位于 `.github/workflows` 目录下，新建完成后目录结构如下

```bash
$ ls ./.github/workflows
main.yml
```

Github Action使用一种模块化的思路，即将很多持续集成的操作写成独立的脚本文件，放到代码仓库，让其它开发者使用。因此进行持续集成时，可以直接引用别人写好的 action，整个持续集成的过程，就是一个 actions 组合的过程。GitHub 做了一个[官方市场](https://github.com/marketplace?type=actions)，可以搜索到他人提交的 actions。另外，还有一个 [awesome actions](https://github.com/sdras/awesome-actions) 的仓库，也可以找到不少 action。

我们的基本思路如下

1. 整个流程在 blog 分支 push 时触发
2. 只有一个job，运行在ubuntu-20.04环境下
3. 使用官方提供的 [action/checkout](https://github.com/actions/checkout) 获取仓库源码，注意添加参数clone主题子模块
4. 使用 [peaceiris/actions-hugo: GitHub Actions for Hugo](https://github.com/peaceiris/actions-hugo) 部署 hugo 环境，注意使用 `extentded` 版本（主题要求）
5. 直接执行 hugo 命令
6. 使用 [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) 将执行的结果部署到GitHub Pages的源目录，默认即main分支的目录下。

完整的`main.yml`脚本内容如下

```yaml
name: hugo push to github pages

on:
  push:
    branches:
    - blog

jobs:
  build-deploy:
    runs-on: ubuntu-20.04
    steps:
    - uses: actions/checkout@v1
      with:
        submodules: true

    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: '0.63.2'
        extended: true

    - name: Build
      run: HUGO_ENV=production hugo --gc --minify

    - name: Deploy
      uses: peaceiris/actions-gh-pages@v2
      env:
        ACTIONS_DEPLOY_KEY: ${{ secrets.ACTIONS_DEPLOY_KEY }}
        PUBLISH_BRANCH: main
        PUBLISH_DIR: ./public
```

保存上面的文件后，将本地仓库推送到远程，Github 检测到 `.github/workflow` 目录和里面的`main.yml` 文件，就会自动运行，在网页端可以查看运行日志，如果出现错误可以根据日志内容就行修改。

等到 workflow 运行结束，访问博客页面，就可以看到更新成功了。切换到 main 分支，也可以看到推送的网页文件，不过因为设置了默认分支为 blog，以后打开网页端该仓库，以及在本地 clone 的时候，默认都是 blog 分支。

## 6. 文章发布

在 content 目录下创建 `posts`文件夹，写作的文章全部放到该目录下，在每篇文章开头添加元数据字段，可以是YAML或TOML格式，示例如下

```toml
title = "Getting Started with Hugo"
description = ""
type = ["posts","post"]
tags = [
    "go",
    "golang",
    "hugo",
    "development",
]
date = "2014-04-02"
categories = [
    "Development",
    "golang",
]
series = ["Hugo 101"]
[ author ]
  name = "Hugo Authors"
```

下面是一篇示例文章

```markdown
---
title: 示例文章
date: 2023-01-30
tags: ["博客搭建"]
categories: ["编程技术"]
---
这是一篇示例文章。
```

文章保存后将仓库新增内容推送到远程仓库：

```zsh
% git add .
% git commit -m "更新了一篇文章"
% git push
```

几分钟后即可在  https://cloud1998.github.io 看到这篇文章。

## 附录I Github Action

[GitHub Actions](https://github.com/features/actions) 是 GitHub 在2018年10月推出的一个[持续集成服务](http://www.ruanyifeng.com/blog/2015/09/continuous-integration.html)，之前一直是试用阶段，2019年末开放，据说比[Travis CI](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html) 更简单更好用。

Github Actions入门可以阅读[官方文档](https://help.github.com/en/actions/automating-your-workflow-with-github-actions)或者阮一峰大神的[GitHub Actions 入门教程](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)。
