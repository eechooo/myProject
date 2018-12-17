---
layout: post
title:  "西雅图 offer阿普 Onsite"
date:   2018-12-16 13:31:01 +0800
categories: jekyll
tag: jekyll
---

* content
{:toc}
<h2>Coding 部分</h2>
<h3>1. Leetcode 652. Find Duplicate Subtrees</h3>
{% highlight java %}
class Solution {
    public List<TreeNode> findDuplicateSubtrees(TreeNode root) {
        List<TreeNode> res = new ArrayList<>();
        HashMap<String,Integer> map = new HashMap<>();
        postOrder(root,map,res);
        return res;    
    }
    
    public String postOrder(TreeNode cur,Map<String,Integer> map,List<TreeNode> res){
        
        if(cur==null) return "#";
        String serial = cur.val + "," + postOrder(cur.left,map,res) + postOrder(cur.right,map,res);
        if (map.getOrDefault(serial, 0) == 1) res.add(cur);
        map.put(serial, map.getOrDefault(serial, 0) + 1);
        return serial;   
    }
}

{% endhighlight %}

{% highlight python %}
class Solution(object):
    def findDuplicateSubtrees(self, root):
        """
        :type root: TreeNode
        :rtype: List[TreeNode]
        """
        m = collections.defaultdict(int)
        res = []
        self.dfs(root, m, res)
        return res
        
    def dfs(self, root, m, res):
        if root is None:
            return '#'
        path = str(root.val) + "," + self.dfs(root.left, m, res) + "," +  self.dfs(root.right, m, res)

        if m[path] == 1:
            res.append(root)
        m[path] += 1
        return path
{% endhighlight %}

<h3>2. Leetcode 200.number of islands</h3>
可以用bfs和 dfs来解决， 后面的followup如下，
{% highlight java %}
1 1 0 0 0 1
0 0 0 0 0 0
1 0 0 0 0 0
1 1 0 1 0 0
{% endhighlight %}

1 island, 0 water, 返回matrix，要求 matrix里面的0 替换成 到达最近岛屿的距离，这道题要求的时间复杂度是O(m*n), 这道题其实可以和leetcode里面一道wall and gates联系在一起， 时间复杂度是O (m * n)的做法就是循环遍历一遍matrix将1的点全部放在queue里面
为什么时间复杂度是O(m * n), 仔细体会一下

<h2>Behavior Question 部分</h2>
<h3>1. Why Offerup</h3>
This is a career path that I want to seek, offerup has many users which can help me built my skill set
1. I have seen in the cruchbase it recored offerup raise a lot of funding recently and grow up fast ，I wanna join such company and grow up with such a company

2. when I was a undergraduate student, I am thinking about develop a online platform for students to trade their second hand book or second hand computer, however, It did not go to public , I would say join offerup match my interest and passsion

those reasons are why I choose offerup
<h3>2. How to Handle Conflicts With Team Members</h3>
when there is a conflict, there are also two different ideas, for example I have plan A, my team member has plan B, I will have a detailed description (maybe this description will in my mind, or description was written down in the paper) before I talk with my team member, I will describe my solution and also 

reach to agreement, maybe in the final we could have plan C, which will be better solution, after all

<h3> 3. how to finish the task in limited time </h3>

1. have overview of the whole project, estimate how long it will take to finish it
2. report this situation to manager, and be sure that I will finished it as soon as possible

I remembered I need to provide weather API solution for our customer, our customer will release their product recently, so it is a very urgent requirement, not only I need to 

firstly I have a overview of the task, and divide them into many small tasks and put priority to the task, and record it in the trello, let team members know my progress in this project, also I will dedicated to this task, if it was a very urgent task, I will stay up to late to finish as much as possible I could and also let my team member know my progress,

if i meet a problem during developing， I will actively seek help smartly,


