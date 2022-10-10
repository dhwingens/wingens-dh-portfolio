---
layout: page
title: Exercises Ch. 5
description: Select exercises from Python Crash Course
---

```python
#5-1, 5-2, 5-6, 5-7, 5-8, 5-9, 5-10 
#5-1
language = "python"

print("is language == 'C'. I predict False")
print(language == 'C')

print("is language == 'python'. I predict True")
print(language == "python")

language = "English"

print("is language == 'French'. I predict False")
print(language == 'French')

print("is language == 'English'. I predict True")
print(language == "English")

color = "Red"

print("is color == 'Blue'. I predict False")
print(color == 'Blue')

print("is color == 'Red'. I predict True")
print(color == "Red")

color = "Yellow"

print("is color == 'Green'. I predict False")
print(color == 'Green')

print("is color == 'Yellow'. I predict True")
print(color == "Yellow")

number = 8

print("is number == 10. I predict False")
print(number == '10')

print("is number == 8. I predict True")
print(number == 8)
```

    is language == 'C'. I predict False
    False
    is language == 'python'. I predict True
    True
    is language == 'French'. I predict False
    False
    is language == 'English'. I predict True
    True
    is color == 'Blue'. I predict False
    False
    is color == 'Red'. I predict True
    True
    is color == 'Green'. I predict False
    False
    is color == 'Yellow'. I predict True
    True
    is number == 10. I predict False
    False
    is number == 8. I predict True
    True



```python
#5-2

fruit = "apple"
print(fruit == "Apple")
Fruit = "Apple"
print(fruit.lower() == Fruit.lower())

print(8 < 99)

age = 10
height = 20

print(age < 11 and height >19)
print(age < 1 and height >19)
print(age < 1 or height >19)

things = ['car', 'apple']
print('item' in things)
print('item' not in things)

```

    False
    True
    True
    True
    False
    True
    False
    True



```python
#5-6
age = 50

if age < 2:
    print("You're a baby")
elif age < 4:
    print("You're a toddler")
elif age < 13:
    print("You're a child")
elif age < 20:
    print("You're a teenager")
elif age < 65:
    print("You're an adult")
else:
    print("You're an elder")


```

    You're an adult



```python
#5-7
favorite_fruits = ['apple', 'orange', 'cherry']

if 'apple' in favorite_fruits:
    print("You really like apples!")
if 'banana' in favorite_fruits:
    print("You really like bananas!")
if 'cherry' in favorite_fruits:
    print("You really like cherries!")
if 'melon' in favorite_fruits:
    print("You really like melons!")
if 'nectarine' in favorite_fruits:
    print("You really like nectarines!")
```

    You really like apples!
    You really like cherries!



```python
#5-8
users = ["admin", "dave", "monkey", "user3", "dude"]

for user in users:
    if user == "admin":
        print("Hello admin, do you want a status report??")
    else:
        print(f'Hi {user}, thanks for logging in!')
```

    Hello admin, do you want a status report??
    Hi dave, thanks for logging in!
    Hi monkey, thanks for logging in!
    Hi user3, thanks for logging in!
    Hi dude, thanks for logging in!



```python
#5-9
users = []
if not users: 
    print("we need to find some users")
```

    we need to find some users



```python
#5-10

current_users = ["Mare", "Yell", "Privy", "user5555", "YankeesRock01"]
new_users = ["Dave", "FreshPasta", "user5555", "foo", "bar"]
for user in current_users:
    user.lower()
for user in new_users:
    user.lower()
for user in new_users:
    if user in current_users:
        print("username already in use. Pick another please")
    else:
        print("nice! you can use that name")


```

    nice! you can use that name
    nice! you can use that name
    username already in use. Pick another please
    nice! you can use that name
    nice! you can use that name

