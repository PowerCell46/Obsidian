# Numpy Array

```python
import numpy as np  
  
arr_1_dimension = np.array([1, 2,  3])  
  
arr_2_dimensions = np.array([  
    [1, 2, 3],  
    [4, 5, 6],  
    [7, 8, 9]]  
)  
  
arr_3_dimensions = np.array([  
    [  
        [1, 2, 3],  
        [4, 5, 6],  
        [7, 8, 9]  
    ],  
    [  
        [10, 11, 12],  
        [13, 14, 15],  
        [16, 17, 18]  
    ],  
    [  
        [19, 20, 21],  
        [22, 23, 24],  
        [25, 26, 27]]  
])
```
## Array property functions

```python
arr_2_dimensions.shape
#  (3, 3)

arr_2_dimensions.ndim
#  Number of Dimensions

arr_2_dimensions.size
#  Number of total elements

x = np.array([[1, 1], [2,3], [3,4]]) 
np.unique(x) 
#  array([1, 2, 3, 4])
```

## Fast creation of Arrays

```python
linear_arr = np.arange(1, 10, 2)
#  Start, End, Step -> 
#  array([1, 3, 5, 7, 9])

random_int_arr = np.random.randint(0, 10, size=(3, 5))
%% [
	[2 0 0 8 3]
	[3 7 9 2 8]
	[2 4 3 1 9]
 ] %%

ones = np.ones((3, 3), dtype=int)
%% [
	[1 1 1]
	[1 1 1]
	[1 1 1]
] %%

zeros = np.zeros((2, 3), dtype=int)
%% [
	[0 0 0]
	[0 0 0]
]%%

random_float_arr_1 = np.random.random((5, 3))  
  
random_float_arr_2 = np.random.rand(5, 3)  
```

## Array math functions I

```python
a = np.array([[1, 2, 3], [4, 5, 6]])  
  
b = np.array([[1, 2, 3], [4, 5, 6]])  

# Multiply
print(a * b)
%% 
[
 [ 1  4  9]
 [16 25 36]
]
%%


a1 = np.array([1, 2, 3])  
  
ones = np.ones((1, 3), dtype=int)  


# Sum
print(ones + a1)
%% [
	[2 3 4]
] %%


# Subtraction
print(a1 - ones)  
 %% [
	 [0 1 2]
] %%


a2 = np.array([[1, 2, 3], [4, 5, 6]])


# Division
print(a2 / a1)
%% [
	[1.  1.  1. ]
	[4.  2.5 2. ]
] %%


# Floor Divison
print(a2 // a1)
%% [
	[1 1 1]
	[4 2 2]
] %%


# Exponentation
print(a2 ** 2)
%% [
	[ 1  4  9]
	[16 25 36]
] %%


# Remainder
print(a2 % 2)
%% [
	[1 0 1]
	[0 1 0]
] %%
```

## Array Functions II

```python
a = np.array([
	[1, 2, 3],
	[4, 5, 6]
])  

c = np.array([
	[1, 2, 3],
	[3, 4, 5],
	[5, 6, 7]
])  
  
print(a.dot(c))  
# 22 = a[0][0] * c[0][0] (1), a[0][1] * c[1][0] (6), a[0][2] * c[2][0] (15) 
# 22 = 1 * 1, 2 * 3, 3 * 5

# Обръшане на матрица
untransposed_matrix = np.array([  
    [1, 2, 3, 4],  
    [5, 6, 7, 8]
])  
  
transposed_matrix = np.transpose(untransposed_matrix)
%% [
	[1 5]
	[2 6]
	[3 7]
	[4 8]
] 
%%

# Reshaping a matrix
print(a.reshape(2, 3))
%% [
	[1 2 3]
	[4 5 6]
] %%

print(a.reshape(1, 6))
%% [
	[1 2 3 4 5 6]
] %%

# Flat a matrix
print(a.flatten())
```

## Array Functions III

```python
# Calculates the exponential
print(np.exp(a1))
%% 
	[ 2.71828183  7.3890561  20.08553692] 
%%


print(np.log(a1))
%% 
	[0. 0.69314718 1.09861229] 
%%

# Numpy's sum -> times faster with np arrays instead of Python's sum method
print(np.sum(arr1))

# Average Value
print(a2.mean())


print(np.sqrt(a2))

# Returns the biggest value from all nested arrays
print(a2.max())

#  Standard Deviation - how spread out a group of numbers is from the mean - sqrt of the variance
print(np.std(a2))


#  Variance measure of the average degree to which each number is different to the mean  
#  Higher variance = wider range of numbers, Lower variance = lower range of numbers  
print(np.var(a2))  

```

# Array Comparisons

```python
a1 = np.array([1, 2, 3])  
  
a2 = np.array([[1, 2, 3.3], [4, 5, 6.5]])  
  
print(a1 > a2)  
  
print(a1 >= a2)
%% [
	[True True False]
	[False False False]
] %%

print(a1 < a2)

print(a1 <= a2)


print(a1 > 5)  
  
print(a1 < 5)  


print(a1 == a2)
```

# Array Sorting

```python
arr = np.random.randint(10, size=(3, 5))

# Sorts them by row only!
print(np.sort(arr))

# Returns the sorted indexes  
print(np.argsort(arr))

# Returns the index of the min value from the flattened matrix
print(np.argmin(arr))  

# Returns the index of the max value from the flattened matrix
print(np.argmax(arr))
```

# Permutations, Combinations

```python
from itertools import permutations, combinations  
  
arr = np.array([1, 2, 3])  
  
for p in permutations(arr):  
    print(p)  

for comb in combinations(arr, 2):  
    print(comb)
```

____
## Algorithms with Python

```python
import numpy as np  
  
# Скалар - единичен основен елемент от данни (2)  
# Вектор - едномерен масив [1, 2, 3]  
# Матрица - многомерен масив [[1], [2], [3]]  
  
# shortNum = np.short(15)  
  
a = np.array([1, 2, 3])  
b = np.arange(0, 10 + 1, 2)  # Create array (start, end, step)  
  
# Math operations with the array's values  
b += 1  
b **= 2  
  
arr1 = np.array([1, 2, 3])  
arr2 = np.array([3, 0, 3])  
  
# Logical Operators  
arr3 = arr1 == arr2  
arr4 = arr1 > arr2  
  
print(np.logical_and(arr1, arr2))  
print(np.logical_or(arr1, arr2))  
print(np.logical_xor(arr1, arr2))
```