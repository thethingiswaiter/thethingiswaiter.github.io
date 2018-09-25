---
layout: post
title: 五个排序算法(Sort)
date: 2018-07-22
categories: blog
tags: [java,算法]
description: 闲来无事,实现了几个基本排序方式,留在这里记录一下
---

[TOC]


```java
public class AllSort {
    
    /**
     * 切换数组中的数值
     */
    public void transposition(int[] array, int i, int j) {
        int temp = array[i];    //暂存数据
        array[i] = array[j];
        array[j] = temp;
    }

    /**
     * 冒泡排序（升序）
     * 不断比较相邻两个数据的大小，如果前小于后，则替换；
     * 可以保证每一次运行都能够找到最大值并放到最后；
     * 故每次都减少一个后面的检测数；
     * <p>
     * 分类 -------------- 内部比较排序
     * 数据结构 ---------- 数组
     * 最差时间复杂度 ---- O(n^2)
     * 最优时间复杂度 ---- 如果能在内部循环第一次运行时,使用一个旗标来表示有无需要交换的可能,可以把最优时间复杂度降低到O(n)
     * 平均时间复杂度 ---- O(n^2)
     * 所需辅助空间 ------ O(1)
     * 稳定性 ------------ 稳定
     *
     * @param array 需要排序的数组
     */
    public void bubble(int[] array) {
        //如果数组为空或长度小于1直接返回
        if (array == null || array.length < 2)
            return;
        boolean flag = true;          //标志是否发生过交换
        int length = array.length;    //数组长度
        while (length > 1) {
            flag = true;
            length--;
            for (int i = 0; i < length; i++) {
                if (array[i] > array[i + 1]) {
                    transposition(array, i, i + 1);
                    flag = false;
                }
            }
            if (flag) break;
        }
    }

    /**
     * 选择排序
     * 每次选出最大值和最小值交换到首尾，依次循环可实现
     * <p>
     * 分类 -------------- 内部比较排序
     * 数据结构 ---------- 数组
     * 最差时间复杂度 ---- O(n^2)
     * 最优时间复杂度 ---- O(n^2)
     * 平均时间复杂度 ---- O(n^2)
     * 所需辅助空间 ------ O(1)
     * 稳定性 ------------ 不稳定
     *
     * @param array 被排序的数组
     */
    public void selection(int[] array) {
        //如果数组为空或长度小于1直接返回
        if (array == null || array.length < 2)
            return;
        int max = 1;                  //最大值
        int min = 0;                  //最小值
        int maxPos = 1;               //最大值位置
        int minpos = 0;               //最小值位置
        int front = 0;                //最前面需要修改的值
        int last = array.length - 1;    //最后面需要修改的值

        while (true) {
            min = array[front];
            max = array[last];
            if (front >= last) {
                break;
            }
            //寻找最小值和最大值
            for (int i = front; i <= last; i++) {
                if (array[i] < min) {
                    min = array[i];
                    minpos = i;
                }
                if (array[i] > max) {
                    max = array[i];
                    maxPos = i;
                }
            }
            if (min >= max) break;
            //交换位置
            if (minpos != -1) {
                transposition(array, front, minpos);
                minpos = -1;
            }
            if (maxPos != -1) {
                transposition(array, last, maxPos);
                maxPos = -1;
            }
            front++;
            last--;
        }
    }

    /**
     * 插入排序
     * 从第二个结点开始不断的往前比较交换可得到结果
     * <p>
     * 分类 -------------- 内部比较排序
     * 数据结构 ---------- 数组
     * 最差时间复杂度 ---- O(n^2)
     * 最优时间复杂度 ---- O(nlogn)
     * 平均时间复杂度 ---- O(n^2)
     * 所需辅助空间 ------ O(1)
     * 稳定性 ------------ 稳定
     *
     * @param array 被排序的数组
     */
    public void insertion(int[] array) {
        //如果数组为空或长度小于1直接返回
        if (array == null || array.length < 2)
            return;
        for (int i = 1; i < array.length; i++) {
            for (int j = i - 1; j >= 0; j--) {
                if(array[j + 1] >= array[j]){
                    break;
                }
                if (array[j + 1] < array[j]) {
                    transposition(array, j, j + 1);
                }
            }
        }
    }

    /**
     * 对两个连续数组部分进行分别排序
     *
     * @param array 排序数组
     * @param left  数组第一个排序部分的起点
     * @param mid   数组第一个排序部分的终点
     * @param right 数组第二个排序部分的终点
     */
    public void merge(int[] array, int left, int mid, int right) {
        if (left == right) return;
        int[] temp = new int[right - left + 1];       //辅助数组
        int index = 0;                            //记录辅助数组位置
        int i = left;                             //第一个排序部分起点
        int j = mid + 1;                            //第二个排序部分起点
        while (i <= mid && right >= j) {
            if (array[i] < array[j]) {
                temp[index++] = array[i++];
            } else {
                temp[index++] = array[j++];
            }
        }
        while (i <= mid) {
            temp[index++] = array[i++];
        }
        while (right >= j) {
            temp[index++] = array[j++];
        }
        index = 0;
        while (index < temp.length) {
            array[left++] = temp[index++];
        }
    }

    /**
     * 归并排序（非递归）
     * 先是2个排，再是4个排，依次增大迭代可完成归并排序
     *
     * @param array 排序数组
     */
    public void mergeSortIteration(int[] array) {
        int left = 0;                     //左起始位置
        int right = 0;                    //右完结位置
        //完成2,4,8，，，的增大归并
        for (int i = 1; i < array.length; i *= 2) {
            left = 0;
            right = -1;
            int mid=0;
            //找出两个归并序列的起始位置
            while (left < array.length) {
                right += i * 2;
                mid=left-1+i;
                if (right >= array.length) {
                    right = array.length - 1;
                }
                if (mid>= array.length) {
                    mid = array.length - 1;
                }
                merge(array, left, mid, right);
                left = right + 1;
            }
        }
    }

    /**
     * 归并排序（递归）
     *
     * @param array 排序数组
     * @param left  左起始位置
     * @param right 右终结位置
     */
    public void mergeSortRecursion(int[] array, int left, int right) {
        //递归出口
        if (left >= right) {
            return;
        }
        //mid只能是(left+right)/2，否则无法达到递归出口
        mergeSortRecursion(array, left, (left + right) / 2);
        mergeSortRecursion(array, (left + right) / 2 + 1, right);
        merge(array, left, (left + right) / 2, right);
    }


    /**
     * 将数组按基准进行分割
     * 分割部分的优化方案：
     * 1.三数取中
     * 2.在较短时加入插排
     * 3.聚集相等元素
     * 4.尾递归
     * 5.双向扫描
     *
     * @param array 排序数组
     * @param left  左首位置
     * @param right 右终位置
     * @return 基准所在位置
     */
    public int[] partition(int[] array, int left, int right) {
        int moveL = left - 1;                   //记录向右移动的步数
        int moveR = right + 1;                  //记录向左移动的步数
        int equalL = left-1;                    //与基准相等的左个数
        int equalR = right+1;                   //与基准相等的右个数
        //基准游标
        int cursor = selectPivotMedianOfThree(array, left, right);
        //等值移动到两侧，确定轴枢，并使得小左大右
        while (true) {
            while (array[--moveR] > cursor) {
            }
            while (array[++moveL] < cursor) {
                if (moveR == left) break;
            }
            if (moveR > moveL) {
                transposition(array, moveR, moveL);
                if (array[moveL] == cursor && moveL != equalL) {
                    transposition(array, ++equalL, moveL);
                }
                if (array[moveR] == cursor && moveR != equalR) {
                    transposition(array, --equalR, moveR);
                }
            } else {
                break;
            }
        }
        //从最后提取一个基准到轴枢
        transposition(array, moveL, right);
        right--;
        moveR = moveL + 1;
        moveL--;
        //将两侧的等值移动到轴枢两侧
        for (int i = left; i <= equalL; i++) { transposition(array, i, moveL--); }
        for (int i = right; i >= equalR; i--) { transposition(array, i, moveR++); }
        return new int[]{moveL, moveR};
    }

    /**
     * 快速排序
     * 由大到小不断排序直至顺序正确
     *
     * @param array 排序数组
     * @param left  左首位置
     * @param right 右终位置
     */
    public void quickSort(int[] array, int left, int right) {
        //递归出口
        if ((right - left) < 20) {
            inserSort(array, left, right);
            return;
        }
        int[] p = partition(array, left, right);
        quickSort(array, p[1], right);
        quickSort(array, left, p[0]);

    }

    /**
     * 插入排序
     * 负责对一部分的数据进行排序
     *
     * @param array 排序数组
     * @param left  左首位置
     * @param right 右终位置
     */
    public void inserSort(int[] array, int left, int right) {
        if (right <= left) {
            return;
        }
        for (int i = left; i <= right; i++) {
            for (int j = i - 1; j >= left; j--) {
                if(array[j + 1] >= array[j]){
                    break;
                }else {
                    transposition(array, j, j + 1);
                    break;
                }
            }
        }
    }

    /**
     * 函数作用：取待排序序列中low、mid、high三个位置上数据
     * 选取他们中间的那个数据作为枢轴
     */
    int selectPivotMedianOfThree(int[] arr, int low, int high) {
        int mid = low + ((high - low) >> 1);//计算数组中间的元素的下标

        //使用三数取中法选择枢轴
        if (arr[mid] > arr[high]) {
            transposition(arr, mid, high);
        }
        if (arr[low] > arr[high]) {
            transposition(arr, low, high);
        }
        if (arr[mid] > arr[low]) {
            transposition(arr, mid, low);
        }
        //此时，arr[mid] <= arr[low] <= arr[high]
        return arr[low];
        //low的位置上保存这三个位置中间的值
        //分割时可以直接使用low位置的元素作为枢轴，而不用改变分割函数了
    }
}

```