# 第 12 章　Matplotlib 不规则子图布局（subplot2grid）

> 对应 notebook：`notebooks/12_Matplotlib不规则子图.ipynb`

---

## 一、实验目的

学习使用 `subplot2grid()` 创建不规则子图布局，理解 `rowspan` 和 `colspan` 的作用，并比较不同网格结构下子图的排列方式。

---

## 二、实验原理

```python
subplot2grid(shape, loc, rowspan=1, colspan=1)
```

先把画布划分为虚拟网格，再从指定位置创建子图：

| 参数 | 含义 |
| --- | --- |
| `shape` | 网格的行数和列数，如 `(2, 3)` |
| `loc` | 子图的**起始位置**，行列索引从 **0** 开始，如 `(0, 0)` |
| `rowspan` | 纵向跨越的行数 |
| `colspan` | 横向跨越的列数 |

### 与普通 subplot 的区别

- 普通 `subplot()` **只能均分画布**，所有子图大小一致；
- `subplot2grid()` 基于虚拟网格，通过 `rowspan`（纵向跨行）、`colspan`（横向跨列）实现**单元格合并**，灵活构造不规则排版。

---

## 三、实验内容一：2 行 3 列布局

```python
import matplotlib.pyplot as plt

fig = plt.figure(figsize=(7, 5))

# 左右子图各纵向跨越 2 行，中间子图各占 1 个网格
ax1 = plt.subplot2grid((2, 3), (0, 0), rowspan=2)   # 左侧通高
ax2 = plt.subplot2grid((2, 3), (0, 1))              # 中上
ax3 = plt.subplot2grid((2, 3), (1, 1))              # 中下
ax4 = plt.subplot2grid((2, 3), (0, 2), rowspan=2)   # 右侧通高

for index, axis in enumerate((ax1, ax2, ax3, ax4), start=1):
    axis.set_title(f"ax{index}")
    axis.set_xticks([])
    axis.set_yticks([])

fig.suptitle("2 行 3 列布局")
fig.tight_layout()
plt.show()
```

### 布局结构

| ax1（跨 2 行） | ax2 | ax4（跨 2 行） |
| --- | --- | --- |
|  | ax3 |  |

### 布局解析

| 子图 | 起始位置 | 跨度 | 效果 |
| --- | --- | --- | --- |
| ax1 | `(0, 0)` | `rowspan=2` | 纵向跨越 2 行，左侧通高 |
| ax2 | `(0, 1)` | 默认 | 网格第 0 行第 1 列 |
| ax3 | `(1, 1)` | 默认 | 网格第 1 行第 1 列 |
| ax4 | `(0, 2)` | `rowspan=2` | 纵向跨越 2 行，右侧通高 |

这种布局适合把两个较大的图形放在左右两侧，把两个较小的辅助图形放在中间。

---

## 四、实验内容二：3 行 2 列布局

```python
fig = plt.figure(figsize=(7, 5))

ax1 = plt.subplot2grid((3, 2), (0, 0), colspan=2)   # 顶部横跨 2 列
ax2 = plt.subplot2grid((3, 2), (1, 0))              # 中间左
ax3 = plt.subplot2grid((3, 2), (1, 1))              # 中间右
ax4 = plt.subplot2grid((3, 2), (2, 0), colspan=2)   # 底部横跨 2 列

for index, axis in enumerate((ax1, ax2, ax3, ax4), start=1):
    axis.set_title(f"ax{index}")
    axis.set_xticks([])
    axis.set_yticks([])

fig.suptitle("3 行 2 列布局")
fig.tight_layout()
plt.show()
```

### 布局结构

| ax1（跨 2 列） |
| --- |
| ax2 | ax3 |
| ax4（跨 2 列） |

顶部 `ax1` 横向跨越 2 列，中间的 `ax2` 和 `ax3` 分别占据一列，底部 `ax4` 再次横向跨越 2 列。

---

## 五、结果分析

两段代码都成功创建了 4 个坐标轴对象：

- 第一种布局突出**左右两侧**的区域；
- 第二种布局突出**上下两条横向**区域。

由此可见，`rowspan` 适合控制**纵向扩展**，`colspan` 适合控制**横向扩展**。

---

## 六、实验总结

`subplot2grid()` 能够突破等尺寸子图的限制，适合制作**仪表盘、综合分析图和多指标展示页面**。实际绘图时，只需在 `ax1`、`ax2`、`ax3` 和 `ax4` 上分别调用 `plot()`、`bar()` 或 `scatter()` 等方法即可填充数据。

---

## 七、本章小结与考点

1. `subplot2grid(shape, loc, rowspan, colspan)` 四个参数的含义；
2. `loc` 的索引**从 0 开始**；
3. `rowspan` 纵向跨行、`colspan` 横向跨列，二者是构造不规则布局的核心；
4. 普通 `subplot()` 只能均分，`subplot2grid()` 可合并单元格；
5. 用 `enumerate` + `set_title(f"ax{index}")` 可快速标注各子图；
6. `fig.tight_layout()` 避免子图标题与标签重叠。
