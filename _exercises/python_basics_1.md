### Assignment = 2.1-2.7 & 2.10


```python
#2.1

message = "Hello, World!"
print(message)
```

    Hello, World!



```python
#2.2
message = "Hello, World!"
print(message)
#Change message
message = "Hollow World?"
print(message)
```

    Hello, World!
    Hollow World?



```python
#2.3
name = "Kate"
print("Hello " + name + ", how are you doing?")
```

    Hello Kate, how are you doing?



```python
#2.4
name = "Daisy"
print(name.lower()) #lowercase
print(name.upper()) #uppercase
print(name.title()) #titlecase
```

    daisy
    DAISY
    Daisy



```python
#2.5
print('Shakespeare said "Brevity is the soul of wit"')
```

    Shakespeare said "Brevity is the soul of wit"



```python
#2.6
famous_person = "Shakespeare"
message = f'{famous_person} said "Brevity is the soul of wit"'
print(message)

```

    Shakespeare said "Brevity is the soul of wit"



```python
name = "\tWilliam\t\nWords\tworth\n"
print(name)
print(name.lstrip())
print(name.rstrip())
print(name.strip())


```

    	William	
    Words	worth
    
    William	
    Words	worth
    
    	William	
    Words	worth
    William	
    Words	worth

