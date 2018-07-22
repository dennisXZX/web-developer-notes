## Python

### Fundamentals

__check if a .py file is run directly__

```python
if __name__ == "__main__":
    print('the .py file is run directly')
else:
    print('the .py file has been imported')
```

__pip (package manager)__

`pip install packageName`

__lambda function (anonymous function)__

```python
myList = []

list(map(lambda num: num ** 2, myList))
```

__define a function accepting uncertain amount of parameters__

```python
# take infinite amount of parameters
# the args parameter is in a tuple format
def myfunc(*args):
    return sum(args) * 0.05
    
myfunc(2, 5)

# take infinite amount of key value pair
# the kwargs is in a dictionary format
def myfunc(**kwargs):
    if 'fruit' in kwargs:
        print('You have fruit')
    
myfunc(fruit='apple')    
```

__define a class and extend it__

```python
class Database():
    # static variables
    URI = 'mongodb://127.0.0.1:27017'
    DATABASE = None
    
    # constructor
    def __init__(self, name):
        self.name = name

    # define a static method
    @staticmethod
    def initialize():
        client = pymongo.MongoClient(Database.URI)
        Database.DATABASE = client['fullstack']

    @staticmethod
    def insert(collection, data):
        Database.DATABASE[collection].insert(data)
        
    # abstract method, which must be implemented by sub-class
    def shutdown(self):
        raise NotImplementedError("Subclass must implement this abstrat method")
        
# create a NewDatabase class inheriting the Database class
class NewDatabase(Database):
    def __init__(self):
        # call parent's constructor
        Database.__init__(self)
```

__Polymorphism__

```python
class Dog():
    def __init__(self, name):
        self.name = name

    def speak(self):
        return self.name + " says woof!"

class Cat():
    def __init__(self, name):
        self.name = name

    def speak(self):
        return self.name + " says meow!"

niko = Dog("niko")
felix = Cat("felix")

# this method does not care what pet class you would pass in
# as long as it has a speak() method
def pet_speak(pet):
    print(pet.speak())

pet_speak(niko)
pet_speak(felix)
```

__string slicing__

```python
# use step size on string slicing
'abcdefg'[::2]  # -> aceg

# use string slicing to reverse a string
'abcdefg'[::-1]  # -> gfedcba
```

__ternary conditional operator__

```python
# <expression1> if <condition> else <expression2>
self.id = uuid.uuid4().hex if id is None else id
```

__List operation__

`list.append('item')` to add a new item to the end of the list
`list.pop()` to remove the last item from the list
`list.pop(index)` to remove an item at the specified index
`list.sort()` to sort the list in place
`list.reverse()` to reverse the list in place

__List comprehension__

List comprehension is a succinct way to generate a list from another list

The verbose way:

```python
for item in list:
    if conditional:
        expression
```

The succinct way:

```python
# [ expression for item in list if conditional ]

# for each student in the MongoDB students collection, extract its mark if it's greater than or equal to 85
collection = database['students']
students = [ student['mark'] for student in collection.find({}) if student['mark'] >= 85 ]
```

__Tuple unpacking__

```python
mylist = [(1, 2), (3, 4)]

# tuple unpacking
for a, b in mylist:
    print(a)
  
# add a counter to an iterable
word = ['a', 'b']

for index, letter in enumerate(word):
    print(f'{index}: {letter}')
  
# zip two lists into one
list1 = [1, 2, 3]
list2 = ['a', 'b', 'c']

for item in zip(list1, list2):
    print(item) # -> (1, 'a')
```

__Dictionary operation__

`dict['key']` to retrieve the value associated with the key
`dict['key1']['key2']` to retrieve nested value
`dict['newKey'] = value` to add a new value to the dictionary
`dict.keys()` returns a list containing all the keys
`dict.values()` returns a list containing all the values
`dict.items()` returns a list of tuple (immutable list), each tuple contains a key and a value

Iterate through a dictioary:

```python
for key, value in dict.items():
    print(value)
```

__Set operation__

`myset = set()` to create a set
`myset.add(1)` to add an item to the set
`set(list)` to cast a list into a set (removing all the duplicate items)

__File operation__

`myfile = open('myfile.txt')` to open a file
`content = myfile.read()` to read all the content into a string (the cursor will move to the end of the file)
`myfile.seek(0)` to reset the cursor to the beginning of the file
`myfile.readlines()` to read the file into a list of string
`myfile.close()` to close the file

```python
# mode='r' is read only
# mode='w' is write only (will overwrite files or create new ones)
# mode='a' is append only (will add on to files)
# mode='r+' is reading and writing
# mode='w+' is writing and reading (will overwrite files or create new ones)

# open a file in read mode without the need of closing it manually
with open('myfile.txt', mode='r') as my_new_file:
    contents = my_new_file.read()

# open a file in append mode
with open('myfile.txt', mode='a') as my_new_file:
    my_new_file.write('new content')
```

__Parse HTML__

```python
import requests
from bs4 import BeautifulSoup

# fetch the HTML content of a page
request = requests.get('https://www.johnlewis.com/john-lewis-isaac-office-chair/p3575108')
content = request.content

'''
Our task is to retrieve the price from the page in this HTML markup
<p class="price price--large"> £279.00 </p>
'''

# parse the HTML
soup = BeautifulSoup(content, "html.parser")
element = soup.find("p", {"class": "price--large"})

# get the string price £279.00, removing all the whitespace
string_price = element.text.strip()

# convert the string price to number, dropping the £ sign
price_without_symbol = float(string_price[1:])

if price_without_symbol < 200:
    print('You should buy it now!')
    print('The current price is {}.'.format(string_price))
else:
    print('No, too expensive!')
```

__Format strings__

```python
age = 5

# I am 5 years and 7 months old
print('I am {} years and {} months old'.format(age, 7))

# I am 8 years old, are you 8 years old too?
print('I am {age} years old, are you {age} years old too?'.format(age=age))

print(f'I am {year} years and {month} months old')
```

__Get user input__

```python
name = input('Enter your name: ')
print(name)
```

### Launch a local server quickly using Python 2 or Python 3

```python
# python 2
python -m SimpleHTTPServer 1337

# python 3
python3 -m http.server 1337
```
