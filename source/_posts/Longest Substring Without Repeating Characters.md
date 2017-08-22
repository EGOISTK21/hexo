---
title: Longest Substring Without Repeating Characters
date: 2016-09-06 16:44:22
tags: [Java, Algorithm, LeetCode]
---

## 最长无重字符子串

给定一个字符串，找到最长子串的长度不重复的字符。

例子：

给定“abcabcbb”，答案是“abc”，它的长度为3。

给定“bbbbb”，答案为“b”，长度为1。

给定“pwwkew”，答案是“wke”，长度为3。

注意答案一定是一个字符串的长度，“pwke”是一个序列，而不是子串。
<!-- more -->

---
这是我第一遍刷算法题，也是第一遍刷Leetcode。碰到这个字符串的问题，一开始我有点懵，这对我太有难度了。但是我忍住没有去看Editorial，一点点自己码。嗯，这是一个好的开端。

题目要求我们获得最长无重子串，所以我从最长入手，用一个变量currLength从最长子串开始递减，再用两个变量i，j遍历currLength子串中是否有重复字符，若有则currLength自减，若无则返回currLength为最长无重子串的长度。若字符串本身长度小于2则直接返回字符串本身长度。但是测试数据的时候出现了问题，原来我忘了考虑currLength自减后，currLength长度的子串不止一个。比如“abcabcbb”的长度为8，它的长度为7的子串有“abcabcb”和“bcabcbb”，两个。所以我又加了一层循环用来**滑动**currLength的子串。

下面就是我的第一个方法：

``` java
public static int lengthOfLongestSubstring(String s) {
    char c;
    boolean flag = false;
    int length = s.length(), currLength = 0;
    if (length < 2) return length;
    //滑动字符子串长度currLength
    for (currLength = length; currLength > 1; currLength--) {
        //滑动字符子串位置head
        for (int head = 0; head <= length - currLength; head++) {
            //滑动字符子串内遍历查询重复字符
            for (int i = head; i < head + currLength - 1; i++) {
                c = s.charAt(i);
                for (int j = i + 1; j < head + currLength; j++) {
                    //如果遍历中遇到重复字符，则跳出循环，字符子串右移一位
                    if (c == s.charAt(j)) {
                        flag = true;
                        break;
                    }
                }
                if (flag) {
                    flag = false;
                    break;
                }
                //如果遍历结束,字符子串内无重复字符，返回当前长度currLength
                if (i == head + currLength - 2) return currLength;
            }
        }
    }
    return currLength;
}    
```

但是这段代码提交后被rejected了。原因是时间复杂度太大，当遇到字符串本身超长且最长无重子串较短时，所需计算时间太长。四层循环，时间复杂度为O(n^4)。所以我需要改进我的算法，从减少循环层数开始，我称之为更加线性化的算法。

这次我从第一个字符开始，用**双指针**的方法，双指针首先指向第m个和第n个字符（m<n），若不相同则尾指针右移；若相同则头指针右移跳出比较循环同时尾指针也右移，因为尾指针所指字符只和头指针所指字符相同，头指针所指字符和**头指针与尾指针之间的字符**都不相同。另外当遍历到最后一个字符的时候，返回maxLength和currLength中较长者。

代码如下：

``` java
public static int lengthOfLongestSubstring(String s) {
    char c;
    boolean flag = false;
    int head = 0, tail = 1, curr, length = s.length(), currLength = 0, maxLength = 0;
    if (length < 2) return length;
    //线性遍历整个字符串s，每次遇到重复字符，记录当前无重字符子串长度并与maxLength比较取大，字符子串头部head右移一位
    for (; head < length - currLength; head++) {
        for (; tail < length; tail++) {
            c = s.charAt(tail);
            for (curr = head; curr < tail; curr++) {
                if (c == s.charAt(curr)) {
                    currLength = tail - head;
                    maxLength = (maxLength > currLength)?maxLength:currLength;
                    head = curr + 1;//head重定位
                                   //tail不重新赋值，也会自加，相当于++tail 
                    flag = true;
                    break;
                }
            }
            if (flag) {
                flag = false;
                continue;
            }
            if (tail == length - 1 && curr == tail) {
                currLength = tail - head + 1;
                return (maxLength > currLength)?maxLength:currLength;
            }
        }
    }
    return maxLength;
}
```

这次提交后总算accepted了，**爽！**

但是隐约中我觉得还有更好的算法，但是目前自己技术有限，等以后再来填坑～

**谢谢阅读**