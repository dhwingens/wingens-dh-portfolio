---
layout: page
title: Exercises Ch. 6
description: Select exercises from Python Crash Course
---

```python
#6-1, 6-2, 6-3, 6-4, 6-5, 6-7, 6-8, 6-9, 6-11
#6-1
person = {'first_name':'Zoe', 'last_name':'Belluck', 'age':22, 'city':'Boston'}

print(person)
```

    {'first_name': 'Zoe', 'last_name': 'Belluck', 'age': 22, 'city': 'Boston'}



```python
#6-2
fav_nums= {
    'Mariel':18,
    'Sofia':9,
    'Zoe':29,
    'David':6,
    'Aaron':99
}
print(fav_nums)

```

    {'Mariel': 18, 'Sofia': 9, 'Zoe': 29, 'David': 6, 'Aaron': 99}



```python
#6-3
glossary = {
    'boolean':'A type of variable that true or false value',
    'list':'A Python data type used to store multiple values in one variable',
    'integer':'A primitive type in programming languages that can take the value of a whole number',
    'string':'A data type made up of multiple characters such as a word or sentence',
    'for loop':'The way to repeat a block of code iteratively'
}

for word, definition in glossary.items():
    print(f'{word}: {definition}')
```

    boolean: A type of variable that true or false value
    list: A Python data type used to store multiple values in one variable
    integer: A primitive type in programming languages that can take the value of a whole number
    string: A data type made up of multiple characters such as a word or sentence
    for loop: The way to repeat a block of code iteratively



```python
#6-4
glossary['if statement'] = 'checks Boolean value, if True, then executes codeblock'
glossary['equality operator'] = 'checks that both sides are exactly the same and returns a boolean'
glossary['assignment operator'] = 'one equals sign. assigns value from RHS to LHS'
glossary['method'] = 'A function built into a class such as .lower() for strings'
glossary['set'] = 'Similar to a list, but each value must be unique'

for word, definition in glossary.items():
    print(f'{word}: {definition}')
```

    boolean: A type of variable that true or false value
    list: A Python data type used to store multiple values in one variable
    integer: A primitive type in programming languages that can take the value of a whole number
    string: A data type made up of multiple characters such as a word or sentence
    for loop: The way to repeat a block of code iteratively
    if statement: checks Boolean value, if True, then executes codeblock
    equality operator: checks that both sides are exactly the same and returns a boolean
    assignment operator: one equals sign. assigns value from RHS to LHS
    method: A function built into a class such as .lower() for strings
    set: Similar to a list, but each value must be unique



```python
#6-5
rivers = {
    'mississippi':'america',
    'yangtze':'china',
    'seine':'france'
}

for river,country in rivers.items():
    print(f'the {river} runs through {country}.')

for river,country in rivers.items():
    print(river)

for river,country in rivers.items():
    print(country)
```

    the mississippi runs through america.
    the yangtze runs through china.
    the seine runs through france.
    mississippi
    yangtze
    seine
    america
    china
    france



```python
#6-7
person1 = {'first_name':'Zoe', 'last_name':'Belluck', 'age':22, 'city':'Boston'}
person2 = {'first_name':'Josh', 'last_name':'Allen', 'age':26, 'city':'Buffalo'}
person3 = {'first_name':'Mike', 'last_name':'Evans', 'age':29, 'city':'Tampa Bay'}

people = [person1, person2, person3]

for person in people:
    print(f"Name: {person['first_name']} {person['last_name']} \n Age:{person['age']} \n city: {person['city']}")
```

    Name: Zoe Belluck 
     Age:22 
     city: Boston
    Name: Josh Allen 
     Age:26 
     city: Buffalo
    Name: Mike Evans 
     Age:29 
     city: Tampa Bay



```python
#6-8

Yogi = {'animal':'dog', 'owner':'Kate'}
Fishy = {'animal':'goldfish', 'owner':'Sam'}

pets = [Yogi, Fishy]

for pet in pets:
    print(f"Animal: {pet['animal']} | Owner: {pet['owner']} ")
```

    Animal: dog | Owner: Kate 
    Animal: goldfish | Owner: Sam 



```python
#6-9
favorite_places = {
    'David':'home',
    'Kate':'Paris',
    'Sam':'the zoo'
}

for person, place in favorite_places.items():
    print(f"{person} likes {place}!")
```

    David likes home!
    Kate likes Paris!
    Sam likes the zoo!



```python
#6-11

cities = {
    'Somerville':{
        'country':'USA',
        'population':81000,
        'fact':'densest city in New England'
    },
    'Brussels':{
        'country':'Belgium',
        'population':2000000000,
        'fact':'HQ of the EU'        
    },
    'Lima':{
        'country':'Peru',
        'population':11000000000,
        'fact':'capital of Peru'
    }
}

for city, info in cities.items():
    print(city)
    print(info)
```

    Somerville
    {'country': 'USA', 'population': 81000, 'fact': 'densest city in New England'}
    Brussels
    {'country': 'Belgium', 'population': 2000000000, 'fact': 'HQ of the EU'}
    Lima
    {'country': 'Peru', 'population': 11000000000, 'fact': 'capital of Peru'}

