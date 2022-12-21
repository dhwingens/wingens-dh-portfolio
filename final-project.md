---
layout: page
title: Final Project
permalink: /final-project/
---
### Writeup and Graphs
## Intro
I went through quite a few ideas for my final project. I was going to do something about Walt Whitman's Leaves of Grass and change over time, and then I was going to potentially encode Leaves of Grass in TEI format. I ended up doing none of those things. I've mentioned quite a few times that I was in a class on Charles Dickens and George Eliot this semester, and as the course was winding down, I decided that I really wanted to do some analysis on texts that I felt like I really knew something about. At the same time, we were learning in class about word/sentence embedding, which really struck me as an interesting an novel way to learn about the texts I've been reading about all semester and to gain some knowledge of machine learning. So, for my project I ran sentence embeddings on all the books I read in my Dickens and Eliot class. These are:
- _Oliver Twist_ by Charles Dickens
- *Great Expectations* by Charles Dickens
- *Adam Bede* by George Eliot
- *The Mill on the Floss* by George Eliot
- *Middlemarch* by George Eliot

I graphed the embeddings on a 2d plane with plotly. I really like how these graphs are interactive and can be explored. I spent a while trying to figure out how to get them to appear on this page as dynamic graphs, but after a couple hours and some very niche internet forums, I sorted it out. In order to analyze these texts, I also tried my hand at some topic modeling of the Dickens novels and eliot novels. The results of that topic modeling are at the bottom of the coding section.

## Data Gathering
All my data came from Project Gutenberg. I used their text files and adapted some code that Micah gave the class. The cleaning of the text is not overly thorough, but I added a line of regex to get rid of any ambiguous unicode characters that popped up in the texts quite a bit (especially when there was an apostrophe). I split the texts into sentences according to the punctuation marks and put them into CSVs. In retrospect, it may have been better to use Beautiful Soup and used the html version of the text. Especially for topic modeling, that might have led to a better result because I could have more easily chosen how to split the text or switched between ways of splitting text. I'm not really sure how I would split the text into paragraphs with the plain text file. Still, the topic modeling seemed to work fine, so I did not think it would be wise to tinker.

## The Graphs
A strong argument could be made that these graphs just have too much going on. It turns out that five long novels adds up to a lot of sentences. Still, I think there are a lot of interesting things going on, and they are not just colorful Rorschach tests! **Please Zoom in, scroll around and hover your mouse to see what each tiny dot represents.**

# Graph 1:
{% include dickens_eliot_plot.html %}

This was the first graph I made. One thing I did not realize beforehand is that every time I ran the sentence embeddings (unless I've gone mad), the graph seemed to be slightly different. I did some research on how these models work, and it does seem like there is some randomness to the process. At the same time, that may just have been a figment of my imagination. Either way, the process of sentence embedding is a dynamic process and there are many different models out there that would have a different view of the text. Just as with human perception of literature, there is not necessarily one right answer.

Now onto the details of the graph. 90% of it is one big blob. I expected there to be a little more differentiation between Dickens and Eliot, but in this graph it is hard to see much of a difference. That being said, there is definitely a section of *Oliver Twist* sentences that are noticeable on the left and *Great Expectations* sentences on the right. I think that the distance between these two is notable because they are texts from opposite ends of Dickens's career. *Great Expectations* is a novel that contains much more Eliotic sympathy and more of a psychologically realist novel in the mold of an Eliot novel. *Oliver Twist* is more full of outrageous characters and heavy politically-minded criticism of society. 

Most interesting to me, however, is the uniqueness of Dorothea from Middlemarch. One of about six protagonists, Dorothea is clearly Eliot's favorite, and serves most closely as a model of a good member of society. I am really quite surprised at how the most defined section of this graph that is farthest away from the blob is full of quotes about Dorothea (the breakaway section around the top left). She is far from the most eccentric character in these novels, but there is undoubtedly something singular to her character that the model also sees.

# Graph 2:
{% include dickens_eliot_plot2.html %}

For this plot, I colorde by book instead of by author. I think this plot is interesting because it pretty clearly shows *Middlemarch* at the middle of things with sentences from *Mill on the Floss* and *Adam Bede* occupying spaces to the left, and Dickens novels mostly on the right. Written more towards the end of the Victorian Era, *Middlemarch* really was the apotheosis and summary of the realist novel of the Victorian Age. In it, everything is contained. I am struck again by the place that Dorothea-related quotes occupy on the plot clustered at the top, slightly removed from everything else.


## Discussing Topic Modeling Attempt
In my code, you'll see that in addition to makinf CSVs of each book, I also kept track of all the dickens texts concatenated and all the Eliot texts concatenated. I used these to topic model all the novels of each author together. I could not really find convincing names for or rationales behind the topics. What is clear is that Eliot's topics are mostly collections of characters, while Dickens has a lot of eccentric adjectives and action verbs. This reflects the more character-driven nature of Eliot (you can also see my post about the Voyant Tools wordcloud of *Middlemarch* which was pretty much all character names). Dickens's novels have interesting characters but they are much more interested in plot and contain many more eccentric descriptions.


### Code Portion!
Below is all of the code I used to make those plots, and at the bottom you can see my topic modeling.

# Part 1: Scraping the Texts


```python
import requests
from nltk.tokenize import sent_tokenize
import pandas as pd
import re
```


```python
def get_text2(url, title, author, start, end, path):
    """this function takes in a host of parameters that let it get a raw text file 
    from the web and then clean it. it is adapted from a function that Micah Saxton wrote
    and is intended for use with Project Gutenberg text files. I added a line to get rid 
    of some of the strange characters that were popping up using regex and I made it 
    return the data frame instead of writing it to a csv in the function to make it a 
    little more versatile"""
    response = requests.get(url)
    raw_text = response.text
    start_index = raw_text.find(start)
    end_index = raw_text.find(end)
    target_text =  raw_text[start_index:end_index]
    target_text = target_text.replace('\n', '')
     # note \r is replaced with a space
    target_text = target_text.replace('\r', ' ') 
     # Keep only ASCII letters and punctuation
    target_text = re.sub(r'[^\x00-\x7fA-Za-z\.,!\?:;\(\)\[\]\{\}\-\_\+\/\|\\]', r'', target_text)
    sentences = sent_tokenize(target_text)
    df = pd.DataFrame(sentences)
    return df

    
```


```python
texts = [
    {
        'url': 'https://www.gutenberg.org/cache/epub/507/pg507.txt',
        'title': 'adam_bede',
        'author': 'eliot',
        'start': 'With a single drop of ink',
        'end': '*** END'
    },
    {
        'url': 'https://www.gutenberg.org/cache/epub/6688/pg6688.txt',
        'title': 'mill_floss',
        'author': 'eliot',
        'start': 'A wide plain',
        'end': '*** END'
    },
    {
        'url': 'https://www.gutenberg.org/cache/epub/145/pg145.txt',
        'title': 'middlemarch',
        'author': 'eliot',
        'start': 'Who that cares much',
        'end': '*** END'
    },
    {
        'url': 'https://www.gutenberg.org/cache/epub/730/pg730.txt',
        'title': 'oliver_twist',
        'author': 'dickens',
        'start': 'Among other public buildings',
        'end': '*** END'
    },
    {
        'url': 'https://www.gutenberg.org/files/1400/1400-0.txt',
        'title': 'great_expectations',
        'author': 'dickens',
        'start': 'My father',
        'end': '*** END'
    }
]
```


```python
dickens_combo = pd.DataFrame()
eliot_combo = pd.DataFrame()
```


```python
dickens_list = []
eliot_list = []

path = 'texts/'
for text in texts:
    url = text['url']
    title = text['title']
    author = text['author']
    start = text['start']
    end = text['end']
    text_df = get_text2(url=url, title=title, author=author, start=start, end=end, path=path)
    text_df.to_csv(f'{path}{author}_{title}.csv')

    if author == 'dickens':
        dickens_list.append(text_df)
    elif author == 'eliot':
        eliot_list.append(text_df)

dickens_combo = pd.concat(dickens_list)
eliot_combo = pd.concat(eliot_list)

dickens_combo.to_csv('texts/dickens_combo.csv')
eliot_combo.to_csv('texts/eliot_combo.csv')

```

# Step 2: Manipulating text data and running embedding

Here, I work the data into a format that works to do sentence embedding, and then
run the embedding


```python
CSVs = [
    'texts/dickens_great_expectations.csv',
    'texts/dickens_oliver_twist.csv',
    'texts/eliot_adam_bede.csv',
    'texts/eliot_mill_floss.csv',
    'texts/eliot_middlemarch.csv',
]

DFs = []
for csv in CSVs:
    df = pd.read_csv(csv).drop('Unnamed: 0',axis=1).rename(columns={'0':'sents'})
    df['author'] = csv.split('_')[0].split('/')[1]
    df['book'] = csv.split('.')[0].split('_')[1]
    DFs.append(df)

tot = pd.concat(DFs).reset_index(drop=True).rename(columns={'Unnamed: 0':'org_index','0':'sents'})
tot
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
      <th>sents</th>
      <th>author</th>
      <th>book</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>My fathers family name being Pirrip, and my Ch...</td>
      <td>dickens</td>
      <td>great</td>
    </tr>
    <tr>
      <th>1</th>
      <td>So, I called myself Pip, and came to be called...</td>
      <td>dickens</td>
      <td>great</td>
    </tr>
    <tr>
      <th>2</th>
      <td>I give Pirrip as my fathers family name, on th...</td>
      <td>dickens</td>
      <td>great</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Joe Gargery, who married the blacksmith.</td>
      <td>dickens</td>
      <td>great</td>
    </tr>
    <tr>
      <th>4</th>
      <td>As I never saw my father or my mother, and nev...</td>
      <td>dickens</td>
      <td>great</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>51486</th>
      <td>But we insignificant people with our daily wor...</td>
      <td>eliot</td>
      <td>middlemarch</td>
    </tr>
    <tr>
      <th>51487</th>
      <td>Her finely touched spirit had still its fine i...</td>
      <td>eliot</td>
      <td>middlemarch</td>
    </tr>
    <tr>
      <th>51488</th>
      <td>Her full nature, like that river of which Cyru...</td>
      <td>eliot</td>
      <td>middlemarch</td>
    </tr>
    <tr>
      <th>51489</th>
      <td>But the effect of her being on those around he...</td>
      <td>eliot</td>
      <td>middlemarch</td>
    </tr>
    <tr>
      <th>51490</th>
      <td>THE END</td>
      <td>eliot</td>
      <td>middlemarch</td>
    </tr>
  </tbody>
</table>
<p>51491 rows × 3 columns</p>
</div>




```python
from sentence_transformers import SentenceTransformer

```


```python

sentences = tot.sents.to_list()
model = SentenceTransformer('sentence-transformers/all-MiniLM-L6-v2')
embeddings = model.encode(sentences) # this will take some time... (about 4 minutes)
```


```python
import umap
```


```python

reducer = umap.UMAP()

new_embeddings = reducer.fit_transform(embeddings)
```


```python
df = pd.DataFrame({"text": sentences, "author":tot.author, "book":tot.book})
df['x'] = new_embeddings[:, 0]
df['y'] = new_embeddings[:, 1] 
```


```python
df.text = df.text.str.wrap(30).apply(lambda x: x.replace('\n', '<br>'))
```

# Step 3: Visualize Embeddings


```python
import plotly.express as px
```


```python
fig = px.scatter(df, x='x', y='y', symbol='book', color='author', template="simple_white", hover_data=['text','author','book'], opacity=0.6)

fig.show()
```




```python
fig.write_html('../_includes/dickens_eliot_plot.html')
```

# Now for some Topic Modeling
The main issue I foresee is that whereas


```python
from sklearn.feature_extraction.text import TfidfVectorizer # <- try this one out at home
from sklearn.feature_extraction.text import CountVectorizer # <- we'll be using this one
from sklearn.decomposition import NMF # <- another topic model, try this one too, https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.NMF.html
from sklearn.decomposition import LatentDirichletAllocation

import pandas as pd
from bs4 import BeautifulSoup
import requests

import pprint
pp = pprint.PrettyPrinter(indent=4)
```


```python
dickens_combo.columns = ['text']
eliot_combo.columns = ['text']

```


```python
cv = CountVectorizer(max_df=0.95, min_df=2, stop_words='english')
dickens_dtm_lda = cv.fit_transform(dickens_combo['text'])
dickens_dtm_lda
```




    <19014x8927 sparse matrix of type '<class 'numpy.int64'>'
    	with 133000 stored elements in Compressed Sparse Row format>




```python
cv = CountVectorizer(max_df=0.95, min_df=2, stop_words='english')
eliot_dtm_lda = cv.fit_transform(eliot_combo['text'])
eliot_dtm_lda
```




    <32477x13902 sparse matrix of type '<class 'numpy.int64'>'
    	with 295956 stored elements in Compressed Sparse Row format>




```python
dickens_lda_model = LatentDirichletAllocation(n_components=10,random_state=1)
dickens_lda_model.fit(dickens_dtm_lda)
```




<style>#sk-container-id-5 {color: black;background-color: white;}#sk-container-id-5 pre{padding: 0;}#sk-container-id-5 div.sk-toggleable {background-color: white;}#sk-container-id-5 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-5 label.sk-toggleable__label-arrow:before {content: "▸";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-5 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-5 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-5 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-5 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-5 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-5 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: "▾";}#sk-container-id-5 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-5 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-5 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-5 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-5 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-5 div.sk-parallel-item::after {content: "";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-5 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-5 div.sk-serial::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-5 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-5 div.sk-item {position: relative;z-index: 1;}#sk-container-id-5 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-5 div.sk-item::before, #sk-container-id-5 div.sk-parallel-item::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-5 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-5 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-5 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-5 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-5 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-5 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-5 div.sk-label-container {text-align: center;}#sk-container-id-5 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-5 div.sk-text-repr-fallback {display: none;}</style><div id="sk-container-id-5" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>LatentDirichletAllocation(random_state=1)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-5" type="checkbox" checked><label for="sk-estimator-id-5" class="sk-toggleable__label sk-toggleable__label-arrow">LatentDirichletAllocation</label><div class="sk-toggleable__content"><pre>LatentDirichletAllocation(random_state=1)</pre></div></div></div></div></div>




```python
eliot_lda_model = LatentDirichletAllocation(n_components=10,random_state=1)
eliot_lda_model.fit(eliot_dtm_lda)
```




<style>#sk-container-id-7 {color: black;background-color: white;}#sk-container-id-7 pre{padding: 0;}#sk-container-id-7 div.sk-toggleable {background-color: white;}#sk-container-id-7 label.sk-toggleable__label {cursor: pointer;display: block;width: 100%;margin-bottom: 0;padding: 0.3em;box-sizing: border-box;text-align: center;}#sk-container-id-7 label.sk-toggleable__label-arrow:before {content: "▸";float: left;margin-right: 0.25em;color: #696969;}#sk-container-id-7 label.sk-toggleable__label-arrow:hover:before {color: black;}#sk-container-id-7 div.sk-estimator:hover label.sk-toggleable__label-arrow:before {color: black;}#sk-container-id-7 div.sk-toggleable__content {max-height: 0;max-width: 0;overflow: hidden;text-align: left;background-color: #f0f8ff;}#sk-container-id-7 div.sk-toggleable__content pre {margin: 0.2em;color: black;border-radius: 0.25em;background-color: #f0f8ff;}#sk-container-id-7 input.sk-toggleable__control:checked~div.sk-toggleable__content {max-height: 200px;max-width: 100%;overflow: auto;}#sk-container-id-7 input.sk-toggleable__control:checked~label.sk-toggleable__label-arrow:before {content: "▾";}#sk-container-id-7 div.sk-estimator input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-7 div.sk-label input.sk-toggleable__control:checked~label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-7 input.sk-hidden--visually {border: 0;clip: rect(1px 1px 1px 1px);clip: rect(1px, 1px, 1px, 1px);height: 1px;margin: -1px;overflow: hidden;padding: 0;position: absolute;width: 1px;}#sk-container-id-7 div.sk-estimator {font-family: monospace;background-color: #f0f8ff;border: 1px dotted black;border-radius: 0.25em;box-sizing: border-box;margin-bottom: 0.5em;}#sk-container-id-7 div.sk-estimator:hover {background-color: #d4ebff;}#sk-container-id-7 div.sk-parallel-item::after {content: "";width: 100%;border-bottom: 1px solid gray;flex-grow: 1;}#sk-container-id-7 div.sk-label:hover label.sk-toggleable__label {background-color: #d4ebff;}#sk-container-id-7 div.sk-serial::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: 0;}#sk-container-id-7 div.sk-serial {display: flex;flex-direction: column;align-items: center;background-color: white;padding-right: 0.2em;padding-left: 0.2em;position: relative;}#sk-container-id-7 div.sk-item {position: relative;z-index: 1;}#sk-container-id-7 div.sk-parallel {display: flex;align-items: stretch;justify-content: center;background-color: white;position: relative;}#sk-container-id-7 div.sk-item::before, #sk-container-id-7 div.sk-parallel-item::before {content: "";position: absolute;border-left: 1px solid gray;box-sizing: border-box;top: 0;bottom: 0;left: 50%;z-index: -1;}#sk-container-id-7 div.sk-parallel-item {display: flex;flex-direction: column;z-index: 1;position: relative;background-color: white;}#sk-container-id-7 div.sk-parallel-item:first-child::after {align-self: flex-end;width: 50%;}#sk-container-id-7 div.sk-parallel-item:last-child::after {align-self: flex-start;width: 50%;}#sk-container-id-7 div.sk-parallel-item:only-child::after {width: 0;}#sk-container-id-7 div.sk-dashed-wrapped {border: 1px dashed gray;margin: 0 0.4em 0.5em 0.4em;box-sizing: border-box;padding-bottom: 0.4em;background-color: white;}#sk-container-id-7 div.sk-label label {font-family: monospace;font-weight: bold;display: inline-block;line-height: 1.2em;}#sk-container-id-7 div.sk-label-container {text-align: center;}#sk-container-id-7 div.sk-container {/* jupyter's `normalize.less` sets `[hidden] { display: none; }` but bootstrap.min.css set `[hidden] { display: none !important; }` so we also need the `!important` here to be able to override the default hidden behavior on the sphinx rendered scikit-learn.org. See: https://github.com/scikit-learn/scikit-learn/issues/21755 */display: inline-block !important;position: relative;}#sk-container-id-7 div.sk-text-repr-fallback {display: none;}</style><div id="sk-container-id-7" class="sk-top-container"><div class="sk-text-repr-fallback"><pre>LatentDirichletAllocation(random_state=1)</pre><b>In a Jupyter environment, please rerun this cell to show the HTML representation or trust the notebook. <br />On GitHub, the HTML representation is unable to render, please try loading this page with nbviewer.org.</b></div><div class="sk-container" hidden><div class="sk-item"><div class="sk-estimator sk-toggleable"><input class="sk-toggleable__control sk-hidden--visually" id="sk-estimator-id-7" type="checkbox" checked><label for="sk-estimator-id-7" class="sk-toggleable__label sk-toggleable__label-arrow">LatentDirichletAllocation</label><div class="sk-toggleable__content"><pre>LatentDirichletAllocation(random_state=1)</pre></div></div></div></div></div>




```python
for index,topic in enumerate(eliot_lda_model.components_):
    print(f'THE TOP 15 WORDS FOR TOPIC #{index+1}')
    print([cv.get_feature_names_out()[i] for i in topic.argsort()[-15:]])
    print('\n')
```

    THE TOP 15 WORDS FOR TOPIC #1
    ['miss', 'im', 'ill', 'just', 'day', 'bit', 'like', 'night', 'away', 'wi', 'tell', 'home', 'maggie', 'come', 'said']
    
    
    THE TOP 15 WORDS FOR TOPIC #2
    ['dorothea', 'turned', 'come', 'little', 'thought', 'voice', 'arthur', 'seth', 'mr', 'hetty', 'dinah', 'poyser', 'mrs', 'adam', 'said']
    
    
    THE TOP 15 WORDS FOR TOPIC #3
    ['glegg', 'brooke', 'maggie', 'bulstrode', 'did', 'know', 'father', 'philip', 'mrs', 'lydgate', 'tulliver', 'casaubon', 'tom', 'said', 'mr']
    
    
    THE TOP 15 WORDS FOR TOPIC #4
    ['head', 'face', 'said', 'away', 'ud', 'little', 'came', 'hands', 'round', 'maggie', 'looked', 'looking', 'eyes', 'look', 'like']
    
    
    THE TOP 15 WORDS FOR TOPIC #5
    ['mind', 'hes', 'theres', 'money', 'ill', 'like', 'ive', 'better', 'make', 'got', 'dont', 'say', 'know', 'shall', 'think']
    
    
    THE TOP 15 WORDS FOR TOPIC #6
    ['deal', 'sure', 'fred', 'like', 'little', 'know', 'young', 'man', 'james', 'mr', 'im', 'come', 'said', 'sir', 'good']
    
    
    THE TOP 15 WORDS FOR TOPIC #7
    ['feel', 'thy', 'like', 'little', 'god', 'felt', 'said', 'yes', 'heart', 'oh', 'hetty', 'did', 'dear', 'thee', 'love']
    
    
    THE TOP 15 WORDS FOR TOPIC #8
    ['way', 'felt', 'eyes', 'away', 'things', 'maggie', 'wish', 'hand', 'face', 'make', 'know', 'chapter', 'little', 'use', 'let']
    
    
    THE TOP 15 WORDS FOR TOPIC #9
    ['looking', 'room', 'raffles', 'great', 'think', 'old', 'human', 'quite', 'rosamond', 'bulstrode', 'mr', 'felt', 'man', 'lydgate', 'life']
    
    
    THE TOP 15 WORDS FOR TOPIC #10
    ['time', 'years', 'world', 'making', 'great', 'life', 'fine', 'woman', 'like', 'little', 'thing', 'thought', 'young', 'man', 'old']
    
    


dickens


```python
for index,topic in enumerate(dickens_lda_model.components_):
    print(f'THE TOP 15 WORDS FOR TOPIC #{index+1}')
    print([cv.get_feature_names_out()[i] for i in topic.argsort()[-15:]])
    print('\n')
```

    THE TOP 15 WORDS FOR TOPIC #1
    ['barn', 'multitude', 'amateurish', 'naight', 'crop', 'gratification', 'dance', 'finished', 'jagged', 'james', 'notorious', 'crockery', 'feverish', 'judged', 'fury']
    
    
    THE TOP 15 WORDS FOR TOPIC #2
    ['je', 'mob', 'averting', 'finished', 'crop', 'firelight', 'adams', 'multitude', 'extraneous', 'naively', 'fowls', 'gratification', 'compound', 'kennel', 'amazed']
    
    
    THE TOP 15 WORDS FOR TOPIC #3
    ['likin', 'gratification', 'finish', 'drawers', 'excursus', 'effect', 'grass', 'crowd', 'ceased', 'moorings', 'physical', 'babby', 'intensest', 'cheltenham', 'judged']
    
    
    THE TOP 15 WORDS FOR TOPIC #4
    ['crowd', 'innocence', 'factor', 'floods', 'life', 'complexion', 'doses', 'bilious', 'gratification', 'enthusiastic', 'arbitratin', 'folds', 'correspondence', 'dose', 'freedom']
    
    
    THE TOP 15 WORDS FOR TOPIC #5
    ['dusty', 'finished', 'doted', 'faut', 'parrot', 'lack', 'featherstones', 'disappointing', 'par', 'floods', 'pickles', 'feel', 'diet', 'grate', 'bran']
    
    
    THE TOP 15 WORDS FOR TOPIC #6
    ['divided', 'arrangements', 'gratuitously', 'panniers', 'befriending', 'pickles', 'dimmed', 'bounds', 'gratification', 'madame', 'composure', 'eyebrows', 'doubtfully', 'clothing', 'mr']
    
    
    THE TOP 15 WORDS FOR TOPIC #7
    ['pictures', 'mounds', 'herand', 'page', 'barrel', 'likin', 'gratification', 'exchange', 'paroxysm', 'floods', 'bats', 'composure', 'eyebrows', 'furtively', 'judged']
    
    
    THE TOP 15 WORDS FOR TOPIC #8
    ['meaner', 'dizzy', 'babby', 'imputation', 'phrases', 'naively', 'drank', 'feverish', 'bib', 'dusty', 'doted', 'himif', 'furtively', 'discussion', 'disappearing']
    
    
    THE TOP 15 WORDS FOR TOPIC #9
    ['jagged', 'floods', 'je', 'naively', 'flirtation', 'naturally', 'judged', 'discussion', 'irwinehe', 'frivolity', 'dizzy', 'parrot', 'divided', 'exercise', 'kept']
    
    
    THE TOP 15 WORDS FOR TOPIC #10
    ['irregular', 'named', 'apocrypha', 'grate', 'near', 'disappeared', 'particularly', 'befriending', 'herand', 'finest', 'disappearing', 'cheered', 'gifted', 'naively', 'exercise']
    
    

