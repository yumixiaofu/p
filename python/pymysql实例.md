# 傻瓜式笨蛋方法代码
```
# -!- coding: UTF-8 -!-
import pymysql

duoshao = []
update=[]
sql = "SELECT vod_name,vod_en,vod_id FROM `mac_vod`"
db = pymysql.connect(host="23.23.22.22", port=3306, user="fdsfdsf", passwd="fdsfsdfsdfsdf", db="fdsfdsf", charset='utf8')
if db.open == True:
    try:
        # https://blog.csdn.net/qq_32806793/article/details/90759956  使用python连接mysql，并一行一行的读取数据表中的记录（适用于数据量比较庞大时）
        cursor = pymysql.cursors.SSCursor(db)
        cursor.execute(sql)
        num = 0
        while True:
            row = cursor.fetchone()
            c = len(str(row[1]))
            if c == 0:
                duoshao.append(row[0])
                print(row[0])

                charujichu="update mac_vod set vod_en="+str(row[2])+ " where vod_id=" + str(row[2])
                update.append(str(charujichu))

            if not row:
                break

    except:
        db.rollback()
        db.close()
else:
    print("数据库连接失败！")

print(len(duoshao))

for i in update:
    db = pymysql.connect(host="23.23.22.22", port=3306, user="fdsfdsf", passwd="fdsfsdfsdfsdf", db="fdsfdsf", charset='utf8')
    if db.open == True:
        try:
            cursor = db.cursor()
            cursor.execute(i)
        except:
            db.rollback()
        db.close()
    else:
        print("数据库连接失败！")

```
# 对于入库的修改

## 原始代码
```
for i in update:
    db = pymysql.connect(host="23.23.22.22", port=3306, user="fdsfdsf", passwd="fdsfsdfsdfsdf", db="fdsfdsf", charset='utf8')
    if db.open == True:
        try:
            cursor = db.cursor()
            cursor.execute(i)
        except:
            db.rollback()
        db.close()
    else:
        print("数据库连接失败！")

```
## 优化后代码应该是这样！
```

db = pymysql.connect(host="23.23.22.22", port=3306, user="fdsfdsf", passwd="fdsfsdfsdfsdf", db="fdsfdsf", charset='utf8')
if db.open == True:
    try:
        cursor = db.cursor()
        for i in update:
          cursor.execute(i)
    except:
        db.rollback()
    db.close()
else:
    print("数据库连接失败！")

```
# 说明
以上代码在windows上面运行没有任何问题，linux没试过！
