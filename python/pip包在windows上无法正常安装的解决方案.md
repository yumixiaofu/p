# pip包在windows上无法正常安装的解决方案
解决 ERROR: Could not find a version that satisfies the requirement xxx 的问题
我们经常通过pip安装东西时常常会出现ERROR: Could not find a version that satisfies the requirement xxx的问题。该问题常常会误导我们认为是下载的安装包之间存在冲突，因而花费大量的时间去配置各种各样的环境。
其实出现这个问题的原因是python国内网络不稳定，直接导致报错。因此我们常用镜像源来解决此问题。如下

pip install 包名 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com


# 正确姿势

pip install 包名 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

# 参考地址

https://blog.csdn.net/qq_45603919/article/details/108906920
