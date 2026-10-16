# 第 2 章　NumPy 广播（Broadcasting）机制

> 对应 notebook：`notebooks/02_NumPy广播机制.ipynb`
> 扩展阅读：`notes/实验报告_广播机制.md`（完整原理推导 + 8 组实验验证）

---

## 一、什么是广播

广播（Broadcasting）是 NumPy 在**两个形状不同的数组之间执行逐元素（element-wise）运算**时采用的自动形状适配规则。它使 `a + b`、`a * b` 这类运算无需手写循环或显式复制数据即可完成。

---

## 二、三条核心规则

设两个数组形状分别为 `s1` 和 `s2`，按以下步骤判定兼容性与结果形状：

| 步骤 | 规则 | 说明 |
| --- | --- | --- |
| ① | **维度对齐** | 维数不同时，把维数较小的形状在**左侧补 1**。如 `(3,)` 视作 `(1, 3)` |
| ② | **逐维判定** | 从**最右侧维度**开始逐维比较：两维**相等**，或其中**一个为 1**，则该维兼容 |
| ③ | **形状确定** | 结果形状每一维取两者中的**较大值**；为 1 的维度被「虚拟复制」为较大值 |

若某一对维度既不相等、又没有一个为 1，则抛出：

```
ValueError: operands could not be broadcast together with shapes (3,4) (3,)
```

### 结果形状速查

```
(3, 4) + (4,)      -> (3, 4)        # 行向量按行复制
(3, 1) + (1, 4)    -> (3, 4)        # 列向量与行向量同时扩展（外积式）
(2, 3, 4) + (4,)   -> (2, 3, 4)     # 低位对齐后广播
(5, 1, 4) + (3, 4) -> (5, 3, 4)     # 第 1 维 1 -> 3
(3, 4) + (3,)      -> ValueError    # 4 与 3 既不等也不含 1
```

---

## 三、实验验证

### 3.1 标量广播（0 维）

```python
import numpy as np

numbers = np.array([1, 2, 3, 4])
result = numbers + 10
print("原数组：", numbers)       # [1 2 3 4]
print("加上标量 10：", result)   # [11 12 13 14]
```

标量被视作形状 `()`，与任何形状都兼容 —— 这是「补 1 对齐 + 维度扩展」的极端情形。

### 3.2 同形数组运算

```python
first = np.array([1, 2, 3])
second = np.array([10, 20, 30])
print(first + second)   # [11 22 33]
```

形状完全相同，逐元素对应运算，无需扩展。

### 3.3 二维数组广播（列向量 + 行向量）

```python
column = np.array([[1], [2], [3]])          # 形状 (3, 1)
row = np.array([[10, 20, 30, 40]])          # 形状 (1, 4)
result = column + row

print("列数组形状：", column.shape)   # (3, 1)
print("行数组形状：", row.shape)      # (1, 4)
print("结果形状：", result.shape)     # (3, 4)
print("广播结果：\n", result)
```

输出：

```
[[11 21 31 41]
 [12 22 32 42]
 [13 23 33 43]]
```

两个维度**同时扩展**，得到 `(3, 4)`，等价于一次外积式运算。

### 3.4 不兼容形状

```python
left = np.ones((2, 3))
right = np.ones((2, 2))

try:
    result = left + right
except ValueError as error:
    print("无法广播：", type(error).__name__)
    print("原因：两个数组的对应维度不满足匹配规则")
```

输出：

```
无法广播： ValueError
原因：两个数组的对应维度不满足匹配规则
```

最右侧维度 `3 vs 2` 既不相等也不含 1，判定不兼容。

---

## 四、关键性质：广播是「虚拟复制」，不是真复制

广播的维度扩展**不在内存中真的复制数据**。NumPy 通过 **stride（步长）技巧**实现：广播维度的步长被置为 **0 字节**，沿该维度「走 1000 步」实际都在读取同一块数据。

```python
small = np.array([1.0, 2.0])
b3 = np.broadcast_to(small, (1000, 2))
print(b3.strides)                    # (0, 8) —— 第 0 维步长为 0
print(np.shares_memory(b3, small))   # True
```

因此广播运算具备三个优点：

- **零拷贝**：不消耗额外内存（对比 `np.tile` 的真实复制）；
- **向量化**：运算在 C 层循环完成，避免 Python 层 for 循环；
- **惰性视图**：`np.broadcast_to` 返回原数组的视图。

---

## 五、工程应用

### 5.1 逐列标准化（Z-score）

```python
data = np.array([[ 1.,  2.,  3.],
                 [ 4.,  5.,  6.],
                 [ 7.,  8.,  9.],
                 [10., 11., 12.]])        # (4,3)
mu    = data.mean(axis=0)                 # (3,)
sigma = data.std(axis=0)                  # (3,)
norm  = (data - mu) / sigma               # (4,3) 与 (3,) 广播
print(norm.mean(axis=0), norm.std(axis=0))   # [0. 0. 0.] [1. 1. 1.]
```

### 5.2 批量图像去均值

```python
imgs = np.random.rand(2, 3, 4)                  # 2 张图、3 通道、4×4
means = imgs.mean(axis=(1, 2), keepdims=True)   # (2,1,1)
centered = imgs - means
print(centered.mean(axis=(1, 2)))               # [-0. 0.]
```

`keepdims=True` 保留维度，使 `(2,1,1)` 能与 `(2,3,4)` 正确广播 —— 这是深度学习中 `LayerNorm`、数据归一化的常用手法。

---

## 六、本章小结与考点

1. **判定顺序**：从右往左逐维比较，维数不足在左侧补 1；
2. **兼容条件**：该维相等，或其中一个为 1；
3. **结果形状**：每维取较大值；
4. **失败条件**：既不相等、也没有一个为 1 → `ValueError`；
5. **本质**：广播是 stride=0 的虚拟复制，零拷贝、向量化；
6. **易错点**：`(N,)` 一维数组用于「列」语义时容易出错，应显式用 `reshape(-1, 1)` 或 `keepdims=True` 控制形状。
