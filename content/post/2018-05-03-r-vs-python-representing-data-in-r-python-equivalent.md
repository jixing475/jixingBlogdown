---
title: 'R vs python'
author: ZERO
date: '2018-05-03'
slug: r-vs-python-representing-data-in-r-python-equivalent
categories:
  - computers
tags:
  - R
  - python
thumbnailImagePosition: left
thumbnailImage: /post/2018-05-03-r-vs-python-representing-data-in-r-python-equivalent_files/曲线身材.jpg
metaAlignment: center
coverMeta: out
---

## <strong style="color: darkred;">Representing Data in R -- Python equivalent</strong> 


```python
import pandas as pd
import numpy as np
```


```python
# 'characters' is equivalent to string
firstName = 'jeff'
print((type(firstName), firstName))
```

    <type 'str'> jeff



```python
# 'numeric' is equivalent to float
heightCM = 188.2
print((type(heightCM), heightCM))
```

    <type 'float'> 188.2



```python
# integer is equivalent to integer
numberSons = 1
print((type(numberSons), numberSons))
```

    <type 'int'> 1



```python
# 'logical' is equivalent to Boolean
teachingCoursera = True
print((type(teachingCoursera), teachingCoursera))
```

    <type 'bool'> True



```python
# 'vectors' is equivalent to numpy array or Python list (I will use array everywhere for consistency)
heights = np.array([188.2, 181.3, 193.4])
print(heights)

firstNames = np.array(['jeff', 'roger', 'andrew', 'brian'])
print(firstNames)
```

    [ 188.2  181.3  193.4]
    ['jeff' 'roger' 'andrew' 'brian']



```python
# 'list' is equivalent to dictionary in Python
vector1 = np.array([188.2, 181.3, 193.4])
vector2 = np.array(['jeff', 'roger', 'andrew', 'brian'])
myList = dict(heights = vector1, firstNames = vector2)
print(myList)

print((myList['heights']))
print((myList['firstNames']))
```

    {'firstNames': array(['jeff', 'roger', 'andrew', 'brian'], 
          dtype='|S6'), 'heights': array([ 188.2,  181.3,  193.4])}
    [ 188.2  181.3  193.4]
    ['jeff' 'roger' 'andrew' 'brian']



```python
# 'matrices' is equivalent to two-dimensional numpy array
myMatrix = np.array([[1, 2], [3, 4]])
print(myMatrix)
```

    [[1 2]
     [3 4]]



```python
# data frame is equivalent to Pandas DataFrame
# this example doesn't work because the input array lengths are not the same
vector1 = np.array([188.2, 181.3, 193.4])
vector2 = np.array(['jeff', 'roger', 'andrew', 'brian'])

# ValueError: arrays must all be same length
# 
myDataFrame = pd.DataFrame(dict(heights = vector1, firstNames = vector2))
```


    ---------------------------------------------------------------------------
    ValueError                                Traceback (most recent call last)

    <ipython-input-10-58e1535d1fac> in <module>()
          6 # ValueError: arrays must all be same length
          7 #
    ----> 8 myDataFrame = pd.DataFrame(dict(heights = vector1, firstNames = vector2))
    

    /opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/pandas/core/frame.pyc in __init__(self, data, index, columns, dtype, copy)
        383             mgr = self._init_mgr(data, index, columns, dtype=dtype, copy=copy)
        384         elif isinstance(data, dict):
    --> 385             mgr = self._init_dict(data, index, columns, dtype=dtype)
        386         elif isinstance(data, ma.MaskedArray):
        387             mask = ma.getmaskarray(data)


    /opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/pandas/core/frame.pyc in _init_dict(self, data, index, columns, dtype)
        515 
        516         return _arrays_to_mgr(arrays, data_names, index, columns,
    --> 517                               dtype=dtype)
        518 
        519     def _init_ndarray(self, values, index, columns, dtype=None,


    /opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/pandas/core/frame.pyc in _arrays_to_mgr(arrays, arr_names, index, columns, dtype)
       5343     # figure out the index, if necessary
       5344     if index is None:
    -> 5345         index = extract_index(arrays)
       5346     else:
       5347         index = _ensure_index(index)


    /opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/pandas/core/frame.pyc in extract_index(data)
       5395             lengths = list(set(raw_lengths))
       5396             if len(lengths) > 1:
    -> 5397                 raise ValueError('arrays must all be same length')
       5398 
       5399             if have_dicts:


    ValueError: arrays must all be same length



```python
# data frame -- fixed
vector1 = np.array([188.2, 181.3, 193.4, 192.3])
vector2 = np.array(['jeff', 'roger', 'andrew', 'brian'])

myDataFrame = pd.DataFrame(dict(heights = vector1, firstNames = vector2))
myDataFrame
```




<div style="max-height:1000px;max-width:1500px;overflow:auto;">
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstNames</th>
      <th>heights</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>   jeff</td>
      <td> 188.2</td>
    </tr>
    <tr>
      <th>1</th>
      <td>  roger</td>
      <td> 181.3</td>
    </tr>
    <tr>
      <th>2</th>
      <td> andrew</td>
      <td> 193.4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>  brian</td>
      <td> 192.3</td>
    </tr>
  </tbody>
</table>
</div>




```python
# factors is equivalent to pandas Categorical
smoker = np.array(['yes', 'no', 'yes', 'yes'])
smokerFactor = pd.Categorical.from_array(smoker)
smokerFactor
```




    Categorical: 
    array(['yes', 'no', 'yes', 'yes'], dtype=object)
    Levels (2): Index(['no', 'yes'], dtype=object)




```python
# R's NA missing values is equivalent to NaN
vector1 = np.array([188.2, 181.3, 193.4, NaN])
print(vector1)
print((isnan(vector1)))
```

    [ 188.2  181.3  193.4    nan]
    [False False False  True]



```python
# subsetting
vector1 = np.array([188.2, 181.3, 193.4, 192.3])
vector2 = np.array(['jeff', 'roger', 'andrew', 'brian'])

myDataFrame = pd.DataFrame(dict(heights = vector1, firstNames = vector2))

print('------------------')
print((vector1[0]))
print('------------------')
print((vector1[[0, 1, 3]]))
print('------------------')
print((myDataFrame.ix[0, 0:2])) # appears transposed as compared to R
print('------------------')
print((myDataFrame['firstNames'])) # there's no 'Levels' as in R
print('------------------')
print((myDataFrame[myDataFrame['firstNames'] == 'jeff']))
print('------------------')
print((myDataFrame[myDataFrame['heights'] < 190]))
```

    ------------------
    188.2
    ------------------
    [ 188.2  181.3  192.3]
    ------------------
    firstNames     jeff
    heights       188.2
    Name: 0
    ------------------
    0      jeff
    1     roger
    2    andrew
    3     brian
    Name: firstNames
    ------------------
      firstNames  heights
    0       jeff    188.2
    ------------------
      firstNames  heights
    0       jeff    188.2
    1      roger    181.3





