---
layout: post
title: "Second Minimum Node In a Binary Tree"
description: "求第二小元素"
category: algo 
tags: [algo,java]
---
{% include JB/setup %}

## 求指定二叉树中的第二小元素

给定一个非空二叉树，它的结点值非负，每个结点都含用零个域2个子结点，如果它有两个子结点，那么子结点的值 都大于它本身的值 。

试求解这个二叉树的第二小结点，如果不存在这种值，则返回-1。

### Example 1:

	Input: 
	    2
	   / \
	  2   5
	     / \
	    5   7

	Output: 5
	Explanation: The smallest value is 2, the second smallest value is 5.
	Example 2:

### Example 2:
	
	Input: 
	    2
	   / \
	  2   2

	Output: -1
	Explanation: The smallest value is 2, but there isn't any second smallest value.

## 解决方案

### 方法一：暴力解决

遍历整棵树，将所有元素保存到集合当中，然后查找第二小元素，记住:最小的元素是*root.val*

*java*

```java
class Solution{
	public void dfs(TreeNode root,Set<Integer>uniques){
		if(root!=null){
			uniques.add(root.val);
			dfs(root.left,uniques);
			dfs(root.right,uniques);
		}
	}
	public int findSecondMinimalValue(TreeNode root){
		Set<Integer>uniques=new HashSet<Integer>();
		dfs(root,uniques);
		
		int min1=root.val;
		long ans=Long.MAX_VALUES;
		for(int v:uniques){
			if(min1<v && v<ans)ans=v;
		}
		return ans<Long.MAX_VALUE?(int)ans:-1;
	}
}

```

*Python*

```python
def findSecondMinimumValue(self, root):
    self.ans = float('inf')
    min1 = root.val

    def dfs(node):
        if node:
            if min1 < node.val < self.ans:
                self.ans = node.val
            elif node.val == min1:
                dfs(node.left)
                dfs(node.right)

    dfs(root)
    return self.ans if self.ans < float('inf') else -1
```

### 方法二：

定义*min1=root.val*.当遍历树的时候，如果*node.val>min1*,我们知道所有子结点的值最小等于*node.val*,所以它的子树中不会有解了，只需停止搜索

另外，由于我们只关心第二小元素，所以只需记录第二小元素，而不必记录所有元素

*java*

```java
class Solution{
	int min1;
	long ans=Long.MAX_VALUE;

	public void dfs(TreeNode root){
		if(root!=null){
			if(min1<root.val&&root.val<ans){
				ans=root.val;
			}
			else if(min1==root.val){
				dfs(root.left);
				dfs(root.right);
			}
		}
	}
	public int findSecondMinimumValue(TreeNode root){
		min1=root.val;
		dfs(root);
		return ans<Long.MAX_VALUE?(int)ans:-1;
	}
}

```

[原文地址](https://leetcode.com/articles/second-minimum-node-in-a-binary-tree/)