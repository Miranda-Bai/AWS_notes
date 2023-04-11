[:, :] literally means [all rows, all columns].

Indexing in python starts from 0 when you go from the first element to the last, but it starts from -1 when you start from the last element.

So, when you do [:, -1] it means you are taking all the rows and only the last column. -1 represents the last column.

When you do [:, :-1], it means you are taking all the rows and all the columns except the last column.

Now, when you do training_data[:, -1] it means from the dataframe training_date, you are using all the rows and only the last column. Similarly training_data[:, :-1] means all the rows and all the columns except the last column.

But:

You might run into a slicing problem if you do training_data[:, -1]. Since you are using integers to slice the df, it is always better to use the .iloc method.

This tutorial How do I select multiple rows and columns from a pandas DataFrame? explains everything clearly. Have a look at it.

example: example
https://i.stack.imgur.com/UHS8X.png
