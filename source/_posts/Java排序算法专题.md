---
title: Java排序算法专题
date: 2016-09-10 00:12:20
tags: [Java, Algorithm, LeetCode]
---

今天晚上做了一下LeetCode上的[Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)这道题，没想到一次性通过了。随即想要归纳整理一下排序算法，废话少说，我们开始吧。

---
### 选择排序
这是一种最简单直观的排序算法，它的工作原理如下：每一趟从待排序的数列中选出最小的（最大的）一个元素，顺序放到已经排好序的数列的最后，直到所有待排元素全部排好。选择排序是[稳定的排序算法](https://zh.wikipedia.org/wiki/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95#.E5.88.86.E9.A1.9E)，最坏[时间复杂度](https://zh.wikipedia.org/wiki/%E6%97%B6%E9%97%B4%E5%A4%8D%E6%9D%82%E5%BA%A6)是O(n^2)，最优时间复杂度是O(n^2)，平均时间复杂度是O(n^2)。

下面是对{1, 3, 5, 7, 9, 2, 4, 6, 8, 0}进行选择排序的具体过程

<!-- more -->

```
                      |1 3 5 7 9 2 4 6 8 0  选择第一小的数与0位交换
                       i j
                      1 3 5 7 9 2 4 6 8 0
                      i                 j
                                       min
                      0| 3 5 7 9 2 4 6 8 1  选择第二小的数与1位交换
                         i j
                      0 3 5 7 9 2 4 6 8 1
                        i               j
                                       min
                      0 1| 5 7 9 2 4 6 8 3  选择第三小的数与2位交换
                      0 1 2| 7 9 5 4 6 8 3  选择第四小的数与3位交换
                      0 1 2 3| 9 5 4 6 8 7  选择第五小的数与4位交换
                      0 1 2 3 4| 5 9 6 8 7  选择第六小的数与5位交换
                      0 1 2 3 4 5| 9 6 8 7  选择第七小的数与6位交换
                      0 1 2 3 4 5 6| 9 8 7  选择第八小的数与7位交换
                      0 1 2 3 4 5 6 7| 8 9  选择第九小的数与8位交换
                      0 1 2 3 4 5 6 7 8| 9  待排只剩一个数，排序结束
```
选择排序(从小到大)的Java实现如下：

```java
public static void selectSort(int[] nums) {
    int min, temp, length = nums.length;
    for (int i = 0; i < length; i++) {
        min = i;
        for (int j = i + 1; j < length; j++) {
            if (nums[min] > nums[j]) {
                min = j;
            }
        }
        temp = nums[i];
        nums[i] = nums[min];
        nums[min] = temp;
    }
}
```

### 插入排序
这也是一种简单直观的排序算法，它的工作原理如下：构建有序序列，即对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。插入排序是稳定的排序算法，最坏时间复杂度是O(n^2)，最优时间复杂度是O(n)，平均时间复杂度是O(n^2)。

下面是对{1, 3, 5, 7, 9, 2, 4, 6, 8, 0}进行插入排序的具体过程

```
                      1 3 5 7 9 2 4 6 8 0
                      1 3 5 7 9 9 4 6 8 0  temp=2
                      1 3 5 7 7 9 4 6 8 0
                      1 3 5 5 7 9 4 6 8 0
                      1 3 3 5 7 9 4 6 8 0
                      1 2 3 5 7 9 4 6 8 0
                      1 2 3 5 7 9 9 6 8 0  temp=4
                      1 2 3 5 7 7 9 6 8 0
                      1 2 3 5 5 7 9 6 8 0
                      1 2 3 4 5 7 9 6 8 0
                      1 2 3 4 5 7 9 9 8 0  temp=6
                      1 2 3 4 5 7 7 9 8 0
                      1 2 3 4 5 6 7 9 8 0
                      1 2 3 4 5 6 7 9 9 0  temp=8
                      1 2 3 4 5 6 7 8 9 0
                      1 2 3 4 5 6 7 8 9 9  temp=0
                      1 2 3 4 5 6 7 8 8 9
                      1 2 3 4 5 6 7 7 8 9
                      1 2 3 4 5 6 6 7 8 9
                      1 2 3 4 5 5 6 7 8 9
                      1 2 3 4 4 5 6 7 8 9
                      1 2 3 3 4 5 6 7 8 9
                      1 2 2 3 4 5 6 7 8 9
                      1 1 2 3 4 5 6 7 8 9
                      0 1 2 3 4 5 6 7 8 9  
```
插入排序(从小到大)的Java实现如下：

```java
public static void insertSort(int[] nums) {
    int temp, length = nums.length;
    for (int i = 1; i < length; i++) {
        temp = nums[i];
        int j = i;
        for (; j >= 1&&temp < nums[j - 1]; j--) {
            nums [j] = nums[j - 1];
        }
        nums[j] = temp;
    }
}
```

### 希尔排序
希尔排序，也称递减增量排序算法，是插入排序的一种高速而稳定的改进版本。我把希尔排序叫做分组插入排序，它的工作原理如下：先把要排序的序列元素以序列长度的1/2为间隔(向下取整)两两分为一组，对每组分别进行插入排序，排完后再以序列长度的1/4为间隔(向下取整)分组，对每组分别进行插入排序，重复上述操作，直至间隔为一，即最后一趟为普通的插入排序(此时序列已基本有序)。希尔排序是不稳定的排序算法，时间复杂度取决于分组间隔gap的取值，目前最佳版本的最坏时间复杂度是O(n(lgn)^2)。一般情况下，最坏时间复杂度是O(n^2)，最优时间复杂度是O(n)。

下面是对{1, 3, 5, 7, 9, 2, 4, 6, 8, 0}进行插入排序的具体过程

```
                      1 3 5 7 9 2 4 6 8 0  gap=5
                      1 3 5 7 9 2 4 6 8 9  temp=0
                      1 3 5 7 0 2 4 6 8 9  gap=2
                      1 3 5 7 5 2 4 6 8 9  temp=0
                      1 3 1 7 5 2 4 6 8 9
                      0 3 1 7 5 2 4 6 8 9
                      0 3 1 7 5 2 5 6 8 9  temp=4
                      0 3 1 7 4 2 5 6 8 9
                      0 3 1 7 4 7 5 6 8 9  temp=2
                      0 3 1 3 4 7 5 6 8 9
                      0 2 1 3 4 7 5 6 8 9
                      0 2 1 3 4 7 5 7 8 9  temp=6
                      0 2 1 3 4 6 5 7 8 9  gap=1
                      0 2 2 3 4 6 5 7 8 9  temp=1
                      0 1 2 3 4 6 5 7 8 9
                      0 1 2 3 4 6 6 7 8 9  temp=5
                      0 1 2 3 4 5 6 7 8 9
```

希尔排序(从小到大)的Java实现如下：

```java
public static void shellSort(int[] nums) {
    int temp, length = nums.length;
    for (int gap = length/2; gap > 0; gap /= 2) {
        for (int i = 0; i < gap ; i++) {
            for (int j = i + gap; j < length; j += gap) {
                temp = nums[j];
                int k = j;
                for (; k >= gap&&temp < nums[k - gap]; k -= gap) {
                    nums[k] = nums[k - gap];
                }
                nums[k] = temp;
            }
        }
    }
}
```

### 冒泡排序
冒泡排序，是一种简单的排序算法。因其排序过程中较大(或小)元素会慢慢“浮到”顶部，就像鱼吐泡泡而得名。它的工作原理如下：重复地遍历要排序的序列，一次比较两个元素，如果它们的顺序错误就把它们交换过来，直到序列有序。冒泡排序是稳定的排序算法，最坏时间复杂度是O(n^2)，最优时间复杂度是O(n)，平均时间复杂度是O(n^2)。

下面是对{1, 3, 5, 7, 9, 2, 4, 6, 8, 0}进行冒泡排序的具体过程

```
                      |1 3 5 7 9 2 4 6 8 0
                      |1 3 5 7 9 2 4 6 0 8
                      |1 3 5 7 9 2 4 0 6 8
                      |1 3 5 7 9 2 0 4 6 8
                      |1 3 5 7 9 0 2 4 6 8
                      |1 3 5 7 0 9 2 4 6 8
                      |1 3 5 0 7 9 2 4 6 8
                      |1 3 0 5 7 9 2 4 6 8
                      |1 0 3 5 7 9 2 4 6 8
                      0 1| 3 5 7 9 2 4 6 8
                      0 1| 3 5 7 2 9 4 6 8
                      0 1| 3 5 2 7 9 4 6 8
                      0 1| 3 2 5 7 9 4 6 8
                      0 1 2 3| 5 7 9 4 6 8
                      0 1 2 3| 5 7 4 9 6 8
                      0 1 2 3| 5 4 7 9 6 8
                      0 1 2 3 4 5| 7 9 6 8
                      0 1 2 3 4 5| 7 6 9 8
                      0 1 2 3 4 5 6 7| 9 8
                      0 1 2 3 4 5 6 7 8 9|
```
冒泡排序(从小到大)的Java实现如下：

```java
public static void bubbleSort(int[] nums) {
    int length = nums.length;
    for (int i = 0; i < length; i++) {
        for (int j = length - 1; j > i; j--) {
            if (nums[j - 1] > nums[j]) {
                int temp = nums[j - 1];
                nums[j - 1] = nums[j];
                nums[j] = temp;
            }
        }
    }
}
```

### 快速排序
快速排序是由东尼·霍尔所发展的一种排序算法。在平均状况下，排序 n 个项目要Ο(nlogn)次比较。在最坏状况下则需要Ο(n^2)次比较，但这种状况并不常见。事实上，快速排序通常明显比其他Ο(nlogn) 算法更快，因为它的内部循环（inner loop）可以在大部分的架构上很有效率地被实现出来，且在大部分真实世界的数据，可以决定设计的选择，减少所需时间的二次方项之可能性。快速排序是不稳定的排序算法，最坏时间复杂度是O(n^2)，最优时间复杂度是O(nlogn)，平均时间复杂度是O(nlogn)。

下面是对{1, 3, 5, 7, 9, 2, 4, 6, 8, 0}进行快速排序(递归版)的具体过程

```
                            第一遍循环
                            取pivot=1
                            1 3 5 7 9 2 4 6 8 0
                            i                 j
                            先从尾部j开始，找到比1小的数字往i的位置复制
                            0 3 5 7 9 2 4 6 8 0
                              i               j
                            0比1小，被复制到i的位置，复制之后i++
                            0 3 5 7 9 2 4 6 8 3
                              i             j
                            这时候要从头部i开始，找到比1大的数字往j的位置复制
                            3比1大，被复制到j的位置，复制之后j--
                            0 3 5 7 9 2 4 6 8 3
                              i
                              j
                            再次从j开始寻找比1小的数字，但是没找到，直到i和j相遇（i=j）
                            第一遍循环结束
                            0 1 5 7 9 2 4 6 8 3
                            把pivot复制到循环结束时i的位置
                            此时pivot把数组分成两部分{0}和{5, 7, 9, 2, 4, 6, 8, 3}
                         
                       第二次循环分两部分进行
                       
       第一部分                                     第二部分
       0                                          取pivot=5
       只有一个数，不用排序                           5 7 9 2 4 6 8 3
       第一部分结束                                 i             j
                                                  和第一遍循环一样先从j开始...
                                                   3 7 9 2 4 6 8 3
                                                     i           j
                                                   3 7 9 2 4 6 8 7
                                                     i         j
                                                   3 4 9 2 4 6 8 7
                                                       i   j
                                                   3 4 9 2 9 6 8 7
                                                       i
                                                         j
                                                   3 4 2 2 9 6 8 7
                                                         i
                                                         j
                                                   3 4 2 5 9 6 8 7
                                                   第二遍循环结束
                                                   只有第二部分进行第三次循环
                                                   此时pivot把数组分成两部分{3, 4, 2}和{9, 6, 8, 7}
                                                   不断循环，排序，分组，直到最后每一组都只剩1个数
                      最后“将所有组合并”（实际上数组没有分组，只是每次对部分数据进行操作）
                      得到{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
```
快速排序(递归版、从小到大)的Java实现如下：

```java
public static void recursiveQuickSort(int[] nums, int head, int tail) {
    int i = head, j = tail;
    int pivot = nums[head];
    while (i < j) {
        while (i < j) {
            if (pivot >= nums[j]) {
                nums[i++] = nums[j];
                break;
            }
            j--;
        }
        while (i < j) {
            if (pivot <= nums[i]) {
                nums[j--] = nums[i];
                break;
            }
            i++;
        }

    }
    nums[i] = pivot;
    if (i - 1 - head > 0) {
        recursiveQuickSort(nums, head, i - 1);
    }
    if (tail - i - 1 > 0) {
        recursiveQuickSort(nums, i + 1, tail);
    }
}
```

**[快速排序非递归版](http://egoistk21.xyz/2017/02/17/Java%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E4%B8%93%E9%A2%98%E7%BB%AD/#快速排序)**

### 归并排序
[Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)这道题我用的排序法就是归并排序，归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法的一个非常典型的应用。它的原理如下：先申请一个空间用于存储排序后的序列，大小为两个已经排序的序列大小之和。在两个已经排序的序列头部分别放置指针，比较指针所指元素的大小，较小的(或较大的)复制到刚刚申请的新序列空间，该指针后移，重复比较、复制到新序列尾部、后移指针，直到遍历完其中一个序列，则另一个序列的剩余元素全部原序复制到新序列尾部。归并排序是稳定的排序算法，最坏时间复杂度是O(nlogn)，最优时间复杂度是O(n)，平均时间复杂度是O(nlogn)，需要O(n)额外空间。

下面是对{1, 3, 5, 7, 9, 2, 4, 6, 8, 0}进行归并排序(递归版)的具体过程

```
                           {1 3 5 7 9 2 4 6 8 0}
     第一层递归         {1 3 5 7 9}    |     {2 4 6 8 0}
     第二层递归       {1 3 5} | {7 9}  |   {2 4 6} | {8 0}
     第三层递归     {1 3} |{5}|{7}|{9} |  {2 4}|{6}|{8}|{0}
     第四层递归    {1}|{3}|{5}|{7}|{9} |{2}|{4}|{6}|{8}|{0}
     第一层归并     {1 3} |{5}| {7 9}  |  {2 4}|{6}| {0 8}
     第二层归并       {1 3 5} | {7 9}  |   {2 4 6} | {0 8}
     第三层归并         {1 3 5 7 9}    |     {0 2 4 6 8}
     第四层归并              {0 1 2 3 4 5 6 7 8 9}             
```
归并排序(递归版、从小到大)的Java实现如下：

```java
public static void merge(int[] nums, int head, int median, int tail) {
    int[] nums1 = new int[median - head + 1];
    int[] nums2 = new int[tail - median];
    int length1 = nums1.length, length2 = nums2.length;
    System.arraycopy(nums, head, nums1, 0, length1);
    System.arraycopy(nums, median + 1, nums2, 0, length2);
    int i = 0, j = 0, k = head;
    while (i < length1&&j < length2) {
        nums[k++] = (nums1[i] < nums2[j])?nums1[i++]:nums2[j++];
    }
    while (i < length1) {
        nums[k++] = nums1[i++];
    }
    while (j < length2) {
        nums[k++] = nums2[j++];
    }
}

public static void recursiveMergeSort(int[] nums, int head, int tail) {
    int median = (head + tail)/2;
    if (median != tail) {
        recursiveMergeSort(nums, head, median);
        recursiveMergeSort(nums, median + 1, tail);
    }
    merge(nums, head, median, tail);
}
```

**[归并排序非递归版](http://egoistk21.xyz/2017/02/17/Java%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E4%B8%93%E9%A2%98%E7%BB%AD/#归并排序)**

### 堆排序
堆排序，是指利用堆这种数据结构所设计的一种排序算法。堆排序的时间复杂度为O(nlogn)，是不稳定的排序算法，且具有空间原址性：任何时候都只需要常数个额外的元素空间存储临时数据。
[二叉堆图文介绍 参考](http://www.cnblogs.com/skywang12345/p/3610390.html)

[堆排序图文介绍 参考](http://www.cnblogs.com/skywang12345/p/3602162.html)

堆排序(大根堆、从小到大)的Java实现如下：

```java
public static void maxHeapDown(int[] nums, int head, int tail) {
    int p = head, l = 2*p + 1, r = l + 1;
    int tmp = nums[p];
    for (; l <= tail; p = l,l = 2*l + 1,r = l + 1) {
        if (l < tail && nums[l] < nums[r]) {
            l = r;
        }
        if (tmp >= nums[l]) {
            break;
        } else {
            nums[p] = nums[l];
            nums[l]= tmp;
        }
    }
}

public static void heapSort(int[] nums) {
    int i, tmp, length = nums.length;
    for (i = length/2 - 1; i >= 0; i--) {
        maxHeapDown(nums, i, length - 1);
    }
    for (i = length - 1; i > 0; i--) {
        tmp = nums[0];
        nums[0] = nums[i];
        nums[i] = tmp;
        maxHeapDown(nums, 0, i - 1);
    }
}
```
**以上7种常见排序都是基于比较的排序，基于比较的排序的时间复杂度的极限是O(nlogn)，下面我给大家介绍三种基于计算的排序算法，它们都是线性的，时间复杂度为O(n)。**

### 桶排序
桶排序的思想特别简单，就是找出所给序列中最大的元素，新建一个大小为最大元素加一的序列并初始化为全0，所给序列中元素的大小与新建的序列的下标相对应，遍历所给序列，每遇到一个元素，以这个元素为下标的新序列的元素就自加1。桶排序是稳定的排序算法，时间复杂度为O(n)，需要O(k)额外空间。

下面我们分析一下对数组{1, 3, 5, 7, 9, 2, 4, 6, 8, 0， 0}进行桶排序的过程

1、遍历所给数组得到最大元素9

2、新建一个长度为9+1的数组，并初始化为全0 {0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}

3、遍历所给数组，遇到第一个元素1，将新建数组中下标为1的元素自加1 {0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0}

4、同理，遇到第二个元素3，将新建数组中下标为3的元素自加1 {0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0}

5、遍历完成后，新数组为 {2, 1, 1, 1, 1, 1, 1, 1, 1, 1}

6、遍历新数组，当遇到非零元素时，为所给数组赋予非零元素个的下标的值，如非零元素2，下标为0，则对所给序列的前两个元素赋值0

桶排序(从小到大)的Java实现如下：

```java
public static int maxElemOfNums(int[] nums) {
    int max = nums[0];
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] > max) {
            max = nums[i] + 1;
        }
    }
    return max;
}

public static void bucketSort(int[] nums) {
    int length = nums.length, max = maxElemOfNums(nums), i, j;
    int[] bucket = new int[max];
    for (i = 0; i < length; i++) {
        bucket[nums[i]]++;
    }
    for (i = 0,j = 0; i < max; i++) {
        while ((bucket[i]--) > 0) {
            nums[j++] = i;
        }
    }
}
```

### 计数排序
计数排序是对桶排序的一种改进，其基本思想是：对于给定序列中的元素x，确定小于(大于)x的元素个数。利用这一信息，可以直接把x放到它在输出序列中的位置上。计数排序是稳定的排序算法，时间复杂度为O(n+k)，需要O(n+k)额外空间。

计数排序(从小到大)的Java实现如下：

```java
public static void countSort(int[] nums) {
    int length = nums.length, max = maxElemOfNums(nums), i, j;
    int[] temp = new int[length];
    System.arraycopy(nums, 0, temp, 0, length);
    int[] bucket = new int[max];
    for (i = 0; i < length; i++) {
        bucket[temp[i]]++;
    }
    for (i = 1; i < max; i++) {
        bucket[i] += bucket[i - 1];
    }
    for (i = 0; i < length; i++) {
        nums[bucket[temp[i]] - 1] = temp[i];
        bucket[temp[i]]--;
    }
}
```

### 基数排序
基数排序是对计数排序的一种改进，它是这样实现的：将所有待比较数值(正整数)统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序(计数排序)。这样从最低位排序一直到最高位排序完成以后，数列就变成一个有序序列。基数排序是稳定的排序算法，时间复杂度为O(kn)，需要O(n)额外空间。

基数排序(从小到大)的Java实现如下：

```java
public static void radixSort(int[] nums) {
    int exp, length = nums.length, max = maxElemOfNums(nums), i;
    for (exp = 1; (max - 1)/exp > 0; exp *= 10) {
        int[] temp = new int[length];
        int[] buckets = new int[max];
        for (i = 0; i < length; i++) {
            buckets[(nums[i]/exp)%10]++;
        }
        for (i = 1; i < max; i++) {
            buckets[i] += buckets[i - 1];
        }
        for (i = length - 1; i >= 0; i--) {
            temp[buckets[(nums[i]/exp)%10] - 1] = nums[i];
            buckets[(nums[i]/exp)%10]--;
        }
        System.arraycopy(temp, 0, nums, 0, length);
    }
}
```

