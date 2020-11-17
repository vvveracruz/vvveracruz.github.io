---
layout: post
title:  "Trying out Jupyter Notebooks and Pandas"
category: compsci
tags: pandas, python, jupyter-notebooks, data
summary: "A first look into the pandas package and local hosting a jupyter notebook."
---

Exported from a Jupyter notebook created in [this course](https://www.udemy.com/course/the-python-mega-course).

---

First we generate a list of all the files in our directory to make it easier to access them:

```python
import os
os.listdir()
```
```
['supermarkets.json',
 'supermarkets.csv',
 'data.csv',
 'How to jupyter & pandas.ipynb',
 'supermarkets-semi-colons.txt',
 'supermarkets.xlsx',
 '.ipynb_checkpoints',
 'Geocoding addresses.ipynb',
 'supermarkets-commas.txt']
```


Now we can import pandas and start reading files:


```python
import sys
sys.path.append('/usr/local/lib/python3.9/site-packages')
    # this is because jupyter struggles to find my installation of pandas
import pandas
```

## Reading files using the pandas library

- Use `read_csv` to read `.csv` and  `.txt` files. You can edit the separator using the `sep` variable.
- Use `read_json`to read `.json` files.
- Use `read_excel`to read `.xlsx` files. It is important to state what sheet to read from using the `sheet_name` variable.


```python
df1 = pandas.read_csv("supermarkets.csv")
df1
```

|    |  ID  |  Address  |  City  |  State  |  Country  |  Name  |  Employees  |
|--|----|-----------|--------|---------|-----------|--------|-------------|
|  0 |  1 |  3666 21st St  |  San Francisco  |  CA 94114  |  USA  | Madeira   |  8  |
|  1 |  2 |  735 Dolores St  |  San Francisco  |  CA 94119  |  USA  | Bready Shop   |  15  |
|  2 |  3 |  332 Hill St  | San Francisco   |  California 94114  |  USA  | Super River   |  25  |
|  3 |  4 |  3995 23rd St  |  San Francisco  |  CA 94114  |  USA  |  Ben's Shop  |  10  |
|  4 |  5 |  1056 Sanchez St |  San Francisco  |  California  |  USA  |  Sanchez  |   12 |
|  5 |  6 |  551 Alvarado St  |  San Francisco  |  CA 94114  |  USA  |  Richvalley  | 20   |



```python
df2 = pandas.read_json("supermarkets.json")
df2
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>ID</th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>




```python
df4 = pandas.read_csv("supermarkets-semi-colons.txt", sep=';')
df4
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>ID</th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



> 🔺  If the data has no headers, it is important to pass this to the reader function. Otherwise the first row will be assigned as headers.


```python
df5 = pandas.read_csv("data.csv", header = None)
df5
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



## Setting column names and indexes
- Use `dataframe.columns` to assign the column names.
- Use `dataframe.set_index( "column_name" )` to set the index column.
  - This does not overwrite the dataframe, but creates a new one with the selected index.
  - This is because this operation is destructive: it drops the previous index column.


```python
df5.columns =[ "ID", "Address", "City", "State", "Country", "Name", "Employees" ]
df5
```




<div>
<table>
  <thead>
    <tr >
      <th></th>
      <th>ID</th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>




```python
df6 = df5.set_index( "ID" )
df6
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



## Selecting sections of the data
### Labels based selection: `loc`
Use `dataframe.loc[ row_range, column_range]` to select a portion of the data using column and row names.


```python
df6.loc["2":"4", "State":"Name"]
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
    </tr>
    <tr>
      <th>3</th>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
    </tr>
  </tbody>
</table>
</div>



You can also select single columns, rows or intersections between these. It is then useful to convert the output to a list.


```python
df6.loc[:, "State"]
```




    ID
    1            CA 94114
    2            CA 94119
    3    California 94114
    4            CA 94114
    5          California
    6            CA 94114
    Name: State, dtype: object




```python
list( df6.loc[:, "State"])
```




    ['CA 94114',
     'CA 94119',
     'California 94114',
     'CA 94114',
     'California',
     'CA 94114']



### Indexing based selection: `iloc`
Instead of passing the names of the columns or rows (`labels`) you pass the index number. Printing the database for reference:


```python
df6
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>




```python
df6.iloc[0:3, 3:6] # this is upper bound exclusive like lists
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>



## Deleting columns and rows
Use the `dataframe.drop` method.
This is also not an inplace operation.

To delete columns: `drop( label, 1)`. To delete rows: `drop( label, 0 )`.


```python
df6.drop( "City", 1)
```




<div>
<table>
  <thead>
    <tr>
      <th></th>
      <th>Address</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



The `drop` method can't be used directly with indexes, but there is a trick:


```python
df6.drop( df6.columns[1:2], 1) # for columns only
```




<div>

<table  >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>




```python
df6.drop( df6.index[0:3], 0) # for rows only
```




<div>

<table  >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



### What are the `index` and `columns` commands?


```python
print( list(df6.columns), '\n Type:', type(df6.columns))
```

    ['Address', 'City', 'State', 'Country', 'Name', 'Employees']
     Type: <class 'pandas.core.indexes.base.Index'>



```python
print( list(df6.index), '\n Type:', type(df6.index))
```

    [1, 2, 3, 4, 5, 6]
     Type: <class 'pandas.core.indexes.numeric.Int64Index'>


## Adding data to an existing dataframe

Using lambda functions:


```python
df6["Continent"]=[ "North America" for i in range( len(df6.index) ) ]
    #  this makes sure the new column is the same length as the existing columns
df6
```




<div>

<table  >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
      <th>Continent</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
      <td>North America</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
      <td>North America</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
      <td>North America</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
      <td>North America</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
      <td>North America</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
      <td>North America</td>
    </tr>
  </tbody>
</table>
</div>



But you can also use the `shape` command. This returns the "shape" of the dataframe; the number of rows and columns.


```python
df6.shape
```




    (6, 7)




```python
df6[ "Type" ] = df6.shape[0] * [ "Supermarket" ] # selecting the no of rows
df6
```




<div>

<table  >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
      <th>Continent</th>
      <th>Type</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
      <td>North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
      <td>North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
      <td>North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
      <td>North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
      <td>North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
      <td>North America</td>
      <td>Supermarket</td>
    </tr>
  </tbody>
</table>
</div>



This is a standard way to generate repeating lists, for example:


```python
7 * ["Welcome to the Jungle"]
```




    ['Welcome to the Jungle',
     'Welcome to the Jungle',
     'Welcome to the Jungle',
     'Welcome to the Jungle',
     'Welcome to the Jungle',
     'Welcome to the Jungle',
     'Welcome to the Jungle']




```python
3 * ['a', 'e', 'i', 'o', 'u']
```




    ['a', 'e', 'i', 'o', 'u', 'a', 'e', 'i', 'o', 'u', 'a', 'e', 'i', 'o', 'u']



### Modifying a column


```python
df6[ "Continent" ] = df6[ "Country" ]+', '+ 'North America'
df6
```




<div>

<table  >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
      <th>Continent</th>
      <th>Type</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
  </tbody>
</table>
</div>



### Adding a row
There is no direct way to do it so we first need to transpose the database, add a column, and transpose it again 😬

The `T` method is used for transposing.


```python
df6.T # transpose the database
```




<div>

<table  >
  <thead>
    <tr >
      <th>ID</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Address</th>
      <td>3666 21st St</td>
      <td>735 Dolores St</td>
      <td>332 Hill St</td>
      <td>3995 23rd St</td>
      <td>1056 Sanchez St</td>
      <td>551 Alvarado St</td>
    </tr>
    <tr>
      <th>City</th>
      <td>San Francisco</td>
      <td>San Francisco</td>
      <td>San Francisco</td>
      <td>San Francisco</td>
      <td>San Francisco</td>
      <td>San Francisco</td>
    </tr>
    <tr>
      <th>State</th>
      <td>CA 94114</td>
      <td>CA 94119</td>
      <td>California 94114</td>
      <td>CA 94114</td>
      <td>California</td>
      <td>CA 94114</td>
    </tr>
    <tr>
      <th>Country</th>
      <td>USA</td>
      <td>USA</td>
      <td>USA</td>
      <td>USA</td>
      <td>USA</td>
      <td>USA</td>
    </tr>
    <tr>
      <th>Name</th>
      <td>Madeira</td>
      <td>Bready Shop</td>
      <td>Super River</td>
      <td>Ben's Shop</td>
      <td>Sanchez</td>
      <td>Richvalley</td>
    </tr>
    <tr>
      <th>Employees</th>
      <td>8</td>
      <td>15</td>
      <td>25</td>
      <td>10</td>
      <td>12</td>
      <td>20</td>
    </tr>
    <tr>
      <th>Continent</th>
      <td>USA, North America</td>
      <td>USA, North America</td>
      <td>USA, North America</td>
      <td>USA, North America</td>
      <td>USA, North America</td>
      <td>USA, North America</td>
    </tr>
    <tr>
      <th>Type</th>
      <td>Supermarket</td>
      <td>Supermarket</td>
      <td>Supermarket</td>
      <td>Supermarket</td>
      <td>Supermarket</td>
      <td>Supermarket</td>
    </tr>
  </tbody>
</table>
</div>




```python
df6_transposed = df6.T

# defining variables for each row (column) to make this more transparent
address = "892 Hill St"
city = "New York"
state = "NY"
country = "USA"
name = "Ve's"
employees = 3
continent = "USA, North America"
type_ = "Supermarket"

# new column
df6_transposed[ "7" ] = [ address, city, state, country, name, employees, continent, type_ ]

#transposing back
df6 = df6_transposed.T
df6
```




<div>

<table  >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
      <th>City</th>
      <th>State</th>
      <th>Country</th>
      <th>Name</th>
      <th>Employees</th>
      <th>Continent</th>
      <th>Type</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Madeira</td>
      <td>8</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St</td>
      <td>San Francisco</td>
      <td>CA 94119</td>
      <td>USA</td>
      <td>Bready Shop</td>
      <td>15</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St</td>
      <td>San Francisco</td>
      <td>California 94114</td>
      <td>USA</td>
      <td>Super River</td>
      <td>25</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St</td>
      <td>San Francisco</td>
      <td>California</td>
      <td>USA</td>
      <td>Sanchez</td>
      <td>12</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St</td>
      <td>San Francisco</td>
      <td>CA 94114</td>
      <td>USA</td>
      <td>Richvalley</td>
      <td>20</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
    <tr>
      <th>7</th>
      <td>892 Hill St</td>
      <td>New York</td>
      <td>NY</td>
      <td>USA</td>
      <td>Ve's</td>
      <td>3</td>
      <td>USA, North America</td>
      <td>Supermarket</td>
    </tr>
  </tbody>
</table>
</div>
