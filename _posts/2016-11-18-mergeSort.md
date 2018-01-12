---
title: 算法-归并排序
layout: post
author: 谢涛
date: '2016-11-18 10:24:24 +0800'
categories: Blog
---
>阅读量真是少，又不想投稿，怎么办

## 前言
归并排序思路简单，速度仅次于快速排序，为稳定排序算法，一般用于对总体无序，但是各子项相对有序的数列。

## 核心思想
比较a[i]和a[j]的大小，若a[i]≤a[j]，则将第一个有序表中的元素a[i]复制到r[k]中，并令i和k分别加上1；否则将第二个有序表中的元素a[j]复制到r[k]中，并令j和k分别加上1，如此循环下去，直到其中一个有序表取完，然后再将另一个有序表中剩余的元素复制到r中从下标k到下标t的单元。归并排序的算法我们通常用递归实现，先把待排序区间[s,t]以中点二分，接着把左边子区间排序，再把右边子区间排序，最后把左区间和右区间用一次归并操作合并成有序的区间[s,t]。
## 示例
#### 分治
1. 首先我们拿到原始数组``{100,5,3,11,33,6,8,7,1,3}``，然后将数组对半分。

2. ``{100,5,3,11,33},{6,8,7,1,3}``，继续半分。

3. ``{100,5,3},{11,33};{6,8,7,},{1,3}``，多于两个值的集合，继续半分。

4. ``{100,5},{3};{11,33};{6,8},{7};{1,3}``，拆分完成后，每个集合先内部排序，再按拆分的顺序合并。

#### 归并
1. ``{100,5}->{5,100} ``对比1次，对比顺序：``5:100``，调整后数组变为``{5,100,3,11,33,6,8,7,1,3}``。

2. ``{5,100},{3}->{3,5,100}``对比1次，对比顺序：``5:3``，调整后数组变为``{3,5,100,11,33,6,8,7,1,3}``。

3. ``{11,33}无变化``对比1次，对比顺序：``11:33``，调整后数组变为``{3,5,100,11,33,6,8,7,1,3}``。

4. ``{3,5,100},{11,33}->{3,5,11,33,100}``对比4次，对比顺序：``3:11,5:11,100:11,100:33``，调整后数组变为``{3,5,11,33,100,6,8,7,1,3}``。

5. ``{6,8}无变化``对比1次，对比顺序：``6:8``，调整后数组变为``{3,5,11,33,100,6,8,7,1,3}``。

6. ``{6,8},{7}->{6,7,8}``对比2次，对比顺序：``6:8,8:7``，调整后数组变为``{3,5,11,33,100,6,7,8,1,3}``。

7. ``{1,3}无变化``对比1次，对比顺序：``1:3``，调整后数组变为``{3,5,11,33,100,6,7,8,1,3}``。

8. ``{6,7,8},{1,3}->{1,3,6,7,8}``对比2次，对比顺序：
``6:1,6:3``，调整后数组变为``{3,5,11,33,100,1,3,6,7,8}``。

9. ``{3,5,11,33,100},{1,3,6,7,8}->{1,3,3,5,6,7,8,11,33,100}``对比7次，对比顺序：``1:3,3:3,5:3,5:6,11:6,11:7,11:8``,整个数组完成排序。

## C语言代码
<pre><code>#pragma mark - 归并排序
void mergeList (int arr[], int first, int middle, int last ,int temp[]) {
    printf("待排序数组\n");

    for (int i = first; i <= last; i++) {
        printf("%d\t",arr[i]);
    }
    
    int leftStartIndex = first;
    int leftEndIndex = middle;
    
    int rightStartIndex = middle + 1;
    int rightEndIndex = last;
    
    int tempIndex = 0;

    // 寻找两个子序列，顺序遍历，将值小的复制到临时数组中，直到其中一个子序列遍历完毕
    while (leftStartIndex <= leftEndIndex && rightStartIndex <= rightEndIndex) {
        // 值小的就复制到临时数组中
        printf("\nleft:%d right:%d\n",arr[leftStartIndex],arr[rightStartIndex]);
        if (arr[leftStartIndex] <= arr[rightStartIndex]) {
            printf("temp加入left:%d\n",arr[leftStartIndex]);
            temp[tempIndex++] = arr[leftStartIndex++];
        } else {
            printf("temp加入right:%d\n",arr[rightStartIndex]);
            temp[tempIndex++] = arr[rightStartIndex++];
        }
    }
    
    // 有可能左子序列更长，因此将剩下的部分直接复制到临时数组中
    while (leftStartIndex <= leftEndIndex) {
        temp[tempIndex++] = arr[leftStartIndex++];
    }
    
    // 有可能右子序列更长，因此将剩下的部分直接复制到临时数组中
    while (rightStartIndex <= rightEndIndex) {
        temp[tempIndex++] = arr[rightStartIndex++];
    }
    
    int i = 0;
    while (first+i <= last) {
        arr[first+i] = temp[i];
        i++;
    }
    
    printf("进行排序数组\n");
    for (int i = first; i <= last; i++) {
        printf("%d\t",arr[i]);
    }
    printf("\n更新数组\n");
    printfArr(arr, 10);
}

void mergeSort (int arr[], int first, int last, int temp[]) {
    if (first == 0 && last == 9) {
        printfArr(arr, 10);
    }
    if (last > first) {
        int middle = (first+last)/2;
        // 归并左方子列
        mergeSort(arr, first, middle, temp);
        // 归并右方子列
        mergeSort(arr, middle+1, last, temp);
        // 合并
        mergeList(arr, first, middle, last, temp);
    }
}
</code></pre>

## 时间复杂度
时间复杂度为O(nlogn) 这是该算法平均的时间性能，空间复杂度为 O(n)，比较操作的次数介于(nlogn) / 2和nlogn - n + 1。

## 结语
从新写一遍算法，理解果然更透彻了，本文没有使用图片，感觉用文字就已经足够描述了😅。

----
最后给出最近几篇关于排序算法的[源代码](https://github.com/xietao3/BaseCalculationDemo)