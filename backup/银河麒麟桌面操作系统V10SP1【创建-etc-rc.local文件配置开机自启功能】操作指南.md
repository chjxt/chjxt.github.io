
可参考链接
https://kb.kylinos.cn/view/helpdoc/818

### 简介
/etc/rc.local文件多数应用于指定开机启动的进程，而银河麒麟桌面操作系统V10SP1中默认没有/etc/rc.local开机自启文件。本文主要介绍了在银河麒麟桌面操作系统V10SP1上创建一个/etc/rc.local文件实现开机自启的操作方法。

### 正文

目录

1、切换到 root 权限
2、备份 /lib/systemd/system/rc-local.service文件
3、编辑 /lib/systemd/system/rc-local.service文件
4、创建/etc/ rc.local 文件，并配置脚本
5、修改/etc/rc.local文件的权限
6、加载rc-local服务并设置该服务开机自启
7、设置/lib/systemd/system/rc-local.service链接到/etc/systemd/system 文件下
8、验证测试
9、问题解决


银河麒麟桌面操作系统V10SP1开机进入系统后，在桌面空白处鼠标右键，点击“打开终端”，再在终端执行以下操作：
1、切换到 root 权限
sudo –i

2、备份 /lib/systemd/system/rc-local.service文件
cp  /lib/systemd/system/rc-local.service  /lib/systemd/system/rc-local.service.bak

3、编辑 /lib/systemd/system/rc-local.service文件
vim  /lib/systemd/system/rc-local.service命令进入到文件里面后，按“i”键进入文本编辑模式，在该文件末尾添加以下内容：
[Install]
WantedBy=multi-user.target
Alias=rc-local.service
添加完成后，按“Esc”键退出文本编辑模式，再输入“:wq”保存退出。

4、创建/etc/ rc.local 文件，并配置脚本
touch /etc/rc.local
vim /etc/rc.local进入到文件里面后，按“i”键进入文本编辑模式，在该文件里添加以下内容：
#!/bin/sh -e
exit 0
添加完成后，按“Esc”键退出文本编辑模式，再输入“:wq”保存退出。

【注意】
1)如果/etc/rc.local文件里不添加“#!/bin/sh -e”和“exit 0”这两个参数，则rc.local服务会启动不了并出现报错，甚至可能开不了机。
2)如果需要在/etc/rc.local文件中添加脚本，则在该文件“exit 0”参数前一行添加。
5、修改/etc/rc.local文件的权限

以上步骤完成后，需要修改/etc/rc.local配置文件的权限为755：

chmod 755 /etc/rc.local   

如果需要修改/etc/rc.local文件，需要给该文件添加可执行权限：        
chmod +x /etc/rc.local             
6、加载rc-local服务并设置该服务开机自启

systemctl daemon-reload 

systemctl start rc-local.service

systemctl enable rc-local.service
【注意】在启动rc-local服务时，若出现以下弹窗提示，点击该弹窗提示中“允许”按钮即可。

7、设置/lib/systemd/system/rc-local.service链接到/etc/systemd/system 文件下
ln -s /lib/systemd/system/rc-local.service /etc/systemd/system

8、验证测试
vim /etc/rc.local，进入该文件后，按“i”键进入文本编辑模式，在该文件“exit 0”前面添加一行“echo  "1111111111"  > /bb.txt”参数，然后按“Esc”键退出文本编辑模式，再输入“:wq”保存退出，最后重启系统使其生效。


重启系统后，打开终端，输入systemctl status rc-local.service查看rc-local服务状态是否正常。


rc-local服务的状态正常后，输入sudo -i命令切换到root权限，再输入ls /命令查看根目录下是否存在一个名为bb的txt文件，若存在，则输入cat /bb.txt命令查看该文件里面的内容是否为1111111111，若是，则与测试脚本一样的结果，说明配置的开机自启脚本成功。

9、问题解决
重启系统后，在终端执行systemctl status rc-local.service命令查看到rc-local服务未启动，


此时，在终端执行systemctl restart rc-local.service命令手动重启rc-local服务后，恢复正常。原因是因为系统开启了麒麟安全管控模块（kysec）。
手动启动rc-local服务时会弹出“麒麟安全授权认证”的提示，点击该弹窗提示中“允许”按钮后，若不生效，则可以在在安全中心里添加/etc/rc.local文件列表或者直接关闭应用程序执行控制使其永久生效即可。

在安全中心里添加/etc/rc.local文件列表，操作步骤如下：
步骤一：点击“开始菜单->设置->安全与更新->安全中心”，打开安全中心页面。


步骤二：在安全中心页面，点击“应用控制与保护”，再点击“高级配置”。


步骤三：在弹出的“高级配置”窗口，点击“添加”按钮，选择rc.local文件所在的目录，即“/etc”，再在文件名称处输入“rc.local”，然后点击“打开”即可。

关闭应用程序执行控制，操作步骤如下：
步骤一：点击“开始菜单->设置->安全与更新->安全中心”，打开安全中心页面。


步骤二：在安全中心页面，点击“应用控制与保护”，在该页面应用程序执行控制处，选择“关闭”后，重启系统生效即可。


