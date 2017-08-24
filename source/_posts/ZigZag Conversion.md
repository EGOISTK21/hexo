---
title: ZigZag Conversion
date: 2016-09-29 15:30:11
tags: [Java, Algorithm, LeetCode]
---

## 锯齿形转换

字符串“PAYPALISHIRING”以给定数字的行数被写入锯齿形图案：

```html
P   A   H   N
A P L S I I G
Y   I   R    
```

然后逐行读取得“PAHNAPLSIIGYIR”。

给定一个字符串，并给出了一个行数，

string convert(string text, int nRows);

如convert("PAYPALISHIRING", 3)这个转换应返回“PAHNAPLSIIGYIR”。

<!-- more -->

---

这个问题困扰了我好久，因为一开始我没看懂ZigZag（锯齿形）这个词。我以为题目要求给定行数，以行数，一个，行数一个······的形式输出，9月30日折腾了一晚上就是没通过，然后，国庆一直在偷懒。昨天（7号）晚上试着提交了一下，然后用custom test得出的正确答案逆推了一下，结果，就是发现了锯齿形这个东西。不怕题难，就怕看错题。现在题意弄清楚了，我们开始吧。

我用两种方法实现了，第一种是比较容易想到的：既然是锯齿形的，那我用一个二维数组记录字符存储的位置就好了。二维数组先全部初始化为-1，再把字符在字符串中的位置序号按锯齿形填进去。这里比较难的就是这个二维数组的第二维长度，下面是第二维长度numColumns的计算公式，其中length是给定的字符串的长度，numRows是给定的行数即第一维长度。

numColumns = length / (2 * numRows - 2) * (numRows - 1) + (length % (2 * numRows - 2) == 0 ? 0 :  (length % (2 * numRows - 2) > numRows ? length % (2 * numRows - 2) - numRows + 1 : 1));

首先length / (2 * numRows - 2)是锯齿形中完整锯齿 V的个数（隐式强制转换，即商取整），然后乘上(numRows - 1) 得到的是除去最后一个不完整的锯齿V（若存在），最后一个不完整的锯齿V如果只有第一条边，则加1；如果含有V的第二条边，则加length % (2 * numRows - 2) - numRows + 1。

这种方法的Java实现如下：

```java
public static String convert(String s, int numRows) {
    if (numRows == 1) return s;
    String ans = "";
    int i, j, k = 0, map[][], numColumns, length = s.length();
    numColumns = length < numRows? length : length / (2 * numRows - 2) * (numRows - 1)
      + (length % (2 * numRows - 2) == 0 ? 0 : (length % (2 * numRows - 2) > numRows ?
        length % (2 * numRows - 2) - numRows + 1 : 1));
    map = new int[numRows][numColumns];
    for (i = 0; i < numRows; i++) {
        for (j = 0; j < numColumns; j++) {
            map[i][j] = -1;
        }
    }
    i = j = 0;
    while (k < length) {
        while (i < numRows - 1 && k < length) {
            map[i++][j] = k++;
        }
        while (i > 0 && j < numColumns && k < length) {
            map[i--][j++] = k++;
        }
    }
    for (i = 0; i < numRows; i++) {
        for (j = 0; j < numColumns; j++) {
            if (map[i][j] != -1) {
              ans += s.charAt(map[i][j]);
            }
        }
    }
    return ans;
}
```

下面我们来介绍第二种方法，这种方法不按图形规律去算， 而是按数字规律去算，即最后输出的字符串中每个字符在原给定字符串中的位置序号的规律。这里就不再阐述了，留给读者自己观察发现。

这种方法的Java实现如下：

```java
public static String convert(String s, int numRows) {
    if (numRows == 1) return s;
    String ans = "";
    int i, j, length = s.length();
    for (i = 0; i < numRows; i++) {
        for (j = i; j < length; j += 2 * numRows -2) {
            ans += s.charAt(j);
            if (i != 0 && i != numRows - 1 && j + 2 * numRows - 2 * i - 2 < length) {
                ans += s.charAt(j + 2 * numRows - 2 * i - 2);
            }
        }
    }
    return ans;
}
```