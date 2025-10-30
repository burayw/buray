# 解决HUGO文档（Archives）中不显示文章的问题


搭建这个博客用的[HUGO](https://gohugo.io/)，[FixIt](https://github.com/hugo-fixit/FixIt)主题，按照文档配置好之后，发现主页没有文章列表，点进Archives里面也没有，自己没什么基础，百思不得其解，网上也没搜到解决方案。  

![](https://burayimg.heisenw.com/blog/img/e66a61c3-ecad-4a51-923d-0d52e048cc88.webp)

![](https://burayimg.heisenw.com/blog/img/5216368c-8a1b-4757-a0bb-f9e362afbdff.webp)

想到个笨办法，找了另一个主题的hugo.toml文件一项一项对比，发现不同的地方就改改看看，还真找到了问题所在：  

FixIt主题下载之后无论是主题默认的还是Demo里面的hugo.toml都没有配置`mainSections`，文档里面也没有具体说明白这个东西怎么用，对于没有基础的菜鸟来说不太友好。  

HUGO官方文档的描述： 

> （string或[ ]string）网站的主要版块。如果已设置，则对象MainSections上的方法Site返回指定的版块；否则，返回页面最多的版块。  

由于是一个新博客，`content`下面就只有一个`posts`,所以`mainSections`留空导致Archives没有办法链接到对应“页面最多的版块”的目录，我猜是这样的。  

所以解决方案：  

```
# the main sections of a site
mainSections = ['posts']
```

恢复正常了，要注意toml的语法，HUGO官方文档有描述

![](https://burayimg.heisenw.com/blog/img/fb2c6463-f0b0-43a1-866c-906c20e85a70.webp)


---

> 作者: [Buray](http://www.burayw.top)  
> URL: http://localhost:1313/posts/fixed-up-hugo-no-archives/  

