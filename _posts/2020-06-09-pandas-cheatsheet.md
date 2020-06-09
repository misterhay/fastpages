---
toc: true
layout: post
description: A cheatsheet for pandas
categories: [markdown]
title: Pandas Cheatsheet
---
# Pandas Cheatsheet

```python
import pandas as pd
```

## Creating a DataFrame

### from online data
```python
df = pd.read_csv(csv_url) # if necessary: skiprows=2  or  encoding='windows-1251'
df = pd.read_excel(excel_url, 'Sheet Name')
df = pd.read_json(json_url_or_string)
df = pd.read_html(page_url)
```

### from input or loop data
```python
df = pd.DataFrame(columns=['Column 1', 'Column 2'])
for thing in list_of_things:
    df = df.append({'Column 1':thing, 'Column 2':'other thing'}, ignore_index=True)
df['New Column'] = df['Column 1']*df['Column 2'] # create a new column from existing one(s)
```

## Displaying Data
```python
df.head(n) # n is number of rows, default is 5
df.tail(n)
df.shape
df.columns
```

## Sorting Data
```python
df.sort_values('Column 1', ascending=False) # default is ascending=True, axis=1 to sort columns
df.sort_index()
```

## Selecting Data
```python
df['Column 1']
df[1:]
df.loc[[0],['Column 1']]
df.iloc[[0, 7]] # select rows with given index values
df[df['Column 1']=='some value']
df[df['Column 1'].isin(['some value', 'other value'])]
df[df['Column 1'].between(5, 10)]
df[(df['Column 1']=='some value') & (df['Column 2']>3)] # use | for "or"
```

## Summarizing Data
```python
df.describe()
df.sum()
df['Column 1'].sum()
df.min()
df.max()
df.mean()
df.median()
df['Column 1'].unique()
df.count() # axis=1 for count by columns, numeric_only=True for just numbers
df.corr() # correlation coefficients
df.std() # standard deviation
df.groupby('Column 2')
df.agg(['min', 'max'])
```

## Cleaning Data
```python
df2 = df.dropna() # drop any "not available" data
df.dropna(axis=0, inplace=True) # drop rows, can also how='any' or how='all'
df2 = df.dropna(axis=1) # drop columns, create a new dataframe
df2 = df.fillna(value=0)
df['Column 1'].fillna(value=0, inplace=True)
df.drop(df.index[[2, 7]], inplace=True)
df.drop(columns=['Column 1', 'Column 3'], inplace=True)
```

## Transforming Data
```python
df.T # transpose rows and columns
df.set_index('Column 1')
df.rename(columns={'Column 1':'New Column Name'}, inplace=True)
df.columns = ['New Column Name', 'Other New Column Name']
df.replace('previous value','new value',regex=True,inplace=True)
```

## Merging Data
```python
df.append(df2) # add rows, make sure column names are the same
new_df = pd.merge(df, df2, on='Column 1') # default on=index
new_df = df.join(other_df)
new_df = pd.concat([df, df2], axis=1) # add columns, make sure row names or indexes are the same
```

# Graphing Data with Cufflinks
```python
import cufflinks as cf # joins pandas and Plotly, similar to pandas's .plot
cf.go_offline()
df.iplot(kind='bar', x='Column 1', y='Column 2')

df.iplot(y='Column 1', title='Graph', xTitle='index', yTitle='y values') # default x is index
# options for kind= are 'bar', 'scatter', 'box', 'histogram', 'spread', 'heatmap', 'bubble', 'pie'
# 3D kind= are 'scatter3d', 'bubble3d', 'surface'
```