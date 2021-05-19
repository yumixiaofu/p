# 简介
lv 分区，vg 卷，pv 硬盘；加s 列出，remove 删除 ；如：lvs和 lvremove 需要fdisk -l 查看LVM硬盘目录，如删除分区：lvremove /dev/mapper/eisc-b

# 代码部分

```

 #  1.首先 将硬盘加入lvm 才能创建LVM 分区
#!/bin/bash
check(){                    #  [ʧɛk] 检查
azlvm=$(yum list installed | grep lvm2)
                        # 定义一个名称为azlvm的变量，值为：
                        # 查看已安装的包，grep 匹配lvm 名称
cdazlvm=$(echo ${#azlvm})    # 定义一个变量为：打印字符串长度
if [ $cdazlvm -lt 1 ]
                        # 判断变量字符串长度小于1
then                        # 然后
        yum install -y lvm2    
        echo "您还没有安装lvm，正在为您安装，请稍后："
else                        # 其他情况
        echo "lvm 已经安装，无需操作,已经安装的信息为：
    $azlvm"
fi                        # if的结束标记 fi
}
check    ;             # 调用方法函数；函数名放出来才会执行
# 2. 输入信息
input(){
echo "lv 分区，vg 卷，pv 硬盘"
lsdisk=$(fdisk -l |  awk -F " " '$1=="Disk" {print i++ " | "  $1 " | "  $2 " | " $3 $4}'  | sed s/\://g )
                        # 查看所有硬盘
                        # -F " " 指定分隔符是空格；$1 是第一列；==等于字符串 Disk； 的所有行
                        # 坐标：行已得到，开始print 打印列； i++ 表示打印序号数数
                        # " | " 是字符串连接符号  |   ； 打印结果的123列内容
                        # sed s/\://g  修改冒号为空格，  符号 \  将特殊字符 冒号 转译 
lvs=$(lvs |grep "-" | awk -F " " '{print i++ "  |分区：  " $1 "  |卷：  " $2 "  |  "  $3 } ')
echo "
查看硬盘：
$lsdisk
查看所有历史分区：
$lsDiskCatalog  
查看当前存在的VG 卷 和LV分区如下：
$lvs
"
##################   用户输入  ################
echo "
read 接收窗口命令界面输入的字符串;-p 加文字说明；需要分区的磁盘变量 a
如果输入错误字符或者闪跳，请Ctrl +c  退出重新输入
请输入你要进行分区的磁盘; 磁盘格式为：sdb "
read -p "请输入要分区的磁盘："  disk ;             
echo "磁盘挂载目录格式为：   /www     将会清空该目录下的文件
下面功能方法涉及不到的参数请回车跳过"
read -p "请输入挂载 [ˈkætəlɔg]目录："  catalog        ;             
read -p "请输入逻辑 [ˈvɑljum] 卷的组名称如 eisc 请您输入名字："  volume    ;     
read -p "请输入 [pɑrˈtɪʃən]分区唯一名称如 a 请您输入名字："  partition ;     
read -p "请输入分区大小，如： 3G 请您输入："  size    ;     
DiskCatalog=$(fdisk -l | grep "\-$partition" | awk -F" " '{print $2}' | sed "s/\://g")
                        # 查看lvm 分区目录;    作为公有变量来使用
                        # grep 匹配字符包含 -a字 符 字段的行; 特殊转译符号 \；  awk -F " " 指定空格为分隔符
                        # 直接打印结果中的第二列
                        # sed "s/\://g"  其中 sed s 修改 g 开启特殊字符转译， 特殊字符转译符号 \  使冒号 : 不被解析；
echo "根据您输入的信息先查找的硬盘目录：
$DiskCatalog
"
}
# 3. 分区
partition(){
echo "当前是新建LVM卷和挂载新分区"
input ；                     # 调用输入函数
pvcreate /dev/$disk        # 创建物理卷，选择的硬盘为 sdc
                        #  create [kriˈet] 创建
pvs                        # 查看所有pv硬盘
pvs /dev/$disk pvscan        # 查看指定pv硬盘
pvdisplay /dev/$disk        # 显示系统上面的pv硬盘状态
                        # display [dɪˈsple] 显示
    
# pvremove　/dev/xxx        # 废除pv硬盘格式，（仅需要了解）
#                        # remove [riˈmuv] 废除
# pvs／pvscan　            # 查看系统里有pv的磁盘
# pvdisplay　                # 显示系统上面的pv状态
vgcreate -s 16M $volume /dev/$disk
                        # 创建一个卷组，大小为16M；名字为：eisc
                        # 选择的硬盘为：sdc
                        # 新建一个vg，-s后面接pe的大小（可选），单位是M，G，可以放多块pv
lvcreate -L $size -n $partition $volume    
                        # 创建逻辑卷，名字为：a
                        # 新建一个lv，-l指定pe的个数，-L指定容量，M，G
# lvs／lvscan　            # 查看系统里有lv的磁盘
# lvdisplay　                # 显示系统上面的lv状态
# lvremove　                # 删除lv
# lvreduce　                # 在lv里减少容量
# lvextend　                # 增加容量
mkfs -t ext4 /dev/mapper/$volume-$partition ； 
mkfs -t ext4 /dev/mapper/$volume-$partition
                        # 格式化分区
rm -rf $catalog
mkdir $catalog            # 创建挂载的目录
mount /dev/mapper/$volume-$partition $catalog
                        # 挂载到目录 /www
df -h                     # 查看挂载的lvm分区
sed -i "/#eisc$partition/d" /etc/fstab ; sed -i "/^$/d" /etc/fstab
                        # 挂载删除写记录; 删除空行
echo "
/dev/mapper/$volume-$partition            $catalog        ext4    defaults        0 0        #eisc$partition
" >> /etc/fstab
}
 # 4. 基本lvm 分区结束，扩容分区和强制调整分区大小
PartitionSize(){
echo "当前是分区扩容，需要提供大小，分区所属硬盘，挂载目录
需要输入大小，和分区名字
"
input ;                    # 调用用户输入方法函数；函数名放出来才会执行
lvextend -L +$size $DiskCatalog    
                        # 扩充这个分区1G容量
resize2fs $DiskCatalog        # 更新文件系统的大小，即激活
lvs                        # 查看分区
}
MandatorySize(){                # [ˈmændəˌtɔri] 强制
echo "当前是分区扩容，需要提供大小，分区所属硬盘，挂载目录
需要输入大小，和分区名字，挂载目录
请注意：需要格式化才能装载（挂载）；需要备份数据
"
input ; 
catalog=$(df -h | grep "\-$partition" | awk -F" " '{print $6}')
umount /dev/mapper/$volume-$partition  $catalog
                        # 取消现有挂载，然后强制设置大小，再然后：
lvreduce -L $size /dev/mapper/$volume-$partition 
                        # 强制设置大小；首先需要：
# resize2fs /dev/mapper/$volume-$partition         # 生效
mkfs -t ext4 /dev/mapper/$volume-$partition
                        # 格式化
mount /dev/mapper/$volume-$partition  $catalog
                        # 重新挂载
df -h                    # 再次查看磁盘容量；发现分区已经调整
}
 # 5.LVM 扩容硬盘：多个硬盘加入LVM;# 如果硬盘（硬件）空间不够：vg空间不够，需先扩展vg，扩展vg就是往vg中加pv
Expansion(){                #  [ɪkˈspænʃən] 扩容
echo "将硬盘加入现有的LVM卷，需要硬盘和卷名称两个参数，其他提示回车跳过。列出当前加入LVM分区的硬盘，和现有的PV卷"
pvs                        # 查看现有pv硬盘：加入lvm的硬盘
vgs                        # LVM卷
input ;                      # 调用输入函数
pvcreate /dev/$disk        # 将硬盘加入lvm， 然后 pvs 再次查看
vgextend $volume /dev/$disk
                        # 将硬盘加入 eisc 卷组，然后总容量是各个硬盘容量之和
pvs                        # 再次查看LVM硬盘
}
 # 6.LVM的缩减删除操作 
Delete(){                    # [dəˈlit] 删除
echo "
删除操作需要满足先后顺序
1.先删除LV分区：只输入分区名字，其他回车跳过
2.再删除VG卷：只输入卷名字
3.最后删除VG硬盘先后顺序
需要提供卷，分区名字，挂载目录"
input ; 
echo "请输入：1删除lv分区，2 删除vg卷，3删除lvm硬盘（取消挂载）
"
read -p "请输入操作：" r
case "$r" in 
"1") 
dcatalog=$(df -h | grep "\-$partition" | awk -F " " '{print $6}')
                            # 查看LVM分区的挂载目录 ： "\-" 将特殊字符 -  转译
umount $DiskCatalog $dcatalog    # 取消挂载的所有分区（lv）才能执行删除
lvremove $DiskCatalog            # 删除lvm分区 a  ;             简称 lv
lvs
;;
"2")
vgremove /dev/mapper/$volume    # 删除eisc卷组（逻辑卷）；    简称：vg
vgs
echo "删除了VG卷，请也删除硬盘重新加入LVM，即重新分区；"
;; 
"3")
pvremove /dev/$disk            # 删除lvm 的硬盘；            简称：pv
pvs
;;
*) echo "
###########################################################
                           输入错误请重新输入
###########################################################
"
Delete
;;
esac
}
home(){
clear            # 清屏
echo "
欢迎来到小绿叶技术博客
www.eisc.cn
LVM自动化分区脚本
请输入对应数字惊醒操作：
1.新建LVM分区                                   2.分区扩容
3.减少分区至指定大小                         4.扩容LVM硬盘
5.lv vg pv 的删除操作
"
read -p " 请输入您的操作：" h
case "$h" in
"1") partition
;;
"2") PartitionSize
;;
"3") MandatorySize
;;
"4") Expansion
;;
"5") Delete
;;
*) echo "输入错误，请重新输入。正在返回主界面"
home
;;
esac
}
home
# 执行脚本：
yum install -y wget 
rm -rf lvmpartition.sh ; wget eisc.cn/file/shell/lvmpartition.sh ; sed -i "/^$/d" lvmpartition.sh ; chmod 755 lvmpartition.sh ; ./lvmpartition.sh

```

# 执行脚本

yum install -y wget
rm -rf lvmpartition.sh ; wget eisc.cn/file/shell/lvmpartition.sh ; sed -i "/^$/d" lvmpartition.sh ; chmod 755 lvmpartition.sh ; ./lvmpartition.sh


# 友情提示

出现任何问题概不负责！
