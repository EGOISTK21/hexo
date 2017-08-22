---
title: Longest Palindromic Substring
date: 2016-09-23 23:27:11
tags: [Java, Algorithm, LeetCode]
---

## 最长回文字符子串

给定一个字符串S，找出S中最长的回文字符子串。

一开始我想用递归，现检验整个字符串longestPalindrome(String s)，若非回文字，则调用自身，传入s.subString(1, s.length)和s.subString(0, s.length - 1)，即不断的左减一，右减一，检验其子串是否回文字。但是我想不好怎么接收return值，所以就用了很水的方法来检验，即对于遍历的每一个字符，检查其左右是否为回文字，这样的回文字判断也分奇数长度的和偶数长度的，如“121”和“1221”。具体实现代码如下：

<!-- more -->

```java
public static String longestPalindrome(String s) {
    if (s.length() < 2) return s;
    String longerPalindrome = "";
    int length = s.length(), i, j, k;
    for (i = 1; i < length; i++) {
        if (s.charAt(i - 1)==s.charAt(i)) {
            j = i - 1;
            k = i;
            while (j >= 0&&k < length) {
                if (s.charAt(j) == s.charAt(k)) {
                    j--;
                    k++;
                } else {
                    break;
                }
            }
            if ((k - j - 1) > longerPalindrome.length()) {
                longerPalindrome = s.substring(j + 1, k);
            }
        }
        if (i < length - 1&&s.charAt(i - 1)==s.charAt(i + 1)) {
            j = i - 1;
            k = i + 1;
            while (j >= 0&&k < length) {
                if (s.charAt(j) == s.charAt(k)) {
                    j--;
                    k++;
                } else {
                    break;
                }
            }
            if ((k - j - 1) > longerPalindrome.length()) {
                longerPalindrome = s.substring(j + 1, k);
            }
        }
    }
    return longerPalindrome;
}
```

**这样的方法竟然给水过了，我想去学习一下字符串匹配的KMP算法，以后有机会在博客中分享。**