# 禁止搜索引擎收录


![](https://burayimg.heisenw.com/blog/img/b1fb23cf-7119-4f6a-afa1-19881abdf2c0.webp)

- 因为做单位内部使用的网站，有部分内容不想公开，所以希望屏蔽所有搜索引擎，整理了网上的几种方法，参考[通过nginx禁止搜索引擎抓取和收录](https://www.rmnof.com/article/block-search-indexing/)这篇文章实现。
  
  ### 通过<meta>标签实现禁止搜索引擎索引：
- 一般适用于某个网页的禁止，在`<head></head>`标签中添加以下<meta>标签实现。
- 共用`head`文件的网站也可以使用这种方式屏蔽整站。
  
  ```
  <meta name="robots" content="noindex"> //禁止所有搜索引擎索引
  <meta name="googlebot" content="noindex"> //禁止google索引
  <meta name="BaiduSpider" content="noindex"> //禁止百度索引
  ```
  
  ### 通过robots.txt实现禁止搜索引擎索引：
- 在网站根目录下新建`robots.txt`文件，在文件中写入以下代码。
  - 一般搜索引擎在收录网站时会首先爬取根目录下的这个文件，但也有的搜索引擎不遵守这个规则。
    
    ```
    User-agent: *
    Disallow: /
    ```
    
    ### 通过nginx配置文件禁止搜索引擎的UA访问：
- 这位老师基本上整理齐全了所有搜索引擎的UA，可以根据需求删减或增加。return 403当然可以返回404甚至444，也可以使用deny all或者其他更骚的方法。
  
  ```
  if ($http_user_agent ~* "qihoobot|Baiduspider|Googlebot|Googlebot-Mobile|Googlebot-Image|Mediapartners-Google|Adsbot-Google|Feedfetcher-Google|Yahoo! Slurp|Yahoo! Slurp China|YoudaoBot|Sosospider|Sogou spider|Sogou web spider|MSNBot|ia_archiver|Tomato Bot")
    {
        return 403;
    }
  ```
  
  ### 通过Apache的htaccess入口文件中配置指令，同理但不同语法。
- 下面是从网上找的语法，我的是nginx，所以没测试。
  
  ```
  ErrorDocument 503 “System Undergoing Maintenance”
  RewriteEngine On
  RewriteCond %{HTTP_USER_AGENT} (qihoobot|Baiduspider|Googlebot|Googlebot-Mobile|Googlebot-Image|Mediapartners-Google|Adsbot-Google|Feedfetcher-Google|Yahoo! Slurp|Yahoo! Slurp China|YoudaoBot|Sosospider|Sogou spider|Sogou web spider|MSNBot|ia_archiver|Tomato Bot) [NC]
  RewriteRule .* – [R=403,L]
  ```

---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/disallow/  

