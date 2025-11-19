# 使用hugo、Github和Cloudflare免费搭建个人博客

![](https://burayimg.heisenw.com/blog/img/a3d62528-67ea-41a8-a7a0-b98c67ee3231.webp)
前阵子给好大儿搭建博客的时候学着用hugo，边学边做，磕磕绊绊地好歹运行起来了，当时就想着，等做完了一定要做个笔记。

## 前期准备
### 工具
#### Visual Studio Code  
  微软出的代码编辑器，功能非常非常强大，同样也意味着相对臃肿，不过几乎可以做到all in one，几乎覆盖了所有的常见语言。
-  [⏬️VS Code官网下载⏬️](https://code.visualstudio.com/)
- 插件
  **Chinese (Simplified) (简体中文) Language Pack**`简体中文语言包，爱国者的福音。`
  **Markdown All in One**`VS Code最受欢迎的markdown插件，集成了写作、预览、目录生成、快捷键、表格对齐、数学公式支持等众多功能。`
  **Markdown Preview Enhanced**`提供Markdown实时预览`
  **Paste Image**`不使用图床的情况下，可以直接使用粘贴功能插入图片，并自动把图片保存到md文件所在目录，我用图床，所以这个没安装`  
上面4个插件都是即装即用，不需要额外设置，非常适合新手。
- VS Code插件安装笔记
  VS Code主界面左侧点击“扩展”图标，然后搜索插件，搜到后点击“安装”就可以了
  ![](https://burayimg.heisenw.com/blog/img/146f1ad1-85f9-4672-9b78-31087fa32b7e.webp)
#### 工具网站
  [Emoji表情大全](https://emoji6.com/emojiall/)
  [icon图标下载](https://icon-icons.com/)

### github
打开[Github](https://github.com/)注册个github账号，现在大部分时间可以访问了，确实访问不了的，想办法科学。
### cloudflare
打开[Cloudflare](https://www.cloudflare-cn.com/)注册个账号，这个有中文版的，很简单。
### 域名
想要自己的域名可以注册一个，不注册也行，使用cloudflare的二级域名，也很简短，*.pages.dav的域名也不是很难记。
不过要是[用Cloudflare的R2对象存储做图床](https://www.buray.top/posts/cloudflare-r2/)的话，还是需要个自己的域名的，大部分顶级域名可以在cloudflare上直接注册，也可以在其他域名注册商那边注册后，托管到Cloudflare上面。域名注册商上网搜一大片，例如namesilo、godaddy、阿里云、腾讯云等，比比价格，哪儿合适在哪儿买，要注意续费价格，有的平台首年价格一般很低，但续费挺贵的。
## 本地搭建
### 下载hugo
1. 打开[HUGO官网](https://gohugo.io/news/)下载最新版HUGO，根据自己的电脑系统下载对应的版本，注意使劲往下拉到底，下载`hugo_extended_withdeploy`版本，主要是有的主题不能适配单纯的`latest`版本hugo。
2. 在D盘（或者随便哪个盘）根目录下新建文件夹，命名`hugo`，将下载的hugo压缩包解压，把hugo.exe粘贴到`hugo`文件夹下。
3. 打开`我的电脑`，在空白处右键➡️属性➡️高级系统设置➡️环境变量➡️系统变量，双击打开Path➡️新建➡️输入`D:\hugo`➡️一路`确定`保存。这一步不操作的话后面命令提示框无法运行`hugo`命令，会提示`'hugo' 不是内部或外部命令，也不是可运行的程序`。
### 创建网站
1. 进入`D:\hugo`，在地址栏输入`cmd`回车，进入命令提示行。
2. 输入`hugo new site xxx`回车（xxx可以改成你的，例如`myblog`，注意不能用中文）,会出现如下提示。
   ```
    Microsoft Windows [版本 10.0.19044.3086]
    (c) Microsoft Corporation。保留所有权利。

    D:\hugo>hugo new site myblog
    Congratulations! Your new Hugo site was created in D:\hugo\myblog.

    Just a few more steps...

    1. Change the current directory to D:\hugo\myblog.
    2. Create or install a theme:
      - Create a new theme with the command "hugo new theme <THEMENAME>"
      - Or, install a theme from https://themes.gohugo.io/
    3. Edit hugo.toml, setting the "theme" property to the theme name.
    4. Create new content with the command "hugo new content <SECTIONNAME>\<FILENAME>.<FORMAT>".
    5. Start the embedded web server with the command "hugo server --buildDrafts".

    See documentation at https://gohugo.io/.
   ```
3. 可以看到`D:\hugo`文件夹下多了`xxx文件夹`，网站就算创建成功了。
  ![](https://burayimg.heisenw.com/blog/img/a00283f5-93fe-4217-82a7-e9e0e9b9facd.webp)
### 安装模板
1. 登录[HUGO模板网站](https://themes.gohugo.io/)，挑一个自己喜欢的主题，本站使用的[FixIt](https://themes.gohugo.io/themes/fixit/)主题，主要是他有中文文档，非常详细和完整，并且一直在保持更新。其他主题也可以用，就是要花一点心思读一下文档，我个人建议新手小白从有中文文档的主题开始，一个主题弄明白了，其他主题的基本原理也差不多就搞懂了。
![](https://burayimg.heisenw.com/blog/img/1f66df2c-3ad7-4a47-8208-637e5360eb7c.webp)
2. 以FixIt主题为例，点击`Download`跳转到[FixIt主题Github](https://github.com/hugo-fixit/FixIt)。
![](https://burayimg.heisenw.com/blog/img/99da0069-b920-48c4-be0c-75555513edcc.webp)
3. 点击右下角的`Releases`里面的`Latest`版本。写这篇文章的时候latest版本是`v0.3.20`，如果有新的正式版本可以下载新的，`Alpha`版本不建议新手用，部分功能可能尚未完善。等会一些了，可以玩玩。
4. 拉到最底下`Assets`里面可以看到代码下载链接了，点击下载即可
![](https://burayimg.heisenw.com/blog/img/394758f8-ae41-4df4-ae0a-944ed73f8fb9.webp)
5. 下载后的文件解压得到`FixIt-0.3.20`文件夹，修改文件夹名为`FixIt`（为了方便后面调用），复制这个文件夹到`D:\hugo\myblog\themes\`文件夹下。
6. 在文件夹内找到`hugo.toml`文件，复制到网站根目录`D:\hugo\myblog\`下，替换掉原来的`hugo.toml`文件。
7. 在网站根目录`D:\hugo\myblog\`下新建`content`文件夹，`content`下新建`posts`文件夹，以后写的文章都放在这个文件夹里。
### 网站配置
1. 打开`hugo.toml`文件（用VS code或者记事本都行，建议用VS code吧，更方便阅读一些）
  ```
  # =====================================================================================
  # It's recommended to use Alternate Theme Config to configure FixIt
  # Modifying this file may result in merge conflict
  # There are currently some restrictions to what a theme component can configure:
  # params, menu, outputformats and mediatypes
  # =====================================================================================

  # -------------------------------------------------------------------------------------
  # Hugo Configuration
  # See: https://gohugo.io/getting-started/configuration/
  # -------------------------------------------------------------------------------------

  # website title
  title = ""
  # Hostname (and path) to the root
  baseURL = "http://localhost:1313"
  # theme list
  # theme = ["FixIt"] # enable in your site config file
  # determines default content language ["en", "zh-cn", "fr", "pl", ...]
  defaultContentLanguage = "en"
  # language code ["en", "zh-CN", "fr", "pl", ...]
  languageCode = "en"
  # language name ["English", "简体中文", "Français", "Polski", ...]
  languageName = "English"
  # whether to include Chinese/Japanese/Korean
  hasCJKLanguage = false
  # default amount of posts in each pages
  paginate = 12
  # copyright description used only for seo schema
  copyright = ""
  # whether to use robots.txt
  enableRobotsTXT = true
  # whether to use git commit log
  enableGitInfo = true
  # whether to use emoji code
  enableEmoji = true
  # the main sections of a site
  mainSections = []

  # -------------------------------------------------------------------------------------
  # Menu Configuration
  # See: https://fixit.lruihao.cn/documentation/basics/#menu-configuration
  # -------------------------------------------------------------------------------------

  [menu]
    [[menu.main]]
      identifier = "archives"
      parent = ""
      # you can add extra information before the name (HTML format is supported), such as icons
      pre = ""
      # you can add extra information after the name (HTML format is supported), such as icons
      post = ""
      name = "Archives"
      url = "/archives/"
      # title will be shown when you hover on this menu link
      title = ""
      weight = 1
      # FixIt 0.2.14 | NEW add user-defined content to menu items
      [menu.main.params]
        # add css class to a specific menu item
        class = ""
        # whether set as a draft menu item whose function is similar to a draft post/page
        draft = false
        # FixIt 0.2.16 | NEW add fontawesome icon to a specific menu item
        icon = "fa-solid fa-archive"
        # FixIt 0.2.16 | NEW set menu item type, optional values: ["mobile", "desktop"]
        type = ""
        # FixIt 0.3.9 | NEW whether to show the submenu item divider line
        divided = false
    [[menu.main]]
      identifier = "categories"
      parent = ""
      pre = ""
      post = ""
      name = "Categories"
      url = "/categories/"
      title = ""
      weight = 2
      [menu.main.params]
        icon = "fa-solid fa-folder-tree"
    [[menu.main]]
      identifier = "tags"
      parent = ""
      pre = ""
      post = ""
      name = "Tags"
      url = "/tags/"
      title = ""
      weight = 3
      [menu.main.params]
        icon = "fa-solid fa-tags"

  # -------------------------------------------------------------------------------------
  # Related content Configuration
  # See: https://gohugo.io/content-management/related/
  # -------------------------------------------------------------------------------------

  [related]
    includeNewer = false
    threshold = 80
    toLower = false
  [[related.indices]]
    applyFilter = false
    cardinalityThreshold = 0
    name = 'keywords'
    pattern = ''
    toLower = false
    type = 'basic'
    weight = 100
  [[related.indices]]
    applyFilter = false
    cardinalityThreshold = 0
    name = 'date'
    pattern = ''
    toLower = false
    type = 'basic'
    weight = 10
  [[related.indices]]
    applyFilter = false
    cardinalityThreshold = 0
    name = 'tags'
    pattern = ''
    toLower = false
    type = 'basic'
    weight = 80
  [[related.indices]]
    applyFilter = false
    name = 'fragmentrefs'
    type = 'fragments'
    weight = 80

  # -------------------------------------------------------------------------------------
  # Modules Configuration
  # See: https://gohugo.io/hugo-modules/configuration/#module-config-imports
  # -------------------------------------------------------------------------------------

  [module]
    [module.hugoVersion]
      extended = true
      min = "0.146.0"

  # -------------------------------------------------------------------------------------
  # Markup related configuration in Hugo
  # See: https://gohugo.io/getting-started/configuration-markup/
  # -------------------------------------------------------------------------------------

  [markup]
    # Syntax Highlighting (https://gohugo.io/content-management/syntax-highlighting)
    [markup.highlight]
      ########## necessary configurations ##########
      # https://github.com/hugo-fixit/FixIt/issues/43
      codeFences = true
      lineNos = true
      lineNumbersInTable = true
      noClasses = false
      ########## necessary configurations ##########
      guessSyntax = true
    # Goldmark is from Hugo 0.60 the default library used for Markdown
    # https://gohugo.io/getting-started/configuration-markup/#goldmark
    [markup.goldmark]
      duplicateResourceFiles = false
      [markup.goldmark.extensions]
        definitionList = true
        footnote = true
        linkify = true
        linkifyProtocol = 'https'
        strikethrough = false
        table = true
        taskList = true
        typographer = true
        # https://gohugo.io/getting-started/configuration-markup/#extras
        [markup.goldmark.extensions.extras]
          [markup.goldmark.extensions.extras.delete]
            enable = true
          [markup.goldmark.extensions.extras.insert]
            enable = true
          [markup.goldmark.extensions.extras.mark]
            enable = true
          [markup.goldmark.extensions.extras.subscript]
            enable = true
          [markup.goldmark.extensions.extras.superscript]
            enable = true
      # TODO passthrough refactor https://gohugo.io/render-hooks/passthrough/
      # TODO hugo 0.122.0 https://gohugo.io/content-management/mathematics/
      # [markup.goldmark.extensions.passthrough]
      #   enable = true
      #   [markup.goldmark.extensions.passthrough.delimiters]
      #     block = [['\[', '\]'], ['$$', '$$']]
      #     inline = [['\(', '\)'], ['$', '$']]
      [markup.goldmark.parser]
        [markup.goldmark.parser.attribute]
          block = true
          title = true
      [markup.goldmark.renderer]
        hardWraps = false
        # whether to use HTML tags directly in the document
        unsafe = true
        xhtml = false

  # -------------------------------------------------------------------------------------
  # Sitemap Configuration
  # See: https://gohugo.io/templates/sitemap-template/#configuration
  # -------------------------------------------------------------------------------------

  [sitemap]
    changefreq = "weekly"
    disable = false
    filename = "sitemap.xml"
    priority = 0.5

  # -------------------------------------------------------------------------------------
  # Permalinks Configuration
  # See: https://gohugo.io/content-management/urls/#permalinks
  # -------------------------------------------------------------------------------------

  [Permalinks]
    # posts = ":year/:month/:filename"
    posts = "posts/:slugorfilename"

  # -------------------------------------------------------------------------------------
  # Privacy Configuration
  # See: https://gohugo.io/about/hugo-and-gdpr/
  # -------------------------------------------------------------------------------------

  [privacy]
    [privacy.twitter]
      enableDNT = true
    [privacy.youtube]
      privacyEnhanced = true

  # -------------------------------------------------------------------------------------
  # Media Types
  # See: https://gohugo.io/templates/output-formats/#media-types
  # -------------------------------------------------------------------------------------

  [mediaTypes]
    # [mediaTypes."application/feed+json"]
    #   suffixes = ["json"]

  # -------------------------------------------------------------------------------------
  # Output Format Definitions
  # See: https://gohugo.io/templates/output-formats/#output-format-definitions
  # -------------------------------------------------------------------------------------

  [outputFormats]
    # FixIt 0.3.0 | NEW Options to make output /archives/index.html file
    [outputFormats.archives]
      path = "archives"
      baseName = "index"
      mediaType = "text/html"
      isPlainText = false
      isHTML = true
      permalinkable = true
      notAlternative = true
    # FixIt 0.3.0 | NEW Options to make output /offline/index.html file
    [outputFormats.offline]
      path = "offline"
      baseName = "index"
      mediaType = "text/html"
      isPlainText = false
      isHTML = true
      permalinkable = true
      notAlternative = true
    # FixIt 0.3.0 | NEW Options to make output readme.md file
    [outputFormats.readme]
      baseName = "readme"
      mediaType = "text/markdown"
      isPlainText = true
      isHTML = false
      notAlternative = true
    # FixIt 0.3.0 | CHANGED Options to make output baidu_urls.txt file
    [outputFormats.baidu_urls]
      baseName = "baidu_urls"
      mediaType = "text/plain"
      isPlainText = true
      isHTML = false
      notAlternative = true
    # FixIt 0.3.10 | NEW Options to make output search.json file
    [outputFormats.search]
      baseName = "search"
      mediaType = "application/json"
      rel = "search"
      isPlainText = true
      isHTML = false
      permalinkable = true

  # -------------------------------------------------------------------------------------
  # Customizing Output Formats
  # See: https://gohugo.io/templates/output-formats/#customizing-output-formats
  # -------------------------------------------------------------------------------------

  # Options to make hugo output files, the optional values are below:
  # home = ["html", "rss", "archives", "offline", "readme", "baidu_urls", "search"]
  # page = ["html", "markdown"]
  # section = ["html", "rss"]
  # taxonomy = ["html"]
  # term = ["html", "rss"]
  [outputs]
    home = ["html", "rss", "archives", "offline", "search"]
    page = ["html", "markdown"]
    section = ["html", "rss"]
    taxonomy = ["html"]
    term = ["html", "rss"]

  # -------------------------------------------------------------------------------------
  # Taxonomies Configuration
  # See: https://gohugo.io/content-management/taxonomies/#configure-taxonomies
  # -------------------------------------------------------------------------------------

  [taxonomies]
    category = "categories"
    tag = "tags"
    collection = "collections"

  # -------------------------------------------------------------------------------------
  # Theme Core Configuration
  # See: https://fixit.lruihao.cn/documentation/basics/#theme-configuration
  # -------------------------------------------------------------------------------------

  [params]
    # FixIt 0.2.15 | CHANGED FixIt theme version
    version = "0.3.X" # e.g. "0.2.X", "0.2.15", "v0.2.15" etc.
    # site description
    description = ""
    # site keywords
    keywords = []
    # site default theme ["light", "dark", "auto"]
    defaultTheme = "auto"
    # which hash function used for SRI, when empty, no SRI is used
    # ["sha256", "sha384", "sha512", "md5"]
    fingerprint = ""
    # NEW date format
    dateFormat = "2006-01-02"
    # website images for Open Graph and Twitter Cards
    images = []
    # FixIt 0.2.12 | NEW enable PWA
    enablePWA = false
    # FixIt 0.2.14 | NEW whether to add external Icon for external links automatically
    externalIcon = false
    # FixIt 0.3.13 | NEW whether to capitalize titles
    capitalizeTitles = true
    # FixIt 0.3.0 | NEW whether to add site title to the title of every page
    # remember to set up your site title in `hugo.toml` (e.g. title = "title")
    withSiteTitle = true
    # FixIt 0.3.0 | NEW title delimiter when the site title is be added to the title of every page
    titleDelimiter = "-"
    # FixIt 0.3.0 | NEW whether to add site subtitle to the title of index page
    # remember to set up your site subtitle by `params.header.subtitle.name`
    indexWithSubtitle = false
    # FixIt 0.3.13 | NEW whether to show summary in plain text
    summaryPlainify = false
    # FixIt 0.2.14 | NEW FixIt will, by default, inject a theme meta tag in the HTML head on the home page only.
    # You can turn it off, but we would really appreciate if you don’t, as this is a good way to watch FixIt's popularity on the rise.
    disableThemeInject = false

    # Author Configuration
    [params.author]
      name = ""
      email = ""
      link = ""
      avatar = ""

    # FixIt 0.3.0 | NEW public Git repository information only then enableGitInfo is true
    [params.gitInfo]
      # e.g. "https://github.com/hugo-fixit/docs"
      repo = ""
      branch = "main"
      # the content directory path relative to the root of the repository
      dir = "content"
      # the issue template for reporting issue of the posts
      # available template params: {title} {URL} {sourceURL}
      issueTpl = "title=[BUG]%20{title}&body=|Field|Value|%0A|-|-|%0A|Title|{title}|%0A|URL|{URL}|%0A|Filename|{sourceURL}|"

    # App icon config
    [params.app]
      # optional site title override for the app when added to an iOS home screen or Android launcher
      title = "FixIt"
      # whether to omit favicon resource links
      noFavicon = false
      # modern SVG favicon to use in place of older style .png and .ico files
      svgFavicon = ""
      # Safari mask icon color
      iconColor = "#5bbad5"
      # Windows v8-10 tile color
      tileColor = "#da532c"
      # FixIt 0.2.12 | CHANGED Android browser theme color
      [params.app.themeColor]
        light = "#f8f8f8"
        dark = "#252627"

    # Search config
    [params.search]
      enable = false
      # type of search engine ["algolia", "fuse", "cse", "post-chat"]
      type = "fuse"
      # max index length of the chunked content
      contentLength = 4000
      # placeholder of the search bar
      placeholder = ""
      # max number of results length
      maxResultLength = 10
      # snippet length of the result
      snippetLength = 30
      # HTML tag name of the highlight part in results
      highlightTag = "em"
      # whether to use the absolute URL based on the baseURL in search index
      absoluteURL = false
      [params.search.algolia]
        index = ""
        appID = ""
        searchKey = ""
      [params.search.fuse]
        # FixIt 0.2.17 | NEW https://fusejs.io/api/options.html
        isCaseSensitive = false
        minMatchCharLength = 2
        findAllMatches = false
        location = 0
        threshold = 0.3
        distance = 100
        ignoreLocation = false
        useExtendedSearch = false
        ignoreFieldNorm = false

    # FixIt 0.3.16 | NEW Custom Search Engine (CSE)
    [params.cse]
      # Search Engine: ["google", "bing"]
      engine = ""
      # Search results page URL (layout: search)
      resultsPage = "/search/"
      # Google: https://programmablesearchengine.google.com/
      # Google Custom Search Engine Context
      [params.cse.google]
        cx = ""
      # Bing (Unsupported): https://www.customsearch.ai/
      [params.cse.bing]

    # Header config
    [params.header]
      # FixIt 0.2.13 | CHANGED desktop header mode ["sticky", "normal", "auto"]
      desktopMode = "sticky"
      # FixIt 0.2.13 | CHANGED mobile header mode ["sticky", "normal", "auto"]
      mobileMode = "auto"
      # Header title config
      [params.header.title]
        # URL of the LOGO
        logo = ""
        # title name
        name = ""
        # you can add extra information before the name (HTML format is supported), such as icons
        pre = ""
        # you can add extra information after the name (HTML format is supported), such as icons
        post = ""
        # whether to use typeit animation for title name
        typeit = false
      # FixIt 0.2.12 | NEW Header subtitle config
      [params.header.subtitle]
        # subtitle name
        name = ""
        # whether to use typeit animation for subtitle name
        typeit = false

    # FixIt 0.2.18 | NEW Breadcrumb config
    [params.breadcrumb]
      enable = false
      sticky = false
      showHome = false
      # FixIt 0.3.13 | NEW
      separator = "/"
      capitalize = true

    # FixIt 0.3.10 | NEW Post navigation config
    [params.navigation]
      # whether to show the post navigation in section pages scope
      inSection = false
      # whether to reverse the next/previous post navigation order
      reverse = false

    # Footer config
    [params.footer]
      enable = true
      # whether to show copyright info
      copyright = true
      # whether to show the author
      author = true
      # Site creation year
      since = ""
      # FixIt 0.2.12 | NEW Public network security only in China (HTML format is supported)
      gov = ""
      # ICP info only in China (HTML format is supported)
      icp = ""
      # license info (HTML format is supported)
      license = ""
      # FixIt 0.3.0 | NEW whether to show Hugo and theme info
      [params.footer.powered]
        enable = true
        hugoLogo = true
        themeLogo = true
      # FixIt 0.2.17 | CHANGED Site creation time
      [params.footer.siteTime]
        enable = false
        animate = true
        icon = "fa-solid fa-heartbeat"
        pre = ""
        value = "" # e.g. "2021-12-18T16:15:22+08:00"
      # FixIt 0.2.17 | NEW footer lines order, optional values: ["first", 0, 1, 2, 3, 4, 5, "last"]
      [params.footer.order]
        powered = 0
        copyright = 0
        statistics = 0
        visitor = 0
        beian = 0

    # FixIt 0.3.0 | NEW Archives page config (all pages of posts type)
    [params.archives]
      # special amount of posts in archives page
      paginate = 20
      # date format (month and day)
      dateFormat = "01-02"

    # Section page config (all pages in section)
    [params.section]
      # special amount of pages in each section page
      paginate = 20
      # date format (month and day)
      dateFormat = "01-02"
      # FixIt 0.3.10 | NEW Section feed config for RSS, Atom and JSON feed.
      [params.section.feed]
        # The number of posts to include in the feed. If set to -1, all posts.
        limit = -1
        # whether to show the full text content in feed.
        fullText = false

    # Term list (category or tag) page config
    [params.list]
      # special amount of posts in each list page
      paginate = 20
      # date format (month and day)
      dateFormat = "01-02"
      # FixIt 0.3.10 | NEW Term list feed config for RSS, Atom and JSON feed.
      [params.list.feed]
        # The number of posts to include in the feed. If set to -1, all posts.
        limit = -1
        # whether to show the full text content in feed.
        fullText = false

    # FixIt 0.3.13 | NEW recently updated pages config for archives, section and term list
    [params.recentlyUpdated]
      archives = true
      section = true
      list = true
      days = 30
      maxCount = 10

    # FixIt 0.2.17 | NEW TagCloud config for tags page
    [params.tagcloud]
      enable = false
      min = 14 # Minimum font size in px
      max = 32 # Maximum font size in px
      peakCount = 10 # Maximum count of posts per tag
      orderby = "name" # Order of tags, optional values: ["name", "count"]

    # Home page config
    [params.home]
      # Home page profile
      [params.home.profile]
        enable = false
        # Gravatar Email for preferred avatar in home page
        gravatarEmail = ""
        # URL of avatar shown in home page
        avatarURL = ""
        # FixIt 0.2.17 | NEW identifier of avatar menu link
        avatarMenu = ""
        # title shown in home page (HTML format is supported)
        title = ""
        # subtitle shown in home page (HTML format is supported)
        subtitle = ""
        # whether to use typeit animation for subtitle
        typeit = true
        # whether to show social links
        social = true
        # disclaimer (HTML format is supported)
        disclaimer = ""
      # Home page posts
      [params.home.posts]
        enable = true
        # special amount of posts in each home posts page
        paginate = 6

    # FixIt 0.2.16 | CHANGED Social config about the author
    [params.social]
      GitHub = ""
      Linkedin = ""
      Twitter = ""
      Instagram = ""
      Facebook = ""
      Telegram = ""
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
      ORCID = ""
      Liberapay = ""
      Ko-Fi = ""
      BuyMeaCoffee = ""
      Linktree = ""
      QQ = ""
      QQGroup = "" # https://qun.qq.com/join.html
      Diaspora = ""
      CSDN = ""
      Discord = ""
      DiscordInvite = ""
      Lichess = ""
      Pleroma = ""
      Kaggle = ""
      MediaWiki= ""
      Plume = ""
      HackTheBox = ""
      RootMe = ""
      Feishu = ""
      TryHackMe = ""
      Douyin = ""
      TikTok = ""
      Credly = ""
      Bluesky = ""
      Phone = ""
      Email = ""
      RSS = true
      # custom social links like the following
      # [params.social.twitter]
      #   id = "lruihao"
      #   weight = 3
      #   prefix = "https://twitter.com/"
      #   Title = "Twitter"
      #   [social.twitter.icon]
      #     class = "fa-brands fa-x-twitter fa-fw"

    # TypeIt config
    [params.typeit]
      # typing speed between each step (measured in milliseconds)
      speed = 100
      # blinking speed of the cursor (measured in milliseconds)
      cursorSpeed = 1000
      # character used for the cursor (HTML format is supported)
      cursorChar = "|"
      # cursor duration after typing finishing (measured in milliseconds, "-1" means unlimited)
      duration = -1
      # FixIt 0.2.18 | NEW whether your strings will continuously loop after completing
      loop = false

    # FixIt 0.2.15 | NEW Mermaid config
    [params.mermaid]
      # For values, see https://mermaid.js.org/config/theming.html#available-themes
      themes = ["default", "dark"]

    # FixIt 0.3.13 | NEW Admonitions custom config
    # See https://fixit.lruihao.cn/documentation/content-management/shortcodes/extended/admonition/#custom-admonitions
    # syntax: <type> = <icon>
    [params.admonition]
      # ban = "fa-solid fa-ban"
    
    # FixIt 0.3.14 | NEW Task lists custom config
    # See https://fixit.lruihao.cn/documentation/content-management/advanced/#custom-task-lists
    # syntax: <sign> = <icon>
    [params.taskList]
      # tip = "fa-regular fa-lightbulb"

    # FixIt 0.3.15 | NEW version shortcode config
    [params.repoVersion]
      # url prefix for the release tag
      url = "https://github.com/hugo-fixit/FixIt/releases/tag/v"
      # project name
      name = "FixIt"

    # FixIt 0.2.12 | NEW PanguJS config
    [params.pangu]
      # For Chinese writing
      enable = false
      selector = "article" # FixIt 0.2.17 | NEW

    # FixIt 0.2.12 | NEW Watermark config
    # Detail config see https://github.com/Lruihao/watermark#readme
    [params.watermark]
      enable = false
      # watermark's text (HTML format is supported)
      content = ""
      # watermark's transparency
      opacity = 0.1
      # watermark's width. unit: px
      width = 150
      # watermark's height. unit: px
      height = 20
      # row spacing of watermarks. unit: px
      rowSpacing = 60
      # col spacing of watermarks. unit: px
      colSpacing = 30
      # watermark's tangent angle. unit: deg
      rotate = 15
      # watermark's fontSize. unit: rem
      fontSize = 0.85
      # FixIt 0.2.13 | NEW watermark's fontFamily
      fontFamily = "inherit"

    # FixIt 0.3.10 | NEW Busuanzi count
    [params.busuanzi]
      # whether to enable busuanzi count
      enable = false
      # busuanzi count core script source. Default is https://vercount.one/js
      source = "https://vercount.one/js"
      # whether to show the site views
      siteViews = true
      # whether to show the page views
      pageViews = true

    # Site verification code config for Google/Bing/Yandex/Pinterest/Baidu/360/Sogou
    [params.verification]
      google = ""
      bing = ""
      yandex = ""
      pinterest = ""
      baidu = ""
      so = ""
      sogou = ""

    # Site SEO config
    [params.seo]
      # image URL
      image = ""
      # thumbnail URL
      thumbnailUrl = ""

    # Analytics config
    [params.analytics]
      enable = false
      # Google Analytics
      [params.analytics.google]
        id = ""
        # whether to anonymize IP
        anonymizeIP = true
      # Fathom Analytics
      [params.analytics.fathom]
        id = ""
        # server url for your tracker if you're self hosting
        server = ""
      # FixIt 0.3.16 | NEW Baidu Analytics
      [params.analytics.baidu]
        id = ""
      # FixIt 0.3.16 | NEW Umami Analytics
      [params.analytics.umami]
        data_website_id = ""
        src = ""
        data_host_url = ""
        data_domains = ""
      # FixIt 0.3.16 | NEW Plausible Analytics
      [params.analytics.plausible]
        data_domain = ""
        src = ""
      # FixIt 0.3.16 | NEW Cloudflare Analytics
      [params.analytics.cloudflare]
        token = ""
      # FixIt 0.3.16 | NEW Splitbee Analytics
      [params.analytics.splitbee]
        enable = false
        # no cookie mode
        no_cookie = true
        # respect the do not track setting of the browser
        do_not_track = true
        # token(optional), more info on https://splitbee.io/docs/embed-the-script
        data_token = ""

    # Cookie consent config
    [params.cookieconsent]
      enable = true
      # text strings used for Cookie consent banner
      [params.cookieconsent.content]
        message = ""
        dismiss = ""
        link = ""

    # CDN config for third-party library files
    [params.cdn]
      # CDN data file name, disabled by default ["jsdelivr.yml", "unpkg.yml", ...]
      # located in "themes/FixIt/assets/data/cdn/" directory
      # you can store your own data files in the same path under your project: "assets/data/cdn/"
      data = ""

    # Compatibility config
    [params.compatibility]
      # whether to use Polyfill.io on cdnjs to be compatible with older browsers
      # https://cdnjs.cloudflare.com/polyfill
      polyfill = false
      # whether to use object-fit-images to be compatible with older browsers
      objectFit = false

    # FixIt 0.2.14 | NEW GitHub banner in the top-right or top-left corner
    [params.githubCorner]
      enable = false
      permalink = "https://github.com/hugo-fixit/FixIt"
      title = "View source on GitHub"
      position = "right" # ["left", "right"]

    # FixIt 0.2.14 | NEW Gravatar config
    [params.gravatar]
      # FixIt 0.2.18 | NEW Depends on the author's email, if the author's email is not set, the local avatar will be used
      enable = false
      # Gravatar host, default: "www.gravatar.com"
      host = "www.gravatar.com" # ["cravatar.cn", "gravatar.loli.net", ...]
      style = "" # ["", "mp", "identicon", "monsterid", "wavatar", "retro", "blank", "robohash"]

    # FixIt 0.2.16 | NEW Back to top
    [params.backToTop]
      enable = true
      # Scroll percent label in b2t button
      scrollpercent = false

    # FixIt 0.2.16 | NEW Reading progress bar
    [params.readingProgress]
      enable = false
      # Available values: ["left", "right"]
      start = "left"
      # Available values: ["top", "bottom"]
      position = "top"
      reversed = false
      light = ""
      dark = ""
      height = "2px"
    
    # FixIt 0.2.17 | NEW Progress bar in the top during page loading.
    # For more information: https://github.com/CodeByZach/pace
    [params.pace]
      enable = false
      # All available colors:
      # ["black", "blue", "green", "orange", "pink", "purple", "red", "silver", "white", "yellow"]
      color = "blue"
      # All available themes:
      # ["barber-shop", "big-counter", "bounce", "center-atom", "center-circle", "center-radar", "center-simple",
      # "corner-indicator", "fill-left", "flash", "flat-top", "loading-bar", "mac-osx", "material", "minimal"]
      theme = "minimal"
    
    # FixIt 0.3.17 | NEW PostChat AI config
    # Based on your posts to build a knowledge base, support AI summary, AI search, and AI Chatbot.
    # Get PostChat Key from my invitation link, thanks for your support!
    # https://ai.tianli0.top/?InviteID=IRE1S88Z
    [params.postChat]
      enable = false
      key = ""
      # How users initiate chats: ["iframe", "magic"]
      userMode = "iframe"
      addButton = true
      defaultInput = false
      left = ""
      bottom = ""
      width = ""
      height = ""
      fill = ""
      backgroundColor = ""
      upLoadWeb = true
      showInviteLink = true
      userTitle = ""
      userDesc = ""
      # dom container to be blacked out, e.g. [".aplayer"]
      blackDom = []
      # Only for iframe mode
      frameWidth = ""     # e.g. "375px"
      frameHeight = ""    # e.g. "600px"
      # only for magic mode
      userIcon = ""
      defaultChatQuestions = []
      defaultSearchQuestions = []

    # FixIt 0.3.17 | NEW Summary AI config
    # See https://postchat.zhheo.com/summary.html
    [params.postSummary]
      enable = false
      # If you set `params.postChat.key`, you don't need to set `params.postSummary.key`
      key = ""
      title = ""
      # themes options: ["", "simple", "yanzhi"]
      theme = ""
      postURL = ""
      blacklist = ""
      wordLimit = 1000
      typingAnimate = true
      beginningText = ""
      loadingText = true

    # FixIt 0.3.10 | NEW Global Feed config for RSS, Atom and JSON feed.
    [params.feed]
      # The number of posts to include in the feed. If set to -1, all posts.
      limit = 10
      # whether to show the full text content in feed.
      fullText = true
      # Site Challenge for Follow: https://follow.is/
      [params.feed.follow]
        feedId = ""
        userId = ""
    
    # FixIt 0.3.17 | NEW Image config
    [params.image]
      # cache remote images for better optimisations
      cacheRemote = false
      # Image resizing and optimisation
      optimise = false

    # FixIt 0.3.12 | NEW Custom partials config
    # Custom partials must be stored in the /layouts/partials/ directory.
    # Depends on open custom blocks https://fixit.lruihao.cn/references/blocks/
    [params.customPartials]
      head = []
      menuDesktop = []
      menuMobile = []
      profile = []
      aside = []
      comment = []
      footer = []
      widgets = []
      assets = []
      postTocBefore = []
      postTocAfter = []
      postContentBefore = []
      postContentAfter = []
      postFooterBefore = []
      postFooterAfter = []

    # FixIt 0.2.15 | NEW Developer options
    # Select the scope named `public_repo` to generate personal access token,
    # Configure with environment variable `HUGO_PARAMS_GHTOKEN=xxx`, see https://gohugo.io/functions/os/getenv/#examples
    [params.dev]
      enable = false
      # Check for updates
      c4u = false

    # Page config
    [params.page]
      # FixIt 0.2.18 | NEW whether to enable the author's avatar of the post
      authorAvatar = true
      # whether to hide a page from home page
      hiddenFromHomePage = false
      # whether to hide a page from search results
      hiddenFromSearch = false
      # FixIt 0.3.0 | NEW whether to hide a page from related posts
      hiddenFromRelated = false
      # FixIt 0.3.10 | NEW whether to hide a page from RSS, Atom and JSON feed
      hiddenFromFeed = false
      # whether to enable twemoji
      twemoji = false
      # FixIt 0.2.18 | CHANGED whether to enable lightgallery
      # set to true, images in the content will be shown as the gallery if the image has a title, e.g. ![alt](src "title")
      # set to "force", images in the content will be forced to shown as the gallery regardless of the image has a title or not, e.g. ![alt](src)
      lightgallery = false
      # whether to enable the ruby extended syntax
      ruby = true
      # whether to enable the fraction extended syntax
      fraction = true
      # whether to enable the fontawesome extended syntax
      fontawesome = true
      # license info (HTML format is supported)
      license = '<a rel="license external nofollow noopener noreferrer" href="https://creativecommons.org/licenses/by-nc-sa/4.0/" target="_blank">CC BY-NC-SA 4.0</a>'
      # whether to show link to Raw Markdown content of the post
      linkToMarkdown = true
      # FixIt 0.3.0 | NEW whether to show link to view source code of the post
      linkToSource = true
      # FixIt 0.3.0 | NEW whether to show link to edit the post
      linkToEdit = true
      # FixIt 0.3.0 | NEW whether to show link to report issue for the post
      linkToReport = true
      # FixIt 0.3.20 | NEW whether to show link to view the post in VSCode
      linkToVscode = true
      # FixIt 0.3.10 | CHANGED Page style ["narrow", "normal", "wide", ...]
      pageStyle = "normal"
      # FixIt 0.2.17 | CHANGED Auto Bookmark Support
      # If true, save the reading progress when closing the page.
      autoBookmark = false
      # FixIt 0.2.17 | NEW whether to enable wordCount
      wordCount = true
      # FixIt 0.2.17 | NEW whether to enable readingTime
      readingTime = true
      # FixIt 0.2.17 | NEW end of post flag
      endFlag = ""
      # FixIt 0.2.18 | NEW whether to enable instant.page
      instantPage = false
      # FixIt 0.3.0 | NEW whether to enable collection list at the sidebar
      collectionList = false
      # FixIt 0.3.0 | NEW whether to enable collection navigation at the end of the post
      collectionNavigation = false

      # FixIt 0.2.15 | NEW Repost config
      [params.page.repost]
        enable = false
        url = ""
      # Table of the contents config
      [params.page.toc]
        # whether to enable the table of the contents
        enable = true
        # whether to keep the static table of the contents in front of the post
        keepStatic = false
        # whether to make the table of the contents in the sidebar automatically collapsed
        auto = true
        # FixIt 0.2.13 | NEW position of TOC ["left", "right"]
        position = "right"
        # FixIt 0.3.12 | NEW supersede `markup.tableOfContents` settings
        ordered = false
        startLevel = 2
        endLevel = 6
      # FixIt 0.2.13 | NEW Display a message at the beginning of an article to warn the reader that its content might be expired
      [params.page.expirationReminder]
        enable = false
        # Display the reminder if the last modified time is more than 90 days ago
        reminder = 90
        # Display warning if the last modified time is more than 180 days ago
        warning = 180
        # If the article expires, close the comment or not
        closeComment = false
      # FixIt 0.3.0 | NEW page heading config
      [params.page.heading]
        # FixIt 0.3.3 | NEW whether to capitalize automatic text of headings
        capitalize = false
        # FixIt 0.3.12 | CHANGED must set `params.page.toc.ordered` to true
        [params.page.heading.number]
          # whether to enable auto heading numbering
          enable = false
          # FixIt 0.3.3 | NEW only enable in main section pages (default is posts)
          onlyMainSection = true
          [params.page.heading.number.format]
            h1 = "{title}"
            h2 = "{h2} {title}"
            h3 = "{h2}.{h3} {title}"
            h4 = "{h2}.{h3}.{h4} {title}"
            h5 = "{h2}.{h3}.{h4}.{h5} {title}"
            h6 = "{h2}.{h3}.{h4}.{h5}.{h6} {title}"
      # FixIt 0.2.16 | CHANGED KaTeX mathematical formulas (https://katex.org)
      [params.page.math]
        enable = true
        # default inline delimiter is $ ... $ and \( ... \)
        inlineLeftDelimiter = ""
        inlineRightDelimiter = ""
        # default block delimiter is $$ ... $$, \[ ... \], \begin{equation} ... \end{equation} and some other functions
        blockLeftDelimiter = ""
        blockRightDelimiter = ""
        # KaTeX extension copy_tex
        copyTex = true
        # KaTeX extension mhchem
        mhchem = true
      # Code wrapper config
      [params.page.code]
        # FixIt 0.3.9 | NEW whether to enable the code wrapper
        enable = true
        # whether to show the copy button of the code wrapper
        copy = true
        # FixIt 0.2.13 | NEW whether to show the edit button of the code wrapper
        edit = true
        # the maximum number of lines of displayed code by default
        maxShownLines = 10
      # Mapbox GL JS config (https://docs.mapbox.com/mapbox-gl-js)
      [params.page.mapbox]
        # access token of Mapbox GL JS
        accessToken = ""
        # style for the light theme
        lightStyle = "mapbox://styles/mapbox/light-v11"
        # style for the dark theme
        darkStyle = "mapbox://styles/mapbox/dark-v11"
        # whether to add NavigationControl
        navigation = true
        # whether to add GeolocateControl
        geolocate = true
        # whether to add ScaleControl
        scale = true
        # whether to add FullscreenControl
        fullscreen = true
      # FixIt 0.3.0 | NEW Related content config (https://gohugo.io/content-management/related/)
      [params.page.related]
        enable = false
        count = 5
        # FixIt 0.3.20 | NEW position of related content, optional values: ["aside", "footer"]
        position = ["aside", "footer"]
      # FixIt 0.2.17 | NEW Donate (Sponsor) settings
      [params.page.reward]
        enable = false
        animation = false
        # position relative to post footer, optional values: ["before", "after"]
        position = "after"
        # comment = "Buy me a coffee"
        # FixIt 0.2.18 | NEW display mode of QR code images, optional values: ["static", "fixed"], default: `static`
        mode = "static"
        [params.page.reward.ways]
          # wechatpay = "/images/wechatpay.png"
          # alipay = "/images/alipay.png"
          # paypal = "/images/paypal.png"
          # bitcoin = "/images/bitcoin.png"
      # social share links in post page
      [params.page.share]
        enable = true
        Twitter = true
        Facebook = true
        Linkedin = false
        Whatsapp = false
        Pinterest = false
        Tumblr = false
        HackerNews = false
        Reddit = false
        VK = false
        Buffer = false
        Xing = false
        Line = false
        Instapaper = false
        Pocket = false
        Flipboard = false
        Weibo = true
        Myspace = false
        Blogger = false
        Baidu = false
        Odnoklassniki = false
        Evernote = false
        Skype = false
        Trello = false
        Mix = false
      # FixIt 0.2.15 | CHANGED Comment config
      [params.page.comment]
        enable = false
        # FixIt 0.2.13 | NEW Artalk comment config (https://artalk.js.org/)
        [params.page.comment.artalk]
          enable = false
          server = "https://yourdomain"
          site = "默认站点"
          # FixIt 0.3.3 | NEW whether use backend configuration
          useBackendConf = false
          placeholder = ""
          noComment = ""
          sendBtn = ""
          editorTravel = true
          flatMode = "auto"
          # FixIt 0.2.17 | CHANGED enable lightgallery support
          lightgallery = false
          locale = "" # FixIt 0.2.15 | NEW
          # FixIt 0.2.18 | NEW
          emoticons = ""
          nestMax = 2
          nestSort = "DATE_ASC" # ["DATE_ASC", "DATE_DESC", "VOTE_UP_DESC"]
          vote = true
          voteDown = false
          uaBadge = true
          listSort = true
          imgUpload = true
          preview = true
          versionCheck = true
        # Disqus comment config (https://disqus.com)
        [params.page.comment.disqus]
          enable = false
          # Disqus shortname to use Disqus in posts
          shortname = ""
        # Gitalk comment config (https://github.com/gitalk/gitalk)
        [params.page.comment.gitalk]
          enable = false
          owner = ""
          repo = ""
          clientId = ""
          clientSecret = ""
        # Valine comment config (https://github.com/xCss/Valine)
        [params.page.comment.valine]
          enable = false
          appId = ""
          appKey = ""
          placeholder = ""
          avatar = "mp"
          meta = ['nick','mail','link']
          requiredFields = []
          pageSize = 10
          lang = ""
          visitor = true
          recordIP = true
          highlight = true
          enableQQ = false
          serverURLs = ""
          # emoji data file name, default is "google.yml"
          # ["apple.yml", "google.yml", "facebook.yml", "twitter.yml"]
          # located in "themes/FixIt/assets/lib/valine/emoji/" directory
          # you can store your own data files in the same path under your project:
          # "assets/lib/valine/emoji/"
          emoji = ""
          commentCount = true # FixIt 0.2.13 | NEW
        # FixIt 0.2.16 | CHANGED Waline comment config (https://waline.js.org)
        [params.page.comment.waline]
          enable = false
          serverURL = ""
          pageview = false # FixIt 0.2.15 | NEW
          emoji = ["//unpkg.com/@waline/emojis@1.1.0/weibo"]
          meta = ["nick", "mail", "link"]
          requiredMeta = []
          login = "enable"
          wordLimit = 0
          pageSize = 10
          imageUploader = false # FixIt 0.2.15 | NEW
          highlighter = false # FixIt 0.2.15 | NEW
          comment = false # FixIt 0.2.15 | NEW
          texRenderer = false # FixIt 0.2.16 | NEW
          search = false # FixIt 0.2.16 | NEW
          recaptchaV3Key = "" # FixIt 0.2.16 | NEW
          turnstileKey = "" # FixIt 0.3.8 | NEW
          reaction = false # FixIt 0.2.18 | NEW
        # Facebook comment config (https://developers.facebook.com/docs/plugins/comments)
        [params.page.comment.facebook]
          enable = false
          width = "100%"
          numPosts = 10
          appId = ""
          languageCode = ""
        # Telegram comments config (https://comments.app)
        [params.page.comment.telegram]
          enable = false
          siteID = ""
          limit = 5
          height = ""
          color = ""
          colorful = true
          dislikes = false
          outlined = false
        # Commento comment config (https://commento.io)
        [params.page.comment.commento]
          enable = false
        # Utterances comment config (https://utteranc.es)
        [params.page.comment.utterances]
          enable = false
          # owner/repo
          repo = ""
          issueTerm = "pathname"
          label = ""
          lightTheme = "github-light"
          darkTheme = "github-dark"
        # FixIt 0.2.13 | NEW Twikoo comment config (https://twikoo.js.org/)
        [params.page.comment.twikoo]
          enable = false
          envId = ""
          region = ""
          path = ""
          visitor = true
          commentCount = true
          # FixIt 0.2.17 | CHANGED enable lightgallery support
          lightgallery = false
          # FixIt 0.2.17 | NEW enable Katex support
          katex = false
        # FixIt 0.2.14 | NEW Giscus comments config
        [params.page.comment.giscus]
          enable = false
          repo = ""
          repoId = ""
          category = ""
          categoryId = ""
          mapping = ""
          origin = "https://giscus.app" # FixIt NEW | 0.3.7 Or set it to your self-hosted domain
          strict = "0" # FixIt NEW | 0.2.18
          term = ""
          reactionsEnabled = "1"
          emitMetadata = "0"
          inputPosition = "bottom" # ["top", "bottom"]
          lang = ""
          lightTheme = "light"
          darkTheme = "dark"
          lazyLoad = true
      # Third-party library config
      [params.page.library]
        [params.page.library.css]
          # someCSS = "some.css"
          # located in "assets/"
          # Or
          # someCSS = "https://cdn.example.com/some.css"
        [params.page.library.js]
          # someJavascript = "some.js"
          # located in "assets/"
          # Or
          # someJavascript = "https://cdn.example.com/some.js"
      # Page SEO config
      [params.page.seo]
        # image URL
        images = []
        # Publisher info
        [params.page.seo.publisher]
          name = ""
          logoUrl = ""
  ```
2. 别慌，只是看起来很复杂，需要修改的内容其实并不多，主要还是看自己的需求，完成一个网站基本的搭建只需要修改以下几个地方，其他需要自定义的可以上[FixIt官方文档-配置篇](https://fixit.lruihao.cn/zh-cn/documentation/getting-started/configuration/)慢慢研究。
   - `website title`指的是网站的名字，会在左上角显示，例如想要显示`张三的博客`，则按如下修改：
      ```
      # website title
      title = "张三的博客"
      ```
   - `Hostname (and path) to the root`指的是你网站的域名，上线之前可以先默认`http://localhost:1313`,上线后修改为自己的域名。
   - `theme list`要注意把`# theme = ["FixIt"] # enable in your site config file`前面的`#`去掉。`#`在toml里指的是`#后面的文字为注释，不运行，直到换行为止`，不去掉的话，就没有定义主题。
   - 下面这几项是语言设置，按照我的填就行
      ```
      # determines default content language ["en", "zh-cn", "fr", "pl", ...]
      defaultContentLanguage = "zh-cn"
      # language code ["en", "zh-CN", "fr", "pl", ...]
      languageCode = "zh-CN"
      # language name ["English", "简体中文", "Français", "Polski", ...]
      languageName = "简体中文"
      # whether to include Chinese/Japanese/Korean
      hasCJKLanguage = true
      ```
   - `mainSections`填写`'posts'`,这一步很重要，具体原因见[解决 HUGO 文档（Archives）中不显示文章的问题](https://www.buray.top/posts/fixed-up-hugo-no-archives/)。
3. 写一篇文章
   - 在`D:\hugo\myblog\content\posts`文件夹内新建个文本文档，修改文件名为`firstpost.md`（也可以随便叫别的什么名字，只要是英文名就行，后缀一定要`.md`）
   - 用VS code打开这个文件，把下面的内容粘贴进去：
      ```
      ---
      title: 
      date: 
      slug: 
      featuredImagePreview: 
      categories:
          - 分类1
          - 分类2
      tags:
          - 标签1
          - 标签2
          - 标签3
      ---

      这是我的第一篇文章。。。
      ```
   - 两个`---`中间的是头文件，以后每一篇文章都要有这个东西;
   - `title`是文章标题，可以用中文了;
   - `date`是文章发布日期，格式为`yyyy-mm-dd`;
   - `slug`是自定义短链接，例如写`fist-post`,那么这篇文章的链接就是`https://你的域名/posts/first-post/`,对SEO有点用;
   - `featuredImagePreview`是封面图片，在网站首页的文章列表中显示的封面图，但不在文章内显示，相对路径或者网络图片链接都可以，不过相对路径挺麻烦的，特别是使用Github时，我用CF做图床，使用的图片链接，用CF做图床可以参考[使用 Cloudflare R2 搭建免费图床](https://www.buray.top/posts/cloudflare-r2/);
   - `categories`是文章分类，根据自己的实际情况填就行，可以填1个，也可以填多个；
   - `tags`是文章标签，也算是分类方式的一种吧，也是根据自己的实际情况填就行，可以填1个，也可以填多个。
   - 下面写文章内容，可能需要学一点markdown语法，不过很简单，可以参考[Markdown 语法学习](https://www.buray.top/posts/markdown-notes/)这篇文章。
   - 写完后保存即可。
4. 新增别的文章跟上面的操作一样，新建个`.md`文件就行。
5. 回到网站根目录`D:\hugo\myblog\`，在地址栏输入`cmd`回车，进入命令提示行。输入`hugo server`回车。看到`Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)`就算是本地搭建成功了
![](https://burayimg.heisenw.com/blog/img/6ed4d383-3585-41f2-94c0-58f793751dfe.webp)
6. 在浏览器打开`http://localhost:1313/`链接，就能看到网站的样子了。
## Github配置【待更新】
## Cloudflare配置【待更新】



---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/hugo/  

