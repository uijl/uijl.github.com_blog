---
title: "Avoiding (nested) for loops"
date: 2020-01-26T12:00:00+01:00
draft: false
tags: [Python, For Loops, Programming, Itertools]
categories: []
---

When I quickly need to write something I ofter resort to nested for loops, it works and I can easily see what I am doing. However, I am frustrated that I resort to this instead of using itertools, which I know is far superior. So this post is to force myself into using itertools and to use as a cheatsheet.

There are three mistakes I no longer want to make so I will go through their alternatives in this post:

1. Do something for i in list_A;
2. For i in list_A, for j in list_A;
3. For i in list_A, for j in list_B.

### Do something for i in list_A

What I want to do is create a new list, by manipulating values in another list. Suppose we have a list with date strings and we want to create a list of datetime objects. The code below will provide the _wrong_ solution, a solution using [generator expressions](https://www.python.org/dev/peps/pep-0289/) and a solution using the build-in [map function](https://docs.python.org/3.8/library/functions.html#map). To be honest I do not know which of the two solutions is superior.

```python
from datetime import datetime

# The strings we want to convert to datetime objects
date_strings = ["2020-01-01", "2020-01-02", "2020-01-03"]

# Wrong solution
date_objects = []

for date in date_datestrings:
    date_objects.append(datetime.strptime(date, "%Y-%m-%d"))

# Solution using a generator expression
date_objects = [datetime.strptime(date, "%Y-%m-%d") for date in date_strings]

# Solution using map()
date_objects = list(map(lambda date: datetime.strptime(date, "%Y-%m-%d"), date_strings))
```

### For i in list_A, for j in list_A

Sometimes you want to do something with all the unique combinations in a list. The easiest way out is a nested for loop but this should be prevented. The easiest way in doing so is using [itertools](https://docs.python.org/3.8/library/itertools.html). A build-in Python module with _Functions creating iterators for efficient looping_. It will require a seperate import statement.

The function we want to use is [permutations](https://docs.python.org/3.8/library/itertools.html#itertools.permutations). You enter the _iterable_ and the number of combined values you wish, every combination represented a nested-for loop.

```python
# The list to combine into unique combinations
list_A = [1, 2, 3, 4, 5]

# Wront solution
combinations = []

for i in list_A:
    for j in list_A:
        if i != j:
            combinations.apend((i, j))

# Solution using itertools
import itertools
combinations = list(itertools.permutations(list_A, 2))
```

### For i in list_A, for j in list_B

The most common nested for loop occurs when you want to do something with all possible combinations of multiple loops. Again, the best solution is provided by [itertools](https://docs.python.org/3.8/library/itertools.html), the [product function](https://docs.python.org/3.8/library/itertools.html#itertools.product) in this case. It will generate all possible combinations between the provided _iterables_.

```python
from datetime import datetime

# The years and months we are interested in
years = [2020, 2021, 2022]
months = [m+1 for m in range(12)]

# Wrong solution
date_objects = []

for year in years:
    for month in months:
        date_objects.append(datetime(year, month, 1))

# Solution using itertools
import itertools
date_objects = [
    datetime(year, month, 1) for year, month in itertools.product(years, months)
]
```

You can take the ```itertools.product``` as far as you want. You can add as many _iterable_ variables as possible and itertools will provide you with every unique combination of the iterables you provided.
