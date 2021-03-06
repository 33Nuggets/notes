# Python Tricks

- numpy
- pandas

## pandas


### time series
#### groupby

##### goal: add new columns to dataframe that uses columns within each group

example: for each stock, add the price difference comparing to last datetime. 

Inspired from [here](https://cmdlinetips.com/2019/10/how-to-add-group-level-summary-statistic-as-a-new-column-in-pandas/)

Good example
```python
groupby = df.groupby(['stock'], sort=False)
df['price_difference'] = groupby['price'].diff()

# for more complicated functions
def complicated_fun(series):
  return series.diff()
df['price_difference'] = groupby['price'].transform(complicated_fun)

```

Bad example
```python
lst_df = []
for t, df_group in df.groupby(['stock'], sort=False):
    df_group['price_difference'] = groupby['price'].diff()
    lst_df.append(df_t)

df = pd.concat(lst_df)
```

##### goal: do multiple and/or complicated aggregations on groupby

Inspired from [here](https://stackoverflow.com/questions/14529838/apply-multiple-functions-to-multiple-groupby-columns)

```python
def f(x):
    d = {}
    d['a_sum'] = x['a'].sum()
    d['a_max'] = x['a'].max()
    d['b_mean'] = x['b'].mean()
    d['c_d_prodsum'] = (x['c'] * x['d']).sum()
    return pd.Series(d, index=['a_sum', 'a_max', 'b_mean', 'c_d_prodsum'])

df.groupby('group').apply(f)

```
