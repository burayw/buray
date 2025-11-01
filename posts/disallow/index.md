# Gitee推送

![](https://burayimg.heisenw.com/blog/img/b2262806-65aa-4c24-9090-8409eb0f5c15.webp)

- 简化版推送是指每次推送都将本地所有文件完全覆盖远程仓库，团队使用可能会出问题，例如将别人的代码覆盖，个人使用会更加简便。不过由于是全部推送，体积很大的项目建议还是不要用了。
- 原文出处:[15分钟快速生成免费超高逼格官方文档](https://www.bilibili.com/video/BV14U4y1x7jH?spm_id_from=333.337.search-card.all.click)
## 正常Gitee推送
- 全局User配置(只用操作一次)
    ```
    git config --global user.name "your_Name"
    git config --global user.email "your_email"
    ```
- 初始化本地Git仓库
    ```
    git init
    ```

- 绑定远程仓库
    ```
    git remote add origin 远程仓库地址
    ```

- 将所有文件添加到暂存区
    ```
    git add *
    ```
- 将暂存区的文件添加到本地仓库
    ```
    git commit -m "注释"
    ```
- 将远程仓库和本地仓库合并(如果合并失败整理好再push 或者 强制push)
    ```
    git pull origin master --allow-unrelated-histories
    ```
- 将本地仓库推送到远程仓库(-u 跟踪分支 -f 强制推送)
    ```
    git push -u origin master 
    git push -f origin master
    ```
## 简化版Gitee推送
- 全局User配置(只用操作一次)
    ```
    git config --global user.name "your_Name"
    git config --global user.email "your_email"
    ```
- 克隆远程仓库
    ```
    git clone 远程仓库地址
    ```
- 将所有文件添加到暂存区
    ```
    git add *
    ```
- 将暂存区的文件添加到本地仓库
    ```
    git commit -m "注释"
    ```
-   将本地仓库推送到远程仓库(-f 强制推送)
    ```
    git push -f origin master
    ```

---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/disallow/  

