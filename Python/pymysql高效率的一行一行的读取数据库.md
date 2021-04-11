# 代码部分
```
# -!- coding: UTF-8 -!-
import pymysql
duoshao = []
sql = "SELECT vod_pic,vod_name FROM `mac_vod`"

db = pymysql.connect(host="203.235.110.25",port=3306,user="xxx",passwd="CndizGxLmd3b4H6c",db="xxx",charset='utf8')
if db.open == True:
    try:
        #https://blog.csdn.net/qq_32806793/article/details/90759956  使用python连接mysql，并一行一行的读取数据表中的记录（适用于数据量比较庞大时）

        cursor = pymysql.cursors.SSCursor(db)
        cursor.execute(sql)
        num = 0
        while True:
            row = cursor.fetchone()
            if not row:
                break
            print(row)
            duoshao.append(row)
    except:
        db.rollback()
        db.close()
else:
    print("数据库连接失败！")

db.close()
print(len(duoshao))


```
# 注意事项
mysql连接的格式一定要正确！
