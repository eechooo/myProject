---
layout: post
title: Copy Array Largest Sum
date:   2018-12-28 00:00:00 +0800
categories: document
tag: 教程
---

* content
寻找一个target, 这个数肯定是在low 和 high 之间

low就是数组中最大的那个数 max(nums)
high 就是把数组中所有的数字和加在一起 sum(nums)
这道题用二分查找来做，第一步找mid of (low + high), 这个mid要满足条件, 就是按照mid来分看看能不能分成 m 份, 如果分出来的份数大于m的话, 相当于这个值太小，改变二分的范围，the low point start from mid + 1

最后返回high的那个值，为什么high的值就是 array的2个数呢 举个例子
如果我们的test case是nums = [7,2,5,10,8], m = 2
low = 10
high = 32
所以第一个mid是 22，发现22是能讲上述数组分成2个部分的 [7, 2, 5, 10], [8]
22是符合条件的, 所以change high to 22 继续找mid = 11，这道题的答案是 18

let us try 17， 17 不能满足条件
let us try 19, 19 can not satisfy condition


{% highlight python %}
class Solution(object):
    def splitArray(self, nums, m):
        """
        :type nums: List[int]
        :type m: int
        :rtype: int
        """
        low = max(nums)
        high = sum(nums)
        print 'low', low
        print 'high', high  
        
        while low < high:
            mid = (high + low) / 2
            if self.valid(mid, m, nums):
                high = mid
            else:
                low = mid + 1
        return high
                
    def valid(self, mid, m, nums):
        cur = 0
        count = 1
        for i in range(len(nums)):
            cur += nums[i]
            if cur > mid:
                cur = nums[i]
                count += 1
                if count > m:
                    return False
        return True
{% endhighlight python %}

这道题还有一种做法就是用DP来做, 如果用dp来做的话, 就要定义
dp[i][j] 的意思就是 讲J个元素分成i个group, dp[i][j] 

就是里面sum最小的那个最大的sum

for example 如何体会这句话, 举个例子更容易明白一点 比如一个nums = [7,2,5,10,8], m = 2


{% highlight python %}
class Solution(object):
    def splitArray(self, nums, m):
        """
        :type nums: List[int]
        :type m: int
        :rtype: int
        """
        
        prefixsum = [0] * (len(nums))
        prefixsum[0] = nums[0]
        dp = [[float('inf') for _ in range(len(nums))] for _ in range(m+1)]
        # print dp
        for i in range(1, len(nums)):
            prefixsum[i] = prefixsum[i-1]+ nums[i]
        
        # print 'prefixsum', prefixsum
        for i in range(len(nums)):
            
            dp[1][i] = prefixsum[i]
            
        
        for i in range(2, m+1):
            for j in range(i-1, len(nums)):
                for k in range(j):
                    dp[i][j] = min(dp[i][j], max(dp[i-1][k], prefixsum[j] - prefixsum[k]))                  
        return dp[m][len(nums)-1]
{% endhighlight python %}
    
    
                

