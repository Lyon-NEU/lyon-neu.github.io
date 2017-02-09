---
layout: post
title:  "Numpy笔记"
description: "Numpu学习笔记"
category:
tags: [Python,Numpy,Array]
---
{% include JB/setup %}

### numpy.sort
numpy.sort(a,axis=-1,kind='quicksort',order=None)
  返回一个排好序的新数组

> 参数: 

>`a`:排序数组

>`axis`: int 或 None,可选参数
       根据哪个Axis排序

>`kind`:{quicksort,mergesort,heapsort},可选，排序算法，默认归并排序

>`order`:str或 str list ,可选，


```python
import numpy as np
a=np.array([[1,4],[3,1]])
np.sort(a)   # sort along the last axis ，对每一行进行排序
```




    array([[1, 4],
           [1, 3]])




```python
np.sort(a,axis=None)  #sort the flatten array
```




    array([1, 1, 3, 4])




```python
np.sort(a,axis=0)    #sort along the first axis，对每一列排序
```




    array([[1, 1],
           [3, 4]])




```python
dtype=[('name','S10'),('height',float),('age',int)]
value=[('Arthur',1.8,41),('Lancelot',1.9,38),('Galahad',1.7,38)]
a=np.array(value,dtype=dtype)  # create a structured array
np.sort(a,order='height')  #sorted by height
```




    array([(b'Galahad', 1.7, 38), (b'Arthur', 1.8, 41), (b'Lancelot', 1.9, 38)], 
          dtype=[('name', 'S10'), ('height', '<f8'), ('age', '<i4')])




```python
np.sort(a,order=['age','height'])  #sort by age ,then height if ages are equal
```




    array([(b'Galahad', 1.7, 38), (b'Lancelot', 1.9, 38), (b'Arthur', 1.8, 41)], 
          dtype=[('name', 'S10'), ('height', '<f8'), ('age', '<i4')])




```python

```
