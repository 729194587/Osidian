
NumPy 是一个扩展 Python 基础功能的库，增加了更丰富的数据集，包括更多的数值类型、向量、矩阵和许多矩阵函数。
Python 算术算子处理 NumPy 数据类型，许多 NumPy 函数也支持 Python 数据类型。

#### NumPy数组

NumPy 的基本数据结构是一个可索引的 n 维数组，包含相同类型（`dtype`）的元素。

```python
# NumPy routines which allocate memory and fill arrays with value
a = np.zeros(4);                print(f"np.zeros(4) :   a = {a}, a shape = {a.shape}, a data type = {a.dtype}")
a = np.zeros((4,));             print(f"np.zeros(4,) :  a = {a}, a shape = {a.shape}, a data type = {a.dtype}")
a = np.random.random_sample(4); print(f"np.random.random_sample(4): a = {a}, a shape = {a.shape}, a data type = {a.dtype}")
```
运行结果：![[NumPy数组初始化.png]]

```python
# NumPy routines which allocate memory and fill arrays with value but do not accept shape as input argument
a = np.arange(4.);              print(f"np.arange(4.):     a = {a}, a shape = {a.shape}, a data type = {a.dtype}")
a = np.random.rand(4);          print(f"np.random.rand(4): a = {a}, a shape = {a.shape}, a data type = {a.dtype}")
```
`np.arange(4.): a = [0. 1. 2. 3.], a shape = (4,), a data type = float64 np.random.rand(4): a = [0.08608833 0.9386982 0.11652006 0.59607103], a shape = (4,), a data type = float64`

```python
# NumPy routines which allocate memory and fill with user specified values
a = np.array([5,4,3,2]);  print(f"np.array([5,4,3,2]):  a = {a},     a shape = {a.shape}, a data type = {a.dtype}")
a = np.array([5.,4,3,2]); print(f"np.array([5.,4,3,2]): a = {a}, a shape = {a.shape}, a data type = {a.dtype}")
```
`np.array([5,4,3,2]):  a = [5 4 3 2],     a shape = (4,), a data type = int64
`np.array([5.,4,3,2]): a = [5. 4. 3. 2.], a shape = (4,), a data type = float64`

这些都生成了一个包含四个元素的一维向量 `a`。`a.shape` 返回尺寸。这里我们看到 `a.shape = ``（4，)`， 表示一个包含 4 个元素的 1 维数组。

#### 向量操作

```python
# vector indexing operations on 1-D vectors
# 对一维向量进行索引操作

a = np.arange(10)
print(a)

# access an element
# 访问（取出）一个元素
print(f"a[2].shape: {a[2].shape} a[2]  = {a[2]}, Accessing an element returns a scalar")
# a[2] 的形状：{a[2].shape}  a[2] = {a[2]}，访问单个元素会返回一个标量（scalar）

# access the last element, negative indexes count from the end
# 访问最后一个元素，负索引从末尾开始计数
print(f"a[-1] = {a[-1]}")

# indexs must be within the range of the vector or they will produce and error
# 索引必须在向量的有效范围内，否则会产生错误
try:
    c = a[10]
except Exception as e:
    print("The error message you'll see is:")
    # 你将看到的错误信息：
    print(e)

```
##### 切片slice
切片使用一组三个值（ `start：stop：step` ）创建索引数组。

```python
# vector slicing operations
# 向量切片操作

a = np.arange(10)
print(f"a         = {a}")

# access 5 consecutive elements (start:stop:step)
# 访问 5 个连续元素（起始:结束:步长）
c = a[2:7:1];     print("a[2:7:1] = ", c)

# access 3 elements separated by two
# 访问 3 个间隔为 2 的元素（每隔两个取一个）
c = a[2:7:2];     print("a[2:7:2] = ", c)

# access all elements index 3 and above
# 访问所有从索引 3 开始（含 3）到末尾的元素
c = a[3:];        print("a[3:]    = ", c)

# access all elements below index 3
# 访问所有索引在 3 以下的元素（不含 3）
c = a[:3];        print("a[:3]    = ", c)

# access all elements
# 访问所有元素
c = a[:];         print("a[:]     = ", c)
```
![[切片运行结果.png]]

相同大小的向量之间可以相加，也可以通过乘除一个常数来缩放向量。

#### 点积

点积将两个向量的数值逐元素相乘，然后对结果求和。向量点积要求两个向量的维度相等。
向量化计算点积比循环计算点积的速度快不少，NumPy 更好地利用了底层硬件中可用的数据并行性。GPU 和现代 CPU 实现了单指令多数据（SIMD）流水线，允许并行运行多个操作。

#### 矩阵

矩阵是二维数组，元素都是同一类型。

```python
a = np.zeros((2, 1))   #两行一列的矩阵。                                             
print(f"a shape = {a.shape}, a = {a}") 
a = np.array([[5], [4], [3]]);   print(f" a shape = {a.shape}, np.array: a = {a}")
#三行一列
np.array([5, 4, 3])
#这个不是一行三列的矩阵，而是一维向量。

np.array([[5, 4, 3]]).shape
#这个才是一行三列的矩阵。
```

##### reshape
```python
a = np.arange(6).reshape(-1, 2)
```
把一个一维向量重塑为列数为2的矩阵。
-1表示自动计算行数。
等价于
```python
a = np.arange(6).reshape(3, -1)
```
把行向量转换成列向量：
```python
a.reshape(-1, 1)
```

##### slice

矩阵的切片需要对列和行同时操作，例如：
```python

A[0:2, :]
#取0-1行，所有列。
A[:, 1:3]
#取1-2列，所有行。
A[0:2, 1:3]
#取0-1行，1-2列。
```


