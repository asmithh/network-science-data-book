# Class 6: Data Science 1 — Pandas, SQL, Regressions

## Pandas

### What is pandas?
(adapted from [10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html))

pandas is a Python library for manipulating & analyzing data. 

We usually import pandas like this:
```
import pandas as pd
```

pandas has two basic units that we care about: a `Series` and a `DataFrame`. `Series` are roughly akin to lists -- they're one-dimensional and can hold data of any type. `DataFrames` are two-dimensional (they have rows and columns) and are indexed by one or more columns. This means that a row can be identified by the labels in the index column. Each column of a `DataFrame` is also a `Series`. 

We'll focus on `DataFrames` for now. We can populate a `DataFrame` in a few different ways. First, let's instantiate a `DataFrame` with our own data. 

```
df = pd.DataFrame({
    "Name": ["Orca", "Blue Whale", "Beluga Whale", "Narwhal"],
    "Weight": [22000, 441000, 2530, 3500],
    "Cute":[True, True, True, True],
    "Habitat": ["Everywhere", "Everywhere", "Arctic", "Arctic"],
})
```

Let's look at the types the columns of `df` have: 
```
df.dtypes
```

Wow! These are a lot of different types! 

We can also access the column names like this:
```
df.columns
```

or see the first 3 entries like this:
```
df.head(3)
```

### Common Pandas operations
#### Sorting
Now we'll practice some common operations in pandas. One thing we might want to do is sort a dataframe by a particular column:
```
df.sort_values(by="Weight", ascending=False)
```

Can you sort the dataframe alphabetically by species name? 

#### Filtering
Another common thing we can do in Pandas is filtering. This means we only select the rows that match a specific condition. Let's filter for all the cetaceans whose weight is over 5000 pounds:
```
df[df.Weight > 5000]
```

Here, we're referring to the `Weight` column as an _attribute_ of our dataframe `df`. However, we can also refer to the column like we would a key in a dictionary, with the same result:
```
df[df['Weight'] > 5000]
```

Can you filter the dataframe to get all the cetaceans that live in the Arctic?

#### Adding a column
We can also add a column to the dataframe as long as it's the same length as the existing dataframe:
```
df["Food"] = ["Everything", "Krill", "Squid, Clams, Octopus, Cod, Herring", "Fish"]
```

And we can create a column from an existing column using the `apply` function:
```
def convert_from_lbs_to_kg(wt):
    return wt * 0.45
df["Weight_kg"] = df["Weight"].apply(convert_from_lbs_to_kg)
```
When we apply a function, we can use an actual function, as we did above, or we can use an _anonymous function_, or a "lambda function". 
```
df["Weight_tons"] = df["Weight"].apply(lambda b: b / 2000)
```

#### Grouping

We also might want to group our dataframe into chunks that have something in common. 

```
groups = df.groupby('Habitat')
for habitat, df_gr in groups:
    print(habitat)
    print(df_gr.head())
```

Here, we're grouping our dataframe by the `Habitat` column. `groups` is an iterable (we can iterate over the items in it, in the order given to us) that has two variables. One of them is the unique value of the column we grouped by (`Everywhere` and `Arctic`) and the other one is the chunk of the dataframe that had that unique value in that column. 

Can you group the cetaceans that weigh more than 5000 pounds apart from the ones that weigh less than 5000 pounds? (Hint: You might have to create a new column!)

### Practicing with Pandas
We can also load a pandas dataframe from a `.csv` (comma separate values) file. We're going to load up `WhaleFromSpaceDB_Whales.csv`, which we sourced from the [UK Polar Data Centre](https://ramadda.data.bas.ac.uk/repository/entry/show?entryid=c1afe32c-493c-4dc7-af9f-649593b97b2c)

```
df_whales = pd.read_csv('data/WhaleFromSpaceDB_Whales.csv')
print(df_whales.head(5))
print(df_whales.columns)
```

Let's explore the dataset a little bit! The `value_counts` function tells us how many times each unique value showed up in a column. 
```
df_whales['MstLklSp'].value_counts()
```
Now let's check for flukes! Flukes are valuable to marine biologists because they help them identify individuals. For each whale species, can you count how many times flukes were and were not seen?

What about the average certainty for each species? (Hint: `df.column.mean()` gives the average value of a column, and `Certainty2` is the column that contains the certainty score!)

## SQL

### What is SQL?
SQL stands for "__Structured Query Language__". It is a way to query a particular kind of database that has **tabular data**. 

#### What is tabular data?
Tabular data lends itself well to being organized in rows and columns. Each row represents one instance/observation, and each column represents a different attribute the instances/observations have. 

#### When is it used?
SQL is used a lot in industry to store and query data for analytic purposes. When you have a ton of data to deal with, SQL is a great way to do that. 

### How do we query an SQL table?
The most basic way to extract data from an SQL table is selecting everything:
```
SELECT * FROM table;
```
This will get us everything in `table`, so sometimes it's not a good idea to pull **all the data**. In this case, we can set a limit of 5 entries:
```
SELECT * FROM table LIMIT 5;
```
But what if we want to filter our data? In this example, we select all entries from the table named `table` that have a value in `column` greater than 5:
```
SELECT * FROM table WHERE column > 5;

```

Or if we want to select based on multiple conditions, we can do this:
```
SELECT * FROM table WHERE column1 > 5 AND column2 < 3;

```

And if we don't want duplicate rows, we can use `SELECT DISTINCT`: 
```
SELECT DISTINCT * FROM table WHERE column1 > 5 AND column2 < 3;

```
### Aggregation
Just like in pandas, sometimes we might want aggregated information about our tables. 
In this case, we can use `GROUP BY` and aggregation functions. 

```
SELECT mean(column1), max(column2) FROM table GROUP BY column3;

```
Here, we're grouping our table by the unique values in `column3`. Then, for each unique value in `column3`, we're going to grab the average value that `column1` takes and the max value of `column2`. This allows us to get aggregate information about our data!

### Practicing an SQL Query on the Cluster

## Regressions

### What is a regression?
Regressions are used to figure out a model for data. 
### When are regressions useful?

### When do they fall short?

### Running a linear regression on a Pandas dataframe


