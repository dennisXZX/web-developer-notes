## Python

### Fundamentals

__define a class__

```python
class Database():
    # static variables
    URI = 'mongodb://127.0.0.1:27017'
    DATABASE = None

    # define a static method
    @staticmethod
    def initialize():
        client = pymongo.MongoClient(Database.URI)
        Database.DATABASE = client['fullstack']

    @staticmethod
    def insert(collection, data):
        Database.DATABASE[collection].insert(data)
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

# for each student in the MongoDB students collection, extract its mark value if it's greater than or equal to 85
collection = database['students']
students = [ student['mark'] for student in collection.find({}) if student['mark'] >= 85 ]
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
