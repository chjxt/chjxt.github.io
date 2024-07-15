### 一、银河麒麟SP1 升级
下载所需deb文件，进入文件夹执行`sudo   dpkg   -i   *.deb`
deb 文件链接 :alien: [银河麒麟v10_SP1-openssh升级文件](https://www.123pan.com/s/aV6VVv-L8ZHd.html)

### 二、麒麟信安V3.3升级

所需文件 :file_folder: [麒麟信安3.3-OPEN_SSH升级文件](https://www.123pan.com/s/aV6VVv-j8ZHd.html)

> :exclamation: 按照顺序安装

1. 安装 zlib

```
tar zxvf zlib-1.2.13.tar.gz
cd zlib-1.2.13
./configure --prefix=/usr/local/zlib
make && make install
```

2. 安装 openssl

```
tar zxvf openssl-1.1.1t.tar.gz
cd openssl-1.1.1t
./config --prefix=/usr/local/ssl -d shared
ldconfig
make && make install
echo '/usr/local/ssl/lib' >> /etc/ld.so.conf
```
3. 安装 openssh
```
yum -y remove openssh
tar zxvf openssh-9.5p1.tar.gz
cd openssh-9.5p1	
./configure --prefix=/usr/local/openssh --with-zlib=/usr/local/zlib --with-ssl-dir=/usr/local/ssl --without-openssl-header-check
make && make install
```
4. 配置
```
echo 'PermitRootLogin yes' >> /usr/local/openssh/etc/sshd_config
echo 'PubkeyAuthentication yes' >> /usr/local/openssh/etc/sshd_config
echo 'PasswordAuthentication yes' >> /usr/local/openssh/etc/sshd_config
cd contrib/redhat/
cp sshd.init  /etc/init.d/sshd
chkconfig --add sshd
cp /usr/local/openssh/etc/sshd_config /etc/ssh/sshd_config 
cp /usr/local/openssh/sbin/sshd /usr/sbin/sshd
cp /usr/local/openssh/bin/ssh /usr/bin/ssh
cp /usr/local/openssh/bin/ssh-keygen /usr/bin/ssh-keygen
cp /usr/local/openssh/etc/ssh_host_ecdsa_key.pub /etc/ssh/ssh_host_ecdsa_key.pub
systemctl start sshd.service
chkconfig --add sshd
chkconfig sshd on

```