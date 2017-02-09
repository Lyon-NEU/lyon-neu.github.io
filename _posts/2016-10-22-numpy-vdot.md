
## numpy.vdot
<font color=blue>numpy.vdot(a,b)</font>

numpy.vdot(a,b)
> Return the dot product of two vectors
> The vdot(a,b) function handles complex numbers differently thant dot(a,b).If the first argument is complex the complex conjugate of the first argument is used for the calculation of the dot product.

> Note that *vdot* handles multivimensional arrays differently than dot,it does not perform a matrix product,but flattens input arguments to 1-D vectors first. Consequently,it should only be used for vectors.

Examples

```python
>>>a=np.array([1+2j,3+4j])
>>>b=np.array([5+6j,7+8j])
>>>np.vdot(a,b)
(70-8j)
>>>np.vdot(b,a)
(70+8j)

```
Note that higher-dimensional arrays are flattened

```python
>>>a=np.array([[1,4],[5,6]])
>>>b=np.array([[4,1],[2,2]])
>>>np.vdot(a,b)
30

```



```python

```
