---
title: Tabletoolz
toc: false
layout: post
description: A paper describing the motivations and functions of the Tabletoolz library.
categories: [markdown, paper, data manipulation, projects]
author: Daniel Lew
---

# Implementing Tabletoolz for SqlAlchemy
## Abstract

Tabletoolz is a python library which provides users with data manipulation approaches used in R’s Dplyr to manipulate tables in SQL and Pyspark. It aims to provide an experience where a user can use R’s Dplyr approach for data manipulation in Python. This paper outlines methods of how this package was implemented accompanied by results of the objective achieved. If a user has used Dplyr, is familiar with its piping capabilities to chain functions together and is looking to work with SQL tables in Python, this is the library to check out\!

## Background

There are many widely used data manipulation packages today in the data science ecosystem. A few names that come up are Dplyr in R and Pandas in Python. However, the style that we are trying to emulate comes from Dplyr. Dplyr is unique in that it has a pipe operator that enables function chaining. On top of that, it builds on its data vocabulary that makes every sequence of action flow nicely. One library in Python that tries to recreate the Dplyr style of data manipulation using the Python Pandas library is dfply. It uses a decorator-based architecture for the piping functionality with the goal of making it concise and easily extensible. With that said, Tabletoolz borrows decorators and pipes from dfply to be used to extend these functionalities to the library. The library already has the foundations of working with SQL tables built.

## Introduction 

Data manipulation is the way in which data can be manipulated and changed. It is the part of the data science workflow that stands between sourcing your data and modelling your data. Without processing your data, your data would not necessarily be in the right form to conduct analysis on. There are many ways in which you can perform data manipulation, one of which is by using the grammar of data manipulation. The grammar of data manipulation provides a consistent set of verbs that solves the most common data manipulation problems, popularized by Hadley Wickham, creator of R’s Tidyverse, a collection of packages designed for data science (Wickham, 2014) In Python, there have been efforts made to reproduce this approach in the likes of dfply for Pandas which this project takes inspiration and borrows from. However, we do not yet have such libraries for SQL and PySpark where we can follow these processes of that in the Tidyverse.

With Tabletoolz, we wanted to improve the tools available in python data management. Our objective was to implement SqlAlchemy into the package by writing functions that work with SQL tables using the grammars for data manipulation (Wickham, 2014). Aside from writing functions, good testing practice was done on each function to ensure that these functions perform as expected. Lastly, documentation was written for each function. On top of the main objectives, some of the additional goals are improving proficiency in software engineering by learning how to write functions that solve data manipulation problems. Additionally, I wanted to gain experience in the development cycle of a software project by learning to compile code into a Python package.

## Methods

To get started, I had to understand what I was working with. The libraries that were available to me were Python and SqlAlchemy. SqlAlchemy is the Python SQL toolkit that gives application developers the full power and flexibility of SQL. As per the SqlAlchemy documentation, “It provides a full suite of well-known enterprise-level persistence patterns, designed for efficient and high-performing database access, adapted into a simple and Pythonic domain language.” Due to the fact that most of the foundational code was already build, my role was to complete the library with functional functions of data manipulation. Below is a figure describing the process.

![](/images/fastpages_posts:2020-06-05-Tabletoolz/image1.png)


It all starts with an idea of what the function does. Take for example the select function. It chooses variables from the table and keeps only the variables chosen. I then check to see if SqlAlchemy already has that implemented. If it does, then a wrapper function goes on top of the pre-existing function. A wrapper function is composed of a decorator like @pipeable (this gives the function capabilities to pipe like R’s Dplyr) and some code tweaks to the function to make it compatible with the library. After the code works, it goes through testing to see if it really works. Testing involves searching for edge cases to break the function. This ensures that the function is robust and accounts for all foreseeable mistakes that a user makes. If everything works as intended, the code can be documented. The docstring describes what the function does. For every function coded, a docstring is written for it. It describes what the functions does, inputs that go into the functions and outputs, alongside types that should be used. If in an event that the code does not work, I go back to the drawing board and draw up new ideas and go through the process again.

## Results

In this section, I will layout the three outcomes achieved in the project. The first “writing functions” is shown with an example of the arrange function from Figure 2. Its usage is to sort a table in either ascending or descending order according to the columns supplied to it. The second objective “writing documentations” (After the def statement in Figure 2) is annotated with “”” strings “”” within its function cell to provide information on how the function work and what the inputs and outputs should be. Additionally, the user is cautioned if there are limits that they should be aware of when using the function.

### Functions

  - Arrange changes the ordering of the rows.
![Arrange function from TableToolz](/images/fastpages_posts:2020-06-05-Tabletoolz/arrange.png)


### Helper functions 

> On top of writing functions, one important aspect I would like to highlight is writing helper functions which provides some useful functionality to complement the main functions within the library. Figure 3 shows examples of helper functions written for the library. The first function intersperse aids the unite function in adding a column separator that unites each column. The following two functions get\_onclause\_col and table\_check are preliminary checks to make sure joins can be performed on tables.

![Helper functions used in TableToolz](/images/fastpages_posts:2020-06-05-Tabletoolz/helper.png)

### Writing tests

The third and final objective is to “write tests”. The methods used for testing are asserting whether the results are the same if one were to do it in pandas (asserting data frames), trying inputs of different data types to break the function and raising exceptions if the data type is not compatible with the function. I implemented several scenarios in Figure 4. The first function, test\_rename, takes the response function and tests it to see whether its output matches that of the operation if done in the same way using the Python Pandas library. The second function, test\_rename\_exception, is to test the outcomes of using different types of inputs to see which inputs are not compatible with the function. The inputs that crash the function may have a different data type that is not supported by the function; therefore, we would have to account for these cases during documentation.

![Sample test cases](/images/fastpages_posts:2020-06-05-Tabletoolz/image4.png)

## Discussion

After going through the results of the project, I will discuss how the package can be used in a setting where it might be used. For example, we are working with a “Car Options” table that contains information about the options that can be modified onto your car. Say we would like to compute the average of each option. First we would have to import the libraries needed which are TableToolz and SqlAlchemy as shown in Figure 6.

![Example part 1](/images/fastpages_posts:2020-06-05-Tabletoolz/image5.png)


Next, Figure 7 shows best practices in Tabletoolz by first turning tables into a statement as SQL works with statements. Additionally, a user can print the statement to see what it looks like and return the results of the table by turning the statement into a pandas dataframe.

![Example part 2](/images/fastpages_posts:2020-06-05-Tabletoolz/image6.png)


Lastly, Figure 8 shows how we would solve a standard data problem in TableToolz. We first define the Mean function then turn the table into a statement, have the variable that we would like to group by and mutate a new variable to compute its average. Then, we can select the columns that we would like to return and finally return its results by turning it into a pandas dataframe.

![Example part 3](/images/fastpages_posts:2020-06-05-Tabletoolz/image7.png)


## Conclusion

In conclusion, I have gone through the goals of the project, methods involved from ideas to coding, documentation and finally testing. All in all, I have written a total of 7 functions for TableToolz Sql which are arrange, drop, rename, unite, left join, inner join, group by. Throughout the course of this project, I have learned a lot about software engineering. The first is going from conceptualizing ideas such as learning about how the functions work and what is it supposed to do then breaking it down into smaller chunks and then translating it into code. I have found that it gave me a better understanding of how it works as a whole. I have also learnt about the importance of testing to make the code more robust and least likely to fail through multiple perspective of test that I have mentioned above. Also the importance of documentations cannot be missed because it provides meaning to the function and how it is used. I wrote documentation for every function in the library. Lastly, on top of the objectives that were achieved, I also learned about structuring directories, cleaning up and organizing files into appropriate folders.

Overall, this capstone project was successful. The objectives met are shown in the results and an example under the discussions section shows a full example of how TableToolz can be incorporated into one’s workflow. In spite of this, there are some limitations to be highlighted, mainly the core SqlAlchemy library where there is not a lot of core data manipulation functions built in compared to libraries like Dplyr. Due to this limitation, a lot of wrangling has to be done in python to find a workaround to implement these functions. However this project can be improved. In the future, refactoring can be done to improve the readability of the code base and more additional functions can be ported into the SQL portion of the library. On top of that, the Pyspark portion remains a work in progress therefore some help could go there.

## Acknowledgements

I would like to thank Dr Iverson for giving me an opportunity to contribute to this project.

## References

Wickham, Hadley & François, Romain. (2014). dplyr: A Grammar of Data Manipulation.

Dfply <https://github.com/kieferk/dfply>

SqlAlchemy <https://docs.sqlalchemy.org/en/13/core/>

Python <https://www.python.org/>

## Appendix

### Functions

  - Drop, drops columns from the data frame.
![Drop function](/images/fastpages_posts:2020-06-05-Tabletoolz/drop.png)

  - Group-by allows operations to be performed by “group”
![Group-by function](/images/fastpages_posts:2020-06-05-Tabletoolz/group_by.png)

  - Rename, changes the name of a column.
![Rename_function](/images/fastpages_posts:2020-06-05-Tabletoolz/rename.png)

  - Unite, concatenates columns together.
![Unite function](/images/fastpages_posts:2020-06-05-Tabletoolz/unite.png)

  - Inner join joins tables using the inner join method.
![Inner join function](/images/fastpages_posts:2020-06-05-Tabletoolz/inner.png)

  - Left join joins tables using the left join method.
  ![Left join function](/images/fastpages_posts:2020-06-05-Tabletoolz/left.png)

### Testing

![Test functions](/images/fastpages_posts:2020-06-05-Tabletoolz/test1.png)

![Test functions](/images/fastpages_posts:2020-06-05-Tabletoolz/test2.png)
