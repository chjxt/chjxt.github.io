### 安装
1.访问如下网址，根据系统的包管理器（deb/rpm）和CPU架构（amd64/arm64）进入对应的路径下，分别下载containerd.io、docker-ce-cli和docker-ce三个包，尽量选择新版本：

[https://download.docker.com/linux/debian/dists/stretch/pool/stable](https://download.docker.com/linux/debian/dists/stretch/pool/stable)
2. 安装下载的docker包：dpkg –i  [docker包名]
3. 安装好后启动docker服务并设置开机自启动：
`systemctl start docker`
`systemctl enable docker`
4. 查看docker是否安装成功：docker –v

### 卸载

1. 查看系统已安装的docker包：dpkg -l | grep docker
2. 卸载已有的docker包：dpkg –P [docker包名]     //[]不是参数，不需要输入
3. 查看docker是否卸载成功：docker –v 报错代表卸载成功
