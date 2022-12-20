---
layout: page
title: Chronicling America Assignment
description: Learnign to use Chronicling America API
---

# Chronicling America API Assignment
In this assignment, you are tasked with:
* searching Chronicling America's API for a key word of your choice
* parsing your results from a dictionary to a `DataFrame` with headings "title", "city", 'date", and "raw_text"
* processing the raw text by removing "\n" characters, stopwords, and then lemmatizing the raw text before adding it to a new column called "lemmas."
* saving your `DataFrame` to a csv file




```python
# imports
import requests
import json
import math
import pandas as pd
import spacy
```

    /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/tqdm/auto.py:22: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
      from .autonotebook import tqdm as notebook_tqdm



```python
# initial search
url = 'https://chroniclingamerica.loc.gov/search/pages/results/?state=&date1=1777&date2=1963&proxtext=judaism&x=0&y=0&dateFilterType=yearRange&rows=20&searchType=basic&format=json'
response = requests.get(url)  
raw = response.text 
results = json.loads(raw)  
```


```python
# find total amount of pages
total_pages = math.ceil(results['totalItems'] / results['itemsPerPage'])
print(total_pages)
```

    1206



```python
# query the api and save to dict 
data = []
start_date = '1777'
end_date = '1963'
search_term = 'judaism'
for i in range(1, 11):  # for sake of time I'm doing only 10, you will want to put total_pages+1
    url = (f'https://chroniclingamerica.loc.gov/search/pages/results/?state=&date1={start_date}'
           f'&date2={end_date}&proxtext={search_term}&x=16&y=8&dateFilterType=yearRange&rows=20'
           f'&searchType=basic&format=json&page={i}')  # f-string
    response = requests.get(url)
    raw = response.text
    print(response.status_code)  # checking for errors
    results = json.loads(raw)
    items_ = results['items']
    for item_ in items_:
        temp_dict = {}
        temp_dict['title'] = item_['title_normal']
        temp_dict['city'] = item_['city']
        temp_dict['date'] = item_['date']
        temp_dict['raw_text'] = item_['ocr_eng']
        data.append(temp_dict)
```

    200
    200
    200
    200
    200
    200
    200
    200
    200
    200



```python
# convert dict to dataframe
df = pd.DataFrame.from_dict(data)
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>city</th>
      <th>date</th>
      <th>raw_text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>jewish outlook.</td>
      <td>[Denver]</td>
      <td>19040805</td>
      <td>The JEWISH OUTLOOK\nA 'Weekly Journal Devoted ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>american jewish world.</td>
      <td>[Minneapolis, Saint Paul]</td>
      <td>19170413</td>
      <td>the gate of the ghetto, but is wear\ning away ...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>jewish outlook.</td>
      <td>[Denver]</td>
      <td>19040805</td>
      <td>2\nof Judaism and religion. That atheism\nand ...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>jewish herald.</td>
      <td>[Houston]</td>
      <td>19100317</td>
      <td>DR WISE IN LONDON\nDr Stephen S Wise rabbi of ...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>jewish outlook.</td>
      <td>[Denver]</td>
      <td>19051124</td>
      <td>26\nContrasting th§ Babylonian code and Is\nra...</td>
    </tr>
  </tbody>
</table>
</div>




```python
# convert date column from string to date-time object
df['date'] = pd.to_datetime(df['date'])
```


```python
# sort by date
sorted_df = df.sort_values(by='date')
```


```python
# write fuction to process text
nlp = spacy.load("en_core_web_sm")
nlp.disable_pipes('ner', 'parser')  # these are unnecessary for the task at hand

#junk_words = ['hesse']  # you can add any words you want removed here

def process_text(text):
    """Remove new line characters and lemmatize text. Returns string of lemmas"""
    text = text.replace('\n', ' ')
    doc = nlp(text)
    tokens = [token for token in doc]
    no_stops = [token for token in tokens if not token.is_stop]
    no_punct = [token for token in no_stops if token.is_alpha]
    lemmas = [token.lemma_ for token in no_punct]
    lemmas_lower = [lemma.lower() for lemma in lemmas]
    lemmas_string = ' '.join(lemmas_lower)
    return lemmas_string
```


```python
# apply process_text function to raw_text column
sorted_df['lemmas'] = sorted_df['raw_text'].apply(process_text)
```


```python
# save to csv
sorted_df.to_csv(f'{search_term}{start_date}-{end_date}.csv', index=False)
```
