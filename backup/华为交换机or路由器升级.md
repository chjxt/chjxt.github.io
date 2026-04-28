1. 首先下载S7700-V200R008C00SPC500.rar升级包，包括升级文件 和升级指南，TFTP用的升级文件传输工具3CDaemon。

2. 解压S7700-V200R008C00SPC500压缩包。

3. 配置电脑和S7706互通配置：

```
  PC：ip add:10.33.33.2/255.255.255.252
  sys
  vlan 233
  int vlan 233
    ip add 10.33.33.1 255.255.255.252
  int gig3/0/30
    port link-type access
    port default vlan 33
```
  配置和电脑相ping互通。

4. 解压3CDaemon，并配置好TFTP Server。

   设置TFTP服务器-->TFTP设置-->上传/下载目录：
   C:\Users\vince\Downloads\S7700-V200R008C00SPC500
   点击右下角的下拉按钮，    点击“TFTP服务器已经启动（点击这里停止服务 
    ）”，再次点击启动TFTP服务器。

5. TFTP下载升级文件到交换机

   到交换机上操作：
   ```
   <huawei>tftp 10.33.33.2 get S7700-V200R008C00SPC500.cc
   <huawei>tftp 10.33.33.2 get S7700-V200R008SPH012.pat
   <huawei>tftp 10.33.33.2 get S7700-V200R008C00SPC500.006.web
   ```

6. 复制升级文件到备用主控板上

 ```
   <huawei>copy S7700-V200R008C00SPC500.cc slave#cfcard:/
   <huawei>copy S7700-V200R008SPH012.pat slave#cfcard:/
   <huawei>copy S7700-V200R008C00SPC500.006.web slave#cfcard:/
```

7. 设置系统软件和补丁软件

```
   startup system-software S7700-V200R008C00SPC500.cc
   startup system-software S7700-V200R008C00SPC500.cc slave-board   
   startup patch S7700-V200R008SPH012.pat
   startup patch S7700-V200R008SPH012.pat slave-board
   display startup  //查看下次启动所用的系统软件和补丁软件是否为新上传的系统和补丁软件。
```

8. 检查一下文件的CRC是否正确

 ```
   check startup crc next
   check startup next crc
   
> 如果都为OK，说明升级正常，否则整个升级的文件是有问题的。

```

9. reboot  //重启

10. 检查单板和配置是否注册正常

   display device  //如果为Registered和Normal则为正常，否则单板存在问题或配置有问题。