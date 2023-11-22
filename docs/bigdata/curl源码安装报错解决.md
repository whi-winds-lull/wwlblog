### curl源码安装报错解决方案

#### 背景

服务器版本：CentOS Linux release 8.1.1911 (Core) 

使用curl报错，并且源码安装报错，经查询和openssl版本有关，以下是解决办法。

#### curl 7.61

```shell
# 先卸载curl
rpm -q curl
rpm -e --nodeps curl-7.61.1-22.el8.x86_64

# 下载curl 7.61tar包,解压
tar -zxvf curl-7.61.1.tar.gz
cd curl-7.61.1

# 下载openssl 旧版本openssl-1.0.2k.tar.gz
# 解压编译，位置放在了/tmp/ssl
./config shared --prefix=/tmp/ssl
make
make install

# 编译curl,一定要加--with-ssl指定 openssl-1.0.2k的位置
./configure --prefix=/usr/local  --with-ssl=/tmp/ssl
make
make install

# 查看是否安装成功
curl --version
```





