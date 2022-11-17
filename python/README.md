# Python Tricks

- numpy
- pandas
- tensorflow

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
    d['c_d_prod_sum'] = (x['c'] * x['d']).sum()
    return pd.Series(d, index=['a_sum', 'a_max', 'b_mean', 'c_d_prod_sum'])

df.groupby('group').apply(f)

"""or even more simplified"""

def f(x):
    return pd.Series({
      'a_sum': x['a'].sum(),
      'a_max': x['a'].max(),
      'b_mean': x['b'].mean(),
      'c_d_prod_sum': (x['c'] * x['d']).sum(),
    })

df.groupby('group').apply(f)

```


## tensorflow

### faster predicting
```python
Model(x)
# is faster than
model.predict(x)
```
[reference](https://stackoverflow.com/questions/60159714/when-to-use-model-predictx-vs-modelx-in-tensorflow)

## things learnt from [here](https://www.youtube.com/watch?v=qUeud6DvOWI)

thing should be doing:

### Not to use bare except

```python
# wrong
while True:
    try:
        s = input('Input a number: ')
        x = int(s)
        break
    except:  # opps! can't CTRL-C to exit
        print('Not a number, try again')

# correct
while True:
    try:
        ...
    # lazy way
    except Exception:
    # most appropriate - specified error type
    except ValueError:

```

### check type equality

```python
# wrong
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
if type(p) == tuple:  # Liskov substitution violation
    pass

# correct
if isinstance(p, tuple):
    pass
```

### better timing

```python
import time

time0 = time.time()
time1 = time.time()
print(time1 - time0)

# more accurate way
time0 = time.perf_counter()
time1 = time.perf_counter()
print(time1 - time0)

```

### print vs logging

```python
# currently
def print_vs_logging():
    print('debug info')
    print('just some info')
    print('bad error')


# professional way
import logging


def print_vs_logging():
    logging.debug('debug info')
    logging.info('just some info')
    logging.error('uh, please do so instead')


def main():
    level = logging.DEBUG
    fmt = '[%(levelname)s] %(asctime)s - %(message)s'
    logging.basicConfig(level=level, format=fmt)

```

### subprocess with shell=True

```python
import subprocess

subprocess.run(
    ['ls -l'],  # <--- avoid putting arguments in a list of elements, e.g. ['ls', '-l']
    capture_output=True,
    shell=True  # <--- This is more secure
)
```

### create package to install instead of relative import

not really that level yet.
