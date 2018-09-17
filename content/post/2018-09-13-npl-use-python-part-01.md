---
title: NLP use python part 01
author: ZERO
date: '2018-09-13'
slug: npl-use-python-part-01
categories:
  - datavis and dataclean
tags:
  - NLP
keywords:
  - tech
thumbnailImagePosition: left
thumbnailImage: https://i.loli.net/2018/09/13/5b99c16ff0094.jpg
metaAlignment: center
coverMeta: out
---

<!--more-->

# Working With Text


```python
text1 = "Ethics are built right into the ideals and objectives of the United Nations "

len(text1) # The length of text1
```




    76




```python
text2 = text1.split(' ')  # Return a list of the words in text2, separating by ' '.

len(text2)
```




    14




```python
text2
```




    ['Ethics',
     'are',
     'built',
     'right',
     'into',
     'the',
     'ideals',
     'and',
     'objectives',
     'of',
     'the',
     'United',
     'Nations',
     '']



<br>
在列表中找到特定的单词：通过增加条件判断


```python
[w 
 for w in text2 if len(w) > 3] # Words that are greater than 3 letters long in text2
```




    ['Ethics',
     'built',
     'right',
     'into',
     'ideals',
     'objectives',
     'United',
     'Nations']




```python
[w for w in text2 if w.istitle()] # Capitalized words in text2
```




    ['Ethics', 'United', 'Nations']




```python
[w for w in text2 if w.endswith('s')] # Words in text2 that end in 's'
```




    ['Ethics', 'ideals', 'objectives', 'Nations']



<br>
## Find unique words using `set()`.


```python
text3 = 'To be or not to be'
text4 = text3.split(' ')

len(text4)
```




    6




```python
len(set(text4))
```




    5




```python
set(text4)
```




    {'To', 'be', 'not', 'or', 'to'}




```python
len(set([w.lower() for w in text4])) # .lower converts the string to lowercase.
```




    4




```python
set([w.lower() for w in text4])
```




    {'be', 'not', 'or', 'to'}



## Processing free-text


```python
text5 = '"Ethics are built right into the ideals and objectives of the United Nations" \
#UNSG @ NY Society for Ethical Culture bit.ly/2guVelr'
text6 = text5.split(' ')

text6
```




    ['"Ethics',
     'are',
     'built',
     'right',
     'into',
     'the',
     'ideals',
     'and',
     'objectives',
     'of',
     'the',
     'United',
     'Nations"',
     '#UNSG',
     '@',
     'NY',
     'Society',
     'for',
     'Ethical',
     'Culture',
     'bit.ly/2guVelr']



<br>
### Finding hastags:


```python
[w for w in text6 
     if w.startswith('#')]
```




    ['#UNSG']



<br>
### Finding callouts:


```python
[w for w in text6 if w.startswith('@')]
```




    ['@']




```python
text7 = '@UN @UN_Women "Ethics are built right into the ideals and objectives of the United Nations" \
#UNSG @ NY Society for Ethical Culture bit.ly/2guVelr'
text8 = text7.split(' ')
```

<br>

### Use regular expressions to help us with more complex parsing. 

For example `'@[A-Za-z0-9_]+'` will return all words that: 
* start with `'@'` and are followed by at least one: 
* capital letter (`'A-Z'`)
* lowercase letter (`'a-z'`) 
* number (`'0-9'`)
* or underscore (`'_'`)


```python
import re # import re - a module that provides support for regular expressions

[w for w in text8 if re.search('@[A-Za-z0-9_]+', w)]
```




    ['@UN', '@UN_Women']



## Regex with pandas


```python
import pandas as pd

time_sentences = ["Monday: The doctor's appointment is at 2:45pm.", 
                  "Tuesday: The dentist's appointment is at 11:30 am.",
                  "Wednesday: At 7:00pm, there is a basketball game!",
                  "Thursday: Be back home by 11:15 pm at the latest.",
                  "Friday: Take the train at 08:10 am, arrive at 09:00am."]

df = pd.DataFrame(time_sentences, columns=['text'])
df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Monday: The doctor's appointment is at 2:45pm.</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Tuesday: The dentist's appointment is at 11:30...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wednesday: At 7:00pm, there is a basketball game!</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Thursday: Be back home by 11:15 pm at the latest.</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Friday: Take the train at 08:10 am, arrive at ...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# find the number of characters for each string in df['text']
df['text']
```




    0       Monday: The doctor's appointment is at 2:45pm.
    1    Tuesday: The dentist's appointment is at 11:30...
    2    Wednesday: At 7:00pm, there is a basketball game!
    3    Thursday: Be back home by 11:15 pm at the latest.
    4    Friday: Take the train at 08:10 am, arrive at ...
    Name: text, dtype: object




```python
# find the number of characters for each string in df['text']
df['text'].str
```




    <pandas.core.strings.StringMethods at 0x11a34eb38>




```python
print(type(df['text'])) 
```

    <class 'pandas.core.series.Series'>



```python
df['text'].str.len()
```




    0    46
    1    50
    2    49
    3    49
    4    54
    Name: text, dtype: int64




```python
# find the number of tokens for each string in df['text']
df['text'].str.split()
```




    0    [Monday:, The, doctor's, appointment, is, at, ...
    1    [Tuesday:, The, dentist's, appointment, is, at...
    2    [Wednesday:, At, 7:00pm,, there, is, a, basket...
    3    [Thursday:, Be, back, home, by, 11:15, pm, at,...
    4    [Friday:, Take, the, train, at, 08:10, am,, ar...
    Name: text, dtype: object




```python
# find the number of tokens for each string in df['text']
#str.split each to a list
df['text'].str.split().str.len()
```




    0     7
    1     8
    2     8
    3    10
    4    10
    Name: text, dtype: int64



#### contain


```python
# find which entries contain the word 'appointment'
df['text'].str.contains('appointment')
```




    0     True
    1     True
    2    False
    3    False
    4    False
    Name: text, dtype: bool



#### count match occurs times


```python
# find how many times a digit occurs in each string
df['text'].str.count(r'[0-9]')
```




    0    3
    1    4
    2    3
    3    4
    4    8
    Name: text, dtype: int64



#### regex ?: matches at most 1 times, 防止贪婪 0次或者1次


```python
# group and find the hours and minutes
df['text'].str.findall(r'(\d?\d):(\d\d)')
```




    0               [(2, 45)]
    1              [(11, 30)]
    2               [(7, 00)]
    3              [(11, 15)]
    4    [(08, 10), (09, 00)]
    Name: text, dtype: object




```python
# replace weekdays with '???'
#\w is equal the [A-z0-9_]
# \b determine the edge of word
df['text'].str.replace(r'\w+day\b', '???')
df['text'].str.replace(r'[A-z0-9_]+day\b', '???')
```




    0          ???: The doctor's appointment is at 2:45pm.
    1       ???: The dentist's appointment is at 11:30 am.
    2          ???: At 7:00pm, there is a basketball game!
    3         ???: Be back home by 11:15 pm at the latest.
    4    ???: Take the train at 08:10 am, arrive at 09:...
    Name: text, dtype: object




```python
# create new columns from first match of extracted groups
df['text'].str.extract(r'(\d?\d):(\d\d)')
```

    /Users/zero/anaconda3/lib/python3.6/site-packages/ipykernel/__main__.py:2: FutureWarning: currently extract(expand=None) means expand=False (return Index/Series/DataFrame) but in a future version of pandas this will be changed to expand=True (return DataFrame)
      from ipykernel import kernelapp as app





<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>45</td>
    </tr>
    <tr>
      <th>1</th>
      <td>11</td>
      <td>30</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7</td>
      <td>00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>11</td>
      <td>15</td>
    </tr>
    <tr>
      <th>4</th>
      <td>08</td>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>




```python
# extract the entire time, the hours, the minutes, and the period
df['text'].str.extractall(r'((\d?\d):(\d\d) ?([ap]m))')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
    </tr>
    <tr>
      <th></th>
      <th>match</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <th>0</th>
      <td>2:45pm</td>
      <td>2</td>
      <td>45</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>11:30 am</td>
      <td>11</td>
      <td>30</td>
      <td>am</td>
    </tr>
    <tr>
      <th>2</th>
      <th>0</th>
      <td>7:00pm</td>
      <td>7</td>
      <td>00</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>3</th>
      <th>0</th>
      <td>11:15 pm</td>
      <td>11</td>
      <td>15</td>
      <td>pm</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">4</th>
      <th>0</th>
      <td>08:10 am</td>
      <td>08</td>
      <td>10</td>
      <td>am</td>
    </tr>
    <tr>
      <th>1</th>
      <td>09:00am</td>
      <td>09</td>
      <td>00</td>
      <td>am</td>
    </tr>
  </tbody>
</table>
</div>




```python
# extract the entire time, the hours, the minutes, and the period with group names
df['text'].str.extractall(r'(?P<time>(?P<hour>\d?\d):(?P<minute>\d\d) ?(?P<period>[ap]m))')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>time</th>
      <th>hour</th>
      <th>minute</th>
      <th>period</th>
    </tr>
    <tr>
      <th></th>
      <th>match</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <th>0</th>
      <td>2:45pm</td>
      <td>2</td>
      <td>45</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>1</th>
      <th>0</th>
      <td>11:30 am</td>
      <td>11</td>
      <td>30</td>
      <td>am</td>
    </tr>
    <tr>
      <th>2</th>
      <th>0</th>
      <td>7:00pm</td>
      <td>7</td>
      <td>00</td>
      <td>pm</td>
    </tr>
    <tr>
      <th>3</th>
      <th>0</th>
      <td>11:15 pm</td>
      <td>11</td>
      <td>15</td>
      <td>pm</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">4</th>
      <th>0</th>
      <td>08:10 am</td>
      <td>08</td>
      <td>10</td>
      <td>am</td>
    </tr>
    <tr>
      <th>1</th>
      <td>09:00am</td>
      <td>09</td>
      <td>00</td>
      <td>am</td>
    </tr>
  </tbody
</table>
</div>

