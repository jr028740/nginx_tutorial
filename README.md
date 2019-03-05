#在windows server操作系统上使用nginx进行前端网页部署  
>部署前端页面需要很多，常用的有apache(httpd)、nginx、微软的IIS等。其中nginx具有支持热部署、能承载多并发流量、兼容主流操作系统等优点，因而本人使用nginx居多，此教程使用nginx进行部署。  

###1.安装nginx  
>(1)浏览器打开 http://nginx.org/download/nginx-1.14.2.zip 链接下载当前稳定版本的nginx的压缩包，然后在任意目录解压缩（建议放在比较容易找到的目录，比如根目录、windows的下载文件夹(X:\Users\用户名\Download)等目录。  
(2)进入解压nginx的目录（可以看到绿色图标的nginx.exe的那个文件夹）下，按住shift（左右shift都可以），然后在弹出的菜单中点击“在此处打开命令窗口”，打开命令提示符窗口。  
(3)在弹出的窗口中，输入`start nginx`，然后按下回车键，启动nginx.  
(4)打开浏览器，访问http://127.0.0.1，如果出现"Welcome to nginx!"页面，说明nginx安装成功并能够正常启动，可以进行接下类的操作。  
(5)如果不能看到(4)中提及的页面，则可以在cmd窗口中输入`tasklist /fi "imagename eq nginx.exe`，然后按下回车键，检查nginx是否启动。  
###2.安装notepad++  
>由于nginx的配置需要修改其中的配置文件，建议下载notepad++ [64位链接](https://notepad-plus-plus.org/repository/7.x/7.6.3/npp.7.6.3.Installer.x64.exe) [32位链接](https://notepad-plus-plus.org/repository/7.x/7.6.3/npp.7.6.3.Installer.exe)（32位64位二选一，如果是64位操作系统直接安装64位）。点击安装包按照引导进行安装。  
###3.部署网页文件  
>(1)在任意目录新建一个`html`文件夹（建议放在比较容易找到的目录，比如`D:\html`）。  
(2)将打包好的前端页面解压缩，将解压后文件夹内的所有内容拷贝到上述html目录中，html文件夹中必须看到js或者html文件才能在首页中打开该网页。  
(3)（可选，根据需要）如果需要另外放入其它前端页面，可以在上述`html`文件夹下再新建一个文件夹，取一个名字作为访问的目录（例如`tianyuan`），然后重复步骤(2)将解压的页面文件拷贝进该目录中。  
###4.配置nginx  
>(1)首先访问解压nginx目录下的conf文件夹，然后使用notepad++打开`nginx.conf`文件（修改文件前建议备份一份该文件并重命名位`nginx.conf.bak`以备不时之需），可以看到该文件基本上由一个个以大括号围成的字符块组成。  
(2)将http块内的所有server块（包括内部的子块）中没有在前方加上`#`号的行之前加上`#`号，将这些行注释。注意，只能一行一行注释，不能批量注释。  
(3)在http字符块的上方任意一空行内插入一行`include 你解压nginx的目录\conf.d\*.conf;`然后保存`nginx.conf`文件并关闭notepad++.  
(4)回到解压nginx目录下，新建一个文件夹，文件夹名位`conf.d`，然后进入该文件夹，新建一个文本文档，取名位`default.conf`（名字随意，只要文件类型是.conf即可，注意开启windows中的扩展名显示）。  
(5)打开default.conf，将下面的字符块全部拷贝到文件中。该字符块由学校服务器直接拷贝得到，用中文注释的行请根据中文注释进行修改，最后保存。确认可以正常访问后可以将这些中文注释删除。  
```
server {
    listen       9004;#监听的端口，修改为想要的端口，默认http端口为80
    server_name  _;#如果使用域名访问，将下划线改成相应的域名
	charset utf-8;

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        root   D:/html;#所配置的html目录的路径，根据实际情况修改
        index  login.html;#南曼前端的首页为login.html,其它项目按需进行修改
    }

	  #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }

}
```
###5.重启nginx  
>(1)步骤1-4完成后，回到解压nginx的目录，重复1中(2)的步骤，打开命令行，输入`nginx -t`然后按下回车，检查配置文件的语法是否正常，当提示下列信息时：  
```
nginx: the configuration file nginx的解压目录/conf/nginx.conf syntax is ok
nginx: configuration file nginx的解压目录/conf/nginx.conf test is successful
```
>说明nginx配置文件没有语法错误，可以重新启动nginx使更改生效。  
(2)输入`nginx -s reload`然后按下回车，重启nginx.如果没有任何信息显示，说明重启成功，此时可以关闭该命令窗口，打开浏览器，输入相应连接进行测试。  
###6.注意事项  
>(1)若在检查配置文件语法时，返回报错信息，请根据相应信息定位相应文件的相应行进行语法检查。  
(2)若配置文件语法通过，但重启nginx后依然不能正常访问网页，请尝试打开解压nginx目录下的logs文件夹，然后打开里面的`error.log`日志文件，定位相应的时间查看相应报错信息，然后进行排查。  
(3)最常见的错误是文件权限问题。如果遇到该错误，请将对应文件赋予管理员读取权限，修改文件权限的方法可以看 https://jingyan.baidu.com/article/64d05a020005b5de55f73bbf.html  
