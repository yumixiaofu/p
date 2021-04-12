# MD5算法例子
```
import hashlib
  ​
  m = hashlib.md5()        #实例化创建加密对象
  m.update(b"nicholas")    #传入待加密的字符串，注意要转换为bytes二进制形式，以下两种写法也是可以的
  # m.update("nicholas".encode("utf-8"))
  # m.update(bytes("nicholas",encoding="utf-8"))
  ​
  v1 = m.hexdigest()
  print(type(v1))
  print(v1)    #返回产生的十六进制的字符串类型数值
  #输出结果532ab4d2bbcc461398d494905db10c95
  """注意这里是十六进制的数,共32个数，由于一位十六进制的数要用4位二进制的数来表示
  这里是产生了32*4=128位二进制数，所以我们经常听到MD5是128位的校验，这里的128位是代表128位的二进制数
  """
  ​
  v2 = m.digest()
  print(v2)          #返回二进制形式的数值,可以转换为十六进制形式，与hexdigest结果一致
  print(type(v2))
  import binascii   #这里为了方便，直接在这里导入binascii ，将二进制转为十六进制
  print(binascii.b2a_hex(v2))  #b'532ab4d2bbcc461398d494905db10c95'
```
## 输出结果
```
<class 'str'>
532ab4d2bbcc461398d494905db10c95
b'S*\xb4\xd2\xbb\xccF\x13\x98\xd4\x94\x90]\xb1\x0c\x95'
<class 'bytes'>
b'532ab4d2bbcc461398d494905db10c95'
```
## 分析

可以看到digest()和hexdigest()产生的数值其实是一样的，只不过一个是作为二进制数据字符串值（bytes类型），一个是十六进制数据字符串值（字符串类型）。


# SHA算法例子

```
import hashlib
​
m1 = hashlib.sha1()        #实例化创建加密对象
m1.update(b"nicholas")    #传入待加密的字符串
m1.update(b"123")
print(m1.hexdigest())    
​
m2 = hashlib.sha1()
m2.update(b"nicholas123")
print(m2.hexdigest())   
```
## 输出结果
ef3f18a2b33e1f5366c25161a4869707503225f2

ef3f18a2b33e1f5366c25161a4869707503225f2
## 分析
可以看到，SHA算法的用法和MD5用法类似，也有分批次传入和一次性传入结果一致的特性。


# 加盐例子
上述各种算法比明文存储密码确实要安全不少。但在有些场景中，用户通常会将密码设置的尤为简单。这样如果数据库泄露，黑客可以通过简单的密码尝试来完成对加密字串的匹配。即：通过撞库可以反解。为了解决这种方法，我们通常需要对密码做“加盐”处理，即有必要对加密算法中添加自定义key再来做加密。
所谓加盐就是在m = hashlib.md5()这里设置参数，如果没有参数，所以md5遵守一个规则，生成同一个对应关系，如果加了参数，就是在原先加密的基础上再加密一层，这样的话参数只有自己知道，防止被撞库，因为别人永远拿不到这个参数。
```
import hashlib
 ​
 m1 = hashlib.md5()        #md5对象，md5不能反解，但是加密是固定的，就是关系是一一对应，所以有缺陷，可以被对撞出来
 m1.update(b"nicholas")    #传入待加密的字符串
 print(m1.hexdigest())
 # 输出结果  532ab4d2bbcc461398d494905db10c95
 """这个结果可以别人也可以直接对简单字符串的进行MD5取值生成一个数据库，拿数据库中的MD5值与这个MD5值进行比对，
 一致则反向找到了原字符串,因此现在网站注册用户一般会提示密码加上字母、数字、特殊字符多种组合
 """
 ​
 m2 = hashlib.md5(b"salt")   #这里加盐设置的参数自己确定，如果没有参数，所以md5遵守一个规则，
 # 生成同一个对应关系，如果加了参数， 就是在原先加密的基础上再加密一层，别人不知道这个对应关系，就加大了撞库破解的难度。
 m2.update(b"nicholas")
 print(m2.hexdigest())
 # 输出结果317bf09f6da0f56f86b896fac6663443

```

# 摘要算法应用场景
 
## （1）大文件md5校验
```
import hashlib
def  file_md5(filename):
    md5_value = hashlib.md5()
    with open(filename, 'rb') as f:
        while True:
            data = f.read(2048) # 每次读取2048个字节数据
            if not data:
                break
            md5_value.update(data)# 计算md5值
    return md5_value.hexdigest()
​
file = input("请输入文件完整路径：")
v = file_md5(file)
print(v)

```
### 输出结果
32a9d0ad680bcdcb712ec802be9971be

## （2）网站用户注册登录
```
import hashlib
import json
import os
​
​
def handle_md5(msg):
    "对关键信息进行MD5处理"
    m = hashlib.md5(bytes("salt",encoding="utf-8"))  #加盐
    m.update(bytes(msg,encoding="utf-8"))
    res = m.hexdigest()
    return res
​
​
def write_data(filename,data):
    with open(filename,"a+",encoding="utf-8") as f:
        json.dump(data,f)
​
​
def read_data(filename):
    with open(filename,"r",encoding="utf-8") as f:
        data = json.load(f)
    return data
​
def login():
    #用户登录模块
    username = input("请输入账户名：").strip()
    userpasswd = input("请输入密码：").strip()
    username_file = "%s.json"%username
    start_file_path = os.path.dirname(os.path.abspath(__file__))
    username_file_path = os.path.join(start_file_path,username_file)
    if os.path.exists(username_file_path):   #判断用户信息文件是否存在
        data = read_data(username_file_path)
        account = data["name"]
        passwd_md5 = data["passwd"]
        userpasswd_md5 = handle_md5(userpasswd)
        if username == account and userpasswd_md5 == passwd_md5:
            print("恭喜%s,登录成功！"%username)
        else:
            print("账号或者密码错误，请重新登录。")
        return "ok"
    else:
        print("账户不存在，请注册！")
        return "no"
​
​
def register():
    #用户注册模块
    username = input("请输入要注册的用户名：").strip()
    userpasswd = input("请输入要注册的密码：").strip()
    user_filename = "%s.json"%username
    user_passwd_md5 = handle_md5(userpasswd)
    user_data ={"name":username,"passwd":user_passwd_md5}
    write_data(user_filename,user_data)
    print("注册成功")
​
​
def main():
    print("欢迎登录XX")
    while True:
        res = login()
        if res == "no":
            register()
        elif res == "ok":
            print("进入了XX网站")
            break
​
​
if __name__ == "__main__":
    main()
   ```
   


# 参考地址：
https://www.cnblogs.com/Nicholas0707/p/9108391.html
