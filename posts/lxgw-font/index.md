# 关于本站字体-霞鹜文楷

![](https://burayimg.heisenw.com/blog/img/6bd9a05a-0231-4bbe-a963-6885b377c293.webp)
其实我平时不怎么在意网页字体什么的，直到无意间看到了霞鹜文楷，真漂亮啊，而且免费开源可商用，于是就用上了。  

## 字体来源  

[霞鹜文楷Github](https://github.com/lxgw/LxgwWenKai)  
[霞鹜文楷屏幕阅读版](https://github.com/CMBill/lxgw-wenkai-screen-web)  

屏幕阅读版比原版更适用屏幕显示，区别其实不是很大，但是看着更顺眼一些。  

## 安装方法  

- 通用安装
其实说明文档中写得很清楚了，直接将文后提供的链接以`<link>`的形式添加到网页的`<head>`内即可
    ```
    <html>
    <head>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-web/style.css" />
    <style>
        body {
        font-family: "LXGW WenKai Screen";
        font-weight: normal;
        }
    </style>
    </head>
    <body>
    
    </body>
    </html>
    ```
- FixIt主题安装霞鹜文楷字体  
  在[FixIt主题的说明文档](https://fixit.lruihao.cn/zh-cn/documentation/advanced/#font-style)中其实也描述了，直接加一个全局覆盖css就可以。  

  以下字体样式均在 assets/css/_override.scss 中定义。 
  自定义全局字体，霞鹜文楷屏幕阅读版:  
    ```
    @import url('https://cdn.jsdelivr.net/npm/lxgw-wenkai-screen-web/style.css');
    $global-font-family: 'LXGW WenKai Screen', $global-font-family;
    ```
    自定义代码字体，以开源字体[Fira Mono](https://github.com/mozilla/Fira)为例：  

    ```
    @import url('https://fonts.googleapis.com/css?family=Fira+Mono:400,700&display=swap&subset=latin-ext');
    $code-font-family: Fira Mono, $code-font-family;
    ```
- Stack主题安装霞鹜文楷字体 




---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/lxgw-font/  

