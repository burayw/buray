# 使用hugo、Github和Cloudflare免费搭建个人博客


前阵子给好大儿搭建博客的时候学着用hugo，边学边做，磕磕绊绊地好歹运行起来了，当时就想着，等做完了一定要做个笔记。

## 前期准备
### 工具
- Visual Studio Code  
  微软出的代码编辑器，功能非常非常强大，同样也意味着相对臃肿，不过几乎可以做到all in one，几乎覆盖了所有的常见语言。
  [⏬️VS Code官网下载⏬️](https://code.visualstudio.com/)
- 插件
  **Chinese (Simplified) (简体中文) Language Pack**`简体中文语言包，爱国者的福音。`
  **Markdown All in One**`VS Code最受欢迎的markdown插件，集成了写作、预览、目录生成、快捷键、表格对齐、数学公式支持等众多功能。`
  **Markdown Preview Enhanced**`提供Markdown实时预览`
  **Paste Image**`不使用图床的情况下，可以直接使用粘贴功能插入图片，并自动把图片保存到md文件所在目录，我用图床，所以这个没安装`  
上面4个插件都是即装即用，不需要额外设置，非常适合新手。
- VS Code插件安装笔记
  VS Code主界面左侧点击“扩展”图标，然后搜索插件，搜到后点击“安装”就可以了
  ![](https://burayimg.heisenw.com/blog/img/146f1ad1-85f9-4672-9b78-31087fa32b7e.webp)
- 工具网站
  [Emoji表情大全](https://emoji6.com/emojiall/)
  [icon图标下载](https://icon-icons.com/)

### github
打开[Github](https://github.com/)注册个github账号，现在大部分时间可以访问了，确实访问不了的，想办法科学。
### cloudflare
打开[Cloudflare](https://www.cloudflare-cn.com/)注册个账号，这个有中文版的，很简单。
### 域名
想要自己的域名可以注册一个，不注册也行，使用cloudflare的二级域名，也很简短，*.pages.dav的域名也不是很难记。
不过要是用Cloudflare的R2对象存储做图床的话，还是需要个自己的域名的，大部分顶级域名可以在cloudflare上直接注册，也可以在其他域名注册商那边注册后，DNS托管到Cloudflare上面。域名注册商上网搜一大片，例如namesilo、godaddy、阿里云、腾讯云等，比比价格，哪儿合适在哪儿买，要注意续费价格，比如godaddy的首年价格一般很低，但续费挺贵的。
## 本地搭建
### 下载hugo
1. 打开[HUGO官网](https://gohugo.io/news/)下载最新版HUGO，根据自己的电脑系统下载对应的版本，注意使劲往下拉到底，下载`hugo_extended_withdeploy`版本。
2. 在D盘根目录下新建文件夹，命名`hugo`，将下载的hugo压缩包解压，把hugo.exe粘贴到`hugo`文件夹下。
3. 打开`我的电脑`，在空白处右键➡️属性➡️高级系统设置➡️环境变量➡️系统变量，双击打开Path➡️新建➡️输入`D:\hugo`➡️一路`确定`保存。
### 创建网站
1. 进入`D:\hugo`，在地址栏输入`cmd`回车，进入命令提示行。
2. 输入`hugo new site xxx`回车（xxx可以改成你的，例如`myblog`，注意不能用中文）。
3. 可以看到`D:\hugo`文件夹下多了`xxx文件夹`，网站就算创建成功了。
### 安装模板
### 网站配置
## Github配置
## Cloudflare配置



---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/hugo/  

