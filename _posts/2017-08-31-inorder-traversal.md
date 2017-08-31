---
layout: post
title: "Inorder Traversal"
description: "Binary tree inorder traversal"
category: algo
tags: [tree]
---
{% include JB/setup %}

## 问题描述

给定一个二叉树，返回中序遍历

如： 给定 <font color='red'>[1,null,2,3]</font> 

	 1
	    \
	     2
	    /
	   3 

返回 <font color='red'>[1,3,2]</font> 



## 解决方案一

解决此问题首先想到的方法是采用递归，

```java

class Solution{
	public List<Integer> inorderTraversal(TreeNode root){
		List<Integer>res=new ArrayList<Integer>();
		helper(root,res);
		return res;
	}
	pubilc voud helper(TreeNode root,List<Integer>res){
		if(root != null){
			if(root.left!=null){
				helper(root.left,res)
			}
			res.add(root.value);
			if(root.right!=null){
				helper(root.right);
			}
		}
	}
}

```
- Complexity Analysis
	+ Time complexity : O(n)O(n). The time complexity is O(n)O(n) because the recursive function is T(n) = 2*T(n/2)+1T(n)=2∗T(n/2)+1.
	+ Space complexity : The worst case space required is O(n)O(n), and in the average case it's O(log(n))O(log(n)) where nn is number of nodes.



## 解决方案二

利用栈实现迭代的方法,如下所示：

	Binary Tree:
	      1
        /   \
       2     3
      / \   /
     4   5 6  

	stack:
	
	 |   |    |   |    | 4 |    |   |    |   |    |   |    |   |    |  |     |   |    |   |    |   |    |  |
 	 |   |    | 2 |    | 2 |    | 2 |    |   |    | 5 |    |   |    |  |     |   |    | 6 |    |   |    |  |
     |_1_|    |_1_|    |_1_|    |_1_|    |_1_|    |_1_|    |_1_|    |__|     |_3_|    |_3_|    |_3_|    |__|
	 push 1   push 2   push 4   pop 4    pop 2    push 5   pop 5    pop 1    push 3   push 6   pop 6    pop 3
	                            add 4    add 2             add 5    add 1                      add 6    add 3
	
	res<>:
 	[]                          [4]     [4,2]            [4,2,5]  [4,2,5,1]                [4,2,5,1,6] [4,2,5,1,6,3]

```java
	public class Solution{
		public List<Integer> inorderTraversal(TreeNode root){
			List<Integer>res=new ArrayList<Integer>();
			Stack<TreeNode>stack=new Stack<TreeNode>();
			TreeNode cur=root;
			while(cur!=null || !stack.empty()){
				while(cur!=null){
					stack.push(cur);
					cur=curl.left;
				}
				cur=statck.pop();
				res.add(cur.value());
				cur=cur.right;
			}
			return res;
		}

	}

```
- Complexity Analysis
	+ Time complexity : O(n)O(n).
	+ Space complexity : O(n)O(n).

## 解决方案三 Morris Traversal

	Step 1: Initialize current as root

	Step 2: While current is not NULL, If current does not have left child

	  	a. Add current’s value

	  	b. Go to the right, i.e., current = current.right

 	Else

	  	a. In current's left subtree, make current the right child of the rightmost node

	  	b. Go to this left child, i.e., current = current.left

	      1
        /   \
       2     3
      / \   /
     4   5 6
     First, 1 is the root, so initialize 1 as current, 1 has left child which is 2, the current's left subtree is

         2
        / \
       4   5
	So in this subtree, the rightmost node is 5, then make the current(1) as the right child of 5. Set current = cuurent.left (current = 2). The tree now looks like:

	         2
	        / \
	       4   5
	            \
	             1
	              \
	               3
	              /
	             6
	For current 2, which has left child 4, we can continue with thesame process as we did above

	        4
	         \
	          2
	           \
	            5
	             \
	              1
	               \
	                3
	               /
	              6
	then add 4 because it has no left child, then add 2, 5, 1, 3 one by one, for node 3 which has left child 6, do the same as above. Finally, the inorder taversal is [4,2,5,1,6,3].

```java

public class Solution{
	public List<Integer> inorderTraversal(TreeNode root){
		List<Integer>res=new ArrayList<>();	
		TreeNode curr=root;
		TreeNode pre;
		while(curr!=null){
			if(curr.left==null){
				res.add(curr.value());
				curr=curr.right;
			}
			else{
				pre=curr.left;
				while(pre.right!=null){
					pre=pre.right;
				}
				pre.right=curr;
				TreeNode tmp=curr;
				curr=curr.left;
				temp.left=null;
			}
		}	
		return res;
	}
}


```