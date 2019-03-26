
# An End to End Data Cleaning Example


## Introduction
In "week 0", we installed the key tools required to do data capture and cleaning with Python and to complete this course (git, GitHub and Anaconda). In week 1, we introduced the basics of Python programming: variables, some simple data types (String, Integer, Floating Point, Boolean and Date Time), some of the more complex data types (List, Tuple, Set and Dictionary), and control flow using if statements, and (for looping over collections), "for-in" loops and (for extra credit) list comprehensions. *Don't worry if you're still a little confused by list comprehensions - you don't have to master them to be able to clean up data!*

A lot of the tasks last week were "small motion" lessons. You got to practice little bits of a project, but didn't really pull it together into a more realistic end-to-end experience. Today, we're going to build on the knowledge you gained last week to do an end to end data cleaning example. We're going to import some company data, clean it up and then save it to a new file.

To keep things easy, we'll just focus on cleaning up the capitalization of text from a csv file filled with company information. In a real world example you'd probably want to do more complicated clean up, but once you've seen how to do the basics, it'll be quite easy to add a little more sophistication or complexity to the data clean up in future lessons.

We're also going to have to introduce a few new concepts along the way. We're going to introduce the concept of libraries (pre-written software that you can use to write your code more quickly), Pandas (one of the most popular libraries for working with data in Python) and the Pandas Series and DataFrame (the standard ways to store information within Pandas). It might seem like a lot of theory, but in practice you'll be using Pandas DataFrames for most of your data import, cleanup and export operations, so it's important to take the time to introduce the concepts now.


## Objectives
You will be able to:
* Explain what a library is and why you might want to use one
* Explain what a Series and DataFrame is and why you would use them
* Import data from a csv file into a DataFrame
* Transform data within a DataFrame to clean it up
* Export data from a DataFrame into a csv

## Review
Lets start with a quick review from last week with a few quick questions . . .
* **Which two of the simple data types above are generally best for working with numbers?**
* **What's the difference between those two types?**
* **How would you take a string containing whole numbers (no fractions or decimals) and turn it into a numeric data type?**
* **If you have an unchanging list (e.g. a list of US states) that should not have any duplicates, what would be the best (complex) data type to store that?**
* **If you have a list of companies with company name, address and website for 50 companies, what two complex data types that we introduced last week would you combine to store that information in Python?**

Great! Now lets learn some new things!

## Introducing Libraries
Libraries are pre-written pieces of software that you can use to write software more quickly. There are lots of pre-written libraries in Python for doing everything from storing and manipulating data to plotting graphs to creating Neural Networks. 

## Introducing Pandas
[Pandas](https://pandas.pydata.org/) is one of the most popular Python libraries for anyone working with data. It is a library providing high-performance, easy-to-use data structures and data analysis tools.

## Introducing the Pandas Series and DataFrame
At the heart of the Pandas library are two special data types - the Series and the DataFrame. A Series is used for storing and operating on a single dimensional data set - something that you might have previously put into a List or a Tuple. An example of the kind of data you'd put into a Series would be a list of US states or a list of company names.

A DataFrame is used for storing and operating on two dimensional data - anything you might put into a spreadsheet that has more than one row and more than one column. It could hold a list of companies with the name, address and phone number for each company or a list of people with the first name, last name, email address and date of birth for each person.

In practice we're going to use a DataFrame most of the time as we'll often be importing two dimensional data from csv files or creating it by pulling information from an API or a website.

## An Overview of the Data Cleaning Process
The overall process of data cleaning that we're going to run through today is pretty straightforward:
* **Import data** - from a csv file into Python (specifically saving it in a Pandas DataFrame)
* **"Look" at the data** - perhaps running some summary statistics 
* **Clean up the data** - iterating over the data in any given column to clean it up
* **Export the data** - typically to a csv file so you can import it into another application for further analysis or processing

Alright, let's give it a try!

## "Importing" the Library
Before we can use a library in Python, we have to tell Python that we want to include that library in our code. That is called "importing" the library. Here's the code. Make sure to run it, otherwise the rest of this notebook isn't going to work.


```python
import pandas as pd
```

Let's have a look at the code:
* **import** is a Python key word telling Python that we're just about to give it the name of a library that we want to be able to use
* **pandas** is the name of the library - we generally capitalize the P when referring to the library in documentation, but when we want to import it into Python, it has to be written using lower case. *If you capitalize the P and run the code you;'ll get a "library not found" error - feel free to try it out in the cell above!*
* **as** is another Python keyword. This is optional, but it allows us to declare a shorter *alias* for the library so for the rest of the notebook instead of having to type `pandas` every time we want to refer to the library, we can type something else (typically something shorter to save us some typing)
* **pd** is the common shortening of `pandas` that most Python programmers will use. It's a good idea to follow common conventions like this so that other programmers looking at your code can understand it more easily.

## Importing the Data

The next step is to import the csv file and to save the information into a Pandas DataFrame. The basic code for that is pretty simple . . . Run the code and see what happens!


```python
df = pd.read_csv('sample_data.csv')
```

So, if the file exists, nothing happens at all - that is good! Try changing the file name and running the code and you'll see a "file not found" error. The code above is basically creating a new variable (by convention it's not unusual to call it "df" for "DataFrame" - although if you're doing something more complex you might want to give it a clearer name - perhaps company_list or something similar.

The way to read the code is that the right hand side of the equals sign gets evaluated. It goes to the Pandas library (remember, we imported Pandas **as** `pd`, so whenever we refer to pd we're refering to the Pandas library) and it's calling a method on Pandas called `read_csv()` (the docs for that method are [here](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html)). The minimum required parameter for that method is the name of the file to import (in this case, `sample_data.csv`), so we're calling the `read_csv()` method in Pandas, passing it the name of the file. It returns the contents of those file in a DataFrame, and we're taking that DataFrame and assigning it to the `df` variable, so if we want to do anything with it, we can just call `df`.

## Looking at the Data

How do we know what the data looks like or even that the data is there in the `df` variable? Well, a good starting point would be to call some of the methods that a DataFrame provides us for investigating the data within it. Let's start with the `.head()` method that shows us the first few row so we can make sure this is a list of companies - not people or sushi orders or someone's favorite surfboards! Run the code below, lets see what happens . . .


```python
df.head()
```

That looks promising! It's pulled in the data, taken the first row as the titles for the columns, and it's showing us the first 5 records. That's the default setting. Guess what happens if you run something like . . . 


```python
df.head(7)
```

Yup - that's right. `head()` takes an optional parameter for the number of records to display and defaults it to 5 if you don't set a value. Note that as with many things in Python this is zero indexed, so the count of the records is from 0-6, not 1-7!

There are *lots* of other things that DataFrames can show you about your data. Once you've looked at `head()`, a good next step is to check out `info()` 


```python
df.info()
```

It's telling us the number of records, the number of columns, the inferred data type for each column and how many non-null entries each of the columns has! That's a pretty good start, but there are many other things we can ask the DataFrame to tell us about the data we loaded.

## Summary Statistics (Numerical Columns)

A good next step is often to check out the summary statistics for the data set. An easy way to start to investigate that is by calling the `describe()` method:


```python
df.describe()
```

It takes any numeric fields in the data sets and provides a range of statistics - from the number of data points to the mean, minimum, maximum and even the standard deviation and quartiles for the values. If you don't happen to know what all of those mean, don't worry - this is a data cleaning class, not a statistics class! **Bottom line, DataFrames will tell you everything you might want to know about a data set - along with a few things you may not even care about!**

If you imported a really large spreadsheet with a lot of numeric columns, you can refine your request by asking (for example) for just the mean or the median, just for one column:


```python
print("The mean is " + str(df["NumberofEmployees"].mean()))
print("The median is " + str(df["NumberofEmployees"].median()))
```

There's a lot going on in the code above, but the important part is that if you just want to get the median for the NumberofEmployees column you can just run `df["NumberofEmployees"].mean()`. Try it in the blank code cell below.

## Summary Statistics (Categorical Columns)

Sometimes you might also want summary statistics for a categorical column. A categorical column is one that contains text, but only certain possible values. For example, if you were filling out a form to describe your conpany, you might have a drop down list for "company type" and "state of incorporation". Those would be categorical columns. An example of a field that is not a categorical column would be the company name or the street address.

Let's have a look at the "EntityType" field. Lets start by getting a list of all the different values and the number of records with each value by using the `value_counts()` method:


```python
df['EntityType'].value_counts()
```

Hmm, that's interesting. It looks like we need to do a little bit of clean up on our data. You can see that Python (as with most programming languages) cares about capitalization, but it means that (for example) it thinks there are three "llc"'s and three "LLC"'s when clearly we'd rather it just tell us that there are six LLC's - irrespective of capitalization.

Now, for a data set this small, we *could* just go in manually and fix all of the data, but if this was a larger data set or even a daily or weekly import, that would become tedious, so let's see if we can fix this. First wee need to figure out how to iterate over the data in a DataFrame, and then we need to take code similar to what we wrote before to fix the capitalization.

## Iterating over a DataFrame

Lets start by figuring out how to iterate over a DataFrame. ***Generally it's a good idea when programming to take very small steps, get them working and then put them together. Usually when you take big steps you end up spending hours falling over your own feet!***

Let's start by just iterating over all the items in the EntityType column and printing them out. Lets try syntax pretty similar to that we used last week for iterating over collections - the `for ... in` syntax:



```python
for i in df.index:
    print(df.at[i, 'EntityType'])
```

That looks pretty good! Now all we need to do is write the code so the cases are consistent. We could write one piece of code for the s corps and another for the c corps, but by capitalizing everything using the `.upper()` method, we can solve all of our data issues in one go.


```python
for i in df.index:
    df.at[i, 'EntityType'] = df.at[i, 'EntityType'].upper()
    print(df.at[i, 'EntityType'])
```

And now if we go back in and re-run the value counts we'll see something much more useful:


```python
df['EntityType'].value_counts()
```

Perfect - now we can easily see the number of S corps, C corps amd LLCs - just as we might want to.

## Using Selectors to Tranform DataFrames

While it's possible (and sometimes useful) to iterate over all of the values for a column in a DataFrame, it's also possible to shortcut the process by using selectors. Let's say I want to take every value in the EntityType column that has a value of "llc" and replace it with "LLC", the following code will do that.


```python
# Lets start by reloading the initial data
df2 = pd.read_csv('sample_data.csv')

df2.loc[df2["EntityType"] == "llc", "EntityType"] = "LLC"
```

Hmm, did that work? Try running the next cell and let's see!


```python
df2['EntityType'].value_counts()
```

That's better! There are lots of ways of applying transformations to information stored within a DataFrame, but hopefully the two examples above have at least given you a starting point. Now the final step is to take our cleaned data, and save it back to a file...


```python
df.to_csv('updated_data.csv')
```

Finally, we just need to check that the export worked by reading the data in and looking at it again . . .


```python
df = pd.read_csv('updated_data.csv')
df.head()
```

Looks like it all worked well!

## Summary

In this lesson we introduced the idea of libraries, the Pandas library in particular, and within that the DataFrame capability for importing, inspecting, transforming and outputting "spreadsheet like" data. It's an incredibly powerful toolkit. Don't worry if you are a little overwhelmed by all the things that it can do. It's a powerful tool and as we progress in the course you'll become increasingly comfortable working with it.
