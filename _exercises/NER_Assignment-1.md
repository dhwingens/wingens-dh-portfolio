---
layout: page
title: NER Assignment
description: Extracting place names from Gibbon ch.59
---

# NER Assignment
Georeferencing automatically detected place names in Edward Gibbon's *Decline and Fall of the Roman Empire* using the Pleiades gazatteer and `spaCy`.

Your assignment this week is to output a `CSV` file of place names, frequency and coordinates, as we did in class for a chapter of Gibbon of your choice. Try to find a chapter with a lot of places as we will be turning this data into an online map next week. The steps are:

* Choose a chapter from the text
* Begin a function by parsing input text
* Create a `spaCy` dictionary of entities and frequency
* Use the Pleiaes data from class to find coordinates for each place name
* Run your function on your chosen chapter
* Save your final `CSV`
* Come to class on Monday, ready to use it


```python
import pandas as pd
import spacy
nlp = spacy.load('en_core_web_sm')
```

    /Library/Frameworks/Python.framework/Versions/3.10/lib/python3.10/site-packages/tqdm/auto.py:22: TqdmWarning: IProgress not found. Please update jupyter and ipywidgets. See https://ipywidgets.readthedocs.io/en/stable/user_install.html
      from .autonotebook import tqdm as notebook_tqdm



```python
import wget
import os
if not os.path.isfile('gibbon_text.csv'):
    wget.download('https://raw.githubusercontent.com/pnadelofficial/FallDHCourseMaterials/main/gibbon_text.csv')
```


```python
# import and download any relevant data
gibbon_by_chapter = pd.read_csv('gibbon_text.csv').rename(columns={'Unnamed: 0':'chapter'})
gibbon_by_chapter
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
      <th>chapter</th>
      <th>StringText</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Chapter 1</td>
      <td>\nIn the second century of the Christian era, ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Chapter 2</td>
      <td>\nIt is not alone by the rapidity or extent of...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Chapter 3</td>
      <td>\nThe obvious definition of a monarchy seems t...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Chapter 4</td>
      <td>\nThe mildness of Marcus, which the rigid disc...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Chapter 5</td>
      <td>\nThe power of the sword is more sensibly felt...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Chapter 67</td>
      <td>\nThe respective merits of Rome and Constantin...</td>
    </tr>
    <tr>
      <th>67</th>
      <td>Chapter 68</td>
      <td>\nThe siege of Constantinople by the Turks att...</td>
    </tr>
    <tr>
      <th>68</th>
      <td>Chapter 69</td>
      <td>\nIn the first ages of the decline and fall of...</td>
    </tr>
    <tr>
      <th>69</th>
      <td>Chapter 70</td>
      <td>\nIn the apprehension of modern times, Petrarc...</td>
    </tr>
    <tr>
      <th>70</th>
      <td>Chapter 71</td>
      <td>\nIn the last days of Pope Eugenius the Fourth...</td>
    </tr>
  </tbody>
</table>
<p>71 rows × 2 columns</p>
</div>




```python
chapter = gibbon_by_chapter['StringText'][58] #ch59
```


```python
#Pleides data for later
if not os.path.isfile('places.csv'):
    wget.download('https://raw.githubusercontent.com/pnadelofficial/FallDHCourseMaterials/main/places.csv')
if not os.path.isfile('names.csv'):
    wget.download('https://raw.githubusercontent.com/pnadelofficial/FallDHCourseMaterials/main/names.csv')
names = pd.read_csv('names.csv')
```


```python
import collections

def get_places_df(chapter):
    chapter_doc = nlp(chapter)

    place_freq = collections.defaultdict(int)
    for entity in chapter_doc.ents:
        if (entity.label_ == 'GPE') or (entity.label_ == 'LOC'):
            place_freq[entity.text] += 1 # the utility of defaultdict!
    place_freq = dict(place_freq)

    return pd.DataFrame.from_dict(place_freq, orient='index').reset_index().rename(columns={'index':'place_name',0:'frequency'})

place_freq_df = get_places_df(chapter)
```


```python
place_freq_df['pleiades_id'] = place_freq_df['place_name'].apply(get_pleiades_id)
place_freq_df
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
      <th>place_name</th>
      <th>frequency</th>
      <th>pleiades_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alexius</td>
      <td>4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nice</td>
      <td>3</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Asia</td>
      <td>10</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Smyrna</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sardes</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>106</th>
      <td>St. John</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>107</th>
      <td>Ptolemais</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>108</th>
      <td>Acre</td>
      <td>1</td>
      <td>678010.0</td>
    </tr>
    <tr>
      <th>109</th>
      <td>Cyprus</td>
      <td>1</td>
      <td>707498.0</td>
    </tr>
    <tr>
      <th>110</th>
      <td>Khalil</td>
      <td>1</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>111 rows × 3 columns</p>
</div>




```python
places = pd.read_csv('places.csv')
places
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
      <th>created</th>
      <th>description</th>
      <th>details</th>
      <th>provenance</th>
      <th>title</th>
      <th>uri</th>
      <th>id</th>
      <th>representative_latitude</th>
      <th>representative_longitude</th>
      <th>bounding_box_wkt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010-06-24T14:11:06Z</td>
      <td>The ancient region known to the Romans as "Gal...</td>
      <td>&lt;p&gt;The Barrington Atlas Directory notes: FRA&lt;/p&gt;</td>
      <td>Barrington Atlas: BAtlas 1 D1 Gallia</td>
      <td>Gallia</td>
      <td>https://pleiades.stoa.org/places/993</td>
      <td>993</td>
      <td>46.705437</td>
      <td>1.013706</td>
      <td>POLYGON ((8.672222 42.4395125, 8.672222 51.981...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-01-10T20:52:00Z</td>
      <td>A Roman house in Pompeii (I, 6, 15), also know...</td>
      <td>&lt;p&gt;The house was excavated in 1913 and 1914. T...</td>
      <td>Pleiades</td>
      <td>House of the Ceii</td>
      <td>https://pleiades.stoa.org/places/999909607</td>
      <td>999909607</td>
      <td>40.750010</td>
      <td>14.489506</td>
      <td>POLYGON ((14.4895058 40.7500099, 14.4895058 40...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2015-05-28T11:48:45Z</td>
      <td>The so-called "House of the Lararium of Achill...</td>
      <td>NaN</td>
      <td>Pleiades</td>
      <td>House of the Lararium of Achilles</td>
      <td>https://pleiades.stoa.org/places/999909608</td>
      <td>999909608</td>
      <td>40.750362</td>
      <td>14.489286</td>
      <td>POLYGON ((14.489286 40.750362, 14.489286 40.75...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-03-20T15:26:53Z</td>
      <td>A necropolis with inhumations dating to the fi...</td>
      <td>&lt;p&gt;A necropolis located close to Monte Bibele ...</td>
      <td>Pleiades</td>
      <td>Monte Tamburino necropolis</td>
      <td>https://pleiades.stoa.org/places/999917206</td>
      <td>999917206</td>
      <td>44.272322</td>
      <td>11.375880</td>
      <td>POLYGON ((11.3758803 44.2723222, 11.3758803 44...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2018-05-30T03:08:22Z</td>
      <td>The megalithic defensive circuit of Rusellae d...</td>
      <td>NaN</td>
      <td>Pleiades</td>
      <td>Circuit wall of Rusellae</td>
      <td>https://pleiades.stoa.org/places/999951524</td>
      <td>999951524</td>
      <td>42.829089</td>
      <td>11.163588</td>
      <td>POLYGON ((11.1635884 42.8290888, 11.1635884 42...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>38948</th>
      <td>2013-08-18T17:03:48Z</td>
      <td>The modern Acquapendente was settled in pre-Ro...</td>
      <td>NaN</td>
      <td>Pleiades</td>
      <td>Acquapendente</td>
      <td>https://pleiades.stoa.org/places/555723846</td>
      <td>555723846</td>
      <td>42.743996</td>
      <td>11.864988</td>
      <td>POLYGON ((11.864988 42.7439961, 11.864988 42.7...</td>
    </tr>
    <tr>
      <th>38949</th>
      <td>2018-04-30T00:12:40Z</td>
      <td>The Roman amphitheater at Bulla Regia</td>
      <td>NaN</td>
      <td>Pleiades</td>
      <td>Roman amphitheater at Bulla Regia</td>
      <td>https://pleiades.stoa.org/places/55568842</td>
      <td>55568842</td>
      <td>36.561892</td>
      <td>8.760049</td>
      <td>POLYGON ((8.760049 36.5618917, 8.760049 36.561...</td>
    </tr>
    <tr>
      <th>38950</th>
      <td>2020-05-14T21:57:48Z</td>
      <td>Gövelek is a village situated 27 km to the eas...</td>
      <td>NaN</td>
      <td>Pleiades</td>
      <td>Gövelek</td>
      <td>https://pleiades.stoa.org/places/555685926</td>
      <td>555685926</td>
      <td>38.532489</td>
      <td>43.620359</td>
      <td>POLYGON ((43.620359 38.5324889, 43.620359 38.5...</td>
    </tr>
    <tr>
      <th>38951</th>
      <td>2021-11-05T14:17:34Z</td>
      <td>The Thermae Helenianae or Thermae Helenae was ...</td>
      <td>NaN</td>
      <td>Pleiades</td>
      <td>Thermae Helenianae</td>
      <td>https://pleiades.stoa.org/places/555157618</td>
      <td>555157618</td>
      <td>41.889222</td>
      <td>12.514661</td>
      <td>POLYGON ((12.514661 41.889222, 12.514661 41.88...</td>
    </tr>
    <tr>
      <th>38952</th>
      <td>2021-02-14T18:15:06Z</td>
      <td>An unusual circular "fort" dating to the Old K...</td>
      <td>&lt;p&gt;According to Mumford 2006, the site was ide...</td>
      <td>Pleiades</td>
      <td>Tell Ras Budran</td>
      <td>https://pleiades.stoa.org/places/555858016</td>
      <td>555858016</td>
      <td>28.984232</td>
      <td>33.181649</td>
      <td>POLYGON ((33.1818643 28.9840031, 33.1818643 28...</td>
    </tr>
  </tbody>
</table>
<p>38953 rows × 10 columns</p>
</div>




```python
place_freq_final = place_freq_df.dropna().reset_index(drop=True)
place_freq_final
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
      <th>place_name</th>
      <th>frequency</th>
      <th>pleiades_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Trebizond</td>
      <td>1</td>
      <td>857359.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Milan</td>
      <td>2</td>
      <td>383706.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rome</td>
      <td>4</td>
      <td>423025.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Laodicea</td>
      <td>2</td>
      <td>678060.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Iconium</td>
      <td>1</td>
      <td>648647.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Cologne</td>
      <td>1</td>
      <td>108751.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Bagdad</td>
      <td>3</td>
      <td>893952.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Aleppo</td>
      <td>4</td>
      <td>658409.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Mosul</td>
      <td>2</td>
      <td>874609.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Nile</td>
      <td>8</td>
      <td>727172.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Arabia</td>
      <td>5</td>
      <td>981506.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Sicily</td>
      <td>3</td>
      <td>462492.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Apulia</td>
      <td>1</td>
      <td>442469.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Naples</td>
      <td>2</td>
      <td>433014.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Africa</td>
      <td>1</td>
      <td>775.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Berytus</td>
      <td>1</td>
      <td>678060.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Acre</td>
      <td>1</td>
      <td>678010.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Cyprus</td>
      <td>1</td>
      <td>707498.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# any helper functions you may need
rid = place_freq_final.pleiades_id.iloc[0]

def get_lat(pl_id):
    places_row = places.loc[places['id'] == pl_id]
    if len(places_row) == 1:
        return places_row.representative_latitude.iloc[0]
    
def get_long(pl_id):
    places_row = places.loc[places['id'] == pl_id]
    if len(places_row) == 1:
        return places_row.representative_longitude.iloc[0]

place_freq_final['lat'] = place_freq_final['pleiades_id'].apply(get_lat)
place_freq_final['long'] = place_freq_final['pleiades_id'].apply(get_long)
```


```python
def get_pleiades_id(term):
    """
    Iterates through all of the possible names in the names.csv file
    Returns None if no matched names
    """
    name_row = names.loc[names['attested_form'] == term]
    if len(name_row) == 1:
        return int(name_row.place_id.iloc[0])
    else:
        name_row = names.loc[names['romanized_form_1'] == term]
        if len(name_row) == 1:
            return int(name_row.place_id.iloc[0])
        else:
            name_row = names.loc[names['romanized_form_2'] == term]
            if len(name_row) == 1:
                return int(name_row.place_id.iloc[0])
            else:
                name_row = names.loc[names['romanized_form_3'] == term]
                if len(name_row) == 1:
                    return int(name_row.place_id.iloc[0])
                else:
                    return None
```


```python
# save result as CSV
place_freq_final.to_csv('ch59gibbon_places.csv')
```
