### 故障原因：
openssh漏洞，进而进行升级，升级后产生此问题；无法进去桌面系统，可以远程ssh进行登录，也可以Ctrl+Alt+F2进入命令行系统。
### 故障排查：
1、普通用户输入ssh -V报错
ssh: error while loading shared libraries: libcrypto.so.3: cannot open shared obiect file: No such file or directory
进入root用户输入ssh -V正常
0penSSH 9.8p1,0penSSl3.2.1 30 Jan 2024
2、百度搜索关键词“银河麒麟循环登录”，查看结果有的是说分区内存占用过大，df -h查看均正常，有的让删除用户目录下的.Xauthority文件，测试过后问题依旧；相关链接如下：
https://kb.kylinos.cn/zsk/view/faq/115
3、查看/var/log/auth.log内容，查看日志发现有PAM文件无法找到的日志内容
![图片1](https://github.com/user-attachments/assets/9ec2eec3-2b32-43fd-88b8-f789c3db223c)
此时，感觉是这个问题导致的。通过安装同一系统进行测试试图还原故障，发现故障并不能复现，在测试机上查看/var/log/auth.log日志也有同样的PAM内容，那这个内容并不是导致问题产生的原因。
4、查看用户目录下的.xsession-errors文件，此文件会记录登录过程中产生的日志。
![图片2](https://github.com/user-attachments/assets/a1398872-e481-49fe-ad1b-d12ab9267519)
此文件内容最下面有一行：user/bin/ssh-agent error while loading shared libraries：libcrypto.so.3 cannot open shared object file:No such file or directory
这个可能就是问题产生的原因所在，也是普通用户输入ssh -V报错的原因
### 故障复现：在测试机
1、查找所缺少的库文件位置find / -name libcrypto.so.3
![图片3](https://github.com/user-attachments/assets/6916635e-4e4f-4f7a-877d-6059ca54a40b)
2、查看user/bin/ssh-agent 依赖关系
![图片4](https://github.com/user-attachments/assets/8b2a06d1-cf02-44aa-ba09-4b1e0ae460e2)
3、情况逐渐明了，让libcrypto.so.3指向的库文件不是当前的内容，那是不是就可以进行故障复现，安装openssl的时候配置的文件是在/etc/ld.so.conf.d/openssl.conf
![图片5](https://github.com/user-attachments/assets/d4a32c88-b9cc-4a3b-9c34-e127c80d54c0)
我们需要打开此文件查看内容
![图片6](https://github.com/user-attachments/assets/7c747226-0a95-43d7-a7a4-4a4dac0fcae5)
把内容删除后执行ldconfig 使之生效。问题复现
![图片7](https://github.com/user-attachments/assets/dfe4664a-f568-4797-a01e-f0cb53947728)
注销后，图形界面输入密码会循环登录
此时执行ldd /usr/bin/ssh-agent
![图片8](https://github.com/user-attachments/assets/052b01bf-cf33-4ad3-80e7-5abf91c85942)
出现libcrypto.so.3 => not found
### 结论：
在故障电脑执行ldd /usr/bin/ssh-agent查看结果是否有libcrypto.so.3 => not found内容。
编辑 /etc/ld.so.conf.d/openssl.conf文件增加一条
/usr/local/openssl/lib64
执行ldconfig
问题解决。



