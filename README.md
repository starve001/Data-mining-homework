# 数据挖掘课程作业（Data Mining Homework）

本仓库整理数据挖掘课程的实验代码与章节知识点笔记，内容涵盖 **Python 数据分析三件套 → 回归分析 → 正则化 → 分类算法 → 模型评估** 的完整学习路径。

---

## 目录结构

```
.
├── README.md                     # 本文件（总索引）
├── requirements.txt              # 依赖清单
├── notebooks/                    # 实验代码（Jupyter Notebook）
│   ├── 01_数据可视化入门.ipynb
│   ├── 02_NumPy广播机制.ipynb
│   ├── 03_NumPy_Pandas_Matplotlib基础.ipynb
│   ├── 04_回归预测.ipynb
│   ├── 05_sklearn建模入门.ipynb
│   ├── 06_过拟合与正则化.ipynb
│   ├── 07_葡萄酒质量回归对比.ipynb
│   ├── 08_逻辑回归与参数调优.ipynb
│   ├── 09_朴素贝叶斯.ipynb
│   ├── 10_混淆矩阵与模型评估.ipynb
│   ├── 11_KNN算法与机器学习概述.ipynb
│   ├── 12_Matplotlib不规则子图.ipynb
│   ├── 13_回归分析实验报告.ipynb
│   └── practice/
│       └── 编程练习_洛谷.ipynb
├── notes/                        # 章节知识点笔记（Markdown）
│   ├── 01_数据可视化入门.md
│   ├── ...
│   ├── 13_回归分析实验报告.md
│   ├── 实验报告_广播机制.md                    # 广播机制专题（完整推导 + 8 组实验）
│   ├── 实验报告_NumPy_Pandas_Matplotlib.md     # 三件套方法总结与验证
│   └── assets/                   # 笔记引用的图表
└── data/                         # 实验数据集
    ├── 2015_data.csv / 2016_data.csv / 2017_data.csv
    ├── insurance.csv
    └── winequality-red.csv
```

---

## 章节导航

| # | 章节 | 代码 | 知识点笔记 | 核心内容 |
| --- | --- | --- | --- | --- |
| 1 | 数据可视化入门 | [01](notebooks/01_数据可视化入门.ipynb) | [01](notes/01_数据可视化入门.md) | Matplotlib / Pandas / NumPy 图表命令、数据预处理、多子图批量绘图 |
| 2 | NumPy 广播机制 | [02](notebooks/02_NumPy广播机制.ipynb) | [02](notes/02_NumPy广播机制.md) | 广播三条规则、形状推导、stride=0 零拷贝本质 |
| 3 | 三件套基础实验 | [03](notebooks/03_NumPy_Pandas_Matplotlib基础.ipynb) | [03](notes/03_NumPy_Pandas_Matplotlib基础.md) | 数组运算、表格处理、六类图表绘制 |
| 4 | 回归预测 | [04](notebooks/04_回归预测.ipynb) | [04](notes/04_回归预测.md) | 最小二乘法、一元/多元线性回归、sklearn 五步流程 |
| 5 | sklearn 建模入门 | [05](notebooks/05_sklearn建模入门.ipynb) | [05](notes/05_sklearn建模入门.md) | 内置数据集、`PolynomialFeatures` 多项式特征 |
| 6 | 过拟合与正则化 | [06](notebooks/06_过拟合与正则化.ipynb) | [06](notes/06_过拟合与正则化.md) | 偏差-方差权衡、Ridge(L2)、Lasso(L1)、标准化 |
| 7 | 葡萄酒质量回归对比 | [07](notebooks/07_葡萄酒质量回归对比.ipynb) | [07](notes/07_葡萄酒质量回归对比.md) | `RidgeCV` / `LassoCV`、MSE/RMSE/MAE/R² 四指标 |
| 8 | 逻辑回归与参数调优 | [08](notebooks/08_逻辑回归与参数调优.ipynb) | [08](notes/08_逻辑回归与参数调优.md) | 分层抽样、`predict_proba`、`GridSearchCV` 网格搜索 |
| 9 | 朴素贝叶斯 | [09](notebooks/09_朴素贝叶斯.ipynb) | [09](notes/09_朴素贝叶斯.md) | 条件独立假设、三种 NB 模型对比、拉普拉斯平滑 |
| 10 | 混淆矩阵与模型评估 | [10](notebooks/10_混淆矩阵与模型评估.ipynb) | [10](notes/10_混淆矩阵与模型评估.md) | TP/FN/FP/TN、精确率/召回率/F1、混淆矩阵可视化 |
| 11 | KNN 与机器学习概述 | [11](notebooks/11_KNN算法与机器学习概述.ipynb) | [11](notes/11_KNN算法与机器学习概述.md) | 监督/无监督、KNN 手写实现、`Counter` 投票 |
| 12 | 不规则子图布局 | [12](notebooks/12_Matplotlib不规则子图.ipynb) | [12](notes/12_Matplotlib不规则子图.md) | `subplot2grid` 的 `rowspan` / `colspan` |
| 13 | 回归分析实验报告 | [13](notebooks/13_回归分析实验报告.ipynb) | [13](notes/13_回归分析实验报告.md) | 保险费用预测、离散特征编码、一元→二元→多元 |

---

## 专题报告

| 报告 | 内容 |
| --- | --- |
| [实验报告_广播机制.md](notes/实验报告_广播机制.md) | 广播机制原理概述 + 8 组实验验证（含 stride 零拷贝证据、性能对比） |
| [实验报告_NumPy_Pandas_Matplotlib.md](notes/实验报告_NumPy_Pandas_Matplotlib.md) | 三个库常用方法分类总结 + 可运行验证代码 + 实际输出 |

---

## 数据集说明

| 文件 | 样本数 | 用途 |
| --- | --- | --- |
| `data/2015_data.csv` | 36 | 各地区经济指标（逗号分隔） |
| `data/2016_data.csv` | 36 | 同上（**分号分隔**，读取需 `sep=';'`） |
| `data/2017_data.csv` | 36 | 同上（逗号分隔） |
| `data/insurance.csv` | 1338 | 保险费用回归（age / sex / bmi / children / smoker / region → charges） |
| `data/winequality-red.csv` | 1599 | UCI 红葡萄酒质量（11 个理化特征 → quality 评分） |

---

## 环境与运行

```bash
pip install -r requirements.txt
```

主要依赖：Python 3.10+、NumPy、Pandas、Matplotlib、scikit-learn、seaborn、Jupyter。

**运行方式**

- Notebook 中的数据集路径使用相对路径 `../data/xxx.csv`，因此请从 `notebooks/` 目录下打开并运行；
- 或使用 Jupyter 打开 `notebooks/` 目录后逐个执行。

---

## 学习路线建议

```
① 数据分析基础          →  第 1、2、3 章（Matplotlib / NumPy / Pandas）
        ↓
② 回归建模              →  第 4、5 章（线性回归、sklearn 流程、多项式特征）
        ↓
③ 模型复杂度与正则化    →  第 6、7 章（过拟合、Ridge / Lasso、指标评估）
        ↓
④ 分类算法              →  第 8、9、11 章（逻辑回归、朴素贝叶斯、KNN）
        ↓
⑤ 模型评估与调优        →  第 10、8 章（混淆矩阵、网格搜索）
        ↓
⑥ 综合实战              →  第 13 章（完整回归分析实验报告）
```
