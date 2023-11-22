# [Linux系统安装高版本Python3

```
1. 进入https://www.python.org/downloads/source/下载相应版本
```

```cmd
# 安装依赖环境
yum -y install gcc

yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel openssl libuuid-devel

# 解压压缩包
tar xvf Python-3.7.3.tar.xz

# 编译
./configure --prefix=/usr/local/python3.7
 make && make install 
 
 # 旧的python3.6软连接
python3 -> /etc/alternatives/python3
pip3 -> /etc/alternatives/pip3
 # 删除旧的软连接
 rm -rf python3
 rm -rf pip3
 # 建立新的软链接
 ln -s /usr/local/python3.7/bin/python3 /usr/bin/python3
 ln -s /usr/local/python3.7/bin/pip3 /usr/bin/pip3
```

