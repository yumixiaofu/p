
# google地图标准格式
```
<?xml version="1.0" encoding="UTF-8"?><urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">

<url><loc>
http:///www.baidu.com/
</loc></url>

</urlset>
```




# 核心代码部分


```
# -!- coding: UTF-8 -!-

import os

domain='填写主域名'
htmlsrc='网站所有html所在的目录'
wjmc='googlexml' #填写生成xml地图文件前面部分
bcsrc='xml/' #填写xml地图保存在哪个目录
l=200#'填写每个地图最多多少个url'
k="""<?xml version="1.0" encoding="UTF-8"?><urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">""" #开始的代码
j="""</urlset>"""#结束的标签
liziz="<url><loc>" #中间的例子
liziy="</loc></url>"

def find_dir(dir_name):
    file_list = []
    for path_name, dir, files_name in os.walk(dir_name):
        file_list.append(path_name.replace("\\","/"))
        for file in files_name:
            clh=str(os.path.join(path_name, file)).replace("\\","/")

            file_list.append(clh)
    return file_list
file_list = find_dir("../mulu/mulu/")#这里获取到了所有url

ll=0
neirong=''
urljia=''
fenye=int(len(file_list)/l)+1 #取得整数页数+1，这里需要多考虑一下！
yeshu=1 #这个是标记页数每次加1
for i in file_list:
    urljia+=liziz+domain+str(i)+liziy
    if(ll>l or yeshu==fenye):#这里是判断条件满足生成xml地图并且重置参数！
        neirong=k+urljia+j#生成200个url的地图写入xml中
        shuzizhuanstr=str(yeshu)
        with open(bcsrc+wjmc+shuzizhuanstr+".xml", "w", encoding='utf-8') as xmlxr:
            xmlxr.write(neirong);
        xmlxr.close()
        neirong=''
        urljia=''
        ll=0
        yeshu=int(yeshu)
        yeshu+=1
    ll+=1




```
