# 本地自动推送Gitee


![](https://burayimg.heisenw.com/blog/img/ea4c9f40-20d7-4b6a-a0b7-769a3bb0257c.webp)

- 脚本
  
  ```
  @ECHO OFF
  title=GIT提交
  set /p input=请输入备注：
  git add *
  git commit -m "%input%"
  git push -f origin master
  echo "提交成功！！！"
  pause
  ```

- 使用方法
  
  - 将脚本另存为`Gitee全推送.bat`,存放在需要推送的文件夹里，每次双击运行，输入备注，确定推送就可以啦。
  - 注意，要将编码方式改为ANSI，否则可能会出现中文乱码。

- 还有个方法，看起来挺好，不过我没那么多内容需要操作，所以就没用
  
  - 传送门：[使用TortoiseGit图形化工具免Git命令远程控制仓库 - Gitee篇](https://blog.csdn.net/weixin_51684729/article/details/123768886)
  
  - 基本完全搬运，个人略微修改，亲测好使
  
  - 原文出处：[本地脚本自动提交文件夹中的内容到Gitee仓库](https://blog.csdn.net/qq_45796486/article/details/119960239)

---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/auto-gitee/  

