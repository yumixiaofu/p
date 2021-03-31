# 使用python中os模块遍历替换文件夹内所有文件字符串
```
import os
# 将传入参数的文件路径中的所有文件路径递归遍历返回到传入的all_files字典中
def show_files(path, all_files):
    # 首先遍历当前目录所有文件及文件夹
    file_list = os.listdir(path)
    # 准备循环判断每个元素是否是文件夹还是文件，是文件的话，把名称传入list，是文件夹的话，递归
    for file in file_list:`
        # 利用os.path.join()方法取得路径全名，并存入cur_path变量，否则每次只能遍历一层目录
        cur_path = os.path.join(path, file)
        # 判断是否是文件夹
        if os.path.isdir(cur_path):
            show_files(cur_path, all_files)
        else:
            all_files.setdefault(path, []).append(file)
            # all_files.append(os.path.join(path, file))
    return all_files

# 传入要替换文件/文件夹中包含的字符串,要替换为的字符串,和文件dict
def modify_file_name(old_str, new_str, all_files, root_src):
    file_count = 0
    modify_folder_count = 0
    modify_file_count = 0
    # 先修改文件名
    for folder in all_files:
        for file in all_files[folder]:
            if old_str in file:
                new_name = file.replace(old_str, new_str)
                os.rename(os.path.join(folder, file), os.path.join(folder, new_name))
                print(file + "已成功修改为: " + new_name)
                modify_file_count += 1
            file_count += 1
    # 再修改文件夹名
    for folder in all_files:
        if old_str in folder:
            os.rename(folder, folder.replace(old_str, new_str))
            modify_folder_count += 1
    return [file_count, modify_folder_count, modify_file_count]

src = "这里是文件夹路径"
thwz="要替换的文字"
thcsm="替换成什么"
get_all_files = show_files(src, {})
count_list = modify_file_name(thwz,thcsm, get_all_files, src)
# print("文件夹数量: " + str(len(get_all_files)))
# print("文件数量: " + str(count_list[0]))
# print("成功修改文件夹: " + str(count_list[1]))
# print("成功修改文件: " + str(count_list[2]))
```


# 使用方法
只需要传入三个变量即可。
src = "这里是文件夹路径"
thwz="要替换的文字"
thcsm="替换成什么"
