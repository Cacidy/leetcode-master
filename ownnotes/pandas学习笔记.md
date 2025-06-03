# 🐼 pandas 学习笔记

## 1.pandas简介

**数据结构**
**Pandas** 的主要数据结构是 Series （一维数据）与 DataFrame（二维数据）。

## 2.Pandas 数据结构 - Series

*一维数组：*Series 中的每个元素都有一个对应的索引值。
*索引：* 每个数据元素都可以通过标签（索引）来访问，默认情况下索引是从 0 开始的整数，但你也可以自定义索引。
*数据类型：* Series 可以容纳不同数据类型的元素，包括整数、浮点数、字符串、Python 对象等。
*大小不变性：*Series 的大小在创建后是不变的，但可以通过某些操作（如 append 或 delete）来改变。
*操作：*Series 支持各种操作，如数学运算、统计分析、字符串处理等。
*缺失数据：*Series 可以包含缺失数据，Pandas 使用NaN（Not a Number）来表示缺失或无值。
*自动对齐：*当对多个 Series 进行运算时，Pandas 会自动根据索引对齐数据，这使得数据处理更加高效。

**Series** 是一种类似于一维数组的对象，它由一组数据（各种 Numpy 数据类型）以及一组与之相关的数据标签（即索引）组成。
DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）。DataFrame 既有行索引也有列索引，它可以被看做由 Series 组成的字典（共同用一个索引）。

**与list,np.array区别**：`Series`支持向量化运算；有索引，并且支持自定义；支持缺失值，比如`np.nan`。`np.array` 是一个 纯粹的数值数组容器，高性能、适合数值计算；`Series`底层是封装的`np.array` ，`np.array`底层是c语言实现。`Series = ndarray + index（标签）+ 丰富方法`

**创建 Series**
可以使用 `pd.Series()` 构造函数创建一个 Series 对象，传递一个数据数组（可以是列表、`NumPy` 数组等）和一个可选的索引数组。
`pandas.Series(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)`

*参数说明：*
data：`Series` 的数据部分，可以是列表、数组、字典、标量值等。如果不提供此参数，则创建一个空的 `Series`。
index：`Series` 的索引部分，用于对数据进行标记。可以是列表、数组、索引对象等。如果不提供此参数，则创建一个默认的整数索引。
dtype：指定 `Series` 的数据类型。可以是 `NumPy` 的数据类型，例如 `np.int64`、`np.float64` 等。如果不提供此参数，则根据数据自动推断数据类型。
name：`Series` 的名称，用于标识 `Series` 对象。如果提供了此参数，则创建的 `Series` 对象将具有指定的名称。
copy：是否复制数据。默认为 `False`，表示不复制数据。如果设置为 `True`，则复制输入的数据。
fastpath：是否启用快速路径。默认为 `False`。启用快速路径可能会在某些情况下提高性能。

[具体函数](https://www.runoob.com/pandas/pandas-series.html)

```python
# 使用 map 函数将每个元素加倍
s_doubled = s.map(lambda x: x * 2)
print("元素加倍后：", s_doubled)
# 计算累计和
cumsum_s = s.cumsum()
print("累计求和：", cumsum_s)

# 查找缺失值（这里没有缺失值，所以返回的全是 False）
print("缺失值判断：", s.isnull())

# 排序
sorted_s = s.sort_values()
print("排序后的 Series：", sorted_s)

# 元素加倍后： a     2
# b     4
# c     6
# d     8
# e    10
# f    12
# dtype: int64
# 累计求和： a     1
# b     3
# c     6
# d    10
# e    15
# f    21
# dtype: int64
# 缺失值判断： a    False
# b    False
# c    False
# d    False
# e    False
# f    False
# dtype: bool
# 排序后的 Series： a    1
# b    2
# c    3
# d    4
# e    5
# f    6
# dtype: int64
```

## 3.Pandas 数据结构 - DataFrame

二维的表格或数据库中的数据表。DataFrame 是一个表格型的数据结构，它含有一组有序的列，每列可以是不同的值类型（数值、字符串、布尔型值）。DataFrame 既有行索引也有列索引，它可以被看做由 Series 组成的字典（共同用一个索引）。

**DataFrame 构造方法如下：**
`pandas.DataFrame(data=None, index=None, columns=None, dtype=None, copy=False)`

*参数说明：*
data：`DataFrame`的数据部分，可以是字典、二维数组、`Series`、`DataFrame` 或其他可转换为 `DataFrame` 的对象。如果不提供此参数，则创建一个空的 `DataFrame`。
index：`DataFrame` 的行索引，用于标识每行数据。可以是列表、数组、索引对象等。如果不提供此参数，则创建一个默认的整数索引。
columns：`DataFrame` 的列索引，用于标识每列数据。可以是列表、数组、索引对象等。如果不提供此参数，则创建一个默认的整数索引。
dtype：指定 `DataFrame` 的数据类型。可以是 `NumPy` 的数据类型，例如 `np.int64`、`np.float64` 等。如果不提供此参数，则根据数据自动推断数据类型。
copy：是否复制数据。默认为 `False`，表示不复制数据。如果设置为 `True`，则复制输入的数据。
Pandas `DataFrame` 是一个二维的数组结构，类似二维数组。

实例 - 使用列表创建

```python
import pandas as pd

data = [['Google', 10], ['Runoob', 12], ['Wiki', 13]]

# 创建DataFrame
df = pd.DataFrame(data, columns=['Site', 'Age'])

# 使用astype方法设置每列的数据类型
df['Site'] = df['Site'].astype(str)
df['Age'] = df['Age'].astype(float)

print(df)
```

[具体函数](https://www.runoob.com/pandas/pandas-dataframe.html)

**访问列**：使用列名作为属性或通过 `.loc[]`、`.iloc[]` 访问，也可以使用标签或位置索引。
**访问行**：使用行的标签和 `.loc[]` 访问。

```python
import pandas as pd

df = pd.DataFrame({
    'A': [10, 20, 30],
    'B': [40, 50, 60]
}, index=['x', 'y', 'z'])

print(df)

df.loc['x']             # 取行标签为 'x' 的整行
df.loc['y', 'A']        # 取第 y 行、第 A 列的值，结果是 20
df.loc['x':'y']         # 注意是**闭区间**，包括 'y'

# 多行多列
df.loc[['x', 'z'], ['B']]  # 取 x 和 z 行，B 列

# ================================================================

df.iloc[0]              # 取第 0 行（相当于 'x'）
df.iloc[1, 0]           # 取第 1 行、第 0 列（20）
df.iloc[0:2]            # 注意是**左闭右开**，不含第 2 行（只取 x 和 y）

# 多行多列
df.iloc[[0, 2], [1]]    # 取第 0 和 2 行，第 1 列（也就是 B 列）

```

**添加新行**：使用 loc或 concat 方法。

```python
# 使用 loc 为特定索引添加新行
df.loc[3] = [13, 14, 15, 16]

# 使用concat添加新行
new_row = pd.DataFrame([[4, 7]], columns=['A', 'B'])  # 创建一个只包含新行的DataFrame
df = pd.concat([df, new_row], ignore_index=True)  # 将新行添加到原始DataFrame

```

**删除 DataFrame 元素**:

```python
df_dropped = df.drop('Column1', axis=1)
df_dropped = df.drop(0)  # 删除索引为 0 的行
```

## 3.Pandas CSV 文件
