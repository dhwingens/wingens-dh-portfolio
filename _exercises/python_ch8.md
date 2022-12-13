---
layout: page
title: Exercises Ch. 8
description: Select exercises from Python Crash Course
---

# Python Crash Course Excercises from Ch. 8
Excercises 8-1, 8-2, 8-3, 8-5, 8-6, 8-7


```python
#8-1
def display_message():
    print("This chapter about functions is pretty epic.")

display_message()
```

    This chapter about functions is pretty epic.



```python
#8-2

def favorite_book(title):
    print(f"My favorite book is {title}")

favorite_book("Cat in the Hat")
```

    My favorite book is Cat in the Hat



```python
#8-3

def make_shirt(size, text):
    print(f"The shirt shall be size {size}, and it shall say {text}")

make_shirt(12, "T-Shirt")
make_shirt(text="Long-sleeve", size=14)
```

    The shirt shall be size 12, and it shall say T-Shirt
    The shirt shall be size 14, and it shall say Long-sleeve



```python
# 8-5
# 8-5. Cities: Write a function called describe_city() that accepts the name of a city and its country. 
# The function should print a simple sentence, such as Reykjavik is in Iceland. 
# Give the parameter for the country a default value. 
# Call your function for three different cities, at least one of which is not in the default country.

def describe_city(city, country="the USA"):
    print(f"{city} is in {country}")

describe_city("Denver")
describe_city("Boston")
describe_city("Paris", "France")
```

    Denver is in the USA
    Boston is in the USA
    Paris is in France



```python
#8-6. City Names: Write a function called city_country() that takes in the name of a city and its country. 
# The function should return a string formatted like this:
# "Santiago, Chile"
# Call your function with at least three city-country pairs, and print the values that are returned.

def city_country(city, country):
    print(f'"{city}, {country}"')

city_country("Paris", "France")
city_country("Boston", "USA")
city_country("Mexico City", "Mexico")
```

    "Paris, France"
    "Boston, USA"
    "Mexico City, Mexico"



```python
#8-7.

def make_album(artist_name, album_title, num_songs=None):
    album = {
        "artist": artist_name,
        "title": album_title
    }
    if num_songs:
        album["num_songs"] = num_songs
    return album

album1 = make_album("Pink Floyd", "The Dark Side of the Moon")
print(album1)

album2 = make_album("Led Zeppelin", "Physical Graffiti", 15)
print(album2)

album3 = make_album("David Bowie", "The Rise and Fall of Ziggy Stardust and the Spiders from Mars")
print(album3)
```

    {'artist': 'Pink Floyd', 'title': 'The Dark Side of the Moon'}
    {'artist': 'Led Zeppelin', 'title': 'Physical Graffiti', 'num_songs': 15}
    {'artist': 'David Bowie', 'title': 'The Rise and Fall of Ziggy Stardust and the Spiders from Mars'}

