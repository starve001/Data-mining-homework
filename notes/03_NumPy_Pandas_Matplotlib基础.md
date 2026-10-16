# 第 3 章　NumPy、Pandas 与 Matplotlib 基础实验

> 对应 notebook：`notebooks/03_NumPy_Pandas_Matplotlib基础.ipynb`
> 扩展阅读：`notes/实验报告_NumPy_Pandas_Matplotlib.md`（完整方法清单 + 验证代码）

---

## 一、实验目的

1. 掌握 NumPy 数组的创建、属性查看、索引切片和基本运算；
2. 理解数组广播机制，能使用统计函数和线性代数函数做简单分析；
3. 掌握 Pandas 中 `Series` 与 `DataFrame` 的基本用法；
4. 学会表格数据的筛选、排序、分组、透视、缺失值处理和文件读写；
5. 掌握折线图、散点图、柱状图、直方图、饼图和箱线图的绘制。

**环境**：Python 3.12.4 / Jupyter Notebook / NumPy、Pandas、Matplotlib；随机数据固定种子保证可复现。

---

## 二、NumPy 数组运算

### 2.1 实验原理

NumPy 的核心对象是多维数组 `ndarray`，元素通常具有相同数据类型，可通过统一的**向量化运算**完成批量计算。相比逐个处理元素，向量化代码更简洁，也更适合科学计算。

### 2.2 数组创建与属性

```python
import numpy as np

array = np.array([1, 2, 3, 4, 5])
zeros = np.zeros((2, 3))
ones = np.ones((2, 2))
sequence = np.arange(0, 10, 2)

print("shape:", array.shape, "ndim:", array.ndim,
      "size:", array.size, "dtype:", array.dtype)
```

| 属性 | 含义 |
| --- | --- |
| `shape` | 形状（各维长度） |
| `ndim` | 维数 |
| `size` | 元素总个数 |
| `dtype` | 数据类型 |

| 创建函数 | 用途 |
| --- | --- |
| `np.array()` | 从列表创建 |
| `np.zeros()` / `np.ones()` | 全 0 / 全 1 |
| `np.eye()` | 单位矩阵 |
| `np.arange(start, stop, step)` | 等差序列（按步长） |
| `np.linspace(start, stop, num)` | 等分序列（按个数） |

### 2.3 索引、切片与广播

```python
matrix = np.arange(12).reshape(3, 4)
x = np.array([1, 2, 3])
y = np.array([4, 5, 6])

print(matrix[1, 2], matrix[:, 1], matrix[matrix > 7])
print(x + y)                                        # 逐元素加
print(np.arange(3).reshape(3, 1) + np.arange(3))    # (3,1)+(3,) -> (3,3)
```

| 索引方式 | 写法 | 含义 |
| --- | --- | --- |
| 多维下标 | `m[1, 2]` | 第 1 行第 2 列 |
| 整行 / 整列 | `m[0, :]` / `m[:, 1]` | 第 0 行 / 第 1 列 |
| 布尔索引 | `m[m > 7]` | 条件筛选 |
| 花式索引 | `m[[0, 2]]` | 按下标列表取多行 |

### 2.4 统计与线性代数

```python
x = np.array([1, 2, 3])
matrix = np.array([[1, 2], [3, 4]])

print("统计:", np.max(x), np.mean(x), np.std(x), np.argmax(x))
print("逆矩阵:\n", np.linalg.inv(matrix))
print("行列式:", np.linalg.det(matrix))
```

| 函数 | 用途 |
| --- | --- |
| `np.max` / `np.min` / `np.mean` / `np.std` | 统计值 |
| `np.argmax` / `np.argmin` | 极值位置 |
| `np.linalg.inv` | 逆矩阵 |
| `np.linalg.det` | 行列式 |
| `np.linalg.eigvals` | 特征值 |

### 2.5 合并与拆分

```python
x = np.array([1, 2, 3])
y = np.array([4, 5, 6])

print("vstack:\n", np.vstack([x, y]))      # 垂直堆叠 -> 2×3
print("hstack:", np.hstack([x, y]))        # 水平拼接 -> 1×6
print("split:", np.split(np.arange(10), 5))
```

### 2.6 结果分析

`shape`、`ndim`、`size`、`dtype` 能快速描述数组结构；布尔索引可筛选满足条件的元素；广播机制使形状兼容的数组直接参与运算；对二维矩阵求逆和行列式可完成基础线性代数分析。

---

## 三、Pandas 表格数据处理

### 3.1 实验原理

Pandas 提供带标签的一维结构 `Series` 和二维表格结构 `DataFrame`。通过行列索引、条件筛选、缺失值处理、分组聚合和透视表，可把原始数据整理为更适合分析的结构。

### 3.2 创建 Series 与 DataFrame

```python
import pandas as pd

series = pd.Series([10, 20, 30], index=["a", "b", "c"], name="销量")
dataframe = pd.DataFrame({
    "姓名": ["张三", "李四", "王五"],
    "成绩": [88, 92, 75],
    "班级": ["1班", "1班", "2班"],
})
```

### 3.3 选择、增加与清洗

```python
dataframe["优秀"] = dataframe["成绩"] >= 85    # 新增布尔列

missing = pd.DataFrame({"A": [1, np.nan, 3], "B": [4, 5, np.nan]})
print("缺失值:\n", missing.isna().sum())       # 逐列统计缺失
print("填充后:\n", missing.fillna(0))          # 填充
```

| 操作 | 方法 |
| --- | --- |
| 取单列 / 多列 | `df['列名']` / `df[['列1','列2']]` |
| 按标签 / 按位置 | `loc[行, 列]` / `iloc[i, j]` |
| 条件筛选 | `df[条件]` / `query()` |
| 缺失值 | `isna()` / `isna().sum()` / `fillna()` / `dropna()` |

> **易错点**：`loc` 按标签索引，`iloc` 按位置索引，两者不可混淆。

### 3.4 分组、排序、合并与透视

```python
scores = pd.DataFrame({"性别": ["男", "女", "男", "女"], "成绩": [88, 92, 75, 95]})

print("平均分:\n", scores.groupby("性别")["成绩"].mean())
print("排序:\n", scores.sort_values("成绩", ascending=False))
print("透视:\n", scores.pivot_table(index="性别", values="成绩", aggfunc="mean"))
```

`groupby` 体现「拆分 — 应用 — 合并」思想；多指标聚合用命名 `agg`：

```python
df.groupby("性别").agg(人数=("成绩", "count"),
                       平均分=("成绩", "mean"),
                       最高分=("成绩", "max"))
```

### 3.5 时间序列与文件读写

```python
sales = pd.DataFrame({
    "日期": pd.date_range("2026-01-01", periods=5),
    "销量": [12, 15, 14, 20, 18],
})
print(sales.resample("ME", on="日期")["销量"].sum())   # 按月重采样求和
sales.to_csv("demo_df.csv", index=False, encoding="utf-8-sig")
```

> `encoding="utf-8-sig"` 可让 Excel 正确识别中文，避免乱码。

### 3.6 结果分析

Pandas 能较方便地完成结构化数据的清洗和汇总：新增「优秀」字段把数值成绩转换为分类信息；`groupby` 比较不同性别的平均成绩；`resample` 按时间周期汇总销售数据并保存为 CSV。

---

## 四、Matplotlib 数据可视化

### 4.1 实验原理

不同图表适合表达不同类型的信息：

| 图表 | 适用场景 |
| --- | --- |
| 折线图 | 观察趋势 |
| 散点图 | 观察变量关系 |
| 柱状图 | 比较类别 |
| 直方图 | 观察分布 |
| 饼图 | 展示比例 |
| 箱线图 | 比较分布特征（中位数、离散程度、异常值） |

### 4.2 折线图与散点图

```python
import matplotlib
matplotlib.use("Agg")          # 非交互后端：保存文件而非弹窗
import matplotlib.pyplot as plt

figure, axes = plt.subplots(1, 2, figsize=(10, 4))
x = np.linspace(0, 2 * np.pi, 100)
axes[0].plot(x, np.sin(x), label="sin(x)")
axes[0].plot(x, np.cos(x), "--", label="cos(x)")
axes[0].set_title("折线图"); axes[0].legend()

xs = rng.normal(0, 1, 100)
ys = 0.8 * xs + rng.normal(0, 0.5, 100)
axes[1].scatter(xs, ys, c=ys, cmap="viridis")
axes[1].set_title("散点图")

figure.tight_layout()
figure.savefig("plot_line_scatter.png", dpi=150)
```

### 4.3 柱状图与直方图

```python
subjects = ["Python", "数学", "英语", "物理"]
bars = axes[0].bar(subjects, [88, 92, 75, 81], color="tab:orange")
axes[0].bar_label(bars, fmt="%d")                    # 柱顶数值标签
axes[1].hist(rng.normal(100, 15, 1000), bins=30)     # 直方图看分布
```

### 4.4 饼图与箱线图

```python
axes[0].pie([35, 25, 20, 20], labels=["A", "B", "C", "D"], autopct="%.1f%%")
groups = [rng.normal(mean, 5, 60) for mean in (60, 65, 70, 75)]
axes[1].boxplot(groups, tick_labels=["甲", "乙", "丙", "丁"], showmeans=True)
```

> **版本注意**：Matplotlib 3.9 起 `boxplot` 的 `labels` 参数更名为 `tick_labels`。

### 4.5 标注与网格

```python
x = np.linspace(0, 10, 200)
y = np.exp(-x / 5) * np.cos(x)
axis.plot(x, y, label="衰减曲线")
peak = np.argmax(y)
axis.annotate("最大值", (x[peak], y[peak]), xytext=(x[peak] + 1, 0.7),
              arrowprops={"arrowstyle": "->"})
axis.grid(alpha=0.3)
axis.legend()
```

> **踩坑记录**：`annotate` 的坐标若写成复合表达式，容易因运算符优先级把索引作用到错误对象上导致 `TypeError`。先算中间变量 `y = ...` 再取 `y[peak]` 更稳妥。

### 4.6 中文显示

```python
plt.rcParams['font.sans-serif'] = ['Microsoft YaHei']   # 否则中文显示为方框
plt.rcParams['axes.unicode_minus'] = False              # 正常显示负号
```

### 4.7 结果分析

绘制结果能直观展示正弦/余弦曲线、随机数据的相关趋势、不同科目成绩差异以及随机样本分布。箱线图可同时反映各组数据的中位数、离散程度和异常值；曲线标注可突出最大值位置。

---

## 五、实验总结

1. **NumPy** 适合高效数组运算和数学计算，广播机制可减少重复代码；
2. **Pandas** 适合处理结构化数据，能快速完成筛选、清洗、统计和保存；
3. **Matplotlib** 能把分析结果转化为直观图形，便于发现趋势、差异和分布；
4. 实际任务中三者结合，形成「数据读取 → 数据清洗 → 统计分析 → 图表展示」的完整工作流；
5. 应根据数据类型和分析目标选择合适的数据结构与图表类型，并注意标题、图例、标注和坐标轴信息的完整性。
