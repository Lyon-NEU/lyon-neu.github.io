---
layout: post
title: "Numpy Array"
descrption : "Numpy学习笔记"
category:
tags : [Python,Numpy,Array]
---
{% include JB/setup %}


学习<font color=blue>Numpy</font>的基本函数，常用操作，请参考[Numpy](http://docs.scipy.org/doc/numpy/reference/routines.html)

## 创建数组

+ Ones and Zeros
	- empty(shape[,dtype,order])
		- 返回指定大小和类型，未初始化的数组
		- shape: int或者int型的元组
		- dtype: 数据类型
		- order: {'C','F'}，可选，以类`C`行为主，或者类`Fortan`列为主的存储方式
		
		```   
		>>> np.empty([2, 2])
			array([[ -9.74499359e+001,   6.69583040e-309],
       		[  2.13182611e-314,   3.06959433e-309]])         #random
		```
		- 注意，不像`zeros`，它对数组没有进行初始化，所以速度会快一些，另一方面却需要用户去初始化
	- empty_like(a,dtype=None,order='K',subok=True)
		- 返回一个与`a`相似的数组
	- eye(N,M=None,k=0;dtype=<type 'float'>)
		- 返回一个二维单位数组，除对角线元素为1，其余位置都为0
		- N: int 数组的行数
		- M: int 可选，如果为None,默认为N
		- k: int,可选，对角线索引，默认0，指向主对线元素，正数相对上面的对角，负数是下面的对象
		- dtyap: 数据类型
		- Returns: I ndarray of shape(N,M)

		```    
		>>> np.eye(2, dtype=int)
		array([[1, 0],
		       [0, 1]])
		>>> np.eye(3, k=1)
		array([[ 0.,  1.,  0.],
       		   [ 0.,  0.,  1.],
       		   [ 0.,  0.,  0.]])
		```
	- identity(n,dtype=None)
		- 返回一个身份数组，它是一个方阵，主对角线为1，其余为0
		- n: int,行数（列数）n*n方阵
		- dtype: 数据类型，默认浮点型

		```  
		>>> np.identity(3)
		array([[ 1.,  0.,  0.],
		       [ 0.,  1.,  0.],
		       [ 0.,  0.,  1.]])
		```
	- ones(shape,dtype=None,order='C')
		- 返回一个给定大小和类型的数组，用1填充
		- shape: 整数或者整数序列e.g.,(2,3)或2
		- dtype: 数据类型,e.g.,numpy.int8,默认是numpy.float64
		- order: {'C','F'},以C-or-Fortran方式存储
		- Returns: ndarray

		```     
		>>> np.ones(5)    
		array([ 1.,  1.,  1.,  1.,  1.])    
		>>>    
		>>> np.ones((5,), dtype=np.int)    
		array([1, 1, 1, 1, 1])    
		>>>    
		>>> np.ones((2, 1))    
		array([[ 1.],    
		       [ 1.]])    
		>>>    
		>>> s = (2,2)    
		>>> np.ones(s)    
		array([[ 1.,  1.],    
		       [ 1.,  1.]])    
		```
	- ones_like(a,dtype=None,order='K',subok=True)
		- 返回和`a`相同大小和类型的数组

		```
		>>> x = np.arange(6)
		>>> x = x.reshape((2, 3))
		>>> x
		array([[0, 1, 2],
		       [3, 4, 5]])
		>>> np.ones_like(x)
		array([[1, 1, 1],
		       [1, 1, 1]])
		>>>
		>>> y = np.arange(3, dtype=np.float)
		>>> y
		array([ 0.,  1.,  2.])
		>>> np.ones_like(y)
		array([ 1.,  1.,  1.])
		```
	- zeros(shape,dtype=float,order='C')
		- 返回指定大小和类型的数组，用0填充
		- 参数与`ones`里的类似

		```
		>>> np.zeros(5)
		array([ 0.,  0.,  0.,  0.,  0.])
		>>>
		>>> np.zeros((5,), dtype=np.int)
		array([0, 0, 0, 0, 0])
		>>>
		>>> np.zeros((2, 1))
		array([[ 0.],
		       [ 0.]])
		>>>
		>>> s = (2,2)
		>>> np.zeros(s)
		array([[ 0.,  0.],
		       [ 0.,  0.]])
		>>>
		>>> np.zeros((2,), dtype=[('x', 'i4'), ('y', 'i4')]) # custom dtype
		array([(0, 0), (0, 0)],
		      dtype=[('x', '<i4'), ('y', '<i4')])
		```
	-full(shape,fill_value,dtype=None,order='C')
		- shape: 整数或整数序列，如(2,3)或2
		- fill_value: 填充数据
		- dtype: 数据类型
		- order: {'C','F'}
		- Returns: ndarray

		```
		>>> np.full((2, 2), np.inf)
		array([[ inf,  inf],
		       [ inf,  inf]])
		>>> np.full((2, 2), 10, dtype=np.int)
		array([[10, 10],
		       [10, 10]])
		```
+ 从已知数据创建

	- array(object[,dtype,copy,order,subok,ndmin])
	- object: array_like ,数组，任何暴露数组接口的对象，
	- dtype: data-type,可选
	- copy: bool（默认True）,可选，
	- order: {'C','F','A'}
	- subok: bool,可选

	```python
	>>> np.array([1, 2, 3])
	array([1, 2, 3])
	Upcasting:

	>>>
	>>> np.array([1, 2, 3.0])
	array([ 1.,  2.,  3.])
	More than one dimension:

	>>>
	>>> np.array([[1, 2], [3, 4]])
	array([[1, 2],
	       [3, 4]])
	Minimum dimensions 2:

	>>>
	>>> np.array([1, 2, 3], ndmin=2)
	array([[1, 2, 3]])
	Type provided:

	>>>
	>>> np.array([1, 2, 3], dtype=complex)
	array([ 1.+0.j,  2.+0.j,  3.+0.j])
	Data-type consisting of more than one element:

	>>>
	>>> x = np.array([(1,2),(3,4)],dtype=[('a','<i4'),('b','<i4')])
	>>> x['a']
	array([1, 3])
	Creating an array from sub-classes:

	>>>
	>>> np.array(np.mat('1 2; 3 4'))
	array([[1, 2],
	       [3, 4]])
	>>>
	>>> np.array(np.mat('1 2; 3 4'), subok=True)
	matrix([[1, 2],
	        [3, 4]])
	```

	-asarray(a,dtype=None,order=None)
	-a: 数组形式的数据，任何可以转换成数组的数据，包括列表/元组等等
	-dtype: 数据类型，默认和输入的数据一致
	-order: {'C','F'}

	```python
	Convert a list into an array:

	>>>
	>>> a = [1, 2]
	>>> np.asarray(a)
	array([1, 2])
	Existing arrays are not copied:

	>>>
	>>> a = np.array([1, 2])
	>>> np.asarray(a) is a
	True
	If dtype is set, array is copied only if dtype does not match:

	>>>
	>>> a = np.array([1, 2], dtype=np.float32)
	>>> np.asarray(a, dtype=np.float32) is a
	True
	>>> np.asarray(a, dtype=np.float64) is a
	False
	Contrary to asanyarray, ndarray subclasses are not passed through:

	>>>
	>>> issubclass(np.matrix, np.ndarray)
	True
	>>> a = np.matrix([[1, 2]])
	>>> np.asarray(a) is a
	False
	>>> np.asanyarray(a) is a
	True
	```
	-asmatrix(data,dtype=None)
	-data: 输入数据，数组形式
	-dtype: 数据类型

	```python
	>>> x = np.array([[1, 2], [3, 4]])
	>>>
	>>> m = np.asmatrix(x)
	>>>
	>>> x[0,0] = 5
	>>>
	>>> m
	matrix([[5, 2],
	        [3, 4]])
	```
