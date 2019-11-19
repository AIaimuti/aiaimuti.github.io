---
layout:     post
title:      快速排序
subtitle:   O(N * logN)时间复杂度
date:       2019-11-15
author:     AIaimuti
header-img: img/post-bg-open-source-blog.jpg
catalog: true
tags:
    - O(N * logN)
---


本文基于阿顾同学博客进行整理

原文链接https://blog.csdn.net/u010452388/article/details/81218540

阿顾同学把左神的课已经整理的很好的,这几个经典算法我在这里就当搬运工了。

基本思想是：从一个数组中随机选出一个数N，通过一趟排序将数组分割成三个部分，1、小于N的区域 2、等于N的区域 3、大于N的区域，然后再按照此方法对小于区的和大于区分别递归进行，从而达到整个数据变成有序数组。

## 图解流程

下面通过实例数组进行排序，存在以下数组:

![](https://img-blog.csdn.net/20180726150438980?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从上面的数组中，随机选取一个数（假设这里选的数是5）与最右边的7进行交换 ，如下图

![](https://img-blog.csdn.net/2018072615070367?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

准备一个小于区和大于区（大于区包含最右侧的一个数）等于区要等最后排完数才会出现，并准备一个指针，指向最左侧的数，如下图

![](https://img-blog.csdn.net/20180726150924373?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

到这里，我们要开始排序了，每次操作我们都需要拿指针位置的数与我们选出来的数进行比较，比较的话就会出现三种情况，小于，等于，大于。三种情况分别遵循下面的交换原则:

1. 指针的数<选出来的数
+ 拿指针位置的数与小于区右边第一个数进行交换
+ 小于区向右扩大一位
+ 指针向右移动一位

2. 选出来的数=选出来的数
+ 指针向右移动一位(当前区向直接后跳一个)

3. 指针的数>选出来的数
+ 拿指针位置的数与大于区左边第一个数进行交换
+ 大于区向左扩大一位
+ 指针位置不动（当前位置不动）

根据上面的图可以看出5=5，满足交换原则第2点，指针向右移动一位，如下图：

![](https://img-blog.csdn.net/20180726151243120?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从上图可知，此时3<5，根据交换原则第1点，拿3和5（小于区右边第一个数）交换，小于区向右扩大一位，指针向右移动一位，结果如下图：

![](https://img-blog.csdn.net/20180726151937689?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从上图可以看出，此时7>5,满足交换原则第3点，7和2(大于区左边第一个数)交换，大于区向左扩大一位，指针不动，如下图：

![](https://img-blog.csdn.net/20180726152651255?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从上图可以看出，2<5,满足交换原则第1点，2和5（小于区右边第一个数）交换，小于区向右扩大一位，指针向右移动一位，得到如下结果：

![](https://img-blog.csdn.net/20180726154237901?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

从上图可以看出，6>5，满足交换原则第3点 ，6和6自己换，大于区向左扩大一位，指针位置不动，得到下面结果

![](https://img-blog.csdn.net/20180726154555130?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

此时，指针与大于区相遇，则将指针位置的数6与随机选出来的5进行交换，就可以得到三个区域：小于区，等于区，大于区，如下：

![](https://img-blog.csdn.net/20180726155235963?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTA0NTIzODg=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

 到此，一趟排序结束了，后面再将小于区和大于区重复刚刚的流程即可得到有序的数组

快排的时间复杂度O(N * logN)，空间复杂度O(logN) （因为每次都是随机事件，坏的情况和差的情况，是等概率的，根据数学期望值可以算出时间复杂度和空间复杂度），不稳定性排序（stable_sort()对快速排序进行了稳定优化）

实际运用中，为了优化算法的分区情况，会随机从L到R位置选取一个数与最后一个数交换。

## 代码
java代码如下：
```
public static void quickSort(int[] arr, int L, int R) {
        if (L < R) {
            //生成一个随机数
            double random = Math.random();
            //在L至R位置随机选取一个数与最右边的数交换
            swap(arr, L + (int) (random * (R - L + 1)), R);
            //此数组长度永远为2,p[0]为等于区的左边界，p[1]为等于区的右边界
            int[] p = partition(arr, L, R);
            //将分出来的小于区重复上面的动作
            quickSort(arr, L, p[0] - 1);
            //将分出来的大于区重复上面的动作
            quickSort(arr, p[1] + 1, R);
        }
 
    }
 
    public static int[] partition(int[] arr, int L, int R) {
        //声明一个小于区的索引
        int less = L - 1;
        //声明一个大于区的索引
        int more = R;
        //声明一个起始索引指针
        int index = L;
        //划分原则：
        /*1 如果arr(index)<arr(R)
          1.1 拿index位置的数与小于区右边第一个数进行交换
          1.2 小于区向右扩大一位
          1.3 index索引向右移动一位
        2 如果arr(index)==arr(R)
          2.1 index索引向右移动一位
        3 如果arr(index)>arr(R)
          3.1 拿index位置的数与大于区左边第一个数进行交换
          3.2 大于区向左扩大一位
          3.3 index索引位置不动
          */
        while (index < more) {
            if (arr[index] < arr[R]) {
                //一行代码完成划分原则的1.1, 1.2, 1.3功能
                swap(arr, index++, ++less);
            } else if (arr[index] == arr[R]) {
                index++;
            } else {
                //一行代码完成划分原则的3.1, 3.2, 3.3功能
                swap(arr, index, --more);
            }
        }
        //如果index索引与more相遇，则退出循环，并且R位置数与more位置数交换
        swap(arr, more, R);
        //用来记录等于区的左边界和右边界对应的索引
        return new int[]{less + 1, more};
    }
 
    //将数组中索引i和j的数进行交换
    public static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
```
C++ 代码(C++中有swap函数就直接使用了) 

```
void QuickSort(vector<int>& nums,int L,int R,int n){
        if(L<R) {
            int random = rand() % (R - L + 1);
            swap(nums[L + random], nums[R]);
            vector<int> p = partition(nums,L,R);
            QuickSort(nums,L,p[0]-1,n);
            QuickSort(nums,p[1]+1,R,n);
        }
    }
vector<int> partition(vector<int>& arr,int L,int R){
    int less = L - 1;
    int more = R;
    int index = L;
    while (index < R){
        if (arr[index] < arr[R]){
            swap(arr[index++], arr[++less]);
        }
        else if (arr[index] == arr[R]){
            index++;
        }
        else{
            swap(arr[index++], arr[--more]);
        }
    }
    swap(arr[more], arr[R]);
    vector <int> a = {less+1, more};
    return a;
}
```
