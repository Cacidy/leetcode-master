# 🐼 pandas 快速复习手册（面试 & 项目实战向）

## 📚 目录

1. 🔧 DataFrame 创建与基础操作  
2. 📅 时间序列处理  
3. 🔄 缺失值与数据清洗  
4. 📊 分组与聚合（groupby）  
5. 🔗 多表合并（merge/join）  
6. 🌀 滚动窗口计算（rolling）  
7. 🧽 apply 与 lambda  
8. 📈 可视化连接

---

## 1. 🔧 DataFrame 创建与基础操作

```python
import pandas as pd

# 创建 DataFrame
data = {'date': ['2024-01-01', '2024-01-02'], 'value': [100, 200]}
df = pd.DataFrame(data)

# 查看结构
df.head()
df.info()
df.describe()

# 修改列
df['value'] = df['value'] * 1.1

# 条件筛选
df[df['value'] > 150]
```

## 2. 📅 时间序列处理

```python
df['date'] = pd.to_datetime(df['date'])  # 转换为时间格式
df.set_index('date', inplace=True)       # 设置为索引

df.resample('W').mean()                  # 重采样为周平均
df['diff'] = df['value'].diff()         # 日变化
```

常用频率代码：D = 日，W = 周，M = 月，H = 小时。（有时候报错改为小写）

## 3. 🔄 缺失值与数据清洗

```python
df.isnull().sum()                         # 查看缺失值
df.fillna(method='ffill', inplace=True)  # 前向填充
df.dropna(inplace=True)                  # 删除缺失值
df['value'] = df['value'].replace(-1, pd.NA)  # 替换异常值
```

## 4. 📊 分组与聚合（groupby）

```python
df = pd.DataFrame({
    'date': ['2024-01-01', '2024-01-01', '2024-01-02'],
    'address': ['A', 'B', 'A'],
    'value': [1.2, -0.5, 0.8]
})

# 多级分组
grouped = df.groupby(['date', 'address'])['value'].sum().reset_index()
```

可使用 `.sum(), .mean(), .count(), .agg(['mean', 'max'])`

## 5. 🔗 多表合并（merge）

```python
fee_df = pd.DataFrame({'date': ['2024-01-01'], 'fee': [10]})
price_df = pd.DataFrame({'date': ['2024-01-01'], 'price': [2000]})

merged_df = pd.merge(fee_df, price_df, on='date')
merged_df['fee_in_token'] = merged_df['fee'] / merged_df['price']
how='inner' | 'left' | 'right' | 'outer' 控制连接方式
```

## 6. 🌀 滚动窗口计算（rolling）

```python
df['rolling_mean'] = df['value'].rolling(window=3).mean()
df['rolling_std'] = df['value'].rolling(window=3).std()
可用于移动平均线、波动率估计、Z-score 异常检测等场景
```

## 7. 🧽 apply 与 lambda（函数式编程）

```python
df['score'] = df['value'].apply(lambda x: x**2 if x > 150 else x)

def compute_ratio(row):
    return row['fee'] / row['price']

df['fee_ratio'] = df.apply(compute_ratio, axis=1)
```

## 8. 📈 可视化连接

```python
import matplotlib.pyplot as plt

df['value'].plot(figsize=(10, 4), title="Value Over Time")
plt.grid(True)
plt.show()
```

## ✅ 综合项目套路演示（TVL + Fee + Price）

### 假设已读取三个 DataFrame：fee_df, tvl_df, price_df，均包含 'date' 和 'value'

```python
fee_df['date'] = pd.to_datetime(fee_df['date'])
tvl_df['date'] = pd.to_datetime(tvl_df['date'])
price_df['date'] = pd.to_datetime(price_df['date'])

merged = fee_df.merge(tvl_df, on='date').merge(price_df, on='date')
merged.columns = ['date', 'fee', 'tvl', 'price']

merged['fee_in_token'] = merged['fee'] / merged['price']
merged['tvl_log_return'] = np.log(merged['tvl']).diff()

merged['rolling_mean'] = merged['tvl_log_return'].rolling(60).mean()
merged['rolling_std'] = merged['tvl_log_return'].rolling(60).std()
merged['z_score'] = (merged['tvl_log_return'] - merged['rolling_mean']) / merged['rolling_std']
```
