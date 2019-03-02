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
<li><a href='#nineth'>Perference List</a></li>
<li><a href='#tenth'>Leetcode 269. Alien Dictionary</a></li>
<li><a href='#eleventh'>Leetcode 756. Pyramid Transition Matrix</a></li>
<li><a href='#12th'>Leetcode 755. Pour Water</a></li>
<li><a href='#13th'>Find median from large file of integers</a></li>
<li><a href='#14th'> Text Justification</a></li>
<li><a href="#15th"> K edit distance</a></li>


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
        # 
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

<p>添加了 removeEle() 方法</p>
{% highlight python %}
class Vector2D(object):

    def __init__(self, vec2d):
        """
        Initialize your data structure here.
        :type vec2d: List[List[int]]
        """
        self.row = 0
        self.col = 0
        self.vec2d = vec2d

    def next(self):
        """
        :rtype: int
        """
        value = self.vec2d[self.row][self.col]            
        self.col += 1
        # 判断self.col 等于当前的vec2d[self.row]的长度的时候, 需要reset坐标, col设为0, add one to row
        if self.col == len(self.vec2d[self.row]):
            self.col = 0
            self.row += 1
        return value
    def hasNext(self):
        """
        :rtype: bool
        """
        while self.row < len(self.vec2d):
            if self.col < len(self.vec2d[self.row]):
                return True
            else:
                self.row += 1
                
        return False
    
    def removeEle(self):
        #Case 1: if the elemnt to remove is the last element of the row
        # self.row should be the same row, however self.col will be equal to zero
        if self.col == 0:
            self.row -= 1
            to_remove_list = self.vec2d[self.row]
            to_remove_col_index = len(self.vec2d[self.row]) - 1
            to_remove_list.pop(to_remove_col_index)
        # the element to remove is not the last element
        else:
            self.col -= 1
            to_remove_list = self.vec2d[self.row]
            to_remove_list.pop(self.col)
            print 'to_remove_list', to_remove_list
        to_remove_list = self.vec2d[self.row]
        print 'secon to_remove_list', to_remove_list
        
        if len(to_remove_list) == 0:
            self.vec2d.remove(to_remove_list)
        print 'self.row', self.row
        print 'self.col', self.col
            
vec = Vector2D([[1,2],[3], [4,5,6]])
while vec.hasNext():
    print vec.next()
    vec.removeEle()
    print vec.vec2d
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

解决这道题首先要知道CSV Parser的规则, 

<p>规则一: 一个field中如果包含双引号的话(double quote),需要将这个双引号保护起来, 也就是再加上quote保护它</p>
<p>规则二: 如果一个field中包含comma的话, 这个comma需要被双引号给保护起来</p>
<p>规则三: 只有当开头有双引号时，一个field中间才可以有双引号，且必须成对出现</p>

不满足以上要求的输入即不符合CSV格式

也就是说，类似
"abc""def"
这样的是合法的输入

类似
""abcdef""
abc""def
这样的是不合法的输入，并不满足CSV格式要求

看下面这个例子, Alex 的左右2边都是有一对双引号的，最外面那一层双引号是为了escape最里面的那一层双引号的,一旦这里面有double quote的话, 最外面那一层也需要加上引号

{% highlight python %}
test = "\"Alexandra \"\"Alex\"\"\",Menendez,alex.menendez@gmail.com,Miami,1";
expected = "Alexandra \"Alex\"|Menendez|alex.menendez@gmail.com|Miami|1";
{% endhighlight %}

{% highlight python %}
def parseCSV(str):
    inQuote = False
    curr = ''
    res = []
    for i in range(len(str)):
        c = str[i]
        
        if inQuote:
            if c == "\"":
                if i < len(str) and str[i+1] == '\"':
                    curr += '\"'
                    i += 1
                else:
                    inQuote = False
            else:
                curr += c
        else:
            if c == '\"':
                inQuote = True
                print i
            elif c == ',':
                res.append(curr)
                curr = ''
            else:
                curr += c
    if len(curr) > 0:
        res.append(curr)
    print res
    return '|'.join(res)
print parseCSV("\"Alexandra \"\"Alex\"\"\",Menendez,alex.menendez@gmail.com,Miami,1") 
{% endhighlight %}       
        


</div>

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
            # from_node 也要加到indegree 字典里面, 因为indegree
                indegree[from_node] = 0
            #为什么
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

<h3 id = 'tenth'>Leetcode 269. Alien Dictionary</h3>

这道题可以和 preference List 进行比较, 也是拓扑排序的题目, 但是这道题与上一题不同的是，这道题需要每次比较
2个单词的同一位置，来判断他们的字符顺序
需要弄明白的是：
第一点: 为什么需要break: 需要break是因为，这道题比较的是字典序，一旦发现A单词有一个字符的 字典序 比 B单词的的字典序要大的话, 就不需要比较下面位置的了

第二点: 这道题的时间复杂度和空间复杂度是多少

{% highlight python %}
class Solution(object):
    def alienOrder(self, words):
        """
        :type words: List[str]
        :rtype: str
        """
        nodes = collections.defaultdict(set)
        indegree = collections.defaultdict(int)
        
        for word in words:
            for i in range(len(word)):
                indegree[word[i]] = 0
        
        for i in range(len(words) - 1):
            cur_word = words[i]
            next_word = words[i+1]
            
            length = min(len(cur_word), len(next_word))
            for j in range(length):
                c1 = cur_word[j]
                c2 = next_word[j]
                
                if c1 != c2 and c2 not in nodes[c1]:
                    nodes[c1].add(c2)
                    indegree[c2] += 1
                    # 为什么 break, 还要仔细想一想
                    break

        # print 'nodes',nodes
        # print 'indegree',indegree
        queue = []
        res = ''
        for c in indegree.keys():
            if indegree[c] == 0:
                queue.append(c)
        
        while len(queue) > 0:
            node_c = queue.pop(0)
            res += node_c
            for node_neighbor in nodes[node_c]:
                indegree[node_neighbor] -= 1
                if indegree[node_neighbor] == 0:
                    queue.append(node_neighbor)
        return res
{% endhighlight %}
<h3 id = "eleventh">Leetcode 756. Pyramid Transition Matrix</h3>
解题思路: 其实就是在Allowed数组里面找到合法的砖块组合，往楼上堆砖块, input是金字塔最低层的那一排

用递归的思路去解决，递归传入的参数是 当前层的字符串cur_level, 以及cur_level的的长度 cur_len
那么问题来了，这个递归函数的base case是什么呢, 这个函数的base_case就是当前的cur_level的长度为一的时候, 这个时候已经达到了金字塔的顶端了

Let us walk through a test case
the test case is: **bottom = "ABC", ["ABD","BCE","DEF","FFF", "ABJ"]**

先构建map, map是通过allowed来构建的，string前面2位是Key, 后面一位是value，所以构建完的map就是

{% highlight python %}
{'AB': set(['J', 'D']), 'DE': set(['F']), 'FF': set(['F']), 'BC': set(['E'])})
{% endhighlight %}

itertools.product函数解释，求2个集合的笛卡尔积，什么叫笛卡尔积呢, 看下面的例子

{% highlight python %}
例如，A={a,b}, B={0,1,2}，则
A×B={(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}
B×A={(0, a), (0, b), (1, a), (1, b), (2, a), (2, b)}
{% endhighlight %}

代码中的传入的参数是itertools.product(next_cand, self.mapping[cur_level[i:i+2]]),其实求的就是next_cand和 self.mapping的笛卡尔积， 如果以ABC为底，在计算第二层的时候，求出来的next_cand就是[' JE', ' DE'] 然后再循环遍历next_cand，去分别查看以 "JE" 和 "DE" 为cur_level的时候，能不能构建成金字塔

下面是代码验证 下一层是 [' JE', ' DE']

{% highlight python %}
import collections
import itertools
bottom = 'ABC'
allowed = ["ABD","BCE","DEF","FFF", "ABJ"]
next_cand = ' '

mapping = collections.defaultdict(set)
for allow in allowed:
    mapping[allow[:2]].add(allow[2:])
print mapping

cur_level, cur_len = bottom, len(bottom)

for i in range(cur_len-1):
    if cur_level[i:i+2] in mapping:
        key = cur_level[i:i+2]
        next_cand = map(''.join, itertools.product(next_cand, mapping[key]))
print next_cand
{% endhighlight %}

{% highlight python %}
class Solution:
    def pyramidTransition(self, bottom, allowed):
        """
        :type bottom: str
        :type allowed: List[str]
        :rtype: bool
        """
        self.mapping = collections.defaultdict(set)
        for brick in allowed:
            self.mapping[brick[:2]].add(brick[2])
            
        cur_level, cur_len = bottom, len(bottom)
        return self.search(cur_level, cur_len)
                             
    def search(self, cur_level, cur_len):
        if cur_len == 1:
            return True
        
        next_cand = ' '        
        for i in range(cur_len-1):
            if cur_level[i:i+2] in self.mapping:
                next_cand = map(''.join, itertools.product(next_cand, self.mapping[cur_level[i:i+2]]))
            else:
                return False
            
        if next_cand: 
            for cand in list(next_cand):
                if self.search(cand[1:], cur_len-1):
                    return True
        return False
{% endhighlight %}

<h3>Leetcode 773. Sliding Puzzle</h3>

我的问题是，如果这样做的话,是可以把所有的可能都遍历完嘛？ 时间复杂度是多少

{% highlight python %}
class Solution(object):
    def slidingPuzzle(self, board):
        """
        :type board: List[List[int]]
        :rtype: int
        """
        target = [[1,2,3], [4,5,0]]
        [(x,y)] = [(x, y) for x in range(2) for y in range(3) if board[x][y] == 0]
        queue = collections.deque([(board, 0, x, y)])
        visited = set(tuple(map(tuple,board)))
        
        while queue:
            board, steps, x, y = queue.popleft()
            
            if board == target:
                return steps
            for di in [(1, 0), (-1, 0), (0, -1), (0, 1)]:
                t_x, t_y = x + di[0], y + di[1]
                if 0 <= t_x < 2 and 0 <= t_y < 3:
                    temp_board = [[i for i in row] for row in board]
                    temp_board[x][y], temp_board[t_x][t_y] = temp_board[t_x][t_y], temp_board[x][y]
                    tuple_b = tuple(map(tuple, temp_board))
                    if tuple_b not in visited:
                        visited.add(tuple_b)
                        queue.append((temp_board, steps + 1, t_x, t_y))
        return -1
{% endhighlight %}

<h3>Leetcode 755. Pour Water</h3>

<p>Poor Water还要求最后打印出来, 水滴和地形, 基本上就是先向左找到非递增的最低点,如果该点和dumpPoint一样高，往右继续找非递增的最低点，如果一样高就放到dumpPoint，不一样的话放置在非递增的最低点</p>

<p>注意体会一下最后把水滴和地形打出来的代码</p>

{% highlight python %}
class Solution(object):
    def pourWater(self, heights, position, count):
        """
        :type heights: List[int]
        :type V: int
        :type K: int
        :rtype: List[int]
        """
        if heights is None or len(heights) == 0:
            return heights
        water = [0 for _ in range(len(heights))]
        
        while count > 0:
            left = position
            right = position
            putLocation = position

            while left > 0:
                if heights[left - 1] + water[left - 1] > heights[left] + water[left]:
                    break
                left -= 1
            
            if heights[left] + water[left] < heights[position] + water[position]:
                putLocation = left
            else:
                while right < len(heights) - 1:
                    if heights[right + 1] + water[right + 1] > heights[right] + water[right]:
                        break
                    right += 1
                if heights[right] + water[right] < heights[right] + water[right]:
                    putLocation = right
            water[putLocation] += 1
            count -= 1
        highest = 0
        for i in range(len(heights)):
            if heights[i] + water[i] > highest:
                highest = heights[i] + water[i]
        
        print 'highest',highest
        for h in range(highest, 0, -1):
            for i in range(len(heights)):
                if heights[i] + water[i] < h:
                    print "  " ,
                elif heights[i] < h:
                    print 'W ' ,
                else:
                    print '# ' ,
            print '\n'
        
        res = [0] * len(heights)
        for i in range(len(heights)):
            res[i] = heights[i] + water[i]
        
        return res
{% endhighlight %}

<h3>Wizards</h3>

There are 10 wizards, 0-9, you are given a list that each entry is a list of wizards known by wizard. Define the cost between wizards and wizard as square of different of i and j. To find the min cost between 0 and 9.

For example:
{% highlight python %}
wizard[0] list: 1, 4, 5 

wizard[4] list: 9

wizard 0 to 9 min distance is (0-4)^2+(4-9)^2=41 (wizard[0]->wizard[4]->wizard[9])

{% endhighlight %}

{% highlight python %}
class wizard(object):
    def __init__(self, id, dist):
        self.id = id
        self.dist = float('inf')



import heapq
import collections
import math

def getShortestPath(wizards, source, target):
    size = len(wizards)
    dic = collections.defaultdict(wizard)
    
    parents = [float('inf')] * len(wizards)
    print 'at beigining', parents
    
    for i in range(size):
        parents[i] = i;
        dic[i] = wizard(i, float('inf'))
    
    print parents
    
    queue = []
    
    node = dic[source]
    
    heapq.heappush(queue, (node.id, 0))
    
    
    while len(queue) > 0:
        cur_id, cur_dist = heapq.heappop(queue)
        # print 'cur_dist', cur_dist
        neighbors = wizards[cur_id]
        for nei in neighbors:
            nei_id, nei_dist = dic[nei].id, dic[nei].dist
            weight = math.pow(nei_id - cur_id, 2)
            if nei_id == 9:
            if cur_dist + weight < nei_dist:
                print 'nei_id', nei_id
                parents[nei_id] = cur_id
                heapq.heappush(queue, (nei_id, cur_dist + weight))
    res = []
    print 'parents', parents
    while target != source:
        res.append(target)
        target = parents[target]
    
    res.append(source)
    print 'res', res
    results = res.reverse()
    return results
    
wizards = [[1, 5, 9], [2, 3, 9], [4], [], [], [9], [], [], [], []]

print getShortestPath(wizards, 0, 9)
{% endhighlight %}


<h3>Find median from large file of integers</h3>

Find the median from a large file of integers. You can not access the numbers by index, can only access it sequentially. And the numbers cannot fit in memory.

这道题是个好题目, 下面是解题思路
思路: 首先， 我们知道对任何大的file median of any int will between INT_MIN and INT_MAX
所以我们就知道了lowerbound 和 upperbound, 我们猜一下median might be "guess = lower+(upper-lower)/2" 验证media是不是这个数的话, 需要我们找到合适的方法去验证, 验证的方法就是
扫一遍这个文件，拿一个count 去记录一下小于等于当前的guess number的个数代码如下, 这样的算法
顶多需要扫描 32次文件

1. 为什么顶多需要扫描32次 文件呢, 因为Integer在内层里面的比特位就是32位，每次取一半 就相当于右边移一位
2. Java里面Integer是 2^32 - 1, 为什么呢 因为32位比特位里面, 第一位是记录符号的，记录当前的数是正数还是负数, 后面31位是表示数字， 2^32 - 1 = 2^31 + 2^30 ..... + 2^1 + 2^0

{% highlight python %}
res = 0
while num = readline():
    if num <= guess:
        count += 1
        # 这里就是求 扫描的这些数当中 在guess左边最大的数
        res = max(res, num)
{% endhighlight %}

{% highlight Java %}
package find_median_in_large_file_of_integers;

import org.junit.*;

import static org.junit.Assert.*;

public class FindMedianinLargeIntegerFileofIntegers {
    /*
        Find Median in Large Integer File of Integers
        AirBnB Interview Question
     */
    public class Solution {
        private long search(int[] nums, int k, long left, long right) {
            if (left >= right) {
                return left;
            }

            long res = left;
            long guess = left + (right - left) / 2;
            int count = 0;
            for (int num : nums) {
                if (num <= guess) {
                    count++;
                    res = Math.max(res, num);
                }
            }

            if (count == k) {
                return res;
            } else if (count < k) {
                return search(nums, k, Math.max(res + 1, guess), right);
            } else {
                return search(nums, k, left, res);
            }
        }

        public double findMedian(int[] nums) {
            int len = 0;
            for (int num : nums) {
                len++;
            }

            if (len % 2 == 1) {
                return (double) search(nums, len / 2 + 1, Integer.MIN_VALUE, Integer.MAX_VALUE);
            } else {
                return (double) (search(nums, len / 2, Integer.MIN_VALUE, Integer.MAX_VALUE) +
                        search(nums, len / 2 + 1, Integer.MIN_VALUE, Integer.MAX_VALUE)) / 2;
            }
        }
    }

    public static class UnitTest {
        @Test
        public void test1() {
            Solution sol = new FindMedianinLargeIntegerFileofIntegers().new Solution();
            assertEquals(3.0, sol.findMedian(new int[]{3, -2, 7}), 1E-03);
            assertEquals(5.0, sol.findMedian(new int[]{-100, 99, 3, 0, 5, 7, 11, 66, -33}), 1E-03);
            assertEquals(4.5, sol.findMedian(new int[]{4, -100, 99, 3, 0, 5, 7, 11, 66, -33}), 1E-03);
        }
    }
}
{% endhighlight %}

<h3>Text Justification</h3>
Text Justification 解题思路, 对于一个给定的maxWidth, 需要将单词排列在上面, 还要保证单词之间的间隙是均匀分布的
首先要知道的知识是 对于一个list的单词来说，假设有n个单词, 那么n个单词之间的空隙数是 n - 1
对于list中的index来说, 
1. 
{% highlight python %}

{% endhighlight %}







        
                    
                    
                
                
        



