---
title: Java排序算法专题续
date: 2017-02-17 15:40:01
tags: [Java, Algorithm, LeetCode]
---

距离上一篇博客更新已经小半年了，这期间我懒懒散散，整天沉迷于Wi-Fi暴力破解、VPS的shadowsocks配置、搭建私有云之类的奇技淫巧，深刻领悟了Linux的魅力，也终于迫于找实习的压力重新开始写代码，更博客。其实上一篇博客之后，原本有一些笔试大题的解答，但那时我还没有学得算法的皮毛，净是些暴力方法，写着写着烂尾了不好意思发布出来。等我最近学习算法的期间若想到了更好的解法，便一并贴出。

<!-- more -->

我的上一篇《Java排序算法专题》收到了好多读者的喜爱，其中仍有多处不足及错误，我会在学习的过程当中不断地进行改进。今天我给大家带来之前承诺过的几种递归排序算法的非递归实现。

### 快速排序

```java
public class Sort {

    private class Region {
        int low;
        int high;

        Region(int low, int high) {
            this.low = low;
            this.high = high;
        }
    }

    public void nonRecursiveQuickSort(int[] nums) {
        int i, j, pivot;
        Region region;
        Stack<Region> regions = new Stack<>();
        regions.add(new Region(0, nums.length - 1));
        while (!regions.empty()) {
            region = regions.pop();
            i = region.low;
            j = region.high;
            pivot = nums[i];
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
            if (i > region.low + 1) regions.add(new Region(region.low, i - 1));
            if (i < region.high - 1) regions.add(new Region(i + 1, region.high));
        }
    }

    public static void main(String[] args) {
        int[] a = {49,38,65,97,76,13,27,49};
        new Sort().nonRecursiveQuickSort(a);
        for (int i:a) System.out.println(i);
    }
}
```

### 归并排序

````java
public void nonRecursiveMergeSort(int[] nums) {
    int loop = (int) Math.ceil(Math.log(nums.length)/Math.log(2));
    Stack<int[]> stack1 = new Stack<>();
    Stack<int[]> stack2 = new Stack<>();
    for (int num : nums) {
        int[] a = {num};
        stack1.push(a);
    }
    while (loop > 0) {
        while (!stack1.empty()) {
            if (stack1.size() > 1) stack2.push(merge(stack1.pop(), stack1.pop()));
            else if (stack1.size() == 1) stack2.push(stack1.pop());
        }
        loop--;
        if (loop == 0) System.arraycopy(stack2.pop(), 0, nums, 0, nums.length);
        while (loop > 0&&!stack2.empty()) {
            if (stack2.size() > 1) stack1.push(merge(stack2.pop(), stack2.pop()));
            else if (stack2.size() == 1) stack1.push(stack2.pop());
        }
        loop--;
        if (loop == 0) System.arraycopy(stack1.pop(), 0, nums, 0, nums.length);
    }
}

public int[] merge(int[] nums1, int[] nums2) {
    int length1 = nums1.length, length2 = nums2.length;
    int[] nums3 = new int[length1 + length2];
    int i = 0, j = 0, k = 0;
    while (i < length1&&j < length2) {
        nums3[k++] = (nums1[i] < nums2[j])?nums1[i++]:nums2[j++];
    }
    while (i < length1) {
        nums3[k++] = nums1[i++];
    }
    while (j < length2) {
        nums3[k++] = nums2[j++];
    }
    return nums3;
}
````

