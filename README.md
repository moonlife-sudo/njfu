# njfu
njfu图书馆抢座系统
网上关于如何在云服务器上配置谷歌浏览器环境来实现selenium框架的步骤非常杂乱，不同操作系统的指令和方式都不同，
下面我以我个人的服务器系统Alibaba Cloud 3 (Soaring Falcon) x86_64(linux)系统为例，来说明一下详细步骤
1.首先登陆阿里云控制台，远程连接云服务器
2.安装最新版的谷歌浏览器（不同系统指令不同，该指令默认下载的是最新版本的浏览器，若是想下载旧版本的谷歌浏览器，
需要自行准备好下载连接，然后在云服务上进行解压） 
yum -y install google-chrome-stable –nogpgcheck

3.查看版本：google-chrome –version
（我的版本显示是120.0.6099.216)
4.找到与谷歌版本对应的驱动，这里要注意的是，最新版的谷歌浏览器驱动改成了二进制文件，
下载网址：https://googlechromelabs.github.io/chrome-for-testing/#stable
这里面会提供三种类型，chrome、chromedriver、chrome-headless-shell，
由于项目部署在云服务器上是没有图形化接口的，所以我们要采用第三种无头模式的浏览器驱动chrome-headless-shell，
复制下载链接到云服务器上
指令： wget https://edgedl.me.gvt1.com/edgedl/chrome/chrome-for-testing/120.0.6099.109/linux64/chrome-headless-shell-linux64.zip


5.将下载好的驱动解压 unzip chrome-headless-shell-linux64.zip

6.移动驱动到系统目录：sudo mv chrome-headless-shell-linux64/chrome-headless-shell /usr/bin/

7.给文件赋予权限：sudo chmod +x /usr/bin/chrome-headless-shell

8.阿里云和宝塔面板开放flask项目端口，通常是5000

9.数据库修改访问权限，改为所有人可访问

10.文件上传到宝塔面板root根目录里面，同时附上依赖项

11.安装对应的python版本

12.运行方式选择gunicom(多线程),网络协议选择wsgi
