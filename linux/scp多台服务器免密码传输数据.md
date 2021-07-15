# scp多台服务器免密码传输数据的配置
A服务器地址：192.168.1.200，下面简称A
B服务器地址：192.168.1.201，下面简称B
# 1.首先在A服务器里面输入下面命令，输入完成之后按回车
```
ssh-keygen -t rsa -P ""
```
执行上述命令，一路回车，会在当前登录用户的root目录下的.ssh目录下生成id_rsa和id_rsa.pub两个文件，分别代表密钥对的私钥和公钥

# 2.拷贝A服务器root目录下.ssh目录下的id_rsa.pub内容到B服务器的root目录下.ssh目录下的authorized_keys文件中

# 3.使用方法（在a服务器上面向b服务器拷贝文件）

scp 带绝对路径的文件或者文件夹 root@B服务器ip:这里是B服务器接收文件或者文件夹的路径

例子：scp /www/wwwroot/1.3.2.4/index.html root@4.54.65.23:/www/wwwroot/4.54.65.23


