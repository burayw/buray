# Docsify学习笔记


![](https://burayimg.heisenw.com/blog/img/bb3b1351-17dd-4e29-a810-ac70f6590340.webp)

## docsify本地部署

### windows环境搭建

- 软件准备
  
  - [node.js中文官网](http://nodejs.cn/)
    - 选择“长期支持版本（LTS）”
  - [git官网](https://git-scm.com/)
    - 下载最新版本

- 软件安装
  
  - node.js 默认安装即可
    - 安装完成后 win+r 打开命令提示符输入 `node` 有版本号输出即可
  - git 安装时注意添加到桌面快捷方式，其他内容默认安装即可
    - 安装完成后 右键能打开 Git Bash Here 即可

- 新建文件夹命名为tcwiki，右键使用 Visual Studio Code 打开
  
  - 发现右键没有 Visual Studio Code
    
    - 解决方案
      
      - 新建文本文档，修改后缀为 .reg
      
      - 右键打开文档，粘贴为以下代码
        
        ```
        Windows Registry Editor Version 5.00
        
        [HKEY_CLASSES_ROOT\*\shell\VSCode]
        @="Open with Code"
        "Icon"="C:\\Users\\Administrator\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
        
        [HKEY_CLASSES_ROOT\*\shell\VSCode\command]
        @="\"C:\\Users\\Administrator\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""
        
        Windows Registry Editor Version 5.00
        
        [HKEY_CLASSES_ROOT\Directory\shell\VSCode]
        @="Open with Code"
        "Icon"="C:\\Users\\Administrator\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
        
        [HKEY_CLASSES_ROOT\Directory\shell\VSCode\command]
        @="\"C:\\Users\\Administrator\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%V\""
        
        Windows Registry Editor Version 5.00
        
        [HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode]
        @="Open with Code"
        "Icon"="C:\\Users\\Administrator\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
        
        [HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode\command]
        @="\"C:\\Users\\Administrator\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%V\""
        ```
      
      - 其中`C:\\Users\\Administrator\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe`为 Visual Studio Code 所在位置路径，可以在 Visual Studio Code 快捷方式上`右键-属性`获得，注意`\`要替换为`\\`。
      
      - 修改完成后保存，双击运行，在文件加上右键，发现`使用 Visual Studio Code 打开`已出现，成功。

- VS Code设置和插件安装
  
  - 安装`Markdown Preview Github Styling` 插件
    - 可以进行Markdown预览
    - 点击插件旁边的小齿轮，设置显示模式为light可以变换右侧预览窗口的颜色

- 新建`demo.md` 学习和测试 `markdown 语法`
  
  - [markdown常用语法](https://zhuanlan.zhihu.com/p/108984311)

- npm更换国内镜像源（淘宝）
  
  - CMD输入以下命令
    
    ```
    npm config set registry https://registry.npm.taobao.org
    ```
    
    使用下面的命令查看是否切换成功
    
    ```
    npm get registry
    ```
    
    ### 部署docsify

- 打开[docsify官方GitHub](https://github.com/docsifyjs/docsify),下拉找到`Documentation`点击进入官方网站。
  
  - 或者可以直接进入其官网[https://docsify.js.org/](https://docsify.js.org/)
  - Translations 切换中文
    - 完犊子，官方的中文翻译404了...
      - 20220814上梯子可以显示了

- `docsify-cli`全局安装
  
  ```
  npm i docsify-cli -g
  ```

- 初始化 docsify
  
  ```
  docsify init ./docs
  ```
  
  提示：
  
  ```
  docsify : 无法加载文件 C:\Users\Administrator\AppData\Roaming\npm\docsify.ps1，因为在此系统上禁止运行脚本。有关详细信息，
  请参阅 https:/go.microsoft.com/fwlink/?LinkID=135170  docsify : 无法加载文件 C:\Users\Administrator\AppData\
  Roaming\npm\docsify.ps1，因为在此系统上禁止运行脚本。
  ```
  
  上网查了一下，是因为`windows 为了安全 默认的执行策略为 Restricted`,调整设置为`RemoteSigned`就可以了。具体操作如下：
  
  > Visual Studio Code中选择任意md文件右键`在集成终端中打开`
  > 
  > > 获取默认的执行策略，终端中运行`get-executionpolicy`。

> > 设置执行策略,继续运行`Set-ExecutionPolicy RemoteSigned`,提示错误。

> > > 原因在于没有获得管理员权限，关闭Visual Studio Code,在其快捷方式上右键`以管理员身份运行`,重新运行即恢复正常。

> > 下面是4种常用的执行策略
> > 
> > > Restricted：禁止运行任何脚本和配置文件。

> > > AllSigned ：可以运行脚本，但要求所有脚本和配置文
> > > 件由可信发布者签名，包括在本地计算机上编写的脚本。

> > > RemoteSigned ：可以运行脚本，但要求从网络上下载的脚本和配置文件由可信发布者签名;不要求对已经运行和已在本地计算机编写的脚本进行数字签名。

> > > Unrestricted ：可以运行未签名脚本。（危险！）

- 重新初始化 docsify
  
  ```
  docsify init ./docs
  ```
  
  成功。自动新建了`docs`文件夹,其中有3个文件
  
  ```
  index.html - 入口文件
  README.md - 主页内容
  .nojekyll - 防止 GitHub Pages 忽略以下划线开头的文件
  ```

- 启动本地服务
  
  ```
  docsify serve docs
  ```

- 不出意外，出现了新的问题
  
  - 页面不显示任何内容
    
    - 各种尝试，各种失败，今晚就到这里吧。 
    
    - 用Chrome检查`Network`，发现`docsify`的`script`和`vue.css`状态为`failed`。
      
      - 检查`index.html`发现这两个引用文件使用了`jsdelivr`的CDN，上网查询发现2021年12月份`jsdelivr`的备案被取消了，所以国内无法访问。需要考虑替换方案。
    
    - 将`cdn.jsdelivr.net/npm`全部替换为`unpkg.com`恢复正常
      
      - 备选方案（来源知乎：[jsdelivr的cdn挂了，很多依赖GitHub的网站打不开了，解决方法来了](https://zhuanlan.zhihu.com/p/517909552?utm_source=wechat_session&utm_medium=social&utm_oi=606048952009887744&utm_campaign=shareopn)）
        
        - 很多GitHub文件的cdn也用的jsdelivr，将http://cdn.jsdelivr.net替换为http://fastly.jsdelivr.net 就行了。
        
        - 如果这个也挂了,打开[https://ipaddress.com/website/raw.githubusercontent.com](https://ipaddress.com/website/raw.githubusercontent.com)将ip地址复制到host文件`C:\Windows\System32\drivers\etc\hosts`即可。
          
          ### 相关设置

- index.html设置
  
  - `window.$docsify`修改，可以在右上角显示GitHub logo并可附带链接。
    
    - ![](/img/githuplogo.png)
    
    - `name`为网站标题
    
    - `repo`为网站链接
    
    - 添加侧边栏
      
      - `loadSidebar: true`。注意，换行时需要以`,`结束上一行代码。
      - 新建`_sidebar.md`文件
        - 在`_sidebar.md`文件里面写入侧边栏主目录，使用相对路径链接。
    
    - 显示目录
      
      - `subMaxLevel: 2`
          这里的`2`指的是侧边栏显示二级目录，即二号标题`##`，这样以来，一号标题`#`就会被隐藏，可以作为备注或描述使用。
    
    - 定制封面
      
      - `coverpage: true`
      - 在根目录下新建`_coverpage.md`文件
    
    - 定制导航栏
      
      - `loadNavbar: true`
      
      - 在根目录下新建`_navbar.md`文件
        
        ## 上传Gitee仓库
        
        ### 仓库配置

- 登录[Gitee](https://gitee.com/)
  
  - 如果未实名，需要先进行实名认证，1天内就可完成

- 新建仓库
  
  - 可以选择`公开`或`私有`（新建仓库时无法选择，可以先建好仓库后修改）
  - 初始化仓库，开源许可选择`MIT`即可
      `MIT`许可简介：我写我的代码，你尽管拿去用，出了问题我不负责
  - 新建完仓库后，进入仓库管理，就可以设置开源了。

- Git上传推送
  
  - 在在docs上级文件夹内右键`Git Bash Here`
    
    - 全局User配置(只用操作一次)
      
      - `git config --global user.name "your_Name"`
      - `git config --global user.email "your_email"`
    
    - 克隆远程仓库
      
      - `git clone 远程仓库地址`
        
        > 通过克隆远程仓库，可以直接自动绑定仓库，省去了绑定仓库的流程。
      
      - 将docs文件夹移动到克隆后的本地项目文件夹内（我新建的仓库项目叫`buray`,所以克隆后本地出现了`buray`文件夹）
    
    - 将所有文件添加到暂存区
      
      - 在项目文件夹内右键`Git Bash Here`
      - `git add *`
    
    - 将暂存区的文件添加到本地仓库
      
      - `git commit -m "注释"`
    
    - 将本地仓库推送到远程仓库(-f 强制推送)
      
      - `git push -f origin master`
        
        > 强制推送会覆盖远程仓库内的所有文件，如果是团队开发需谨慎使用。
      
      - 输入`Gitee`帐号密码，按下回车即可推送

- 开启 `Gitee Pages`
  
  - 登录`Gitee`进入仓库，点击服务开启`Gitee Pages`
    
    ## 宝塔面板同步Gitee

- 实现效果：当Gitee仓库发生任何变化时，触发webhook将最新内容覆盖到服务器端（宝塔面板）

- 注意：先不要在宝塔上`添加站点`,因为使用webhook会将整个仓库的内容都push过来，但仓库根目录实际并不是网站根目录，网站的根目录是`docs`文件夹，所以等整个webhook都设置完成并且测试没问题后再添加站点。当然先添加了也不要紧，后期到网站管理里面改一下根目录就可以了。
  
  ### 宝塔终端生成公钥

- 首先去宝塔终端查看是否有装git（一般默认是安装了的）
  `git --version`

- 如果没有就自行安装
  `yum install git`

- 生成公钥
  `ssh-keygen -t rsa -C "你的邮箱地址"`

- 查看公钥
  `cat ~/.ssh/id_rsa.pub`

- 复制公钥
  
  - 从`ssh-rsa`开始一直复制到最后
    
    ### Gitee仓库添加公钥

- 使用公钥就不用用户名密码了，更加方便

- 进入仓库点击`管理`——`部署公钥管理`——`添加公钥`
  
  - 标题自己起一个，自己能看懂就行
  
  - 下面粘贴刚复制的公钥
    
    ### 宝塔安装WebHook插件并配置

- 宝塔软件商店里搜索webhook并安装

- 安装完成后点击设置，添加脚本
  
  - 脚本代码：
    
    ```
    #!/bin/bash
    echo ""
    #输出当前时间
    date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"
    echo "-------开始-------"
    #判断宝塔WebHook参数是否存在
    if [ ! -n "buray" ];
    then 
        echo "param参数错误"
        echo "End"
        exit
    fi
    #服务器 git 项目路径
    gitPath="/www/wwwroot/buray"
    #码云项目 git 网址
    gitHttp="git@gitee.com:buray/buray.git"
    
    echo "路径：$gitPath"
    
    #判断项目路径是否存在
    if [ -d "$gitPath" ]; then
      cd $gitPath
      #判断是否存在git目录
      if [ ! -d ".git" ]; then
              echo "在该目录下克隆 git"
              git clone $gitHttp gittemp
              mv gittemp/.git .
              rm -rf gittemp
      fi
    #拉取最新的项目文件
      git reset --hard origin/master
    #git clean -f
      git pull origin master
      echo "拉取完成"
      #执行npm
      #执行编译
      #npm run build
      #设置目录权限
      chown -R www:www $gitPath
      echo "-------结束--------"
      exit
    else
      echo "该项目路径不存在"
      echo "End"
      exit
    fi
    ```
  
  - 这个脚本代码需要说明一下，我在测试时遇到了各种问题
    
    > 网上很多教程，包括官方说法中`if [ ! -n "buray" ]`这里都是使用`if [ ! -n "$1" ]`,其中`$1`指的是`param`后面的仓库名称，但我这样操作后无论在终端测试还是在webhook测试都提示`param参数错误`,最后将脚本中所有的`$1`都固定为我的仓库名称`buray`这个报错才消失。
    
    > 所有的配置都完成后，在Gitee端测试没问题，在宝塔这边测试也提示`拉取完成`,但实际上并没有生效，代码也没有更新。自己啥也不懂，折腾了半晚上也没弄明白，最后瞎猫碰到死耗子，解决了。
    
    > > 首先，在网站根目录下（脚本执行目录）新建了一个`test.sh`文件，将脚本代码全部粘进去。
    
    > > 然后，登录终端，cd到网站根目录后，使用`bash test.sh`命令运行这个脚本。脚本运行到最后，弹出了一个窗口，提示`Merge branch ‘master’ of …`。脚本就是卡在这个步骤上面了。
    
    > > 上网一顿找，找到了[解决方案](https://blog.csdn.net/weixin_44094836/article/details/122058107),在脚本中`git pull origin master`前面，加上一行`git config --global pull.rebase true`，问题得到了解决。
  
  - 网上说代码放进去了可能会被过滤，添加好后点击编辑再确认一下，如果被过滤就再把代码复制进去然后保存。不过反正我是没被过滤。
    
    > [!warning]
    > 由于webhook后来出了问题，我把整个服务器重装了，顺便由Ubuntu20.04改成了CentOS8.2，结果在上面这步的时候出了问题，日志只显示了“开始”没有运行脚本，尝试bash了一下，提示`11.sh: line 47: syntax error: unexpected end of file`,47行是最后一行`fi`之后的空行，删除之后运行提示`11.sh: line 46: syntax error near unexpected token fi`。
  
  - 参考了[解决执行脚本报syntax error: unexpected end of file或syntax error near unexpected token `fi'错误的问题](https://blog.csdn.net/u010735147/article/details/81066564)得以解决：
    
    - 两个问题都是由于.sh文件的格式为dos格式。而linux只能执行格式为unix格式的脚本。因为在dos/window下按一次回车键实际上输入的是“回车（CR)”和“换行（LF）”，而Linux/unix下按一次回车键只输入“换行（LF）”，所以修改的sh文件在每行都会多了一个CR，所以Linux下运行时就会报错找不到命令。
    
    - 我们可以查看该脚本文件的格式，方法是使用命令进入编辑文件：
      
      ```
      vim nginx_check.sh
      ```
    
    - 直接输入下面的命令回车
      
      ```
      :set ff
      ```
    
    - 可以看到当前脚本格式是dos，我们需要把格式改为unix，方法是继续输入下面的命令回车：
      
      ```
      :set ff=unix
      ```
    
    - 或者也可以输入`:set fileformat=unix`
    
    - 然后我们再输入`:set ff`来查看格式，可以看到当前脚本格式变成了我们想要的"unix"了。
    
    - 输入`:wq`回车，保存并推出即可。
    
    - 这时再进行bash就正常了，直接在宝塔文件管理里面打开文件，全选复制粘贴到webhook设置里面，点击测试，一切恢复正常。
  
  - 重启一下宝塔，在终端输入命令`/etc/init.d/bt restart`
    
    ### 配置Gitee的WebHooks

- 宝塔进入软件商店，进入webhook，查看刚生成的密钥，复制`GET/POST:`后面的链接。
  
  > 注意`@param access_key string HOOK密钥
  > @param param string 自定义参数（在hook脚本中使用$1接收）`这两段不要复制。

- 进入Gitee仓库，管理，WebHooks，添加WebHook
  
  > `URL`就是刚才复制的链接，注意要修改最后面`param=aaa`中的`aaa`为自己的仓库名。
  > 
  > > 这个就是前面说的`$1`，由于我直接固定了，没用`$1`，所以理论上这个aaa我随便写或者不改也没啥影响。这里留个记号，说不准以后能用到。
  > > `WebHook密码/签名密钥`这里选择`WebHook密码`即可，后面填入webhook的密钥（去宝塔webhook查看密钥那里复制即可）。
  > > `选择事件`使用默认的`Push`和`激活`即可，点`添加`
  > > [!attention]
  > > 20220828这两天发现webhook异常，日志显示拉取成功，gitee仓库的webhook日志也显示成功，但就是不更新。手动bash了一下脚本，更新正常。不知道是什么原因造成的。怀疑是近期宝塔面板的安全检测提醒部分目录权限问题，手动修改了部分目录的权限导致的，有时间得研究一下，实在不行就卸载webhook重新安装，重新走一遍这个流程试试。

- 仔细测试了宝塔webhook和Gitee的webhook，发现Gitee的webhook日志里Response显示：
  
  ```
  <!doctype html>
  拒绝访问
  抱歉,您没有访问权限
  请使用正确的域名访问!
  查看许可域名: cat /www/server/panel/data/domain.conf
  关闭访问限制: rm -f /www/server/panel/data/domain.conf
  ```

- 原来域名没备案下来的时候我设置的webhook，使用的是服务器IP，现在我把IP访问关闭了，导致域名错误。

- 回到宝塔面板打开webhook，查看密钥，果然密钥链接改成域名了，复制过来更新一下Gitee的webhook，测试，通信成功！

- 不过高兴地太早了，网站并没有更新。检查两边的webhook日志，发现通信没问题。

- 那么网站没更新，是脚本的问题

- bash测试了一下脚本，发现能够更新，不过`head`好像拉取的不对，但文件更新没有问题。而且最后出现了一行提示
  
  ```
  chown: changing ownership of '/www/wwwroot/docs/.user.ini': Operation not permitted
  ```

- 在[‘.user.ini’: Operation not permitted 权限不足无法操作.user.ini 的解决办法](https://laowangblog.com/user-ini-operation-not-permitted.html)这篇文章上找到了一点提示。
  
  - 应该是前一段时间宝塔面板提醒我安全监测有几项高危的文件权限问题，我根据面板的提示进行了修改导致webhook插件没有权限，现在尝试恢复权限。
    
    ```
    chown -R www:www /www/wwwroot/docs
    ```
  
  - 依旧提示
    
    ```
    chown: changing ownership of '/www/wwwroot/docs/.user.ini': Operation not permitted
    ```
  
  - 不慌，有这个提示是正常的，这还是因为 .user.ini 权限问题，我们现在就是要解决这个问题，在修改 .user.ini 权限组或者删除 .user.ini 前，我们需要手动解除 .user.ini 文件的锁定：
    
    ```
    chattr -i /www/wwwroot/docs/.user.ini
    ```
  
  - 然后再执行上面的修改权限命令
    
    ```
    chown -R www:www /www/wwwroot/docs
    ```
  
  - 这会儿没有错误提示了，随便更新个文件，看看能不能拉取过来
  
  - 不行，还是没拉取过来，再跑一下脚本，没再提示权限问题。这就有点脑大了，手工bash没问题，说明脚本也没问题，大概率还是webhook的权限问题啊。看日志提示`拉取成功`，说明脚本能够自动运行，检查网站目录，有的目录是644权限，全部改成755试试，本来就是个学习网站，数据多处有备份，也不怕攻击，何况没什么被攻击的价值😂。
  
  - 还是不行，这是要闹哪样！！！！卸载webhook，重新安装，重新配置试试。仍然是老样子，冷静！
  
  - 难道是前几天改的那几个文件的权限没有恢复正确？话说我也忘了之前是什么权限了啊，找个同样的环境安装一套宝塔看看。NAS有docker，安装个环境研究研究。
  
  - 尝试了按照新安装的bt修改文件权限，不行，而且发现了新的问题，不知道是什么原因nginx开机不自启动，必须手动启动。
  
  - 尝试重新设置公钥，依旧无法解决。放弃了，决定整个服务器重置。可惜之前没有备份，所有的东西都需要重新做，哎，长个心眼儿吧，以后要注意备份。
    
    > [!note]
    > 考虑到这个服务器之前做过很多次奇奇怪怪的试验，直接重装了，从头慢慢捋了一遍，终于恢复正常了，赶紧备份系统镜像。
    > [!attention]
    > 20220901发现webhook再度异常，每次更新都提示拉取成功，但每次都是拉取的同一个版本，就是第一次在ssh上bash时的那个版本，说实话这种基础的知识网上的内容太少了，最后受[使用WEBHOOK 拉取的问题求帮助](https://www.bt.cn/bbs/forum.php?mod=viewthread&tid=7485)这篇帖子的提示，发现还是权限问题，在所有的命令前面加个sudo，使用root用户运行就好了。其实还有个问题，目前还没找到解决办法，就是每次拉取内容都能成功，但是提示的`HEAD is now at`却是上一个版本的，找一顿也没弄明白是怎么回事，管他呢，能更新就行。
  
  - 最后成功的脚本如下：
    
    ```
    #!/bin/bash
    echo ""
    #输出当前时间
    date --date='0 days ago' "+%Y-%m-%d %H:%M:%S"
    echo "-------开始-------"
    #判断宝塔WebHook参数是否存在
    if [ ! -n "buray" ];
    then 
        echo "param参数错误"
        echo "End"
        exit
    fi
    #服务器 git 项目路径
    gitPath="/www/wwwroot/buray"
    #码云项目 git 网址
    gitHttp="git@gitee.com:buray/buray.git"
    
    echo "路径：$gitPath"
    
    #判断项目路径是否存在
    if [ -d "$gitPath" ]; then
      cd $gitPath
      #判断是否存在git目录
      if [ ! -d ".git" ]; then
              echo "在该目录下克隆 git"
              sudo git clone $gitHttp gittemp
              sudo mv gittemp/.git .
              sudo rm -rf gittemp
      fi
      echo "拉取最新的项目文件"
      sudo git reset --hard origin/master
      sudo git pull        
      echo "设置目录权限"
      sudo chown -R www:www $gitPath
      echo "End"
      exit
    else
      echo "该项目路径不存在"
              echo "新建项目目录"
      mkdir $gitPath
      cd $gitPath
      #判断是否存在git目录
      if [ ! -d ".git" ]; then
              echo "在该目录下克隆 git"
              sudo git clone $gitHttp gittemp
              sudo mv gittemp/.git .
              sudo rm -rf gittemp
      fi
      echo "拉取最新的项目文件"
      sudo git reset --hard origin/master
      sudo git pull
      echo "设置目录权限"
      sudo chown -R www:www $gitPath
      echo "End"
      exit
    fi
    ```
    
    ### 将Gitee仓库clone到服务器的本地目录中

- 这一步实际上先做后做都行，不做我觉得应该也是可以的，但我没试过。

- 终端cd到网站根目录`www/wwwroot`下，首次clone需要设置一下下面这些内容，一条命令一条命令的运行，好像就是全局设置作者信息，跟前面本地clone仓库一样。
  
  ```
  git config --global user.name "用户名"
  git config --global user.email "邮箱"
  git config --global credential.helper store //会生成.gitconfig 的文件
  cat .gitconfig   //如果报错: No such file or directory，就用下一行的代码
  cat ~/.gitconfig  //显示内容
  ```

- 然后将Gitee仓库clone下来
  
  ```
  git clone https://gitee.com/XXX/XXX.git  //clone后面是仓库链接
  ```

- 据说第一次clone需要输入用户名和密码，不过我好像没有，可能以前输过。
  
  ```
  Username for 'https://gitee.com': your@email.com
  Password for 'https://xxxx@xxxx.com@gitee.com': yourPassword（看不到输入内容）
  ```
  
  ### 测试

- 好了，都配置完了，测试就很简单了。

- Gitee的WebHooks测试
  
  > WebHooks管理里面有个测试，点一下，在下面的`请求历史`里面显示出来日志，`密码请求`后面显示`200`就说明Gitee这边没啥问题了（我也不知道啥意思）
  > 上宝塔webhook，查看一下日志，应该就能看到刚才发起的测试运行日志。不过我这边一开始的时候显示`拉取完成`，但实际内容却没改，解决方案在上面`脚本`部分谢了。
  > 宝塔webhook这边也有个测试，也可以测试一下。
  > 当然，最终测试还是在本地push一些新文件到远程仓库，再上服务器上看看文件有没有同步更新。
  
  ## 网站上线

- 其实上面操作结束后网站就已经上线了，这里有些小问题要处理一下。
  
  ### docsify上传到服务器（宝塔面板）

- 宝塔面板，网站，添加站点，注意一下前面所说的`根目录`问题，`PHP版本`选择`纯静态`。

- 打开首页显示404
  
  > 网上找的解决方案，出现这个问题主要是宝塔面板在添加站点是，默认设置了禁止访问`Readme.md`文件。
  > 宝塔找到对应的站点，配置文件，找到` #禁止访问的文件或目录`,把最后面的`|README.md`删掉后保存就可以了。


---

> 作者: [Buray](http://www.burayw.top)  
> URL: http://www.buray.top/posts/docsify-notes/  

