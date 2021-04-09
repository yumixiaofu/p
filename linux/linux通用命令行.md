# linux curl 将查询结果输入到文件

例子：curl -o result.txt http://ip:port/xxxxxx?access_token=xxxx\&DQZT=xxxx\&page=7\&per_page=1000

说明：
1、-o result.txt 及为将查询结果输入到文件result.txt中；

2、多参数拼接&前面需要增加\。因此拼接参数时增加 \&


# scp命令 远程拷贝服务器的目录
scp -r root@185.245.1.23:/d /www/p/xs

格式解说：
scp -r 固定的，root@是用户名，后面跟上ip地址然后：冒号跟上文件夹绝对路径，然后空格，跟上复制到本地服务器的哪个文件夹的绝对路径！

