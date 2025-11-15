# 使用Cloudflare R2 搭建免费图床

## 我觉得CF是世界上最良心的网络公司，他提供的免费功能几乎可以完全覆盖一个普通人对于网络发布的需求，而且质量很稳定很好。
## Cloudflare 配置
- 当然是先注册个Cloudflare账号，很简单，所以就略过了
- 登录CF后在左侧导航栏进入`存储和数据库`-`R2对象存储`-`概述`
  ![](https://burayimg.heisenw.com/blog/img/1b487d46-4421-4b07-aa0e-8f67f497222f.webp)
  
  新用户的话会要求绑定支付账户，应为CF的存储桶每月免费流量是10G，对于做个博客来讲是足够用的，但是CF要求必须绑定支付协议才可以，以避免超量后没有支付。我是绑了Paypal，我的Paypal绑定的是一张交行的银联卡，里面木有钱，以防万一。

- 点击右上角`创建存储桶`，起一个名字，要特别注意`位置`是不是`亚太地区`，如果不是，可以点击下面的`提供位置提示（可选）`选择`亚太地区`。
  ![](https://burayimg.heisenw.com/blog/img/934d6703-8b0d-4d15-9a7f-8478d22506d5.webp)
- 进入刚创建的存储桶，点击`设置`，先设置一下`自定义域`，如果实在CF上面注册购买的域名，这里可以直接设置；如果不是，则需要先将你的域名托管到CF上面，再设置这里。
  ![](https://burayimg.heisenw.com/blog/img/3176cde5-03e8-4e96-8226-d678ccadcca6.webp)
  `.r2.cloudflarestorage.com/xxx`前面这段就是你的`account_id`，后面配置的时候会用得到。
- 回到`存储和数据库`-`R2对象存储`-`概述`，右下角有`API Tokens`，点击`Manage`，进入后`创建Account API 令牌`，令牌名称可以自己起一个，权限选择`对象读和写`，其他的默认就可以，点击最下面的`创建Account API 令牌`。
  ![](https://burayimg.heisenw.com/blog/img/d4d21776-2710-4348-8825-14ed61a0c697.webp)
- 注意创建令牌后不要关闭页面，这个页面只显示这一次，关闭后就再也不找不到ID了。
  ![](https://burayimg.heisenw.com/blog/img/25d460fb-d456-4a1a-8ff9-5f0afe13be07.webp)
- `访问密钥 ID`就是`access_key_id`
- `机密访问密钥`就是`secret_access_key`
## Python 配置
- 脚本制作
  1. 新建文本文档，随便命个名（例如`r2`）,修改后缀为`.py`，用vscode打开。
  2. 复制下面的脚本粘贴进去：
   ```
    import keyboard
    import pyperclip
    from PIL import ImageGrab, Image
    import io
    import boto3
    from botocore.config import Config
    import time
    import uuid
    import pyautogui
    import os
    from io import BytesIO
    # 示例配置
    # # R2 配置
    # R2_CONFIG = {
    #     'account_id': '你的account id',
    #     'access_key_id': '你的令牌key id',
    #     'secret_access_key': '你的令牌密码',
    #     'bucket_name': '桶名称'
    # }

    # # OSS 配置
    # OSS_CONFIG = {
    #     'url': 'sb-eo-r2.2x.nz',
    #     'prefix': '/fuwari-blog/img'
    # }
    #########################################################
    # R2 配置
    R2_CONFIG = {
        'account_id': '',
        'access_key_id': '',
        'secret_access_key': '',
        'bucket_name': ''
    }

    # OSS 配置
    OSS_CONFIG = {
        'url': '你的自定义域',
        'prefix': '存储路径'
    }
    #########################################################
    def init_r2_client():
        """初始化 R2 客户端"""
        return boto3.client(
            's3',
            endpoint_url=f'https://{R2_CONFIG["account_id"]}.r2.cloudflarestorage.com',
            aws_access_key_id=R2_CONFIG['access_key_id'],
            aws_secret_access_key=R2_CONFIG['secret_access_key'],
            config=Config(signature_version='s3v4'),
            region_name='auto'
        )

    def get_image_from_clipboard():
        """从剪贴板获取图片"""
        try:
            image = ImageGrab.grabclipboard()
            if image is None:
                return None

            # 如果是列表（多个文件），取第一个
            if isinstance(image, list):
                if len(image) > 0:
                    # 如果是图片文件路径，打开它
                    try:
                        return Image.open(image[0])
                   except Exception as e:
                        print(f"打开图片文件失败: {e}")
                        return None
                return None

            # 如果直接是 Image 对象
            if isinstance(image, Image.Image):
                return image

           return None
        except Exception as e:
            print(f"获取剪贴板图片失败: {e}")
            return None

    def convert_to_webp(image):
       """将图片转换为 webp 格式"""
        if not image:
            return None

        try:
            buffer = BytesIO()
           # 确保图片是 RGB 模式
           if image.mode in ('RGBA', 'LA'):
               background = Image.new('RGB', image.size, (255, 255, 255))
                background.paste(image, mask=image.split()[-1])
                image = background
           elif image.mode != 'RGB':
               image = image.convert('RGB')

           image.save(buffer, format="WEBP", quality=80)
            return buffer.getvalue()
        except Exception as e:
            print(f"转换图片失败: {e}")
           return None

    def upload_to_r2(image_data):
        """上传图片到 R2"""
        if not image_data:
           return None

       client = init_r2_client()

       # 生成基础文件名
       base_filename = f"{uuid.uuid4()}.webp"
        filename = base_filename

       try:
         # 检查文件是否已存在
           attempt = 1
           while True:
               try:
                   # 尝试获取文件信息，如果文件存在会返回数据，不存在会抛出异常
                   client.head_object(
                       Bucket=R2_CONFIG['bucket_name'],
                       Key=f"{OSS_CONFIG['prefix'].strip('/')}/{filename}"
                   )
                    # 如果文件存在，修改文件名
                   name_without_ext = base_filename.rsplit('.', 1)[0]
                   filename = f"{name_without_ext}_{attempt}.webp"
                   attempt += 1
                   print(f"文件名已存在，尝试重命名为: {filename}")
               except client.exceptions.ClientError as e:
                    # 如果是 404 错误，说明文件不存在，可以使用这个文件名
                   if e.response['Error']['Code'] == '404':
                       break
                   raise e  # 其他错误则抛出

           # 上传文件
           client.put_object(
               Bucket=R2_CONFIG['bucket_name'],
               Key=f"{OSS_CONFIG['prefix'].strip('/')}/{filename}",
               Body=image_data,
                ContentType='image/webp'
            )
            return filename
        except Exception as e:
            print(f"上传失败: {e}")
           return None

    def generate_markdown_link(filename):
       """生成 Markdown 图片链接"""
        if not filename:
           return None

        url = f"https://{OSS_CONFIG['url']}{OSS_CONFIG['prefix']}/{filename}"
       return f"![]({url})"

    def type_markdown_link(markdown_link):
        """模拟键盘输入 Markdown 链接"""
       if not markdown_link:
            return

        pyperclip.copy(markdown_link)
       pyautogui.hotkey('ctrl', 'v')

    def handle_upload():
        """处理图片上传的主函数"""
        print(f"\n[{time.strftime('%Y-%m-%d %H:%M:%S')}] 收到粘贴请求")

        print("正在检查剪贴板...")
        # 获取剪贴板图片
        image = get_image_from_clipboard()
        if not image:
            print("❌ 剪贴板中没有图片")
           return
       print("✅ 获取到剪贴板图片")

        # 转换为 webp
        print("正在转换为 WebP 格式...")
        image_data = convert_to_webp(image)
        if not image_data:
            print("❌ 图片转换失败")
            return
        print(f"✅ 转换完成，大小: {len(image_data)/1024:.2f}KB")

        # 上传到 R2
        print("正在上传到 R2...")
        filename = upload_to_r2(image_data)
        if not filename:
            print("❌ 上传失败")
            return
        print(f"✅ 上传成功，文件名: {filename}")

        # 生成并输入 Markdown 链接
        markdown_link = generate_markdown_link(filename)
        if markdown_link:
            print(f"生成的 URL: https://{OSS_CONFIG['url']}{OSS_CONFIG['prefix']}/{filename}")
            print(f"模拟键入: {markdown_link}")
            type_markdown_link(markdown_link)
            print("✅ 操作完成")

    def main():
        """主函数"""
        print("=" * 50)
        print("R2 图片上传插件已启动")
        print(f"当前配置:")
        print(f"- OSS 域名: {OSS_CONFIG['url']}")
        print(f"- 存储路径: {OSS_CONFIG['prefix']}")
        print(f"- R2 存储桶: {R2_CONFIG['bucket_name']}")
        print("使用 Ctrl+Alt+V 上传剪贴板中的图片")
        print("=" * 50)

        # 注册快捷键
        keyboard.add_hotkey('ctrl+alt+v', handle_upload)

        # 保持程序运行
        keyboard.wait()

    if __name__ == "__main__":
        main()
     ```
    3. 编辑脚本中R2和OSS配置的内容
   ![](https://burayimg.heisenw.com/blog/img/6db33e04-da2d-433a-9be2-54379b69a955.webp)  
   只需要编辑图中红框部分即可。    
    account_id：前文提到的`.r2.cloudflarestorage.com/xxx`前面部分。  
    access_key_id：前文提到的`访问密钥 ID`  
    secret_access_key：前文提到的`机密访问密钥`  
    bucket_name：你自己设置的`桶名称`  
    OSS里面的url: 你的自定义域，这个根据你前面设定的自定义域填写即可，不用写`https//`。  
    OSS里面的prefix：存储路径，可以自己定义，例如我的存储路径是`/blog/img`，这样上传的图片都会再这个存储桶的/blog/img/文件夹内。  
- 下载安装最新版Python。[Python官网](https://www.python.org/)，建议安装`Stable Releases`里面的最新版本。  
  ![](https://burayimg.heisenw.com/blog/img/632af889-fc74-43df-81da-01c776f7bded.webp)  
  安装的是以后特别注意勾选`Add Python to PATH`，不然安装完了还得手动添加变量，怪麻烦的。  
  安装完了重启电脑  
## 图床使用
- 将刚才配置的py脚本文件复制到需要的位置，例如我把我的脚本文件`r2.py`放在我hugo网站主目录`D:\hugo\buray`下，在此目录下打开`CMD`，输入`python r2.py`回车运行。  
- 会提示`ModuleNotFoundError`，就是模块缺失提示，使用pip安装提示缺失的库即可，具体的命令为`pip install xxxx`。  
  例如第一个缺失模块提示一般是`ModuleNotFoundError: No module named 'keyboard'`，那么就接着输入`pip install keyboard`进行安装，安装完成后再运行`python r2.py`，会提示第二个缺失模块，依次安装即可。  
  需要注意的是，期间会有一个提示`PIL`模块缺失，这个模块直接`pip install PIL`是安装不了的，因为`PIL`是一个简写，指的是`基于Python的图像处理库`，库名应该是`pillow`，安装这个库的时候使用`pip install pillow`，其他的缺失库都按照提示安装就行。  
- 全部安装后运行`python r2.py`会有如下提示：  
  ![](https://img.heisenw.com/blog/img/39e45501-46ee-4c19-90bc-6e91469e3354.webp)  
- 使用的时候也很简单，确保py脚本运行状态后，再md文件中使用`Ctrl+Alt+V`快捷键，图片就会自动上传到图床中，同时自动在你的md文件光标处输入图片链接：  
  ![](https://burayimg.heisenw.com/blog/img/c232914b-a194-4c6e-bdf7-9d5f202a854b.webp)  
  ![](https://burayimg.heisenw.com/blog/img/288eacef-b1d3-4a36-94de-86d40726bf1d.webp)  
  这样就算大功告成了。  

本文主要学习二叉树树大神的[Hugo博客搭建教程以及配置调优](https://blog.2b2x.cn/posts/hugo/)

---

> 作者: [Buray](http://www.buray.top)  
> URL: http://www.buray.top/posts/cloudflare-r2/  

