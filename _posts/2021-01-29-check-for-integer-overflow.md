---
layout: post
title:  "check for integer overflow"
date:   2021-01-29 15:38:00 +0800
categories: Computer Science
---

# 检查整数类型相加是否溢出

如果**两个符号不相同的整数**相加，和的绝对值**必定小于等于**其中一个加数的绝对值，因此不可能出现数值溢出的情况；

如果**两个符号相同的整数**相加，才有可能出现数值溢出的情况。



[TOC]




## 方法一

### 1. 两个符号相同的整数相加，如果结果的符号位改变，说明数值溢出

#### 两个正整数相加

32位的整数类型表示的正整数最大值（2^31 - 1）的原码表示为：

```
0111 1111 1111 1111 1111 1111 1111 1111
```

正数的源码和补码相同，所以补码表示为：

```
0111 1111 1111 1111 1111 1111 1111 1111
```

当两个正整数相加时，如果**数值溢出**，其结果总是在以下范围内（补码表示）：

```
1000 0000 0000 0000 0000 0000 0000 0000 = MAX_VALUE + 1
<= a1 + a2 <= 
1111 1111 1111 1111 1111 1111 1111 1110 = MAX_VALUE + MAX_VALUE
```

可知在数值溢出的情况下，结果的符号位和正整数的符号位**总是相反**。

#### 两个负整数相加

32位的整数类型表示的负整数最大值（-2^31）的原码表示为：

```
(1000) 1000 0000 0000 0000 0000 0000 0000 0000
```

负数的补码是对原码的数值位按位取反再加1，所以补码表示为：

```
(0111) 1000 0000 0000 0000 0000 0000 0000 0000
```

当两个负整数相加时，如果**数值溢出**，其结果总是在以下范围内（补码表示）：

```
(0001) 0111 1111 1111 1111 1111 1111 1111 1111
= 1000 0000 0000 0000 0000 0000 0000 0000 + 1111 1111 1111 1111 1111 1111 1111 1111
= MAX_VALUE + (-1)
>= a1 + a2 >= 
(0001) 0000 0000 0000 0000 0000 0000 0000 0000
= 1000 0000 0000 0000 0000 0000 0000 0000 + 1000 0000 0000 0000 0000 0000 0000 0000
= MAX_VALUE + MAX_VALUE
```

可知在数值溢出的情况下，结果的符号位和负整数的符号位**总是相反**。

#### 代码实现

不论加数的符号是什么，以下代码都可以检查两个整数类型相加是否溢出：

```
int x = a + b;

if ((x^a) < 0 && (x^b) < 0) {
    // overflow
}
```



## 方法二

利用整数类型的最大值或者最小值减去某个加数，其结果和另一个加数的大小进行比较。

代码实现如下：

```
// 检查两个正整数相加是否溢出
if (a > 0 && b > 0 && a > MAX_VALUE - b) {
    // overflow
}

// 检查两个负整数相加是否溢出
if (a < 0 && b < 0 && a < MIN_VALUE - b) {
    // overflow
}
```


