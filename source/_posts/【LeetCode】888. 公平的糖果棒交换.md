---
title: 【LeetCode】888. 公平的糖果棒交换
date: 2021年2月01日15:43:12
updated: 
type:	
comments: true
description:
keywords: KeyWords
mathjax: 
katex:
aside:
aplayer:
highlight_shrink:
tags: 数组
categories: LeetCode
cover: https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2205532744,1760304010&fm=11&gp=0.jpg
copyright_author: Bear 
copyright_author_href: https://329821258.github.io/
copyright_url: 
copyright_info: 

---

## 题目描述：

爱丽丝和鲍勃有不同大小的糖果棒：A[i] 是爱丽丝拥有的第 i 根糖果棒的大小，B[j] 是鲍勃拥有的第 j 根糖果棒的大小。

因为他们是朋友，所以他们想交换一根糖果棒，这样交换后，他们都有相同的糖果总量。（一个人拥有的糖果总量是他们拥有的糖果棒大小的总和。）

返回一个整数数组 ans，其中 ans[0] 是爱丽丝必须交换的糖果棒的大小，ans[1] 是 Bob 必须交换的糖果棒的大小。

如果有多个答案，你可以返回其中任何一个。保证答案存在。



### 示例1

```
输入：A = [1,1], B = [2,2]
输出：[1,2]
```

### 示例2

```
输入：A = [1,2], B = [2,3]
输出：[1,2]
```

### 示例3

```
输入：A = [2], B = [1,3]
输出：[2,3]
```

### 示例4

```
输入：A = [1,2,5], B = [2,4]
输出：[5,4]
```

**提示：**

​	1 <= A.length <= 10000
​	1 <= B.length <= 10000
​	1 <= A[i] <= 100000
​	1 <= B[i] <= 100000
​	保证爱丽丝与鲍勃的糖果总量不同。
​	答案肯定存在。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fair-candy-swap
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

## 解题思路：

得到A集合和B集合的总值，然后相减得到差值sub(可能是负数)。

要使得A B两集合总和相等，则A集合需要增加/减少 sub/2，同样的B集合需要减少/增加 sub/2。

假定A交换值为a,B为b,当A B两集合交换时 A集合总和增加了a-b(可能是负数)

所以得出 sub/2 = a-b;

``` JAVA
public int[] fairCandySwap(int[] A, int[] B) {
        //记录差值
        int sub = 0;
        Set<Integer> set = new HashSet<>();
        //遍历A数组 计算A数组的总和 存放A集合的值 用于后续获取
        for (int i : A) {
            sub+=i;
            set.add(i);
        }
        //遍历B数组 计算B数组和A数组的差值
        for (int i : B) {
            sub-=i;
        }
        //得到实际差值
        sub=sub/2;
        //由于题目说了必定有答案 所以可以通过反推来得到另一个的值
        for (int i : B) {
            //得到反推算后的值
            int temp = sub+i;
            //存在即返回
            if(set.contains(temp)){
                return new int[]{temp,i};
            }
        }
        return null;
    }
```

