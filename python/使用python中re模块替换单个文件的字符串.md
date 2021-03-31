# 使用python中re模块替换单个文件的字符串
```
import re

#直接替换当前文件中的字符
#分析：先以只读模式打开后，对文件每一行进行readlines()操作，并保存到新的列表中。然后随之关闭。 再以'w+'方式进行读写打开，对已经保存的列表用re.sub()进行替换操作，并用f.writelines()函数写入。

wjlj='要替换的文件路径'
thwz='要替换的文字'
thc='要替换成什么'

f=open(wjlj,'r')
alllines=f.readlines()
f.close()
f=open(wjlj,'w+')
for eachline in alllines:
    a=re.sub(thwz,thwz,eachline)
    f.writelines(a)
f.close()


```

