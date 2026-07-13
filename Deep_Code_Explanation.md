# 📘 Deep Code Explanation Guide
## Credit Risk & Customer Financial Health Analytics
### ITS 2122 – Python for Data Science & AI | HDSE 73
**For a Total Beginner | Viva / Presentation Preparation**

---

This document breaks down **every single line of code** in the technical appendix notebook. It is designed for someone who has never programmed before but needs to defend this code in a presentation or viva.

---

## 🟢 CELL 01 — Import Libraries

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import requests
import warnings

warnings.filterwarnings('ignore')

sns.set_theme(style='whitegrid', palette='deep')
plt.rcParams['figure.dpi'] = 100
plt.rcParams['axes.titleweight'] = 'bold'

pd.set_option('display.max_columns', 50)
pd.set_option('display.width', 140)

print("Libraries imported successfully.")
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `import pandas as pd` | Bring in the pandas library and call it "pd" for short. | `import` loads external code. `pandas` is for data manipulation (like Excel). `as pd` is an alias so we don't have to type "pandas" every time. |
| 2 | `import numpy as np` | Bring in the numpy library, call it "np". | `numpy` does fast maths. Used under the hood by pandas. |
| 3 | `import matplotlib.pyplot as plt` | Bring in the main charting tool, call it "plt". | `matplotlib.pyplot` is the core plotting library in Python. |
| 4 | `import seaborn as sns` | Bring in the advanced charting tool, call it "sns". | `seaborn` builds on matplotlib to make charts look professional automatically. |
| 5 | `import requests` | Bring in the library that lets Python use the internet. | `requests` sends HTTP requests to fetch data from web APIs. |
| 6 | `import warnings` | Bring in the warnings manager. | Built-in Python module to handle warning messages. |
| 8 | `warnings.filterwarnings('ignore')` | Tell Python to hide annoying warning messages. | `.filterwarnings()` accepts a string. `'ignore'` means do not print warnings to the screen (keeps the notebook clean). |
| 10 | `sns.set_theme(style='whitegrid', palette='deep')` | Set the default look for all our charts. | `style='whitegrid'` gives charts a white background with light grey gridlines. `palette='deep'` uses a specific set of deep, professional colours. |
| 11 | `plt.rcParams['figure.dpi'] = 100` | Make the charts sharper. | `rcParams` is a dictionary of settings. `figure.dpi` controls Dots Per Inch (resolution). 100 makes it crisp on modern screens. |
| 12 | `plt.rcParams['axes.titleweight'] = 'bold'` | Make all chart titles bold. | `axes.titleweight` controls font weight. `'bold'` is a string value. |
| 14 | `pd.set_option('display.max_columns', 50)` | Let pandas show up to 50 columns without hiding them. | `set_option` configures pandas. Our dataset has 46 columns; default pandas hides columns if there are more than 20. |
| 15 | `pd.set_option('display.width', 140)` | Make the text output wider so tables don't wrap awkwardly. | `display.width` sets the max characters per line in the console. |
| 17 | `print("...")` | Print a success message to the screen. | `print()` outputs text. Strings are wrapped in quotes `" "`. |

### Key Python Concepts
- **Libraries/Modules (`import`)**: Python is like a bare toolbox. `import` goes to the store and brings back specific power tools (pandas for tables, seaborn for charts).

### 🎤 Viva Q&A
**Q: What is pandas and why did you import it as pd?**
**A:** Pandas is the standard Python library for working with tabular data (like spreadsheets). We import it `as pd` simply to save typing — it's the universal convention in data science, so `pd.read_csv` is understood by everyone to mean pandas.

---

## 🟢 CELL 04 — Load Data

```python
RAW_PATH = 'default_of_credit_card_clients.csv'
df_raw = pd.read_csv(RAW_PATH, header=1)
print(f"Rows loaded: {df_raw.shape[0]:,}")
print(f"Columns loaded: {df_raw.shape[1]}")
df_raw.head()
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `RAW_PATH = '...'` | Create a variable holding the name of our data file. | `RAW_PATH` is a string variable. Using ALL_CAPS indicates it's a constant (a value that won't change). |
| 2 | `df_raw = pd.read_csv(RAW_PATH, header=1)` | Tell pandas to read the CSV file, skipping the first row, and save it as `df_raw`. | `.read_csv()` reads a comma-separated file into a DataFrame. `header=1` tells it to use row index 1 (the second row) as the column names, because row index 0 in this specific UCI file contains useless codes like X1, X2. |
| 3 | `print(f"Rows loaded: {df_raw.shape[0]:,}")` | Print the number of rows, formatted with a comma (e.g. 30,000). | `f"..."` is an f-string, letting us put code inside `{}`. `df_raw.shape` is a tuple `(rows, columns)`. `[0]` gets the first item (rows). `:,` formats it with a thousands separator. |
| 4 | `print(f"Columns loaded: {df_raw.shape[1]}")` | Print the number of columns. | `[1]` gets the second item from the shape tuple (columns). |
| 5 | `df_raw.head()` | Show the first 5 rows of the table. | `.head()` returns the top 5 rows. By putting it at the end of the cell, Jupyter renders it as a pretty HTML table. |

### Key Python Concepts
- **DataFrame (`df`)**: A 2-dimensional table of data with rows and columns, exactly like an Excel sheet, but stored in computer memory.
- **Tuples & Indexing (`[0]`)**: `shape` returns a tuple like `(30000, 25)`. In Python, counting starts at 0. So index `[0]` is 30000, and index `[1]` is 25.

### 🎤 Viva Q&A
**Q: Why did you use `header=1` when loading the CSV?**
**A:** The raw dataset from the UCI repository has two header rows. Row 0 contains generic labels (X1, X2, Y), and Row 1 contains the actual descriptive variable names (LIMIT_BAL, SEX, etc.). In Python, index 1 means the second row. If we didn't specify this, pandas would use the generic X/Y codes as column names.

---

## 🟢 CELL 05 — Assert Shape

```python
assert df_raw.shape[0] == 30000, "Unexpected row count vs. UCI documentation"
assert df_raw.shape[1] == 25, "Unexpected column count vs. UCI documentation"
print("Row/column counts match the UCI documentation (30,000 rows x 25 columns).")
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `assert df_raw.shape[0] == 30000, "..."` | Make sure rows equal 30000. If not, crash and show this error message. | `assert` is a debugging keyword. `==` is the equality operator (checks if left equals right). The string after the comma is the error message thrown if the check fails. |
| 2 | `assert df_raw.shape[1] == 25, "..."` | Make sure columns equal 25. If not, crash. | Same as above, checking the column count (`shape[1]`). |
| 3 | `print("...")` | Print success message. | Only runs if both `assert` statements pass without crashing. |

### Key Python Concepts
- **`==` vs `=`**: Single `=` assigns a value (e.g. `x = 5`). Double `==` asks a question ("is x equal to 5?").
- **`assert`**: A defensive programming technique. It prevents the notebook from running on broken or corrupted data.

---

## 🟢 CELL 07, 08, 09 — Initial Assessment

```python
# CELL 07
df_raw.info()

# CELL 08
df_raw.describe(include='all').T

# CELL 09
null_counts = df_raw.isnull().sum()
print("Total missing values across the dataset:", null_counts.sum())
null_counts[null_counts > 0]
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| C07 | `df_raw.info()` | Print a technical summary of the DataFrame. | Shows column names, non-null counts, memory usage, and data types (e.g., `int64` = 64-bit integer). |
| C08 | `df_raw.describe(include='all').T` | Generate statistical summaries (mean, min, max) and transpose the result. | `.describe()` calculates stats. `include='all'` forces it to include categorical columns if any existed. `.T` transposes the result (swaps rows and columns) so it's easier to read vertically. |
| C09-1 | `null_counts = df_raw.isnull().sum()` | Find missing data: check every cell for blanks, then sum them up per column. | `.isnull()` returns a DataFrame of True/False (True if blank). `.sum()` adds them up (True = 1, False = 0), resulting in a Series showing the count of blanks per column. |
| C09-2 | `null_counts.sum()` | Add up all the column totals to get one grand total. | The second `.sum()` adds up the numbers in the Series. Returns 0 for this dataset. |
| C09-3 | `null_counts[null_counts > 0]` | Filter the list to only show columns that actually have missing data. | Boolean indexing: `null_counts > 0` creates a True/False mask. Applying it with `[]` filters the list. Since there are 0 missing values, this outputs an empty list. |

### 🎤 Viva Q&A
**Q: What is the difference between `.info()` and `.describe()`?**
**A:** `.info()` tells you the structure of the data — the data types (integers, strings), memory usage, and whether there are empty cells. `.describe()` gives you the mathematics of the data — the average, minimum, maximum, and percentiles for each column.

---

## 🟢 CELL 10 — Value Counts

```python
for col in ['SEX', 'EDUCATION', 'MARRIAGE']:
    print(f"--- {col} ---")
    print(df_raw[col].value_counts().sort_index())
    print()
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `for col in ['SEX', ...]:` | Loop through these three specific column names. | A `for` loop. The variable `col` will temporarily take the value of each string in the list `['SEX', 'EDUCATION', 'MARRIAGE']`, one by one. |
| 2 | `print(f"--- {col} ---")` | Print the name of the column as a header. | f-string formatting injects the variable `col`. |
| 3 | `print(df_raw[col].value_counts().sort_index())` | Count how many times each unique number appears in this column, sort the numbers in order, and print. | `df_raw[col]` extracts one column as a Series. `.value_counts()` counts the frequencies of each unique value. `.sort_index()` sorts the output by the index (the unique values themselves, e.g., 0, 1, 2, 3) rather than by the count. |
| 4 | `print()` | Print a blank line. | Just for visual spacing in the output. |

### Key Python Concepts
- **`for` loops**: Repeating an action for a list of items. It saves us from writing the same three lines of code three times.

---

## 🟢 CELL 13 — Rename Columns

```python
rename_map = {
    'LIMIT_BAL': 'limit_balance',
    'default payment next month': 'default_next_month',
    # ... (other columns omitted for brevity)
}
df = df_raw.rename(columns=rename_map).copy()
df.columns.tolist()
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1-4 | `rename_map = {'OLD': 'new', ...}` | Create a "dictionary" that maps old names to new names. | A Python dictionary (`dict`) uses `{key: value}` pairs. Keys are the current column names, values are the desired names. |
| 5 | `df = df_raw.rename(columns=rename_map).copy()` | Create a new DataFrame (`df`) by taking the raw one, renaming the columns using our dictionary, and making a hard copy. | `.rename(columns=...)` applies the map. `.copy()` ensures we create a completely detached new object in memory, leaving `df_raw` safely untouched. |
| 6 | `df.columns.tolist()` | Output the list of new column names to verify. | `.columns` gets the index of column names; `.tolist()` converts it to a standard Python list. |

### 🎤 Viva Q&A
**Q: Why use `.copy()` here?**
**A:** In pandas, operations sometimes return a "view" of the original data rather than a separate object. If you modify a view, you might accidentally modify the original data and get a `SettingWithCopyWarning`. Using `.copy()` guarantees `df` is a completely fresh, independent table in memory.

---

## 🟢 CELL 15 — Decode Categories

```python
sex_map = {1: 'Male', 2: 'Female'}
df['sex_label'] = df['sex'].map(sex_map)

education_map = {1: 'Graduate School', 2: 'University', 3: 'High School', 4: 'Others', 0: 'Unknown', 5: 'Unknown', 6: 'Unknown'}
df['education_label'] = df['education'].map(education_map)

# ... (marriage map similar)
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `sex_map = {1: 'Male', 2: 'Female'}` | Create a dictionary mapping the number 1 to 'Male' and 2 to 'Female'. | Keys are integers (the data type in the column), values are strings. |
| 2 | `df['sex_label'] = df['sex'].map(sex_map)` | Create a NEW column called `sex_label`. Fill it by taking the `sex` column and translating every number using our dictionary. | `df['new_col'] = ...` creates a column. `.map()` is a pandas method that looks up each value in the Series against the provided dictionary and replaces it. |

### 🎤 Viva Q&A
**Q: What is the `.map()` function doing?**
**A:** `.map()` looks at every single row in a specific column. If it sees a `1` in the `sex` column, it looks up `1` in the `sex_map` dictionary, finds the word `'Male'`, and puts `'Male'` into the new `sex_label` column for that row. It is essentially a VLOOKUP in Python.

---

## 🟢 CELL 18 — Duplicates

```python
full_row_duplicates = df.duplicated().sum()
id_duplicates = df['id'].duplicated().sum()
df = df.drop_duplicates(subset='id', keep='first').reset_index(drop=True)
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `full_row_duplicates = df.duplicated().sum()` | Count how many rows are 100% identical to another row. | `.duplicated()` returns True for rows that are exact copies of an earlier row. `.sum()` counts the Trues. |
| 2 | `id_duplicates = df['id'].duplicated().sum()` | Count how many times a customer ID is repeated. | `df['id']` isolates just the ID column. Two rows might have the same ID but different data — this catches that. |
| 3 | `df.drop_duplicates(subset='id', keep='first')` | Remove any rows where the ID is a duplicate, keeping only the first one it finds. | `subset='id'` tells it to look only at the ID column for duplication. `keep='first'` dictates which duplicate to retain. |
| 3 | `.reset_index(drop=True)` | Re-number the rows from 0 neatly. | When rows are dropped, their original row numbers (index) disappear. `.reset_index(drop=True)` throws away the old disjointed row numbers and generates fresh sequential ones (0, 1, 2...). |

---

## 🟢 CELL 20 — Validate PAY Columns

```python
pay_cols = [f'pay_status_{i}' for i in range(1, 7)]
pay_range_check = df[pay_cols].apply(lambda s: s.between(-2, 9)).all()
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `pay_cols = [f'pay_status_{i}' for i in range(1, 7)]` | Magically create a list of names: 'pay_status_1' through 'pay_status_6'. | **List Comprehension**: `[expression for item in iterable]`. `range(1, 7)` generates numbers 1,2,3,4,5,6 (stops *before* 7). The f-string inserts `i` into the string. |
| 2 | `df[pay_cols]` | Select only those 6 columns from the table. | Passing a list of column names to `df[]` returns a subset of the DataFrame. |
| 2 | `.apply(lambda s: s.between(-2, 9))` | For each of those columns, check if every number is between -2 and 9. | `.apply()` runs a function on each column. `lambda s:` is an anonymous (unnamed) mini-function where `s` is the column. `.between(-2, 9)` returns True/False for every cell. |
| 2 | `.all()` | Check if ALL the answers were True. | Evaluates if every boolean in the column is True. Returns a summary for each column. |

### Key Python Concepts
- **List Comprehension**: A fast, elegant way to generate lists in Python. Instead of writing `['pay_status_1', 'pay_status_2', ...]`, we use a loop inside square brackets.
- **Lambda Functions**: `lambda` is a way to create a tiny, one-line function on the fly without giving it a formal name using `def`.

---

## 🟢 CELL 24 — Feature Engineering (The most complex cell)

```python
# 1. Averages
df['avg_bill_amount'] = df[bill_cols].mean(axis=1)
df['avg_pay_amount'] = df[pay_amt_cols].mean(axis=1)

# 2. Payment-to-bill ratio
def safe_ratio(pay, bill):
    if bill > 0:
        return pay / bill
    elif pay > 0:
        return 1.0
    else:
        return 0.0

df['payment_to_bill_ratio'] = [safe_ratio(p, b) for p, b in zip(df['avg_pay_amount'], df['avg_bill_amount'])]
df['payment_to_bill_ratio'] = df['payment_to_bill_ratio'].clip(upper=5)

# 3. Delays
df['num_delayed_months'] = (df[pay_cols] > 0).sum(axis=1)
df['max_delay'] = df[pay_cols].clip(lower=0).max(axis=1)

# 4. Trend
df['balance_trend'] = df['bill_amount_1'] - df['bill_amount_6']

# 5. Utilisation
df['credit_utilisation'] = (df['avg_bill_amount'] / df['limit_balance']).clip(lower=0, upper=3)
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 2 | `df[bill_cols].mean(axis=1)` | Calculate the average across the 6 bill columns for each customer. | `axis=1` means "calculate horizontally across the columns for each row". `axis=0` would calculate the average down the column for all customers. |
| 6-12 | `def safe_ratio(pay, bill): ...` | Define a custom function to divide payment by bill, with safety checks to prevent crashing if bill is 0. | `def` creates a function. `if/elif/else` handles logic branches. If bill > 0, do normal math. If bill=0 but they paid something, return 1.0 (100% paid). Else return 0. |
| 14 | `zip(df['avg_pay_amount'], df['avg_bill_amount'])` | Pair up each customer's payment and bill like a zipper. | `zip()` takes two lists and pairs them: `(pay1, bill1), (pay2, bill2), ...` |
| 14 | `[safe_ratio(p, b) for p, b in zip(...)]` | Run our `safe_ratio` function on every paired payment (`p`) and bill (`b`). | Another list comprehension. It creates a completely new list of calculated ratios. |
| 15 | `.clip(upper=5)` | Cap the maximum ratio at 5.0. | `.clip()` enforces boundaries. If someone's ratio was 100.0 (because they paid off a tiny 1-dollar bill with 100 dollars), it gets forced down to 5.0 so it doesn't skew our statistics. |
| 18 | `(df[pay_cols] > 0).sum(axis=1)` | Count how many months the customer was late. | `df > 0` converts the table of numbers into True/False (True if late). `.sum(axis=1)` adds up the Trues horizontally for each customer. |
| 19 | `df[pay_cols].clip(lower=0).max(axis=1)` | Find the worst single month of delay. | `.clip(lower=0)` changes all negative numbers (paid on time) to 0. `.max(axis=1)` finds the highest number horizontally. |
| 24 | `df['avg_bill_amount'] / df['limit_balance']` | Divide average bill by credit limit to find utilisation. | Standard pandas vectorised division. It divides row 1 by row 1, row 2 by row 2, extremely fast. |

### 🎤 Viva Q&A
**Q: What does `axis=1` mean?**
**A:** In pandas, a DataFrame has two axes. `axis=0` refers to the rows (going downwards vertically). `axis=1` refers to the columns (going horizontally). When we say `mean(axis=1)`, we are telling pandas to calculate the average *across* the columns for a single customer, rather than the average of a column for all customers.

---

## 🟢 CELL 30 — Age Bands and Limit Tiers

```python
age_bins = [18, 25, 30, 35, 40, 45, 50, 60, 100]
age_labels = ['18-25', '26-30', '31-35', '36-40', '41-45', '46-50', '51-60', '60+']
df['age_band'] = pd.cut(df['age'], bins=age_bins, labels=age_labels, right=True)

df['limit_tier'] = pd.qcut(df['limit_balance'], q=4, labels=['Low', 'Medium', 'High', 'Very High'])
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 3 | `pd.cut(..., right=True)` | Sort ages into specific custom buckets. | `pd.cut()` takes continuous numbers and groups them into specific, predefined bins (ranges). `right=True` means a 25-year-old goes into the '18-25' bucket, not '26-30' (the interval includes the right boundary). |
| 5 | `pd.qcut(..., q=4)` | Sort credit limits into 4 groups of equal size (quartiles). | `pd.qcut()` (quantile cut) splits data so that an equal *number of customers* ends up in each bucket, regardless of the numerical width of the bucket. |

---

## 🟢 CELL 31 — Visualisation (matplotlib / pandas plotting)

```python
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

default_by_age.plot(kind='bar', ax=axes[0, 0], color='#4C72B0')
axes[0, 0].set_title('Default Rate by Age Band')
axes[0, 0].set_ylabel('Default Rate')
axes[0, 0].axhline(overall_default_rate, color='red', linestyle='--')
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `fig, axes = plt.subplots(2, 2, figsize=(14, 10))` | Create a blank canvas with a 2x2 grid for charts, 14 inches wide by 10 inches tall. | `subplots` returns `fig` (the whole window/image) and `axes` (a 2D numpy array holding the 4 individual chart areas). |
| 3 | `default_by_age.plot(kind='bar', ax=axes[0, 0], color='#4C72B0')` | Draw a bar chart of the age data inside the top-left box. | `ax=axes[0, 0]` targets row 0, column 0. `color` accepts a HEX code for a specific shade of blue. |
| 6 | `axes[0, 0].axhline(overall_default_rate, color='red', linestyle='--')` | Draw a horizontal dashed red line showing the average default rate. | `.axhline()` draws a horizontal line across the entire axis at the specified Y-value. `linestyle='--'` makes it dashed. |

---

## 🟢 CELL 45 — Correlation Heatmap (seaborn)

```python
corr_matrix = df[corr_cols].corr()

fig, ax = plt.subplots(figsize=(10, 8))
sns.heatmap(corr_matrix, annot=True, fmt='.2f', cmap='coolwarm', center=0,
            square=True, linewidths=0.5, ax=ax)
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 1 | `df[corr_cols].corr()` | Calculate the correlation between every pair of columns. | `.corr()` uses Pearson correlation by default, generating a matrix of values from -1.0 to 1.0. |
| 4 | `sns.heatmap(...)` | Draw a heatmap using the seaborn library. | Powerful seaborn function that turns a 2D matrix into coloured squares. |
| 4 | `annot=True` | Write the numbers inside the squares. | `annot` stands for annotation. |
| 4 | `fmt='.2f'` | Format the numbers to 2 decimal places. | String formatting code. `.2f` means "float with 2 digits after the decimal". |
| 4 | `cmap='coolwarm'` | Use a colour map that goes from blue (cool) to red (warm). | `cmap` defines the colour palette. `coolwarm` is ideal for correlations. |
| 4 | `center=0` | Make sure the colour perfectly in the middle (white) represents zero. | Anchors the colour map. Negative is blue, positive is red, 0 is neutral. |
| 5 | `square=True, linewidths=0.5` | Make the boxes perfectly square and draw a 0.5px line between them. | Aesthetic parameters to make the chart look professional. |

---

## 🟢 CELL 52 — Risk Scoring (Quantiles and Ranking)

```python
def score_dimension(series, ascending_risk=True, bins=5):
    ranks = series.rank(method='first')
    quantile_labels = range(1, bins + 1) if ascending_risk else range(bins, 0, -1)
    scored = pd.qcut(ranks, q=bins, labels=quantile_labels)
    return scored.astype(int)

df['delinquency_score'] = score_dimension(df['delinquency_intensity'], ascending_risk=True)
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 2 | `ranks = series.rank(method='first')` | Rank everyone from 1 to 30,000. If two people have the exact same score, rank the first one we see slightly higher. | `.rank()` assigns rankings. `method='first'` is critical here: it breaks ties. Without it, `qcut` crashes if there are too many identical zeroes in the data. |
| 3 | `range(1, bins + 1)` | Create labels [1, 2, 3, 4, 5]. | `range(start, stop)` goes up to, but does not include, the stop number. So `range(1, 6)` produces 1,2,3,4,5. |
| 3 | `range(bins, 0, -1)` | Create inverted labels [5, 4, 3, 2, 1]. | The third argument `-1` is the step size (counting backwards). |
| 4 | `pd.qcut(ranks, q=bins, labels=quantile_labels)` | Cut the ranked data into 5 equal buckets and apply the labels. | `q=5` means quintiles. ~6,000 customers will get score 1, ~6,000 will get score 2, etc. |

### 🎤 Viva Q&A
**Q: Why do you rank the data first before applying `pd.qcut`?**
**A:** Because many customers have perfectly clean repayment records (delinquency = 0). If over 20% of the dataset is identical (0), `pd.qcut` fails because it cannot find mathematical boundaries to split the data into 5 equal bins. By using `.rank(method='first')`, we artificially break the ties (0 becomes rank 1, rank 2, etc.), which allows `qcut` to slice the dataset cleanly.

---

## 🟢 CELL 58 — Segmentation and Aggregation

```python
def assign_segment(score):
    if score < 2.0: return 'Healthy'
    elif score < 3.0: return 'Watchlist'
    elif score < 4.0: return 'At-Risk'
    else: return 'Critical / Intervention'

segment_summary = df.groupby('risk_segment').agg(
    customers=('id', 'count'),
    pct_of_base=('id', lambda x: len(x) / len(df)),
    default_rate=('default_next_month', 'mean'),
)
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 7 | `df.groupby('risk_segment')` | Group the DataFrame into 4 mini-DataFrames, one for each segment. | `.groupby()` is the pandas equivalent of an Excel Pivot Table. |
| 7 | `.agg(...)` | Calculate multiple summary statistics at once for each group. | `.agg()` (aggregate) lets us specify exactly what math to do on which columns. |
| 8 | `customers=('id', 'count')` | Create a column called `customers` by counting the `id` column. | This is "Named Aggregation" syntax: `New_Column_Name=('Target_Column', 'Math_Function')`. |
| 9 | `pct_of_base=('id', lambda x: len(x) / len(df))` | Create a column calculating what percentage of the total dataset this group represents. | Uses a lambda function. `x` is the specific group (e.g. all Healthy IDs). `len(x)` is the count of Healthy. `len(df)` is the total 30,000. |
| 10 | `default_rate=('default_next_month', 'mean')` | Calculate the default rate by finding the average of the 1s and 0s. | Mean of a boolean/binary column mathematically equals the percentage of 1s. |

---

## 🟢 CELL 73 — API Integration (Advanced Networking)

```python
import json as _json_lib
API_URL = "https://open.er-api.com/v6/latest/USD"
FALLBACK_USD_TO_TWD = 32.50

def fetch_usd_to_twd_rate():
    try:
        response = requests.get(API_URL, timeout=8)
        response.raise_for_status()
        payload = response.json()
        rate = payload["rates"]["TWD"]
        return rate, payload
    except requests.exceptions.RequestException as exc:
        return FALLBACK_USD_TO_TWD, None
    except (KeyError, ValueError) as exc:
        return FALLBACK_USD_TO_TWD, None
```

### Line-by-Line Breakdown
| Line | Code | Plain English | Technical Detail |
|---|---|---|---|
| 6 | `try:` | "Try to run the following block of code. If anything fails, don't crash — jump to 'except'." | `try/except` is Python's Error Handling architecture. |
| 7 | `requests.get(API_URL, timeout=8)` | Reach out to the website URL and download the data. Give up if it takes more than 8 seconds. | `.get()` makes an HTTP GET request. `timeout=8` prevents the notebook from freezing forever if the server is unresponsive. |
| 8 | `response.raise_for_status()` | Check if the server gave us an error code (like 404 Not Found). If yes, trigger an error immediately. | Ensures we don't try to process an error page as if it were data. |
| 9 | `response.json()` | Convert the text we downloaded into a Python dictionary. | APIs return data in JSON (JavaScript Object Notation) format. This decodes it. |
| 10 | `payload["rates"]["TWD"]` | Look inside the dictionary to find the "rates" section, then find the "TWD" value. | Dictionary chaining. `payload` has a key `"rates"`, whose value is another dictionary containing `"TWD"`. |
| 12 | `except requests.exceptions.RequestException:` | "If the internet is down, or the site timed out, run this code instead." | Catches all network-related errors. |
| 14 | `except (KeyError, ValueError):` | "If we connected fine, but the data wasn't JSON, or didn't contain the word 'TWD', run this code." | Catches data-parsing and format errors. |
| 15 | `return FALLBACK_USD_TO_TWD, None` | Give back the hardcoded emergency exchange rate. | Ensures the notebook can still finish running even without internet access. Returns a tuple of two values. |

### 🎤 Viva Q&A
**Q: Why do you need `try/except` around the API call?**
**A:** Because networks are unreliable. If we just wrote `requests.get()` and the internet was disconnected (e.g. on a grading computer), Python would crash with a `ConnectionError` and stop running the rest of the notebook. The `try/except` block catches the error gracefully, uses a hardcoded fallback rate of 32.50, and allows the code to finish completely.

---
*Created for ITS 2122 Group Project Viva Preparation.*
