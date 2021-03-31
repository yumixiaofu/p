# python排序算法之整数排序算法代码
```

def BubbleSort(lst):
    n=len(lst)
    if n<=1:
        return lst
    for i in range (0,n):
        for j in range(0,n-i-1):
            if lst[j]>lst[j+1]:
                (lst[j],lst[j+1])=(lst[j+1],lst[j])
    return lst

arr=[1,3,4,5,6,7,65,434,4343,43434,42556,34]

arr=BubbleSort(arr)
#print(arr)
print("数列按序排列如下：")
for i in arr:
    print(i,end=' ')

```
# 结果
数列按序排列如下：
1 3 4 5 6 7 34 65 434 4343 42556 43434 

# 使用方法
传入arr里面的数字即可！

