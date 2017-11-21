---
layout: post
title: "Median of Two Sorted Array"
description: "计算两个排序数序数组的中位数"
category: algo
tags: []
---
{% include JB/setup %}

## 计算两个排序数组的中位数

### 问题描述:

有两个排序数组_nums1_ and _nums2_ ,它们的长度为别为*m*和*n*,要求找出它们的中位数，时间复杂度应是*O(log(m+n))*.

#### Example 1:

	nums1=[1,3]
	nums2=[2]

	中位数是 2.0

#### Example 2:

	nums1=[1,2]
	nums2=[3,4]

	中位数是 (2+3)/2=2.5

### 解决方案

如果两个数组长度为奇数，则中位数是两个数组排序后最中间那个，如果为偶数则是最中间的两个数平均数。

首先定义中位数：

> 将数组分成两部分，其中一个子集都比另一个子集大(小)

将 _A_ 在任意位置 _i_ 切分成两部分:

 	      left_A             |        right_A    
	A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]  

含有*m* 个无素的 _A_ 可以有 *m+1* 种划分(i=0~m)

	len(left_A)=i,len(right_A)=m−i.  

	Note: when i=0, left_A is empty, and when i=m, right_A is empty.

同样，也可以将 _B_ 切分为两部分：

	      left_B             |        right_B             
	B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1] 

假设满足以下两个条件:

	1. len(left_part)=len(right_part)   
	2. max(left_part) ≤ min(right_part)

这样所有的元素就分为 _{A,B}_ 两个长度相等的子集中，其中的一个子集始终比另一个大，那么:

>             $median=\frac{max(left_part)+min(right_part)}{2}$    

为了满足上述要求，我们只需确保下术2个条件：   

	1. i+j=m-i=n-j (or: m-i+n-j+1), 如果 n>=m, i=0~m,$j=\frac{m+n+1}{n}-i$
	2. B[j-1]<=A[i] and A[i-1]<=B[j]

我们所需要做的仅仅是：

	在[0,m]中搜索i，使得： B[j-1]<=A[i] and A[i-1]<B[j] ,其中$j=\frac{m+n+1}{2}-i$

可以使用二分搜场来实现查找，具体过程如下：

	1. 令 imin=0,imax=m,然后在[imin,imax]中开始搜索；
	2. $i=\frac{imin+imax}{2}$ $j=\frac{m+n+1}{2}-i$
	3. 由于Len(left_part)=len(right_part),所以只有以下3种情况：

		- B[j-1] <= A[i] and A[i-1]<B[j],即符合要求，停止搜索
		- B[j-1]>A[i], A[i] 偏小，增加i;
		- A[i-1]>B[j], A[i-1]偏大，减小i;

```java
class Solution{
	public double findMedianSortedArrays(int[]A,int[]B){
		int m=A.length;
		int n=B.length;
		if(m>n){  //ensure m<=n (why?)
			int[]temp=A;A=B;B=temp;
			int tmp=m;m=n;n=tmp;
		}
		int iMin=0,iMax=m,halfLen=(m+n+1)/2;
		while(iMin<=iMax){
			int i=(iMin+iMax)/2;
			int j=halfLen-i;
			if(i<iMax && B[j-1]>A[i]){
				//i is too small ,increse
				iMin=iMin+1;
			}
			else if(i>iMin && A[i-1]>B[j]){
				iMax=iMax-1;
			}
			else{
				int maxLeft = 0;
                if (i == 0) { maxLeft = B[j-1]; }
                else if (j == 0) { maxLeft = A[i-1]; }
                else { maxLeft = Math.max(A[i-1], B[j-1]); }
                if ( (m + n) % 2 == 1 ) { return maxLeft; }

                int minRight = 0;
                if (i == m) { minRight = B[j]; }
                else if (j == n) { minRight = A[i]; }
                else { minRight = Math.min(B[j], A[i]); }

                return (maxLeft + minRight) / 2.0;
			}
		}
		return 0;
	}
}


```

网友采用归并排序思路求解，即根据数且长度求解归并排序的第K个元素或者K,K+1个元素

```java
	public double findMedian(int []arr1,int[]arr2){
		int len1=arr1.length;
		int len2=arr2.length;
		int index=(len1+len2)/2;
		boolean isOdd=(len1+len2)%2==1;
		if(isOdd){
			return find(arr1,len1,arr2,len2,index);
		}
		else{
			return (find(arr1,len1,arr2,len2,index)+find(arr1,len1,arr2,len2,index))/2;
		}
	}
	/**
	 * 返回两个数组中的第K个元素
	 * @param arr1 
	 * @param len1
	 * @param arr2
	 * @param len2
	 * @param index
	 * @return
	 */
	public double find(int[]arr1,int len1,int[] arr2,int len2,int index){
		int i=0,j=0;
		boolean is1=false;
		boolean is2=false;
		int k;
		for(k=0;k<=index;k++){
			if(k>=len1){
				is2=true;
				j++;
			}
			else if(k>=len2){
				is1=true;
				i++;
			}
			else if(arr1[i]<arr2[j]){
				is1=true;
				i++;
			}
			else{
				is2=true;
				j++;
			}
		}
		if(j-1<0){
			return arr1[i-1];
		}
		if(i-1<0){
			return arr2[j-1];
		}
		return Math.max(arr1[i-1], arr2[j-1]);
	}

```


[leetcode](https://leetcode.com/articles/median-of-two-sorted-arrays/)