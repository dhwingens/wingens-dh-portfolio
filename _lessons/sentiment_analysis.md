---
layout: page
title: Sentiment Analysis for Exploratory Data Analysis
description: This lesson uses Python to apply sentiment analysis to Chevron emails
---

## Source
[Zoë Wilkinson Saldaña, "Sentiment Analysis for Exploratory Data Analysis," Programming Historian (2018), https://doi.org/10.46430/phen0079.](https://programminghistorian.org/en/lessons/sentiment-analysis)

## Reflection

Sentiment analysis is an important tool for distant reading, and involves using software to look at the language of a text and give it a grade based on the negativity or positivity of its sentiment. I was interested in this because it seems like one of the cornerstones of distant reading. It is also interesting becasuse it employs computers for what would seem to be the most human of tasks -- deriving some sense of emotion from a text. THe library used in this demo ranks texts on their negative, neutral and positive sentiment and then gives a composite score. A score between -0.5 and 0.5 is mostly neutral while a score outside the range in the negatives is negative and in the positives, positive.

Before I started the exercise, I assumed it could be useful for analyzing literary corpora to understand the sentimental bents of different writers on a macro level. This could lend some empirical credibility to certain opinions I have about authors who tend to write in more melancholy language and authors whose writing tends to be more upbeat. This lesson seems to suggest that sentiment analysis is more tailored for datasets that are more organically generated from the speech of real people. The examples used in the exercise are emails from inside Enron before their legendery collapse, but it mentions that the library "nltk" was made with Twitter in mind. I see how this could be a useful tool for analyzing how language on Twitter changes over time or with certain events. For example, I'd be interested to see a breakdown of the sentiment of language on Twitter before and after a polarizing political event like an election or January 6th, 2021. There are also some subreddits whose language I'm sure would be very interesting to look at.


```python
import nltk
import ssl

try:
    _create_unverified_https_context = ssl._create_unverified_context
except AttributeError:
    pass
else:
    ssl._create_default_https_context = _create_unverified_https_context

nltk.download()
```

    showing info https://raw.githubusercontent.com/nltk/nltk_data/gh-pages/index.xml



```python
# first, we import the relevant modules from the NLTK library
from nltk.sentiment.vader import SentimentIntensityAnalyzer
```


```python
# next, we initialize VADER so we can use it within our Python script
sid = SentimentIntensityAnalyzer()
```


```python
# the variable 'message_text' now contains the text we will analyze.
message_text = '''Like you, I am getting very frustrated with this process. I am genuinely trying to be as reasonable as possible. I am not trying to "hold up" the deal at the last minute. I'm afraid that I am being asked to take a fairly large leap of faith after this company (I don't mean the two of you -- I mean Enron) has screwed me and the people who work for me.'''

```


```python
print(message_text)

# Calling the polarity_scores method on sid and passing in the message_text outputs a dictionary with negative, neutral, positive, and compound scores for the input text
scores = sid.polarity_scores(message_text)
```

    Like you, I am getting very frustrated with this process. I am genuinely trying to be as reasonable as possible. I am not trying to "hold up" the deal at the last minute. I'm afraid that I am being asked to take a fairly large leap of faith after this company (I don't mean the two of you -- I mean Enron) has screwed me and the people who work for me.



```python
# Here we loop through the keys contained in scores (pos, neu, neg, and compound scores) and print the key-value pairs on the screen
for key in sorted(scores):
        print('{0}: {1}, '.format(key, scores[key]), end='')
```

    compound: -0.3804, neg: 0.093, neu: 0.836, pos: 0.071, 


```python
#Running on different texts
alt_text1 = "Looks great.  I think we should have a least 1 or 2 real time traders in Calgary."
alt_scores1 = sid.polarity_scores(alt_text1)

alt_text2 = '''I think we are making great progress on the systems side.  I would like to
set a deadline of November 10th to have a plan on all North American projects
(I'm ok if fundementals groups are excluded) that is signed off on by
commercial, Sally's world, and Beth's world.  When I say signed off I mean
that I want signitures on a piece of paper that everyone is onside with the
plan for each project.  If you don't agree don't sign. If certain projects
(ie. the gas plan) are not done yet then lay out a timeframe that the plan
will be complete.  I want much more in the way of specifics about objectives
and timeframe.

Thanks for everyone's hard work on this.'''
alt_scores2 = sid.polarity_scores(alt_text2)


```


```python
for key in sorted(alt_scores1):
        print('{0}: {1}, '.format(key, alt_scores1[key]), end='')
print("\n")
for key in sorted(alt_scores2):
        print('{0}: {1}, '.format(key, alt_scores2[key]), end='')
```

    compound: 0.6249, neg: 0.0, neu: 0.745, pos: 0.255, 
    
    compound: 0.8951, neg: 0.042, neu: 0.821, pos: 0.136, 

### Trying my own example from Twitter


```python
alt_text3 = "MARINERS TAKE THE LEAD IN THE TOP OF THE 9TH‼️"
alt_scores3 = sid.polarity_scores(alt_text3)

for key in sorted(alt_scores3):
        print('{0}: {1}, '.format(key, alt_scores3[key]), end='')
```

    compound: 0.2023, neg: 0.0, neu: 0.833, pos: 0.167, 


```python
# Continue with the same code the previous section, but replace the *message_text* variable with the new e-mail text:

message_text = '''It seems to me we are in the middle of no man's land with respect to the  following:  Opec production speculation, Mid east crisis and renewed  tensions, US elections and what looks like a slowing economy (?), and no real weather anywhere in the world. I think it would be most prudent to play  the markets from a very flat price position and try to day trade more aggressively. I have no intentions of outguessing Mr. Greenspan, the US. electorate, the Opec ministers and their new important roles, The Israeli and Palestinian leaders, and somewhat importantly, Mother Nature.  Given that, and that we cannot afford to lose any more money, and that Var seems to be a problem, let's be as flat as possible. I'm ok with spread risk  (not front to backs, but commodity spreads). The morning meetings are not inspiring, and I don't have a real feel for  everyone's passion with respect to the markets.  As such, I'd like to ask  John N. to run the morning meetings on Mon. and Wed.  Thanks. Jeff'''

print(message_text)

scores = sid.polarity_scores(message_text)

for key in sorted(scores):
        print('{0}: {1}, '.format(key, scores[key]), end='')
```

    It seems to me we are in the middle of no man's land with respect to the  following:  Opec production speculation, Mid east crisis and renewed  tensions, US elections and what looks like a slowing economy (?), and no real weather anywhere in the world. I think it would be most prudent to play  the markets from a very flat price position and try to day trade more aggressively. I have no intentions of outguessing Mr. Greenspan, the US. electorate, the Opec ministers and their new important roles, The Israeli and Palestinian leaders, and somewhat importantly, Mother Nature.  Given that, and that we cannot afford to lose any more money, and that Var seems to be a problem, let's be as flat as possible. I'm ok with spread risk  (not front to backs, but commodity spreads). The morning meetings are not inspiring, and I don't have a real feel for  everyone's passion with respect to the markets.  As such, I'd like to ask  John N. to run the morning meetings on Mon. and Wed.  Thanks. Jeff
    compound: 0.889, neg: 0.096, neu: 0.765, pos: 0.14, 


```python
import nltk.data
from nltk.sentiment.vader import SentimentIntensityAnalyzer
from nltk import sentiment
from nltk import word_tokenize

# Next, we initialize VADER so we can use it within our Python script
sid = SentimentIntensityAnalyzer()

# We will also initialize our 'english.pickle' function and give it a short name

tokenizer = nltk.data.load('tokenizers/punkt/english.pickle')

message_text = '''It seems to me we are in the middle of no man's land with respect to the  following:  Opec production speculation, Mid east crisis and renewed  tensions, US elections and what looks like a slowing economy (?), and no real weather anywhere in the world. I think it would be most prudent to play  the markets from a very flat price position and try to day trade more aggressively. I have no intentions of outguessing Mr. Greenspan, the US. electorate, the Opec ministers and their new important roles, The Israeli and Palestinian leaders, and somewhat importantly, Mother Nature.  Given that, and that we cannot afford to lose any more money, and that Var seems to be a problem, let's be as flat as possible. I'm ok with spread risk  (not front to backs, but commodity spreads). The morning meetings are not inspiring, and I don't have a real feel for  everyone's passion with respect to the markets.  As such, I'd like to ask  John N. to run the morning meetings on Mon. and Wed.  Thanks. Jeff'''

# The tokenize method breaks up the paragraph into a list of strings. In this example, note that the tokenizer is confused by the absence of spaces after periods and actually fails to break up sentences in two instances. How might you fix that?

sentences = tokenizer.tokenize(message_text)

# We add the additional step of iterating through the list of sentences and calculating and printing polarity scores for each one.

for sentence in sentences:
        print(sentence)
        scores = sid.polarity_scores(sentence)
        for key in sorted(scores):
                print('{0}: {1}, '.format(key, scores[key]), end='')
        print()

```

    It seems to me we are in the middle of no man's land with respect to the  following:  Opec production speculation, Mid east crisis and renewed  tensions, US elections and what looks like a slowing economy (?
    compound: -0.5267, neg: 0.197, neu: 0.68, pos: 0.123, 
    ), and no real weather anywhere in the world.
    compound: -0.296, neg: 0.216, neu: 0.784, pos: 0.0, 
    I think it would be most prudent to play  the markets from a very flat price position and try to day trade more aggressively.
    compound: 0.0183, neg: 0.103, neu: 0.792, pos: 0.105, 
    I have no intentions of outguessing Mr. Greenspan, the US.
    compound: -0.296, neg: 0.216, neu: 0.784, pos: 0.0, 
    electorate, the Opec ministers and their new important roles, The Israeli and Palestinian leaders, and somewhat importantly, Mother Nature.
    compound: 0.4228, neg: 0.0, neu: 0.817, pos: 0.183, 
    Given that, and that we cannot afford to lose any more money, and that Var seems to be a problem, let's be as flat as possible.
    compound: -0.1134, neg: 0.097, neu: 0.823, pos: 0.081, 
    I'm ok with spread risk  (not front to backs, but commodity spreads).
    compound: -0.0129, neg: 0.2, neu: 0.679, pos: 0.121, 
    The morning meetings are not inspiring, and I don't have a real feel for  everyone's passion with respect to the markets.
    compound: 0.5815, neg: 0.095, neu: 0.655, pos: 0.25, 
    As such, I'd like to ask  John N. to run the morning meetings on Mon.
    compound: 0.3612, neg: 0.0, neu: 0.848, pos: 0.152, 
    and Wed.
    compound: 0.0, neg: 0.0, neu: 1.0, pos: 0.0, 
    Thanks.
    compound: 0.4404, neg: 0.0, neu: 0.0, pos: 1.0, 
    Jeff
    compound: 0.0, neg: 0.0, neu: 1.0, pos: 0.0, 

