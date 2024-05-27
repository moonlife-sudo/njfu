# njfu
项目体验地址：xtygame.fun
联系方式：qq 1538188531
一、njfu图书馆抢座系统

网上关于如何在云服务器上配置谷歌浏览器环境来实现selenium框架的步骤非常杂乱，不同操作系统的指令和方式都不同，

下面我以我个人的服务器系统Alibaba Cloud 3 (Soaring Falcon) x86_64(linux)系统为例，来说明一下详细步骤

1.首先登陆阿里云控制台，远程连接云服务器

2.安装最新版的谷歌浏览器（不同系统指令不同，该指令默认下载的是最新版本的浏览器，若是想下载旧版本的谷歌浏览器，

需要自行准备好下载连接，然后在云服务上进行解压） 

yum -y install google-chrome-stable –nogpgcheck

（卸载谷歌浏览器指令：sudo yum remove google-chrome-stable
）

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

二、宝塔面板上python flask项目SSL证书配置

1.从阿里云或其他平台下载SSL证书，并将pem和key两个文件上传到宝塔面板对应项目目录里面

2.在app.py(main.py)里面添加

    if __name__ == '__main__':

    app.run(debug=True, host='0.0.0.0', port=443, ssl_context=('certificate.crt', 'private.key'))

其中'certificate.crt', 'private.key'是你对应pem和key文件的目录地址

3.在宝塔面板的配置文件中的server里面配置如下所示：

server

{

    listen 80;
    
    listen 443 ssl http2;
    
    server_name "你的域名";
    
    index index.html index.htm default.htm default.html;
    
    root /www/wwwroot/library;
    
    if ($server_port !~ 443){
    
        rewrite ^(/.*)$ https://$host$1 permanent;
        
    }
    
#将http访问切换到https访问

    ssl_certificate    /www/wwwroot/library/xtygame.fun.pem;
    
#pem和key的目录地址

    ssl_certificate_key   /www/wwwroot/library/xtygame.fun.key;
    
     ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
     
    ssl_ciphers EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
    
    ssl_prefer_server_ciphers on;
    
    ssl_session_cache shared:SSL:10m;
    
    ssl_session_timeout 10m;
    
    add_header Strict-Transport-Security "max-age=31536000";
    
    error_page 497  https://$host$request_uri;
    
#处理http请求重定向到https请求

}

4.重启项目即可

