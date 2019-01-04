---
layout: post
title: MINI_Parser
date:   2018-12-27 00:00:00 +0800
categories: document
tag: 教程
---

* content

MINI Parser 这道题是有2钟解法，第一种解法是递归的解法,m==--l
{:toc}

{% highlight python %}
class Solution(object):
    def deserialize(self, s):
        """
        :type s: str
        :rtype: NestedInteger
        """
        if '[' != s[0]: return NestedInteger(value=int(s))
        res = NestedInteger()
        buff = ''
        left = 0
        for c in s[1:-1]:
            if c == ',' and left == 0:
                res.add(self.deserialize(buff))
                buff = ''
            else:
                buff += c
                if c == '[':
                    left += 1
                elif c == ']':
                    left -= 1
        print buff
        if buff: res.add(self.deserialize(buff))
        return res

class Solution:
    def deserialize(self, s):
        stack, num, last = [], "", None
        for c in s:
            if c.isdigit() or c == "-": num += c
            elif c == "," and num:
                stack[-1].add(NestedInteger(int(num)))
                num = ""
            elif c == "[":
                elem = NestedInteger()
                if stack: stack[-1].add(elem)
                stack.append(elem)
            elif c == "]":
                if num:
                    stack[-1].add(NestedInteger(int(num)))
                    num = ""
                last = stack.pop()
        return last if last else NestedInteger(int(num))

{% endhighlight %}
