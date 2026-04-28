一、背景与前提

1.1 背景

客户环境局限性：

1、禁止使用U盘外设接入；

2、终端为国产化信创和Kylin麒麟系统，安装软件。

3、从管理平台下发客户软件（每个200M以上），下载缓慢，安装一台需要1-2个小时。

于是提出，从客户终端之间进行软件的传输的方法，国产化信创和麒麟系统内核为Linux系统，一般有集合python软件。

1.2 前提

1、保证终端之间的连通性，确认可互相ping通；

2、关闭防火墙或放通对应http的端口。

二、 配置

2.1 Linux

2.1.1 查询python版本

linux软件自带python命令可直接执行命令。若未带有Python命令则可以通过curl命令下载对应版本的python。

根据不同python版本执行不同的python语句:执行python -v查看版本信息，通过exit()退出。                               

图2.1.1 python版本查询

2.1.2 命令执行

Python2:python -m SimpleHTTPServer 80

Python3:python -m http.server 80

注：端口可以自定义

若存在权限限制可尝试切换权限并在命令前加上sudo。

2.2 Windows

2.2.1 安装python软件

软件链接：https://www.python.org/downloads/release/python-3101/ 

2.2.2 更新环境变量

安装Python时需要勾选path的环境变量选项

2.2.3 完成安装

2.2.4 命令执行

登录cmd，进入要上传软件的路径。输入命令“python -m http.server 80” 

注：端口可以自定义

 验证

通过浏览器访问本地笔记本的地址（此处为[http://localhost:80](http://localhost/)）,选定需要的软件即可下载传输。