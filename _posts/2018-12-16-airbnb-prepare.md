---
layout: post
title:  "airbnb面试"
date:   2018-12-17 13:31:01 +0800
categories: jekyll
tag: jekyll
---

* content
{:toc}
<li><a href="#one">Leetcode 125. Valid Palindrome</a></li>
<li><a href="#second">Leetcode 336. Palindrome Pairs</a></li>
<li><a href="#third">Round Numbers</a></li>
<li><a href="#fourth">Leetcode 251. Flatten 2D Vector</a></li>
<li><a href="#fifth">Leetcode IP to CIDR</a></li>
<li><a href="#sixth">Menu Order(Double Combination Sum)</a></li>
<li><a href="#seventh">Leetcode 269. Alien Dictionary</a></li>
<li><a href="#eighth">Parse CSV</a></li>
<li><a href='nineth'>Perference List</a></li>




https://yezizp2012.github.io/2017/06/01/airbnb%E9%9D%A2%E8%AF%95%E9%A2%98%E6%B1%87%E6%80%BB/


<h3 id = 'one'>Leetcode 125. Valid Palindrome</h3>
{% highlight python %}
class Solution(object):
    def isPalindrome(self, s):
        """
        :type s: str
        :rtype: bool
        """
        l = 0
        r = len(s) - 1
        while l < r:
            while l < r and not s[l].isalnum():
                l += 1
            while l < r and not s[r].isalnum():
                r -= 1
            
            if s[l].lower() != s[r].lower():
                return False
            l += 1
            r -= 1
        return True
{% endhighlight %}

<h3 id = 'second'>Leetcode 336. Palindrome Pairs</h3>
第一种方法就是brute force的做法，判断average string的长度是L的话，时间复杂度就是 O(n\*n\*l)
{% highlight python %}
class Pair(object):
    
    def __init__(self, start, end, s):
        self.start = start
        self.end = end
        self.s = s

class Solution(object):
    def palindromePairs(self, words):
        """
        :type words: List[str]
        :rtype: List[List[int]]
        """
        res = []
        for i in range(len(words)):
            for j in range(len(words)):
                if i != j:
                    pairsStr = words[i] + words[j]
                    if self.isValidPalindrom(pairsStr):
                        pairs = Pair(i, j, pairsStr)
                        res.append(pairs)
        
        results = []
        for i in range(len(res)):
            results.append([res[i].start, res[i].end])
        return results
            
    def isValidPalindrom(self, s):
        
        l = 0
        r = len(s) - 1
        
        while l < r:
            
            if s[l].lower() != s[r].lower():
                return False
            l += 1
            r -= 1
        return True
{% endhighlight %}

{% highlight python %}
class Solution(object):
    def palindromePairs(self, words):
        """
        :type words: List[str]
        :rtype: List[List[int]]
        """
        lookup = {}
        for index, word in enumerate(words):
            lookup[word] = index
        
        res = []
        for i in range(len(words)):
            for j in range(len(words[i]) + 1):
                first_part = words[i][j:]
                second_part = words[i][:j]
                
                if first_part == first_part[::-1] and second_part[::-1] in lookup and lookup[second_part[::-1]] != i:
                    res.append([i, lookup[second_part[::-1]]])
                if j > 0 and second_part == second_part[::-1] and first_part[::-1] in lookup and lookup[first_part[::-1]] != i: 
                    #注意append的顺序， index是 first-part在前
                    res.append([lookup[first_part[::-1]], i])
        return res
{% endhighlight %}

<h3 id = 'third'>Round Numbers</h3>
<h3 id = 'fourth'>Leetcode 251. Flatten 2D Vector</h3>
需要便利2d vector里面所有的元素 所以时间复杂度是 所有元素的个数
{% highlight python %}
class Vector2D(object):
    def __init__(self, vec2d):
        """
        Initialize your data structure here.
        :type vec2d: List[List[int]]
        """
        self.vec2d = vec2d
        self.x = 0
        self.y = 0
        
    def next(self):
        """
        :rtype: int
        """
        res = self.vec2d[self.y][self.x]
        self.x += 1
        return res
    
        

    def hasNext(self):
        """
        :rtype: bool
        """
        while self.y < len(self.vec2d):
            if self.x < len(self.vec2d[self.y]):
                return True
            else:
                self.y += 1
                self.x = 0

# Your Vector2D object will be instantiated and called as such:
# i, v = Vector2D(vec2d), []
# while i.hasNext(): v.append(i.next())
{% endhighlight %}

<h3 id = 'fifth'>Leetcode 751. IP to CIDR</h3>

CIDR: A CIDR is the string consisting of an IP, follwed by a slash, and then prefix length, For example: "123.45.67.89/20". That prefix length "20" represents the number of common prefix bits in the specified range.

首先定义一个function，将ip地址转换成一个数字，其中用到了python中的map function, numbers 是包含4个元素的数组，第一个表示最高的八位，所以在计算成数字的时候，需要将 numbers 左移24位
>map(fun, iter)
>>NOTE : The returned value from map() (map object) then can be passed to functions like list() (to create a list), set() (to create a set) .
fun : It is a function to which map passes each element of given iterable. 
iter : It is a iterable which is to be mapped.


{% highlight python %}
    def ip2number(self, ip):
        numbers = list(map(int, ip.split(".")))
        n = (numbers[0]<<24) + (numbers[1]<<16) + (numbers[2]<<8) + numbers[3]
        return n
{% endhighlight %}

{% highlight python %}
    def number2ip(self,n):
        return ".".join([str(n>>24&255), str(n>>16&255),str(n>>8&255), str(n&255)])
{% endhighlight %}


<h3 id = 'sixth'>Menu Order</h3>

Round 1: Given a menu (list of items prices), find all  possible combinations of items that sum a particular value K. (A variation of the typical 2sum/Nsum questions).

{% highlight python %}
def search(res, tempList, centsPrices, centsTarget, index):
    if centsTarget < 0:
        return
    
    if centsTarget == 0:
        res.append(list(tempList))
    
    for i in range(index, len(centsPrices)):
        tempList.append(centsPrices[i])
        search(res, tempList, centsPrices, centsTarget-centsPrices[i], i + 1)
        tempList.pop()
    

def getCombos(prices, target):
    res = []
    tempList = []
    
    if prices is None or len(prices) == 0 or target < 0:
        return res
    
    centsTarget = int(round(target * 100))
    print centsTarget
    sorted(prices)
    centsPrices = [ int(round(prices[i]*100)) for i in range(len(prices))]
    print centsPrices
    search(res, tempList, centsPrices, centsTarget, 0)
    return res
    
print getCombos([10.02, 1.11, 2.22, 3.01, 4.02, 2.00, 5.03], 7.03) 
{% endhighlight %}

<h3 id = 'seventh'>Leetcode 269. Alien Dictionary</h3>

Round 2: Given a flight itinerary consisting of starting city, destination city, and ticket price (2d list) - find the optimal price flight path to get from start to destination. (A variation of Dynamic Programming Shortest Path) Given a list of sorted words from an alien dictionary, find the order of the alphabet. (Alien Dictionary Topological Sort - https://discuss.leetcode.com/topic/22476/16-18-lines-python-30-lines-c)

<div>
<h3>Parse CSV</h3>
</div>
<div>
<h3 id = 'nineth'>Perference List</h3>
Description: Given a list of everyone's preferred city list, output the city list following the order of everyone's preference order.
For example, input is [[3, 5, 7, 9], [2, 3, 8], [5, 8]]. One of possible output is [2, 3, 5, 8, 7, 9].

这道题目是用拓扑排序来做, 向面试官解释的时候, 可以先举个简单的例子, for example, 当只有一个list的时候， 比如[1,2,3]最后的结果就是 1,2,3, 当有2个list的时候, for example, [1,2,3],[4,5],就是1,4,2,5,3 由此可以推出这里我们可以构建一个图, 如果图的indegrees是0的时候，我就可以当前的节点加进queue里面，由此可以推导出这道题可以用拓扑排序来做

code如下
{% highlight python %}
# Hello World program in Python
import collections

def findOrder(preferenceLists):

    nodes = collections.defaultdict(set)
    indegree = collections.defaultdict(int)
    for preference in preferenceLists:
        for i in range(len(preference) - 1):
            from_node  = preference[i]
            to_node  = preference[i+1]
            # if from_node not in nodes
            #     nodes.get(from_node).add(to_node)
            if from_node not in indegree:
                indegree[from_node] = 0
            indegree[to_node] += 1
            nodes[from_node].add(to_node)
    
    print indegree
    queue = []
    res = []
    for key in indegree.keys():
        if indegree[key] == 0:
            queue.append(key)
    while len(queue):
        node = queue.pop(0)
        res.append(node)
        node_neighbors = nodes[node]
        for nextNode in node_neighbors:
            indegree[nextNode] -= 1
            if indegree[nextNode] == 0:
                queue.append(nextNode)
    return res

preferenceLists = [[3, 5, 7, 9], [2, 3, 8], [5, 8]]
findOrder(preferenceLists)           
{% endhighlight %}

</div>
