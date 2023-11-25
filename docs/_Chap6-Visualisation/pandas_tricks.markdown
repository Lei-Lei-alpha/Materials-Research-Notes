---
title: Pandas tricks
author: Lei Lei
category: notes
layout: post
---

## Dataframes
### Merge dataframes

```python
#on: the key (common column names) of the two dataframes;
#how: inner (keep common values only), outer: keep all values
df = pd.merge(df1, df2, how="inner",on="wavelength")
```

### Concatenate

```python
pandas.concat(objs, axis=0, join='outer', join_axes=None, ignore_index=False, keys=None, levels=None, names=None, verify_integrity=False, sort=None, copy=True)
```
### Add new row to dataframe

Just assign row to a particular index, using `loc`:

```python
df.loc[-1] = [2, 3, 4]  # adding a row
df.index = df.index + 1  # shifting index
df = df.sort_index()  # sorting by index
```

```python
**#add row to end of DataFrame
df.loc[len(df.index)] = [value1, value2, value3, ...]**
```

### Add a column to pandas dataframe

> You can use the assign() function to add a new column to the end of a pandas DataFrame: df = df.assig......

* * *

You can use the **assign()** function to add a new column to the end of a pandas DataFrame:

```python
df = df.assign(col_name=[value1, value2, value3, ...])
```

And you can use the **insert()** function to add a new column to a specific location in a pandas DataFrame:

```python
df.insert(position, 'col_name', [value1, value2, value3, ...])
```

The following examples show how to use this syntax in practice with the following pandas DataFrame:

```python
import pandas as pd

#create DataFrame
df = pd.DataFrame({'points': [25, 12, 15, 14, 19, 23],
                   'assists': [5, 7, 7, 9, 12, 9],
                   'rebounds': [11, 8, 10, 6, 6, 5]})

#view DataFrame
df

points    assists    rebounds
0    25    5    11
1    12    7    8
2    15    7    10
3    14    9    6
4    19    12    6
5    23    9    5
```

#### **Example 1: Add New Column to End of DataFrame**

The following code shows how to add a new column to the end of the DataFrame:

```python
#add 'steals' column to end of DataFrame
df = df.assign(steals=[2, 2, 4, 7, 4, 1])

#view DataFrame
df

points    assists    rebounds steals
0    25    5    11     2
1    12    7    8     2
2    15    7    10     4
3    14    9    6     7
4    19    12    6     4
5    23    9    5     1
```

#### **Example 2: Add Multiple Columns to End of DataFrame**

The following code shows how to add multiple new columns to the end of the DataFrame:

```python
#add 'steals' and 'blocks' columns to end of DataFrame
df = df.assign(steals=[2, 2, 4, 7, 4, 1],
               blocks=[0, 1, 1, 3, 2, 5])

#view DataFrame
df

points    assists    rebounds steals    blocks
0    25    5    11     2    0
1    12    7    8     2    1
2    15    7    10     4    1
3    14    9    6     7    3
4    19    12    6     4    2
5    23    9    5     1    5
```

#### **Example 3: Add New Column Using Existing Column**

The following code shows how to add a new column to the end of the DataFrame, based on the values in an existing column:

```python
#add 'half_pts' to end of DataFrame
df = df.assign(half_pts=lambda x: x.points / 2)

#view DataFrame
df

        points    assists    rebounds half_pts
0    25    5    11     12.5
1    12    7    8     6.0
2    15    7    10     7.5
3    14    9    6     7.0
4    19    12    6     9.5
5    23    9    5     11.5
```

#### **Example 4: Add New Column in Specific Location of DataFrame**

The following code shows how to add a new column by inserting it into a specific location in the DataFrame:

```python
#add 'steals' to column index position 2 in DataFrame
df.insert(2, 'steals', [2, 2, 4, 7, 4, 1])

#view DataFrame
df

points    assists    steals    rebounds
0    25    5    2    11
1    12    7    2    8
2    15    7    4    10
3    14    9    7    6
4    19    12    4    6
5    23    9    1    5
```

## Data

### Replace values

~~~python
# Replace a Single Value
df['Age'] = df['Age'].replace(23, 99)

# Replace Multiple Values
df['Age'] = df['Age'].replace([23, 45], [99, 999])

# Also works in the Entire DataFrame
df = df.replace(23, 99)
df = df.replace([23, 45], [99, 999])

# Replace Multiple Values with a Single Value
df['Age'] = df['Age'].replace([23, 45, 35], 99)

# Using a Dictionary (Dict is passed into to_replace=)
df['Age'] = df['Age'].replace({23:99, 45:999})

# Using a Dictionary for Column Replacements (key:value = column:value)
df = df.replace({'Name': 'Jane', 'Age': 45}, 99)
~~~

## Index, slicing, filtering & sorting

### Reset index

[pandas.DataFrame.reset_index — pandas 1.2.4 documentation (pydata.org)](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.reset_index.html)

```python
df.reset_index(drop=True)
```

### Set first column as index

```python
df = pd.read_excel(file, header=0, index_col=0)
```

### Select specified columns:

```python
df.filter(["species","bill_length_mm"])
```

### Filtering a column by multiple conditions:

```python
dataFrame.loc[(dataFrame['Salary']>=100000) & (dataFrame['Age']< 40) & (dataFrame['JOB'].str.startswith('D')),
             ['Name','JOB']]
```

```python
# Select the data by condition: 449.75<data['e loss']<470.25
data[(449.75<data['e loss']) & (data['e loss']<470.25)]
# Select the data by condition: data['e loss'] <  449.75or data['e loss'] > 470.25
data[(449.75<data['e loss']) | (data['e loss']<470.25)]
```

### Slicing

```python
#select the values after row 209
df=df1.iloc[209:]
#select
df=df1.iloc[156:190]
```

### Sorting

```python
df_sort = df.sort_values(by = 'column_1')
df_sort = df.sort_values(by = ['column_1', 'column_2'])
```

## File I/O
### Read txt

```python
import pandas as pd
data = pd.read_csv('output_list.txt', header = None)
```

### Skip rows

```python3
pandas.read_csv(filepath_or_buffer, skiprows=N, ....)
```

If the unwanted rows are begin with \#, than these rows can be ignored by:

```python3
df=pd.read_csv("1-Signal-43135.msa", header=None, sep = ',', comment='#', delimiter=',',dtype=float)
```

### Specify column names

Use `names` to specify the column names:

```python3
df = pd.read_csv(path, header=None, sep = ',', comment='#',
                 delimiter=',', names=('e loss', 'intensity'),
                 dtype=float)
```

### Write data to excel file

Create, write to and save a workbook:

```python3
df1 = pd.DataFrame([['a', 'b'], ['c', 'd']],
                   index=['row 1', 'row 2'],
                   columns=['col 1', 'col 2'])
df1.to_excel("output.xlsx")
```

To specify the sheet name:

```python
df1.to_excel("output.xlsx",
             sheet_name='Sheet_name_1')
```

If you wish to write to more than one sheet in the workbook, it is necessary to specify an ExcelWriter object:

```python
df2 = df1.copy()
with pd.ExcelWriter('output.xlsx') as writer:  
    df1.to_excel(writer, sheet_name='Sheet_name_1')
    df2.to_excel(writer, sheet_name='Sheet_name_2')
```

ExcelWriter can also be used to append to an existing Excel file:

```python
with pd.ExcelWriter('output.xlsx', engine="openpyxl",
                    mode='a') as writer:  
    df.to_excel(writer, sheet_name='Sheet_name_3')
```

To set the library that is used to write the Excel file, you can pass the engine keyword (the default engine is automatically chosen depending on the file extension):

```python
df1.to_excel('output1.xlsx', engine='xlsxwriter')
```
