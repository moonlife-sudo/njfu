# njfu
njfu图书馆抢座
1.连接云服务器
2.安装最新版的谷歌浏览器 
yum -y install google-chrome-stable –nogpgcheck

3.查看版本：google-chrome –version

4.安装对应的驱动：https://googlechromelabs.github.io/chrome-for-testing/#stable

5.将下载好的驱动解压 unzip chrome-headless-shell-linux64.zip

6.移动驱动到系统目录：sudo mv chrome-headless-shell-linux64/chrome-headless-shell /usr/bin/

7.给文件赋予权限：sudo chmod +x /usr/bin/chrome-headless-shell

8.阿里云和宝塔面板开放flask项目端口，通常是5000

9.数据库修改访问权限，改为所有人可访问

10.文件上传到宝塔面板root根目录里面，同时附上依赖项

11.安装对应的python版本

12.运行方式选择gunicom(多线程),网络协议选择wsgi
