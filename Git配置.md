安装Git (Ubuntu Linux)

$ sudo apt install git 

设置邮箱用户名

$ git config --global user.name "Your name"

$ git config --global user.email "email@example.com"

创建本地仓库

1.

$ mkdir learngit #创建空目录

$ cd learngit #进入空目录

$ pwd #显示当前目录

/Users/your name/learngit

2.

$ git init #把这个目录变成Git可以管理的仓库

Initialized empty Git repository in /Users/your name/learngit.git/

创建SSH Key

$ ssh-keygen -t rsa -C "youremail@example.com"

输完后一直回车，使用默认值，然后在主目录中找到.ssh文件夹，（由于.ssh为隐藏文件夹，此时可以Ctrl+H显示隐藏文件夹寻找），打开后有id_rsa（私钥）和id_rsa.pub（公钥）。

远程仓库与本地仓库的连接

1.登录GitHub，在设置中找到SSH，打开后点击“ADD SSH Key”，填任意title，在Key文本框里粘贴id_rsa.pub里的公钥，最后点击Add Key。

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760440720723-0d3bdcb2-2506-4bc7-89e8-39c1c79186aa.png)

![](https://cdn.nlark.com/yuque/0/2025/png/61414303/1760440737111-5da8e26d-464a-458a-99a2-bf4f40bc3c35.png)

完成后

![](https://cdn.nlark.com/yuque/0/2025/jpeg/61414303/1760440591153-9b4b8b76-3b86-4091-8d1e-faa027797402.jpeg)

2.创建一个新仓库

3.打开终端，使用cd进入learngit仓库，输入

$ git remote add origin git@github.com:your name/远程仓库名.git

$ git push -u origin master

检查本地仓库是否连接远程仓库

![](https://cdn.nlark.com/yuque/0/2025/jpeg/61414303/1760440624218-5edee69f-932f-454e-a1b3-77c4fd3564b4.jpeg)

语雀链接https://www.yuque.com/u59714887/thp4hi/qgq3op2xh4ypagtu
