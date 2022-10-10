---
layout: page
title: Exercises Ch. 3 and 4
description: Select exercises from Python Crash Course
---

```python
#3-1, 3-2, 3-3, 3-4, 3-5, 3-6, 3-7, 3-8, 4-1, 4-2, 4-3, 4-6, 4-10, 4-11 

#3-1

names = ['Jonah', 'Mariel', 'Matthew']

print(names[0])
print(names[1])
print(names[2])

#3-2

for name in names:
    print(f'Hello, {name}')

```

    Jonah
    Mariel
    Matthew
    Hello, Jonah
    Hello, Mariel
    Hello, Matthew



```python
#3-3
modes_of_transportation = ['Mazda3', 'Amtrak', 'Citi Bike']

for mode in modes_of_transportation:
    print(f'I like to take {mode}')

```

    I like to take Mazda3
    I like to take Amtrak
    I like to take Citi Bike



```python
#3-4
guests = ['Isaac Newton', 'Albert Einstein', 'Moses']

for guest in guests:
    print(f'Please come to my dinner, {guest}')

#3-5
guests[2] = 'Marie Curie'

for guest in guests:
    print(f'Please come to my dinner, {guest}')


```

    Please come to my dinner, Isaac Newton
    Please come to my dinner, Albert Einstein
    Please come to my dinner, Moses
    Please come to my dinner, Beyonce
    Please come to my dinner, Isaac Newton
    Please come to my dinner, Albert Einstein
    Please come to my dinner, Marie Curie



```python
#3-6
guests = ['Isaac Newton', 'Albert Einstein', 'Moses']

guests.insert(0, 'Beyonce')
guests.insert(2, 'Meghan Thee Stallion')
guests.append('Bruno Mars')

print('My table has expanded!')

for guest in guests:
    print(f'Please come to my dinner, {guest}')
```

    Please come to my dinner, Beyonce
    Please come to my dinner, Isaac Newton
    Please come to my dinner, Meghan Thee Stallion
    Please come to my dinner, Albert Einstein
    Please come to my dinner, Moses
    Please come to my dinner, Bruno Mars



```python
#3-7
print('I only have room for two guests :(')

while len(guests) > 2:
    guests.pop()

for guest in guests:
    print(f'Please come to my dinner, {guest}')
```

    I only have room for two guests :(
    Please come to my dinner, Beyonce
    Please come to my dinner, Isaac Newton



```python
#3-8
destinations = ['Paris', 'Australia', 'Vienna', 'Cairo', 'Lagos']
print(destinations)

print(sorted(destinations))

print(destinations) #showing that the list has not been changed

destinations.reverse()
print(destinations)

destinations.reverse() #putting the list back in original order
print(destinations)

destinations.sort() #alphabetize
print(destinations)

destinations.sort(reverse=True) #reverse alphabetize
print(destinations)

```

    ['Paris', 'Australia', 'Vienna', 'Cairo', 'Lagos']
    ['Australia', 'Cairo', 'Lagos', 'Paris', 'Vienna']
    ['Paris', 'Australia', 'Vienna', 'Cairo', 'Lagos']
    ['Lagos', 'Cairo', 'Vienna', 'Australia', 'Paris']
    ['Paris', 'Australia', 'Vienna', 'Cairo', 'Lagos']
    ['Australia', 'Cairo', 'Lagos', 'Paris', 'Vienna']
    ['Vienna', 'Paris', 'Lagos', 'Cairo', 'Australia']



```python
##4-1, 4-2, 4-3, 4-6, 4-10, 4-11 
#4-1

pizzas = ['cheese', 'pineapple', 'white']

for pizza in pizzas:
    print(f'{pizza} pizza is epic.')

print('All pizza is epic!')
```

    cheese pizza is epic.
    pineapple pizza is epic.
    white pizza is epic.
    All pizza is epic!



```python
#4-2
animals = ['wolves', 'sheep', 'cats']

for animal in animals:
    print(f'{animal} have four legs')

print('all these animals have four legs')

```

    wolves have four legs
    sheep have four legs
    cats have four legs
    all these animals have four legs



```python
#4-3

for i in range(1,21):
    print(i)
```

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10
    11
    12
    13
    14
    15
    16
    17
    18
    19
    20



```python
#4-6

for i in range(1,21,2):
    print(i)
```

    1
    3
    5
    7
    9
    11
    13
    15
    17
    19



```python
#4-10
animals = ['wolves', 'sheep', 'cats', 'dogs', 'mice', 'hippos']

print(f'The first three items in the list are {animals[:3]}')
print(f'The first three items in the list are {animals[2:5]}')
print(f'The first three items in the list are {animals[-3:]}')


```

    The first three items in the list are ['wolves', 'sheep', 'cats']
    The first three items in the list are ['cats', 'dogs', 'mice']
    The first three items in the list are ['dogs', 'mice', 'hippos']



```python
#4-11
pizzas = ['cheese', 'pineapple', 'white']
friend_pizzas = ['cheese', 'pineapple', 'white']

friend_pizzas.append('margharita')

print('My favorite pizzas are: ', end='')
for pizza in pizzas:
    print(pizza, end=' ')

print("\nMy friend's favorite pizzas are: ", end='')
for pizza in friend_pizzas:
    print(pizza, end=' ')
```

    My favorite pizzas are: cheese pineapple white 
    My friend's favorite pizzas are: cheese pineapple white margharita 
