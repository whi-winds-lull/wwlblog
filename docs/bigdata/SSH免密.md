## SSH免密方法

```
ssh-keygen -t rsa (连续回车,即在本地生成了公钥和私钥,不设置密码)

直接登录到服务器使用命令cat ~/.ssh/id_rsa.pub查看内容(公钥)，将内容复制 追加到主机B、C的文件~/.ssh/authorized_keys中（ 请注意不要删除或覆盖该文件中已有的内容）
```

注意事项：

```
报错：
RSA host key for [ip] has changed and you have requested strict checking.
Host key verification failed.

Warning: the ECDSA host key for 'datalab' differs from the key for the IP address '192.168.0.40'

解决办法是在主机A运行：
ssh-keygen -R BHostIP
原因是：
比如主机A和主机B，用户之前在主机A上使用ssh命令登录过主机B，而后主机B被重装但保留了主机B的IP。之后用户在主机A上再ssh继续登录主机B时，就会报这个错误。
```

