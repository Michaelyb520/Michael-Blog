---
title: 算法-冒泡排序
layout: post
author: Michael
date: '2016-11-15 10:28:24 +0800'
Categories: Blog
tags: 算法设计
---
>貌似是程序员基础，我一个高级开发竟然只会冒泡（羞耻ing...）

## 前言
之前买了``剑指offer``,一直搁那里没怎么看，现在挑灯夜读挤出点时间学习下，在这之前还是得先把基础给打牢，这里先介绍算法入门-**冒泡排序**。
## 核心思想
冒泡排序的核心思想就是通过与相邻元素的比较和交换，把小的数交换到最前面。因为这个过程类似于水泡向上升一样，因此被命名为冒泡排序。
## 示例
* 第一轮过程: i=0~9；依次进行arr[i]与arr[i+1]对比；arr[i]小则不交换，arr[i+1]小则与arr[i]交换位置，由于100在这里面是最大的，100遇到任何数都要交换，最后换到末尾位置;
![冒泡过程](http://upload-images.jianshu.io/upload_images/1319710-9213f24f29d2d142.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 第二轮与第一轮一样从i=0开始，依次进行arr[i]与arr[i+1]对比，数组长度为n,则执行n-1轮后结束排序。
* 整个排序过程：
<pre>
原始长度数组 长度10
100		5	3	11	33	6	8	7	1	3	
第1轮
5		3	11	33	6	8	7	1	3	100	
第2轮
3		5	11	6	8	7	1	3	33	100	
第3轮
3		5	6	8	7	1	3	11	33	100	
第4轮
3		5	6	7	1	3	8	11	33	100	
第5轮
3		5	6	1	3	7	8	11	33	100	
第6轮
3		5	1	3	6	7	8	11	33	100	
第7轮
3		1	3	5	6	7	8	11	33	100	
第8轮
1		3	3	5	6	7	8	11	33	100	
第9轮
1		3	3	5	6	7	8	11	33	100	
</pre>

## 时间复杂度
从算法思想可知，冒泡排序需要两个循环来控制遍历，也就是需要n * n趟才能判断、交换完成。冒泡排序的时间复杂度为O ( n^2)。
## C语言代码
<pre><code>void paopaoSort(int arr[] ,int len) {
    printf("原始长度%d\n",len);
    printfArr(arr, len);
    for (int i = 0; i < len-1; i++) {
        printf("第%d轮\n",i+1);
        for (int j = 0; j < len-1; j++) {
            int preNum = arr[j];
            int nexNum = arr[j+1];
            if (preNum<=nexNum) {
            }else{
                int temp = preNum;
                arr[j] = nexNum;
                arr[j+1] = temp;
            }
        }
    }
}
</code></pre>
## 总结
在对比每一轮结构后可以发现，后面部分数组已经有序，可以无需对比，可以将i<len-1改成i<len-n-1，理解算法的思想，通过思考优化算法。