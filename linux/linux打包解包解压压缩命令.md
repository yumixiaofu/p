```
命令	英文释义	释义
-c/-x	create/extract	打包/解包
-v	verbose	显示打包/解包的详细过程
-f	file	表示file
```
```

.tar
　　解包：tar xvf FileName.tar
　　打包：tar cvf FileName.tar DirName
　　（注：tar是打包，不是压缩！）
　　———————————————
.gz
　　解压1：gunzip FileName.gz
　　解压2：gzip -d FileName.gz
　　压缩：gzip FileName
.tar.gz 和 .tgz
　　解压：tar zxvf FileName.tar.gz
　　压缩：tar zcvf FileName.tar.gz DirName
　　———————————————
.bz2
　　解压1：bzip2 -d FileName.bz2
　　解压2：bunzip2 FileName.bz2
　　压缩： bzip2 -z FileName
.tar.bz2
　　解压：tar jxvf FileName.tar.bz2 或tar –bzip xvf FileName.tar.bz2
　　压缩：tar jcvf FileName.tar.bz2 DirName
　　———————————————
.bz
　　解压1：bzip2 -d FileName.bz
　　解压2：bunzip2 FileName.bz
　　压缩：未知
.tar.bz
　　解压：tar jxvf FileName.tar.bz
　　压缩：未知
　　———————————————
.Z
　　解压：uncompress FileName.Z
　　压缩：compress FileName
.tar.Z
　　解压：tar Zxvf FileName.tar.Z
　　压缩：tar Zcvf FileName.tar.Z DirName
　　———————————————
.zip
　　解压：unzip FileName.zip
　　压缩：zip FileName.zip DirName
　　压缩一个目录使用 -r 参数，-r 递归。例： $ zip -r FileName.zip DirName
　　———————————————
.rar
　　解压：rar x FileName.rar
　　压缩：rar a FileName.rar DirName

看看你要解压的文件的具体路径
tar -ztf xx.tar.gz | grep file_you_want_to_get 
假设为 path/to/file
tar -zxf xx.tar.gz path/to/file
```
