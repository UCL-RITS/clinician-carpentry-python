---
title: Indexing, Slicing and Subsetting DataFrames in Python
teaching: 30
exercises: 30
questions:
    - "How can I access specific data within my data set?"
    - "How can Python and Pandas help me to analyse my data?"
objectives:
    - "Describe what 0-based indexing is."
    - "Manipulate and extract data using column headings and index locations."
    - "Employ slicing to select sets of data from a DataFrame."
    - "Employ label and integer-based indexing to select ranges of data in a dataframe."
    - "Reassign values within subsets of a DataFrame."
    - "Create a copy of a DataFrame."
    - "Query / select a subset of data using a set of criteria using the following operators:
       `==`, `!=`, `>`, `<`, `>=`, `<=`."
    - "Locate subsets of data using masks."
    - "Describe BOOLEAN objects in Python and manipulate data using BOOLEANs."
keypoints:
    - "In Python, portions of data can be accessed using indices, slices, column headings, and
       condition-based subsetting."
    - "Python uses 0-based indexing, in which the first element in a list, tuple or any other data
       structure has an index of 0."
    - "Pandas enables common data exploration steps such as data indexing, slicing and conditional
       subsetting."
---

In the first episode of this lesson, we read a CSV file into a pandas' DataFrame. We learned how to:

- save a DataFrame to a named object,
- perform basic math on data,
- calculate summary statistics, and
- create plots based on the data we loaded into pandas.

In this lesson, we will explore ways to access different parts of the data using:

- indexing,
- slicing, and
- subsetting.

## Loading our data

We will continue to use the synthetic patients dataset that we worked with in the last
episode. Let's reopen and read in the data again:

~~~
# Make sure pandas is loaded
import pandas as pd

# Read in the patients CSV
patients_df = pd.read_csv("data/synthetic_data_clean.csv")
~~~
{: .language-python}

## Indexing and Slicing in Python

We often want to work with subsets of a **DataFrame** object. There are
different ways to accomplish this including: using labels (column headings),
numeric ranges, or specific x,y index locations.


## Selecting data using Labels (Column Headings)

We use square brackets `[]` to select a subset of a Python object. For example,
we can select all data from a column named `height` from the `patients_df`
DataFrame by name. There are two ways to do this:

~~~
# TIP: use the .head() method we saw earlier to make output shorter
# Method 1: select a 'subset' of the data using the column name
patients_df['height']

# Method 2: use the column name as an 'attribute'; gives the same output
patients_df.height
~~~
{: .language-python}

We can also create a new object that contains only the data within the
`height` column as follows:

~~~
# Creates an object, patients_height, that only contains the `height` column
patients_height = patients_df['height']
~~~
{: .language-python}

We can pass a list of column names too, as an index to select columns in that
order. This is useful when we need to reorganize our data.

**NOTE:** If a column name is not contained in the DataFrame, an exception
(error) will be raised.

~~~
# Select the height and weight columns from the DataFrame
patients_df[['height', 'weight']]

# What happens when you flip the order?
patients_df[['weight, 'height']]

# What happens if you ask for a column that doesn't exist?
patients_df['weightt']
~~~
{: .language-python}

Python tells us what type of error it is in the traceback, at the bottom it says
`KeyError: 'weightt'` which means that `weightt` is not a valid column name (nor a valid key in
the related Python data type dictionary).

> ## Reminder
> The Python language and its modules (such as Pandas) define reserved
> words that should not be used as identifiers when assigning objects
> and variable names. Examples of reserved words in Python include Boolean values
> `True` and `False`, operators `and`, `or`, and `not`, among others. The full list
> of reserved words for Python version 3 is provided at
> <https://docs.python.org/3/reference/lexical_analysis.html#identifiers>.
>
> When naming objects and variables, it's also important to avoid using
> the names of built-in data structures and methods. For example, a _list_ is a built-in
> data type. It is possible to use the word 'list' as an identifier for a new object,
> for example `list = ['apples', 'oranges', 'bananas']`. However, you would then
> be unable to create an empty list using `list()` or convert a tuple to a 
> list using `list(sometuple)`.
{: .callout}

## Extracting Range based Subsets: Slicing

> ## Reminder
> Python uses 0-based indexing.
{: .callout}

Let's remind ourselves that Python uses 0-based
indexing. This means that the first element in an object is located at position
`0`. This is different from other tools like R and Matlab that index elements
within objects starting at 1.

~~~
# Create a list of numbers:
a = [1, 2, 3, 4, 5]
~~~
{: .language-python}

![indexing diagram](../fig/slicing-indexing.png)
![slicing diagram](../fig/slicing-slicing.png)


> ## Challenge - Extracting data
>
> 1. What value does the code below return?
>
>    ~~~
>    a[0]
>    ~~~
>    {: .language-python }
>
> 2. How about this:
>
>    ~~~
>    a[5]
>    ~~~
>    {: .language-python }
>
> 3. In the example above, calling `a[5]` returns an error. Why is that?
>
> 4. What about?
>
>    ~~~
>    a[len(a)]
>    ~~~
>    {: .language-python }
{: .challenge}


## Slicing Subsets of Rows in Python

Slicing using the `[]` operator selects a set of rows and/or columns from a
DataFrame. To slice out a set of rows, you use the following syntax:
`data[start:stop]`. When slicing in pandas the start bound is included in the
output. The stop bound is one step BEYOND the row you want to select. So if you
want to select rows 0, 1 and 2 your code would look like this:

~~~
# Select rows 0, 1, 2 (row 3 is not selected)
patients_df[0:3]
~~~
{: .language-python}

The stop bound in Python is different from what you might be used to in
languages like Matlab and R.

~~~
# Select the first 5 rows (rows 0, 1, 2, 3, 4)
patients_df[:5]

# Select the last element in the list
# (the slice starts at the last element, and ends at the end of the list)
patients_df[-1:]
~~~
{: .language-python}

We can also reassign values within subsets of our DataFrame.

But before we do that, let's look at the difference between the concept of
copying objects and the concept of referencing objects in Python.

## Copying Objects vs Referencing Objects in Python

Let's start with an example:

~~~
# Using the 'copy() method'
true_copy_patients_df = patients_df.copy()

# Using the '=' operator
ref_patients_df = patients_df
~~~
{: .language-python}

You might think that the code `ref_patients_df = patients_df` creates a fresh
distinct copy of the `patients_df` DataFrame object. However, using the `=`
operator in the simple statement `y = x` does **not** create a copy of our
DataFrame. Instead, `y = x` creates a new variable `y` that references the
**same** object that `x` refers to. To state this another way, there is only
**one** object (the DataFrame), and both `x` and `y` refer to it.

In contrast, the `copy()` method for a DataFrame creates a true copy of the
DataFrame.

Let's look at what happens when we reassign the values within a subset of the
DataFrame that references another DataFrame object:

~~~
# Assign the value `0` to the first three rows of data in the DataFrame
ref_patients_df[0:3] = 0
~~~
{: .language-python}

Let's try the following code:

~~~
# ref_patients_df was created using the '=' operator
ref_patients_df.head()

# patients_df is the original dataframe
patients_df.head()
~~~
{: .language-python}

What is the difference between these two dataframes?

When we assigned the first 3 columns the value of `0` using the
`ref_patients_df` DataFrame, the `patients_df` DataFrame is modified too.
Remember we created the reference `ref_patients_df` object above when we did
`ref_patients_df = patients_df`. Remember `patients_df` and `ref_patients_df`
refer to the same exact DataFrame object. If either one changes the object,
the other will see the same changes to the reference object.

**To review and recap**:

- **Copy** uses the dataframe's `copy()` method

  ~~~
  true_copy_patients_df = patients_df.copy()
  ~~~
  {: .language-python }
- A **Reference** is created using the `=` operator

  ~~~
  ref_patients_df = patients_df
  ~~~
  {: .language-python }

Okay, that's enough of that. Let's create a brand new clean dataframe from
the original data CSV file.

~~~
patients_df = pd.read_csv("data/synthetic_data_clean.csv")
~~~
{: .language-python}

## Slicing Subsets of Rows and Columns in Python

We can select specific ranges of our data in both the row and column directions
using either label or integer-based indexing.

- `loc` is primarily *label* based indexing. *Integers* may be used but
  they are interpreted as a *label*.
- `iloc` is primarily *integer* based indexing

To select a subset of rows **and** columns from our DataFrame, we can use the
`iloc` method. For example, we can select `ph_abg`, `hco3_abg` and `temp_c` (columns 2, 3
and 4 if we start counting at 1), like this:

~~~
# iloc[row slicing, column slicing]
patients_df.iloc[0:3, 0:4]
~~~
{: .language-python}

which gives the **output**

~~~
   ph_abg  hco3_abg  temp_c
0    7.44      28.0    36.9
1    7.30      20.2    36.6
2    7.48      24.6    37.2
~~~
{: .output}

Notice that we asked for a slice from 0:3. This yielded 3 rows of data. When you
ask for 0:3, you are actually telling Python to start at index 0 and select rows
0, 1, 2 **up to but not including 3**.

Let's explore some other ways to index and select subsets of data:

~~~
# Select all columns for rows of index values 0 and 10
patients_df.loc[[0, 10], :]

# What does this do?
patients_df.loc[0, ['height', 'dob', 'weight']]

# What happens when you type the code below?
patients_df.loc[[0, 10, 35549], :]
~~~
{: .language-python}

**NOTE**: Labels must be found in the DataFrame or you will get a `KeyError`.

Indexing by labels `loc` differs from indexing by integers `iloc`.
With `loc`, both the start bound and the stop bound are **inclusive**. When using
`loc`, integers *can* be used, but the integers refer to the
index label and not the position. For example, using `loc` and select 1:4
will get a different result than using `iloc` to select rows 1:4.

We can also select a specific data value using a row and
column location within the DataFrame and `iloc` indexing:

~~~
# Syntax for iloc indexing to finding a specific data element
dat.iloc[row, column]
~~~
{: .language-python}

In this `iloc` example,

~~~
patients_df.iloc[2, 6]
~~~
{: .language-python}

gives the **output**

~~~
'F'
~~~
{: .output}

Remember that Python indexing begins at 0. So, the index location [2, 6]
selects the element that is 3 rows down and 7 columns over in the DataFrame.



> ## Challenge - Range
>
> 1. What happens when you execute:
>
>    - `patients_df[0:1]`
>    - `patients_df[:4]`
>    - `patients_df[:-1]`
>
> 2. What happens when you call:
>
>    - `patients_df.iloc[0:4, 1:4]`
>    - `patients_df.loc[0:4, 1:4]`
>
> - How are the two commands different?
{: .challenge}


## Subsetting Data using Criteria

We can also select a subset of our data using criteria. For example, we can
select all rows that refer to patients born after 1960:

~~~
patients_df['dob'] = pd.to_datetime(patients_df['dob'])
patients_df[patients_df.dob > pd.Timestamp(1960,12,31)]
~~~
{: .language-python}

Which produces the following output:

~~~
 ph_abg  hco3_abg  temp_c  temp_nc  urea      creatinine   na    k  ...        discharge_dttm        dob  vital_status  sex    id  lactate_1hr  lactate_6hr  lactate_12hr
0       7.44      28.0    36.9     37.1   3.0   80 micromol/L  135  4.2  ...  2014-01-02T01:06:16Z 1964-01-01             A    F     1          1.2          0.3           0.1
1       7.30      20.2    36.6     36.4   5.8   93 micromol/L  140  4.4  ...  2014-01-05T00:35:32Z 1984-01-01             A    M     2          1.8          0.5           0.3
2       7.48      24.6    37.2     36.2   4.5   75 micromol/L  140  3.8  ...  2014-01-05T13:24:13Z 1964-01-01             A    F     3          1.4          1.7           1.2
7       7.40      23.1    35.9     36.1   9.4  251 micromol/L  138  5.8  ...  2014-01-04T02:26:12Z 1974-01-02             A    M     8          1.2          0.7           0.4
8       7.36      24.0    35.9     36.6  12.9  390 micromol/L  134  6.5  ...  2014-01-16T22:52:17Z 1964-01-02             A    M     9          1.6          1.8           1.8
...      ...       ...     ...      ...   ...             ...  ...  ...  ...                   ...        ...           ...  ...   ...          ...          ...           ...
4987    7.38      19.5    36.3     38.0  37.2  355 micromol/L  131  3.8  ...  2019-09-23T11:52:30Z 1979-09-19             A    F  4988          0.6          0.7           0.8
4991    7.42      21.8    37.7     37.5   2.1   45 micromol/L  136  4.4  ...  2019-09-26T03:37:53Z 1989-09-22             A    F  4992          1.2          1.2           0.9
4992    7.29      17.3    35.5     33.6   4.8  104 micromol/L  150  4.0  ...  2019-10-07T01:43:08Z 1969-09-22             D    F  4993          2.2          2.2           2.1
4993    7.31      32.3    36.7     36.3   8.7  115 micromol/L  139  4.4  ...  2019-09-23T17:02:29Z 1999-09-22             A    F  4994          0.8          0.7           0.7
4997    7.21      17.0    34.7     36.5   3.9   54 micromol/L  139  4.2  ...  2019-09-26T08:28:12Z 1979-09-23             A    F  4998          0.7          0.2           0.2

[1672 rows x 32 columns]
~~~
{: .language-python}

Or we can select all rows with patients born in 1960 or before:

~~~
patients_df[patients_df.dob <= pd.Timestamp(1960,12,31)]
~~~
{: .language-python}

We can define sets of criteria too:

~~~
patients_df[(patients_df.dob >= pd.Timestamp(1980,1,1)) & (patients_df.dob <= pd.Timestamp(1985,12,31))]
~~~
{: .language-python}

### Python Syntax Cheat Sheet

We can use the syntax below when querying data by criteria from a DataFrame.
Experiment with selecting various subsets of the "patients" data.

* Equals: `==`
* Not equals: `!=`
* Greater than, less than: `>` or `<`
* Greater than or equal to `>=`
* Less than or equal to `<=`


> ## Challenge - Queries
>
> 1. Select a subset of rows in the `patients_df` DataFrame that contain data from patients born in
>   1989 with weights of less than or equal to 80kg. How
>   many rows did you end up with? What did your neighbour get?
>
> 2. You can use the `isin` command in Python to query a DataFrame based upon a
>   list of values as follows:
>
>    ~~~
>    patients_df[patients_df['height'].isin([list_goes_here])]
>    ~~~
>    {: .language-python }
>
>   Use the `isin` function to find patients with heights of either 1.65 m or 1.85 m
>   in the "patients" DataFrame. How many records contain these values?
>
> 3. Experiment with other queries. Create a query that finds all rows with a
>   weight value > or equal to 75.
>
> 4. The `~` symbol in Python can be used to return the OPPOSITE of the
>   selection that you specify in Python. It is equivalent to **is not in**.
>   Write a query that selects all rows with sex NOT equal to 'F' in
>   the "patients" data.
{: .challenge}


# Using masks to identify a specific condition

A **mask** can be useful to locate where a particular subset of values exist or
don't exist - for example,  NaN, or "Not a Number" values. To understand masks,
we also need to understand `BOOLEAN` objects in Python.

Boolean values include `True` or `False`. For example,

~~~
# Set x to 5
x = 5

# What does the code below return?
x > 5

# How about this?
x == 5
~~~
{: .language-python}

When we ask Python whether `x` is greater than 5, it returns `False`.
This is Python's way to say "No". Indeed, the value of `x` is 5,
and 5 is not greater than 5.

To create a boolean mask:

- Set the True / False criteria (e.g. `values > 5 = True`)
- Python will then assess each value in the object to determine whether the
  value meets the criteria (True) or not (False).
- Python creates an output object that is the same shape as the original
  object, but with a `True` or `False` value for each index location.

Let's try this out. Let's identify all locations in the patient data that have
null (missing or NaN) data values. We can use the `isnull` method to do this.
The `isnull` method will compare each cell with a null value. If an element
has a null value, it will be assigned a value of  `True` in the output object.

~~~
pd.isnull(patients_df)
~~~
{: .language-python}

A snippet of the output is below:

~~~
      ph_abg  hco3_abg  temp_c  temp_nc   urea  creatinine     na      k  ...  discharge_dttm    dob  vital_status    sex     id  lactate_1hr  lactate_6hr  lactate_12hr
0      False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
1      False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
2      False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
3      False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
4      False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
...      ...       ...     ...      ...    ...         ...    ...    ...  ...             ...    ...           ...    ...    ...          ...          ...           ...
4995   False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
4996   False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
4997   False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
4998   False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False
4999   False     False   False    False  False       False  False  False  ...           False  False         False  False  False        False        False         False

[5000 rows x 32 columns]
~~~
{: .language-python}

To select the rows where there are null values, we can use
the mask as an index to subset our data as follows:

~~~
# To select just the rows with NaN values, we can use the 'any()' method
patients_df[pd.isnull(patients_df).any(axis=1)]
~~~
{: .language-python}

We will explore ways of dealing with null values in the next episode on [Data Types and Formats]({{ page.root }}{% link _episodes/04-data-types-and-format.md %}).

We can run `isnull` on a particular column too. What does the code below do?

~~~
# What does this do?
empty_dobs = patients_df[pd.isnull(patients_df['dob'])]['dob']
print(empty_weights)
~~~
{: .language-python}

Let's take a minute to look at the statement above. We are using the Boolean
object `pd.isnull(patients_df['dob'])` as an index to `patients_df`. We are
asking Python to select rows that have a `NaN` value of date of birth.


> ## Challenge - Putting it all together
>
> 1. Create a new DataFrame that contains only observations that are of sex male
>   or female and where height values are greater than 1.75. Create a stacked bar
>   plot of average height with male vs female values stacked for each
>   plot.
{: .challenge}

{% include links.md %}
