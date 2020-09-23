---
layout: post
title: Google面试准备
date:   2019-03-02 01:00:00 +0800
categories: 计算机技术
tag: 刷题
---

<h1>LeetCode 187. Repeated DNA Sequences</h1>

首先这道题可以用HashMap来做, 每次截取长度为10的字符串， 放在HashMap里面去, HashMap的key是String, value是整数,

一个字符在UTF-8里面, 一个英文字符等于一个字节，一个中文（含繁体）等于三个字节, 所以10个英文字符, 就是相当于10 * 3 = 30个字节 就相当于240位

计算机是美国人发明的, 因此, 最早只有127个字符被编码到计算机里, 也就是大小写英文字母, 数字和一些符号, 这个编码表被称为ASCII编码, 比如大写字母A的编码是65, 小写字母z的编码是122，但是处理中文显然一个字节是不够的,至少需要2个字节, 中国制定了GB2312编码，用来把中文编进去。但可以想到的是全世界有上百种语言, 日本把日文编辑到Shift_JIS里面,各国都有各国的标准,所以Unicode就应运而生了,Unicode是把所有的语言都统一到一套编码中.

最新Python 3的版本中,字符串是以Unicode编码的


由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。

2. 按位或运算 按位或运算符“|”是双目运算符。其功能是参与运算的两数各对应的二进位相或。只要对应的二个二进位有一个为1时，结果位就为1。参与运算的两个数均以补码出现。
例如：9|5可写算式如下： 00001001|00000101
00001101 (十进制为13)可见9|5=13

<h1>Guess The Word</h1>
题目大致的意思就是在一个wordlist里面, 我们有一个exactly word是match的，如果我们随机的去猜一个数，会返回多个字符匹配exactly这个单词的

<h1>LeetCode 904: Fruit Into Baskets</h1>

题意是有一排树, 第ith tree的数组的值tree[i], 代表tree type = tree[i]

在数组里面随机选一个数,然后随机的去重复以下步骤,

1. 往篮子里面放tree
2. 看右边的树, 如果右边没有树了，就停止

perform step 1 and step 2, then back to step 1, and so on until you stop

之后你只有2个篮子, 这2个篮子里面可以任意数量的的水果, 返回

example 1:
Input: [1, 2, 1]
Output: 3
Explanation: We can collect [1,2,1]


本质上就是longest substring with at most two characters

思路就是创建一个HashMap 如果map的size小于2的时候，放进去map中，key是tree[i], value是index
当map的size大于 2的时候, 获取到min_index of tree type, 这个必须是在 if len(map) > 2的statement中
因为min_index 每次都需要更新

```
class Solution {
    public int totalFruit(int[] tree) {
        HashMap<Integer, Integer> map = new HashMap<>();

        int i = 0;
        int j = 0;
        int max = 0;
        while (j < tree.length) {
            if (map.size() <= 2) {
                map.put(tree[j], j++);
            }
            if (map.size() > 2) {
                int minIndex = tree.length - 1;
                for (Integer value: map.values()) {
                    minIndex = Math.min(minIndex, value);
                    i = minIndex + 1;
                }
                map.remove(tree[minIndex]);
            }

            max = Math.max(max, j - i);
        }
        return max;
    }
}
```

