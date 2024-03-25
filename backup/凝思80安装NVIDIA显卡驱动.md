```
将驱动文件拷贝至桌面，设置执行权限chmod +x NVIDIA-XXX.run
vi /etc/modprobe.d/blacklist.conf，
填入blacklist nouveau，
然后保存退出
执行update-initramfs  -u，
然后reboot
开机选择高级选项，
进入recovery模式，
这时候输入root密码，
显卡驱动安装
cd Desktop/
./NVIDIA-XXX.run
一路回车
出现complete字样即安装完成，输入reboot重启系统
输入命令查看是否正确安装
nvidia-smi -a
cat /proc/driver/nvidia/version
```
