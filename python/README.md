# Python Tricks

- numpy
- pandas

## pandas


### time series
#### groupby




goal: add new columns to dataframe that uses columns within each group
example: for each stock, add the price difference comparing to last datetime

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
