# 第 5 章　sklearn 建模入门（内置数据集与多项式特征）

> 对应 notebook：`notebooks/05_sklearn建模入门.ipynb`

---

## 一、sklearn 的两种数据来源

| 来源 | 方式 | 示例 |
| --- | --- | --- |
| **内置数据集** | `sklearn.datasets` 直接加载 | `load_iris()`、`load_diabetes()`、`load_breast_cancer()` |
| **自定义数据集** | 用 pandas 读取 CSV 后自行分离 `X` / `y` | `pd.read_csv(...)` |

内置数据集返回的是**类字典结构**，统一具备以下键：

| 键 | 含义 | 维度 |
| --- | --- | --- |
| `data` | 特征矩阵 | 二维 |
| `target` | 标签 | 一维 |
| `feature_names` | 特征名 | —— |
| `target_names` / `DESCR` | 类别名 / 数据集说明 | —— |

---

## 二、内置数据集

### 2.1 鸢尾花数据集 load_iris

```python
from sklearn.datasets import load_iris
iris = load_iris()
```

- 150 个样本、4 个特征、3 个类别；
- 标签 `0 / 1 / 2` 分别对应鸢尾花的 3 个类别；
- 特征为花瓣（petal）和花萼（sepal）的长与宽，据此判断鸢尾花类别。

### 2.2 糖尿病数据集 load_diabetes

```python
from sklearn import datasets
diabetes = datasets.load_diabetes()
```

- 10 个特征、1 个标签；
- 回归任务，标签为连续数值。

---

## 三、线性回归标准流程

```python
from sklearn.linear_model import LinearRegression
from sklearn import datasets

# 1. 加载数据集
diabetes = datasets.load_diabetes()

# 2. 选择模型（实例化）
line_model = LinearRegression()

# 3. 训练模型
line_model.fit(diabetes.data, diabetes['target'])

# 4. 预测（取前 10 行）
line_model.predict(diabetes.data[:10])       # 等价于 diabetes.data[:10, :]

# 5. 评估模型（R²）
line_model.score(diabetes.data, diabetes.target)

# 查看形状
diabetes.data.shape
```

**要点**

- `diabetes['target']` 与 `diabetes.target` 两种写法等价（字典式访问 / 属性访问）；
- `data[:10]` 与 `data[:10, :]` 等价，后者更明确地表示「前 10 行、所有列」。

---

## 四、非线性问题的思路

### 4.1 线性模型的局限

若数据本身不是线性关系，直接用线性回归会出现：

- 拟合效果差（R² 低）；
- 残差呈现明显的规律性而非随机分布。

非线性数据的常见特点：

- 输入特征维度高；
- 特征分布不均匀；
- 特征之间存在依赖关系；
- 特征之间存在非线性关系；
- 特征之间存在多态。

### 4.2 多项式特征展开

思路：**先做特征变换，再用线性模型拟合**。把原始特征做多项式展开，相当于在更高维空间里做线性回归，从而具备拟合非线性的能力。

```python
from sklearn.preprocessing import PolynomialFeatures

# 1. 实例化（degree 为多项式阶数）
pf = PolynomialFeatures(degree=10)

# 2. 训练并转换（fit 学参数，transform 应用）
pf.fit(diabetes.data)
data_new = pf.transform(diabetes.data)
data_new.shape          # 特征数显著增加

# 3. 用变换后的特征训练线性回归
line_model_new = LinearRegression()
line_model_new.fit(data_new, diabetes.target)

# 4. 评估
line_model_new.score(data_new, diabetes.target)
```

> `fit_transform(X)` 等价于 `fit(X)` 后再 `transform(X)`，是最常用的简写。

---

## 五、实验总结

本节学习了 `scikit-learn` 的基本使用流程：

1. **加载数据**：了解数据集中的特征和标签；
2. **选择模型**：实例化 `LinearRegression`；
3. **训练**：`fit()` 完成训练；
4. **预测**：`predict()` 进行预测；
5. **评估**：`score()` 评估模型效果。

实验还尝试了多项式特征方法。由于原始数据的特征之间可能存在非线性关系，使用 `PolynomialFeatures(degree=10)` 对数据进行多项式展开，再输入线性回归模型进行训练和评分。可见**线性回归也可以通过特征变换拟合一定程度的非线性关系**，但多项式次数过高会显著增加特征数量，可能带来计算量增大和**过拟合**问题，因此实际应用中需要结合模型评分、数据规模和验证集结果选择合适的参数。

---

## 六、本章小结与考点

1. 内置数据集是**类字典结构**，`data` 二维、`target` 一维；
2. sklearn 建模统一五步：加载 → 实例化 → `fit` → `predict` → `score`；
3. `PolynomialFeatures` 属于**数据预处理（processing）**，不是模型；
4. 多项式阶数越高，特征数增长越快，越容易过拟合 —— 这是下一章「过拟合与正则化」的引入点；
5. `fit_transform` = `fit` + `transform` 的简写。
