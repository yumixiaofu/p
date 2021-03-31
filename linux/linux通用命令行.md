# linux curl 将查询结果输入到文件

例子：curl -o result.txt http://ip:port/xxxxxx?access_token=xxxx\&DQZT=xxxx\&page=7\&per_page=1000

说明：
1、-o result.txt 及为将查询结果输入到文件result.txt中；

2、多参数拼接&前面需要增加\。因此拼接参数时增加 \&
