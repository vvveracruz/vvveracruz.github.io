---
layout: post
title:  "Geocoding in Python"
category: compsci
tags: pandas, python, geopy, jupyter-notebooks, data
summary: "First look into the geopy package for Python."
---
Exported from a Jupyter notebook created in [this course](https://www.udemy.com/course/the-python-mega-course).

---

__Geocoding__ is the process of converting addresses to longitude and latitude coordinates. __Reverse geocoding__ is the inverse operation.

The package `geopy` helps do both these things.

> 🔺  You need an internect connection for this since geopy retrieves the data from the internet.

## Set up


```python
import sys
sys.path.append('/usr/local/lib/python3.9/site-packages')
    # this is because my Jupyter installation doesn't look in this directory for packages & it is were they are kept
```


```python
import os
os.listdir()
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



## Testing geocoding


```python
from geopy.geocoders import ArcGIS
nom = ArcGIS()
```

We can test this out with an address fed in manually, this example is just copy and pasted from the table above. It will return the full address as well as a tuple with the longitude and latitude.


```python
n = nom.geocode( "3666 21st St, San Francisco, CA 94114" )
n
```




    Location(3666 21st St, San Francisco, California, 94114, (37.756648011392286, -122.42937496976432, 0.0))



Printing this address prints only the verbatim address and not the latitude and longitude tuple. To print these they need to be selected:


```python
print(     'This is what is printed when you select n only  :  ', n,
       '\n\nThis is what is printed when specifying n[1]    :  ', n[1] )
```

    This is what is printed when you select n only  :   3666 21st St, San Francisco, California, 94114

    This is what is printed when specifying n[1]    :   (37.756648011392286, -122.42937496976432)


If the address doesn't exist, it will return `None`:


```python
print( nom.geocode( "añlkdsfhlajks" ) )
```

    None


The best way to extract the latitude and longitude is to use the built in methods:


```python
n.longitude        # same can be done for n.latitude
```




    -122.42937496976432



This is because `n` is a special type of object:


```python
type(n)
```




    geopy.location.Location



## Manipulating the dataframe to make it easily accessible to `geopy`

We will be editing the `Address` column that we can then feed straight into the `geocode` method to return a latitude and longitude. At the moment this data is spread out over three columns: `Address`, `City` and `State`.

First, we import the data.


```python
import pandas

data = pandas.read_csv("supermarkets.csv")
data = data.set_index( "ID" )
data
```




<div>

<table >
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
data[ "Address" ] = data[ "Address" ] + ', ' + data[ "City" ] + ', ' + data[ "State" ] + ", " + data[ "Country" ]

# we can now drop all the other columns since they've been integrated
data = data.drop( columns = [ "City", "State", "Country" ] )
data
```




<div>

<table >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
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
      <td>3666 21st St, San Francisco, CA 94114, USA</td>
      <td>Madeira</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St, San Francisco, CA 94119, USA</td>
      <td>Bready Shop</td>
      <td>15</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St, San Francisco, California 94114, USA</td>
      <td>Super River</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St, San Francisco, CA 94114, USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St, San Francisco, California, USA</td>
      <td>Sanchez</td>
      <td>12</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St, San Francisco, CA 94114, USA</td>
      <td>Richvalley</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
</div>



Now we can use `geopy` to find the coordinates for each address, and `pandas` to store them in the dataframe.

The `apply` method (from `pandas`) allows us to apply a method to every row of a dataframe without a for loop.


```python
data[ "Location" ] = data[ "Address" ].apply( nom.geocode )  #  location object

# column for latitude and longitude
data[ "Coordinates" ] = data[ "Location" ].apply( lambda x: (x.latitude, x.longitude) )

data = data.drop( columns = [ "Location" ] )

data
```




<div>

<table >
  <thead>
    <tr >
      <th></th>
      <th>Address</th>
      <th>Name</th>
      <th>Employees</th>
      <th>Coordinates</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>3666 21st St, San Francisco, CA 94114, USA</td>
      <td>Madeira</td>
      <td>8</td>
      <td>(37.756648011392286, -122.42937496976432)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>735 Dolores St, San Francisco, CA 94119, USA</td>
      <td>Bready Shop</td>
      <td>15</td>
      <td>(37.757819005175406, -122.42533698790956)</td>
    </tr>
    <tr>
      <th>3</th>
      <td>332 Hill St, San Francisco, California 94114, USA</td>
      <td>Super River</td>
      <td>25</td>
      <td>(37.755874990371936, -122.42881598064156)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3995 23rd St, San Francisco, CA 94114, USA</td>
      <td>Ben's Shop</td>
      <td>10</td>
      <td>(37.75292200397371, -122.43169700840102)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1056 Sanchez St, San Francisco, California, USA</td>
      <td>Sanchez</td>
      <td>12</td>
      <td>(37.75213100377104, -122.43002800384072)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>551 Alvarado St, San Francisco, CA 94114, USA</td>
      <td>Richvalley</td>
      <td>20</td>
      <td>(37.75351100030984, -122.43322896884439)</td>
    </tr>
  </tbody>
</table>
</div>
