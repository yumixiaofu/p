# 直接上安装代码
 pip install bypy==1.6.10
 
 # 解释注意事项
 一定要使用这个指定版本的方式，否则会无法正常使用
 
 # 常用命令
 bypy info；
 bypy list；
 bypy upload；
 bypy help；
 
 # 高超技能
 
 第一种情况：遇到以下这个属于正常的断点续传，耐心等待即可。这个情况的出现意味着您上传的是大文件。
 ```
 <E> [18:18:32] Waiting 10 seconds before retrying...
<E> [18:18:42] Request Try #2 / 5
```

# 正在上次大文件的提示
命令行：bypy upload 文件路径；

```
 CryptographyDeprecationWarning: Python 2 is no longer supported by the Python core team. Support for it is now deprecated in cryptography, and will be removed in the next release.
  from cryptography import utils, x509
[____________________] 1% (160.0MB/9.4GB)  <E> [18:18:32] Waiting 10 seconds before retrying...
<E> [18:18:42] Request Try #2 / 5
[____________________] 2% (240.0MB/9.4GB)  
```
