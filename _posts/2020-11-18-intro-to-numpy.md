---
layout: post
title:  "Intro to Numpy in Python"
category: compsci
tags: pandas, python, geopy, jupyter-notebooks, data, numpy
summary: "Introduction to the numpy package."
---

Exported from a Jupyter notebook created in [this course](https://www.udemy.com/course/the-python-mega-course).

---

```python
import numpy
```


```python
n = numpy.arange(27)
n
```




    array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16,
           17, 18, 19, 20, 21, 22, 23, 24, 25, 26])



This is kind of like a list but not quite:


```python
type(n)
```




    numpy.ndarray



Can also be printed in a nicer way.


```python
print(n)
```

    [ 0  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
     24 25 26]


## Reshaping

We can change the shape of this array, for example by making it into a __2D array__, for example 3 by 9.

> Note that this doesn't overwrite the original variable `n`.


```python
n.reshape( [3, 9] )
```




    array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8],
           [ 9, 10, 11, 12, 13, 14, 15, 16, 17],
           [18, 19, 20, 21, 22, 23, 24, 25, 26]])



We can also reshape this into a __3D array__.


```python
n.reshape( [3,3,3] )
```




    array([[[ 0,  1,  2],
            [ 3,  4,  5],
            [ 6,  7,  8]],

           [[ 9, 10, 11],
            [12, 13, 14],
            [15, 16, 17]],

           [[18, 19, 20],
            [21, 22, 23],
            [24, 25, 26]]])



You can also convert a preexisting list into a numpy array using the `asarray` method.


```python
l = [[1, 2, 3], [4, 5, 6]]
n = numpy.asarray( l )
print(n)
```

    [[1 2 3]
     [4 5 6]]


## Image manipulation using the `OpenCV` library


```python
import cv2   # this is the openCV library
```

### Creating arrays out of images
`OpenCV` can be used to import an image as an array of the individual values of the pixels.

The `cv2.imread` method takes two parameters:
- the filepath to the image to be opened, and
- an integer `0` or `1`, where `0` means the image is to be interpreted in grayscale and `1` means it is to be interpreted in BGR.


```python
img = cv2.imread( "smallgray.png", 0 )
img
```




    array([[187, 158, 104, 121, 143],
           [198, 125, 255, 255, 147],
           [209, 134, 255,  97, 182]], dtype=uint8)



### Creating images out of arrays
For this we can use the `cv2.imwrite` method.


```python
cv2.imwrite( "newsmallgray.png", img)
    # outputs `True` if the file has been created successfully.
```




    True




```python
import os
os.listdir()
```




    ['smallgray.png',
     'Intro to numpy.ipynb',
     'newsmallgray.png',
     '.ipynb_checkpoints']



There is a new `newsmallgray.png` file in this directory.


```python
os.remove( "newsmallgray.png" ) # deleting the file since it was only there for demonstration purposes
os.listdir()
```




    ['smallgray.png', 'Intro to numpy.ipynb', '.ipynb_checkpoints']



## Indexing, slicing and iterating arrays

Indexing works the same as for lists, except now there are more dimensions.


```python
img[0:2, 2:4]
```




    array([[104, 121],
           [255, 255]], dtype=uint8)




```python
img[2, 4]
```




    182



Iterating also works as expected:


```python
for row in img:
    print( row )
```

    [187 158 104 121 143]
    [198 125 255 255 147]
    [209 134 255  97 182]



```python
for column in img.T: # accessing the transpose
    print( column )
```

    [187 198 209]
    [158 125 134]
    [104 255 255]
    [121 255  97]
    [143 147 182]



```python
for value in img.flat: # accessing individual elements
    print( value )
```

    187
    158
    104
    121
    143
    198
    125
    255
    255
    147
    209
    134
    255
    97
    182


## Stacking and splitting


```python
# horizontal stack
imgimg = numpy.hstack( (img, img) ) # these must be passed as a list or tuple
imgimg
```




    array([[187, 158, 104, 121, 143, 187, 158, 104, 121, 143],
           [198, 125, 255, 255, 147, 198, 125, 255, 255, 147],
           [209, 134, 255,  97, 182, 209, 134, 255,  97, 182]], dtype=uint8)




```python
# vertical stack
iimmgg = numpy.vstack( (img, img) )
print( iimmgg )
```

    [[187 158 104 121 143]
     [198 125 255 255 147]
     [209 134 255  97 182]
     [187 158 104 121 143]
     [198 125 255 255 147]
     [209 134 255  97 182]]


Similarly for splits:


```python
lst = numpy.hsplit( img, 5 )
lst
```




    [array([[187],
            [198],
            [209]], dtype=uint8),
     array([[158],
            [125],
            [134]], dtype=uint8),
     array([[104],
            [255],
            [255]], dtype=uint8),
     array([[121],
            [255],
            [ 97]], dtype=uint8),
     array([[143],
            [147],
            [182]], dtype=uint8)]




```python
lst = numpy.vsplit( img, 3)
lst
```




    [array([[187, 158, 104, 121, 143]], dtype=uint8),
     array([[198, 125, 255, 255, 147]], dtype=uint8),
     array([[209, 134, 255,  97, 182]], dtype=uint8)]
