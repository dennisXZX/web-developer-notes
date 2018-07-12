## Python

#### Fundamentals

Parse HTML

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

# get the string price £279.00
string_price = element.text.strip()

# convert the string price to number
price_without_symbol = float(string_price[1:])

if price_without_symbol < 200:
    print('You should buy it now!')
    print('The current price is {}.'.format(string_price))
else:
    print('No, too expensive!')
```

Format strings

```python
age = 5

# I am 5 years and 7 months old
print('I am {} years and {} months old'.format(age, 7))

# I am 8 years old, are you 8 years old too?
print('I am {age} years old, are you {age} years old too?'.format(age=age))
```

Get user input

```python
name = input('Enter your name: ')
print(name)
```

#### Launch a local server quickly using Python 2 or Python 3

```python
# python 2
python -m SimpleHTTPServer 1337

# python 3
python3 -m http.server 1337
```
