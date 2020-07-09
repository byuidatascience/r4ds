# DataFrame




## Introduction

Throughout this book we work with pandas _DataFrame__. Python is an old language, and the tools for data in Python that were useful 10 or 20 years ago now get in your way. Here we will describe the __DataFrame__ data structure, which provides opinionated data frames that make working in with data easier. In most places, I'll use the term data frame.

If this chapter leaves you wanting to learn more about data frames, you might enjoy reading [the documentation](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html).

### Prerequisites

In this chapter we'll explore the __DataFrame__, a foundational element of pandas.


```python
import pandas as pd
import pandas as pd
import altair as alt
import numpy as np
```

## Creating tibbles

Almost all of the functions that you'll use in this book produce data frames, as data frames are one of the unifying features of pandas. 

You can create a new data frames from individual vectors with `pd.DataFrame()`. `pd.DataFrame()` will automatically recycle inputs of length 1 but does not allow you to refer to variables that you just created.


```python
pd.DataFrame({
  'x': [1, 2, 3, 4, 5],
  'y': 1}
).assign(z = lambda x: x.x**2 + x.y)
#>    x  y   z
#> 0  1  1   2
#> 1  2  1   5
#> 2  3  1  10
#> 3  4  1  17
#> 4  5  1  26
```

Unlike R data frames, pandas data frames can have column names that are not valid R variable names, aka __non-syntactic__ names. For example, they might not start with a letter, or they might contain unusual characters like a space. Notice the use of index as we are passing all scalar values:


```python
tb = pd.DataFrame({
  ':)': 'smile',
  ' ' : 'space',
  '2000': 'number'},index=[0])
  
tb
#>       :)           2000
#> 0  smile  space  number
```

Another way to create a tibble is with `np.arrray()`.  Sometimes `np.array()` makes it possible to lay out small amounts of data in easy to read form.


```python
pd.DataFrame(np.array(
  [["a", 2, 3.6], 
  ["b", 1, 8.5]]), columns = 
  ['x', 'y', 'z'])
#>    x  y    z
#> 0  a  2  3.6
#> 1  b  1  8.5
```

## Printing

Data frames have a refined print method that shows only the first and last 5 rows, and all the columns that fit on screen. This makes it much easier to work with large data. 


```r
tibble(
  a = lubridate::now() + runif(1e3) * 86400,
  b = lubridate::today() + runif(1e3) * 30,
  c = 1:1e3,
  d = runif(1e3),
  e = sample(letters, 1e3, replace = TRUE)
)
#> # A tibble: 1,000 x 5
#>   a                   b              c     d e    
#>   <dttm>              <date>     <int> <dbl> <chr>
#> 1 2020-06-25 19:24:08 2020-07-02     1 0.368 n    
#> 2 2020-06-26 13:29:17 2020-07-07     2 0.612 l    
#> 3 2020-06-26 07:52:56 2020-07-17     3 0.415 p    
#> 4 2020-06-25 21:14:13 2020-07-16     4 0.212 m    
#> 5 2020-06-25 17:38:30 2020-07-13     5 0.733 i    
#> 6 2020-06-26 04:39:27 2020-07-09     6 0.460 n    
#> # ... with 994 more rows
```

Data frames are designed so that you don't accidentally overwhelm your console when you print large data frames. But sometimes you need more output than the default display. There are a few options that can help.

First, you can return the data frame using  `.head()` on the data frame and control the number of rows (`n`) of the display. In the interactive Python viewer in VS Code you can scroll to see the other columns.


```python
flights.head(20)
```

You can also control the default print behaviour by [setting options](https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html):

* `pd.set_option("display.max_rows", 101)`: if more than `101`
  rows, print only `n` rows. 
  
* `pd.set_option('precision', 5)` will set the number of decimals that are shown. 

You can see a complete list of options by looking at the [pandas help](https://pandas.pydata.org/pandas-docs/stable/user_guide/options.html).

### Subsetting

So far all the tools you've learned have worked with complete data frames. If you want to pull out a single variable, you need some new tools, `[`. `[` can extract by name or position.


```python
df = pd.DataFrame({
  'x': np.random.uniform(size = 5),
  'y': np.random.normal(size = 5)})

# Extract by name as pandas data frame
df[["x"]]
# Extract by name as array
#>           x
#> 0  0.614754
#> 1  0.972031
#> 2  0.848065
#> 3  0.843117
#> 4  0.485332
df["x"]
# Extract by position
#> 0    0.614754
#> 1    0.972031
#> 2    0.848065
#> 3    0.843117
#> 4    0.485332
#> Name: x, dtype: float64
df.iloc[:, 1]
#> 0   -1.394471
#> 1   -0.780052
#> 2    0.990076
#> 3   -0.219665
#> 4    0.337708
#> Name: y, dtype: float64
df[df.columns[1]]
#> 0   -1.394471
#> 1   -0.780052
#> 2    0.990076
#> 3   -0.219665
#> 4    0.337708
#> Name: y, dtype: float64
```



## Exercises


1.  If you have the name of a variable stored in an object, e.g. `var <- "mpg"`,
    how can you extract the reference variable from a tibble?

1.  Practice referring to non-syntactic names in the following data frame by:

    1.  Extracting the variable called `1`.

    1.  Plotting a scatterplot of `1` vs `2`.

    1.  Creating a new column called `3` which is `2` divided by `1`.
        
