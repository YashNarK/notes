# Data Engineering

> 🗺️ **Main entry point** for all data engineering notes — covers Python for data, pandas, data manipulation, and analysis workflows.

## Table of contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Python for Data Engineering](#python-for-data-engineering)
- [Pandas — Data Manipulation & Analysis](#pandas--data-manipulation--analysis)
  - [Core Data Structures](#core-data-structures)
  - [Reading & Writing Data](#reading--writing-data)
  - [Exploring a DataFrame](#exploring-a-dataframe)
  - [Selecting Data](#selecting-data)
  - [Filtering Data](#filtering-data)
  - [Modifying Data](#modifying-data)
  - [Handling Missing Values](#handling-missing-values)
  - [Aggregation & GroupBy](#aggregation--groupby)
  - [Merging, Joining & Concatenating](#merging-joining--concatenating)
  - [String Operations](#string-operations)
  - [Apply, Map & Lambda](#apply-map--lambda)
  - [Reshaping Data](#reshaping-data)
  - [Date & Time](#date--time)
  - [MultiIndex](#multiindex)
  - [Categorical Data & Memory Optimization](#categorical-data--memory-optimization)
  - [Visualization with Pandas](#visualization-with-pandas)
- [Resources](#resources)

---

## Overview

Data engineering involves building systems to collect, store, transform, and serve data. This section focuses on the **Python-first data engineering stack** — using Python and its ecosystem libraries to manipulate, analyse, and pipeline data.

**Scope of these notes:**

| Layer | Tool / Concept | Notes |
|---|---|---|
| Language | Python | [`language/Python.md`](../language/Python.md) |
| Data manipulation | Pandas | [`pandas_learn.ipynb`](pandas_learn.ipynb) |
| Data source | CSV / Excel / SQL | Covered in pandas notebook |
| Visualisation | Pandas `.plot()`, Matplotlib | Covered in pandas notebook |

---

## Prerequisites

Before working through data engineering topics, you should be comfortable with core Python. Key areas:

- Data types, comprehensions, and built-ins (`map`, `filter`, `zip`)
- Functions, `*args`/`**kwargs`, and lambda expressions
- Classes and OOP basics
- File I/O and the standard library
- `pip` and virtual environments

📖 **See:** [Python Language Notes](../language/Python.md)

---

## Python for Data Engineering

Python is the dominant language for data engineering. Key reasons:

- **Rich ecosystem** — pandas, NumPy, SQLAlchemy, PySpark, Airflow, dbt
- **Readable syntax** — concise data transformation code
- **REPL / Jupyter** — interactive exploration before scripting pipelines

### Essential Python patterns for data work

```python
# List comprehensions — transform collections concisely
squares = [x ** 2 for x in range(10)]

# Dict comprehensions — reshape mappings
name_to_salary = {row['name']: row['salary'] for row in records}

# Generator expressions — memory-efficient iteration over large data
total = sum(row['salary'] for row in records)

# zip — pair up columns
paired = list(zip(names, salaries))

# enumerate — index + value together
for i, name in enumerate(names, start=1):
    print(f"{i}. {name}")
```

### Working with files

```python
# Read CSV manually (prefer pandas for real work)
import csv

with open('data.csv', newline='', encoding='utf-8') as f:
    reader = csv.DictReader(f)
    rows = list(reader)

# Write CSV
with open('out.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.DictWriter(f, fieldnames=['name', 'salary'])
    writer.writeheader()
    writer.writerows(rows)
```

### Type hints in data pipelines

```python
from typing import TypedDict

class Employee(TypedDict):
    id: int
    name: str
    salary: float
    department: str

def avg_salary(employees: list[Employee]) -> float:
    return sum(e['salary'] for e in employees) / len(employees)
```

📖 **Full Python reference:** [Python Language Notes](../language/Python.md)

---

## Pandas — Data Manipulation & Analysis

Pandas is the primary Python library for tabular data. Think of it as a programmable spreadsheet.

```python
import pandas as pd

df = pd.read_csv('sample.csv')
```

> 📓 **Full interactive notes with runnable code:** [`pandas_learn.ipynb`](pandas_learn.ipynb)
>
> The notebook uses `sample.csv` (10 employee records) and walks through every concept below with working examples.

---

### Core Data Structures

| Structure | Dimensions | Analogy |
|---|---|---|
| `Series` | 1-D (single column) | A single Excel column |
| `DataFrame` | 2-D (rows × columns) | A full Excel sheet |

```python
import pandas as pd

# Series
s = pd.Series([75000, 50000, 82000], index=['Alice', 'Bob', 'Charlie'])

# DataFrame — every column is a Series
df = pd.DataFrame({
    'name': ['Alice', 'Bob', 'Charlie'],
    'salary': [75000, 50000, 82000],
    'department': ['Engineering', 'HR', 'Engineering']
})
```

---

### Reading & Writing Data

```python
# Read
df = pd.read_csv('data.csv')
df = pd.read_csv('data.csv', sep=';', index_col='id', parse_dates=['joined'])
df = pd.read_excel('data.xlsx', sheet_name='Sheet1')

# Write
df.to_csv('out.csv', index=False)
df.to_excel('out.xlsx', index=False)
```

---

### Exploring a DataFrame

```python
df.shape          # (rows, cols)
df.dtypes         # column data types
df.info()         # non-null counts + dtypes
df.describe()     # statistical summary (numeric columns)
df.head(5)        # first 5 rows
df.tail(5)        # last 5 rows
df.columns        # column names
df.index          # index labels
df.value_counts() # frequency count (on a Series)
```

---

### Selecting Data

| Method | Selects by | Example |
|---|---|---|
| `df['col']` | Column name | `df['name']` |
| `df[['a','b']]` | Multiple columns | `df[['name','salary']]` |
| `.loc[row, col]` | **Label** | `df.loc[0, 'name']` |
| `.iloc[row, col]` | **Integer position** | `df.iloc[0, 1]` |

```python
df['salary']                    # Series
df[['name', 'salary']]          # DataFrame (subset of columns)
df.loc[0]                       # row by label
df.loc[0:2, 'name':'salary']    # slice by label (inclusive)
df.iloc[0:3, 1:3]               # slice by position (exclusive end)
```

---

### Filtering Data

```python
# Boolean mask
mask = df['salary'] > 60000
df[mask]                              # rows where mask is True

# Inline
df[df['department'] == 'Engineering']

# Multiple conditions
df[(df['salary'] > 60000) & (df['department'] == 'Engineering')]
df[(df['department'] == 'HR') | (df['department'] == 'Finance')]

# .isin() — match against a list
df[df['department'].isin(['HR', 'Engineering'])]

# .query() — SQL-like syntax
df.query("salary > 60000 and department == 'Engineering'")
```

---

### Modifying Data

```python
# Add a column
df['bonus'] = df['salary'] * 0.1

# Update a column
df['salary'] = df['salary'] * 1.05   # 5% raise

# Rename columns
df.rename(columns={'name': 'full_name', 'salary': 'base_salary'}, inplace=True)

# Drop columns
df.drop(columns=['bonus'], inplace=True)

# Drop rows
df.drop(index=[0, 2], inplace=True)

# Sort
df.sort_values('salary', ascending=False, inplace=True)

# Reset index after filtering/sorting
df.reset_index(drop=True, inplace=True)
```

> ⚠️ **Copy vs. View gotcha:** Use `.copy()` when slicing to avoid `SettingWithCopyWarning`.
> ```python
> subset = df[df['salary'] > 60000].copy()
> subset['bonus'] = subset['salary'] * 0.1   # safe — operates on a copy
> ```

---

### Handling Missing Values

```python
df.isnull().sum()              # count NaN per column
df.dropna()                    # drop rows with any NaN
df.dropna(subset=['salary'])   # drop only if salary is NaN
df.fillna(0)                   # replace NaN with 0
df['salary'].fillna(df['salary'].mean(), inplace=True)  # fill with mean
```

---

### Aggregation & GroupBy

GroupBy follows **Split → Apply → Combine**:

```python
# Single aggregation
df.groupby('department')['salary'].mean()

# Multiple aggregations
df.groupby('department')['salary'].agg(['mean', 'min', 'max', 'count'])

# Named aggregations (pandas ≥ 0.25)
df.groupby('department').agg(
    avg_salary=('salary', 'mean'),
    headcount=('name', 'count')
)

# Pivot table
df.pivot_table(values='salary', index='department', aggfunc='mean')
```

---

### Merging, Joining & Concatenating

| Operation | SQL equivalent | Use when |
|---|---|---|
| `pd.concat()` | `UNION ALL` | Stacking rows or columns |
| `pd.merge()` | `JOIN` | Combining on a shared key column |
| `df.join()` | `JOIN on index` | Joining on the index |

```python
# Stack DataFrames vertically
combined = pd.concat([df1, df2], ignore_index=True)

# Merge on a shared key (like SQL JOIN)
merged = pd.merge(employees, departments, on='dept_id', how='left')
# how: 'inner' | 'left' | 'right' | 'outer'
```

---

### String Operations

Use the `.str` accessor to apply string methods element-wise (no loops needed):

```python
df['name'].str.upper()
df['name'].str.lower()
df['name'].str.strip()
df['name'].str.len()
df['name'].str.contains('Ali', case=False)
df['name'].str.replace('Alice', 'Alicia', regex=False)
df['name'].str.split(' ', expand=True)  # splits into separate columns
```

---

### Apply, Map & Lambda

| Method | Applied to | Use when |
|---|---|---|
| `Series.map()` | Each element of a Series | Simple value mapping or substitution |
| `Series.apply()` | Each element of a Series | Custom function on a Series |
| `DataFrame.apply()` | Each column or each row | Custom function across an axis |
| `DataFrame.map()` | Every individual cell | Element-wise transformation on whole DF |

```python
# map — replace values using a dict or function
df['department'].map({'HR': 'Human Resources', 'Engineering': 'Eng'})

# apply on a Series
df['salary'].apply(lambda x: 'high' if x > 70000 else 'low')

# apply on a DataFrame (row-wise)
df.apply(lambda row: row['salary'] * 1.1 if row['department'] == 'Engineering' else row['salary'],
         axis=1)
```

---

### Reshaping Data

```python
# Wide → Long (unpivot)
melted = df.melt(id_vars=['name'], value_vars=['salary', 'bonus'],
                 var_name='metric', value_name='amount')

# Long → Wide (pivot)
pivoted = df.pivot(index='name', columns='department', values='salary')

# Stack / Unstack (MultiIndex)
stacked = df.set_index(['department', 'name'])['salary'].unstack()
```

---

### Date & Time

```python
# Parse strings to datetime
df['joined'] = pd.to_datetime(df['joined'])

# Extract components via .dt accessor
df['year'] = df['joined'].dt.year
df['month'] = df['joined'].dt.month
df['weekday'] = df['joined'].dt.day_name()

# Date arithmetic
df['tenure_days'] = (pd.Timestamp.today() - df['joined']).dt.days

# Generate a date range
dates = pd.date_range(start='2024-01-01', periods=12, freq='MS')  # monthly start
```

---

### MultiIndex

A MultiIndex allows multiple levels of indexing (common after multi-key `groupby`):

```python
# Create
multi = df.set_index(['department', 'name'])

# Select
multi.loc['Engineering']                  # all Engineering rows
multi.loc[('Engineering', 'Alice')]       # specific employee

# Reset back to flat
multi.reset_index(inplace=True)
```

---

### Categorical Data & Memory Optimization

When a string column has low cardinality (few unique values repeated many times), converting to `category` saves memory and speeds up groupby/sort:

```python
# Check memory before
df.memory_usage(deep=True)

# Convert to category
df['department'] = df['department'].astype('category')

# Check memory after (typically 80–90% reduction for low-cardinality columns)
df.memory_usage(deep=True)
```

---

### Visualization with Pandas

Pandas wraps Matplotlib for quick exploratory plots:

```python
import matplotlib.pyplot as plt

df['salary'].plot(kind='hist', bins=10, title='Salary Distribution')
df.groupby('department')['salary'].mean().plot(kind='bar', title='Avg Salary by Department')
df.plot(kind='scatter', x='age', y='salary', title='Age vs Salary')

plt.tight_layout()
plt.show()
```

For production charts use `matplotlib` or `seaborn` directly.

---

## Resources

- 📓 **[Pandas Interactive Notebook](pandas_learn.ipynb)** — runnable examples for every section above
- 📖 **[Python Language Notes](../language/Python.md)** — prerequisite Python reference
- 🌐 [Pandas Official Docs](https://pandas.pydata.org/docs/)
- 🌐 [Pandas Cheat Sheet (PyDataScience)](https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf)
