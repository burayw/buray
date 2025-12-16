# 威联通NAS虚拟机安装飞牛系统


### 背景说明
- 几年前花两千大洋买了个威联通（血亏啊，现在小黄鱼上也就卖400块钱），但这玩意儿功能太复杂了，玩不明白，一直当私有云盘用。去年接触到了飞牛，这个简单，特别是影音功能好玩。上网查了一大堆资料，硬装的话有点舍不得，而且似乎会出现各种问题，例如风扇不能控制等。偶然发现可以在威联通自带的虚拟机上安装飞牛，决定试试。  
### 前期准备
- 上飞牛官网下载最新版本系统iso文件：[飞牛官网fnnas.com](https://www.fnnas.com/)，下载后存储到NAS随便哪个文件夹里。
- 威联通`App Center`下载安装`Virtualization Station`  
- 用`File Station`在需要的地方新建一个共享文件夹，自己命个名，例如`fnos`，配置默认即可  
### 虚拟机配置
- `Virtualization Station`新建虚拟机，名称随便写，文件位置选刚才创建的文件夹，操作系统选`Linux`，操作系统版本选`Debian 9.1.0`  
  ![](https://burayimg.heisenw.com/blog/img/bfb8a06a-ec2f-41d5-85a1-6aaf2f6894d9.webp)  
- CPU型号选Passthrough，CPU至少选2核，内存至少给2G  
  ![](https://burayimg.heisenw.com/blog/img/5c670020-d6f3-45ff-b21a-7d2e45df1a5e.webp)  
- 硬盘、网卡配置默认即可，光驱控制器选SCSI，光盘选之前下载的飞牛iso文件。  
  ![](https://burayimg.heisenw.com/blog/img/f88077f4-666e-4994-8259-79b79c4a1c16.webp)  
- 显卡选QXL
  ![](https://burayimg.heisenw.com/blog/img/f855d959-abc5-4a99-bec8-797d197c7d39.webp)
- 然后完成创建即可（其实可以勾选“创建之后自动启动虚拟机”，不过我忘了），创建之后回到`Virtualization Station`，找到刚才创建的虚拟机，点一下后面的齿轮，点开始。  
  ![](https://burayimg.heisenw.com/blog/img/a00840e9-6ef1-441c-9bcc-5d9b44762807.webp)  
- 然后单机左下角这个小屏幕，就可以进入虚拟机的界面了。
  ![](https://burayimg.heisenw.com/blog/img/a94b1674-72b3-4069-8906-774b1e384ff2.webp)
- 这里我遇到了一个问题，使用默认的`Graphical Install`(图形化安装)一直在报错，我也不知道是什么原因，如果遇到跟我一样的问题，可以重启虚拟机，然后选择`Rescuing Install`(应急模式安装)，所有选项默认即可。  
  ![](https://burayimg.heisenw.com/blog/img/51d4aeb6-d00c-4d81-a571-1c88c347b51c.webp)  
- 会报一些奇奇怪怪的错误，不过不用管。  
  ![](https://burayimg.heisenw.com/blog/img/fb25b633-21cc-4b4d-8baf-38db241c977e.webp)  
- 等到提示`Install TRIM OK!`回车。  
  ![](https://burayimg.heisenw.com/blog/img/c160c3fc-ce01-4805-97d0-7c3b5ba7699c.webp)  
- 出来命令行，输入`reboot`重启虚拟机。  
  ![](https://burayimg.heisenw.com/blog/img/27ccd61f-0f32-4b8e-9cc0-c81536bab1e1.webp)  
- 出来这个界面，意味着飞牛已经安装成功了！后面就是飞牛的正常操作了。  
  ![](https://burayimg.heisenw.com/blog/img/e786bea0-504b-49c1-a91d-66208890d406.webp)  





---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/qnap-fnos/  

