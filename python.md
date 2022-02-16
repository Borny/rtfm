# Python

- Data Types
- Print
- Variables
- Lists
- Functions
- Set
- Tuple
- Dictionary
- Comments and Doc Strings
- Loops
- Conditional
- Packages
- Files
- Errors
- Object Oriented Programming (OOP)
- Modules

## Data Types

- Number
- Boolean
- String
- None

### Number

- underscore
- // operator
- int()
- float()

Numbers can be **integers** or **floats**: 1, 2, 4556, 4.345.

#### underscore

Underscore can be used and will be removed to store the value:  
`some_number = 1_000_000 => 1000000`

#### // operator

The // operator will get rid of the decimal part when deviding a number:

```python
number = 7 / 2 => will output 3.5
number = 7 // 2 => will output 3
```

#### int()

- int() => will round down and convert a string to a number (if it can be converted)
  `ìnt('1') => 1`
  `ìnt(2.7) => 2`
  `ìnt(True) => 1`
  `ìnt(False) => 0`
  `ìnt('Whats up ?') => error`

#### float()

- float() => will add a decimal to the number

### Boolean

**True** or **False**

### String

- Syntax
- Methods

#### Syntax

Any word surrounded by single or double quotes:  
'whatever you say' || "Okey dokey"  
Multiline text:  
'''
something written on
multiple lines
'''

#### Methods

- str() => will convert other type to string
  `str(456) => '456'`

- my_string[3] => will output the letter at index 3  
  `name = 'Spike'`  
  `name[2] => 'i'`

- replace() => will replace a letter  
  `name = 'Jet'`  
  `name.replace('e', "y') => 'Jyt'`

- format()

Can be used for placeholders in a string:

```python
  age = 20
  name='Vi'
  presentation='I am {} and I am {} years old'
  print(presentation.format(name, age))
```

Can control the output:

```python
fund=150.987
print('Funds: {:.2f}'.format(fund)) # will output '150.99'
print('Funds: {:10.2f}'.format(fund)) # will output 'Funds:     150.99'
print('Funds: {:>10.2f}'.format(fund)) # will align the placeholder to the right (default) 'Funds:     150.99'
print('Funds: {:<10.2f}'.format(fund)) # will align the placeholder to the left 'Funds:150.99     '
print('Funds: {:^10.2f}'.format(fund)) # will center the placeholder 'Funds:  150.99  '
```

### None

**None** is equivalent to **Null** in Javascript

## Print

Use the function **print()** to print to the console:

```python
some_variable = 'Python rocks!'
print(some_variable)
```

## Variables

Variables are declared using a name and the equal sign:  
`name = 1234`  
`some_name = 'and a value'` => use **underscore** for multiple words

Global scope vs Local scope

```python
name = 'Faye' # this variable is declared globally

def return_name():
  global name # will refer to the global variable 'name'
  name = input('Your name ') # declared locally, only available inside the function

return_name()
```

## Lists

- append()
- pop()
- len()
- comprehension
- copy
- is and ==
- all
- any

**Lists** in Python are like **Arrays** in Javascript:  
`some_list = [0, 23, True, 'some text', "some other text"]`  
`some_list[2] => True`  
`some_list[3] => 'some text'`

### append

Will add an item at the end of the List
`some_list = [2, 56]`  
 `some_list.append('some item') => some_list = [2, 56, 'some item']`

### pop

Will delete the last item

### len

Will return the **length** of a **List** or of a **String**

```python
name = 'Captain Carter'
len(name) # => 14

what_if_avengers = ['Thor','Captain Carter','Blackwidow','Killmonger','Star Lord', 'Doctor Strange Supreme']

len(what_if_avengers) # => 6
```

### comprehension

List comprehension makes it possible to create a **List** based on another List:

```python
justice_league = ['Batman', 'Superman', 'Aquaman']
justice_league_updated = [member + 'zzz' for member in justice_league]
=> justice_league_updated = ['Batmanzzz', 'Supermanzzz', 'Aquamanzzz']
```

Use **if** statement inside a List comprehension:

```python
simple_count = [1, 2, 3, 4]
double_count = [num * 2 for num in simple_count if num % 2 == 0 ] # will only double the numbers that can be divided by 2 (2, 4)
=> double_count = [4,8]
```

### Copy

Copying a list in the following way **won't** work:

```python
list_one = [1,2,3,4]
list_two = list_one
list_two[0] = 'See you!'
print(list_two) # will output ['See you!',2,3,4]
# but list_one will also holds ['See you!',2,3,4]
print(list_one) # will output ['See you!',2,3,4]
```

This is due to the fact that we made a copy of the **reference**, not the **value**.

Copy a list using the **:** syntax:

```python
first_list = [1,2,3,4]
second_list = first_list[:] # will copy the value of first_list
```

### Copy a range

Copy a range by adding indexes:

```python
first_list = [1,2,3,4]
second_list = first_list[2:4]
print(second_list) # => [3,4]
```

### is and ==

The **is** keyword will check for the same _object_ but **==** will check for the value:

```python
first_list = [1,2,3,4]
second_list = [1,2,3,4]
first_list == second_list # will return True as the lists have the same values
first_list is second_list # will return False as they are different objects. They have two different references
```

### all

Will check if all the values in a list are True:

```python
first_list = [True, True, False]
second_list = [True,True,True]
all(first_list) # will return False
all(second_list) # will return True
```

### any

Will check if all the values in a list are True:

```python
first_list = [True, True, False]
second_list = [False,False, False]
any(first_list) # will return True
any(second_list) # will return False
```

## Functions

- About
- Arguments
- Unpacking arguments

### About

**Function** (or _subfunction_, a function within the file) will be defined with the **def** keyword:

```python
def function_name(): # defining the function
        print('string to print to the console') # print method that will print a string to the console

function_name() # calling the function
```

### Arguments

**Passing arguments**:

```python
def greet(name):
  print('Hello ' + name + ' !')

greet('Spike') # will output 'Hello Spike !'
```

**Default arguments**:

```python
def greet(name='Cowboy'):
  print(name)

greet('Jet') # will output 'Jet'
greet() # will output 'Cowboy'
```

**Keyword arguments**. The order of the arguments can be changed by adding the name of the argument when calling the function:

```python
def greet(name='Cowboy', profession='Bounty Hunter'):
  print('Hi, my name is ' + name, 'I am a ' + profession)

greet(profession='Spartan', name="Leonidas") # will output 'Hi, my name is Leonidas, I am a Spartan'
```

### Unpacking arguments

Use the **\*** keyword to unpack arguments:

```python
def unpack(*args): # will create a list with the arguments passed
  for argument in args:
    print(argument)

unpack(1,2,3,4)
```

## Set

Used to keep a list of unique values. A Set is **mutable**, the values can be modified but duplicates are not allowed. Defined with **{}**:

`some_set = {'Lanfeust', 'Cixi'}`

## Tuple

A Tuple can't be modified (immutable) but the duplicates are allowed. Defined with **()**:

`some_tuple = ('Kran', 'Kunu', 'Garou')`

**Tuple unpacking**, unpack the values of a Tuple and store them in variables, like array destructuring in Javascript:

```python
some_tuple = ('Kran', 'Garou')
prince, monster = some_tuple # prince => 'Kran' // monster => 'Garou'
```

## Dictionary

Key-value pairs structure. Can be changed (mutable). No duplicate key allowed:

`some_dictionary = {'name': 'Hawaii', 'distance': 4000}`

### Comprehension

Will create a **Dictionary** from a **List**

```python
places_climate = [('Hawaii', 'Hot'), ('Iceland', 'Freezing'), ('New Zealand','Moderate')]
# turning that list into a dictionary
dict_places = {key: value for (key, value) in places_climate}
dict_places = {'Hawaii': 'Hot', 'Iceland': 'Freezing', 'New Zealand':'Moderate'}
```

## Comments and Doc Strings

```python
# This is a comment

def add_name_and_age(name, age):
  """ Returns the name and age

    Arguments:
      :name: The name of the person
      :age: The age of the person
  """

  return 'I am ' + str(age) + ' and my name is ' + name

print(add_name_and_age('Spike', 28))
```

## Loops

- for in
- while
- break
- continue

### for in

The **For** loop will go through a List and stop once the end of the List is reached.

```python
avengers = ['Cap','Thor','Hulk','Hawkeye']

for member in avengers:
  print(member) # will output every avengers in order
```

### while

The **while** loop will execute as long as the condition provided is true.

```python
while True:
  print('The loop is still going')

print('If you see this, the while loop stopped...')
```

### break

**break** will exit the loop:

```python
for element in list:
  if element == 'some value':
    print(element)
  elif element == '':
    break # the loop will stop if one of the element is equal to en empty string
```

### continue

**continue** will skip one iteration of the loop

```python
aliases = ['Cap', 'Shellhead', '', 'Legolas']

for alias in aliases:
    if alias == '':
        continue

    print(alias)

# will output all the aliases except the empty ones
```

## Conditional

- operators
- if - elif - else

### operators

- == => will check for equality
- != => will check for not equal
- \> => greater than
- < => smaller than
- \>= => greater or equal
- <= => smaller or equal
- is => will check for the same value and type
- in => is present in an Iterable
- not => will check for the opposite of **is** or **in**
- and => will check for another condition, all of them have to be met
- or => will check for another condition, only one of them needs to be met

### if - elif - else

```python
age = 38
if age > 40:
  return 'The age is smaller than 40'
elif age < 20:
  return 'The age is greater than 20'
else:
  return 'The age is between 20 and 40'

power = 'Super strenght'
print('S' not in power) # will print False
```

## Packages

- Import

### Import

Importing packages in the following ways:

```python
import random # importing the random library
random.shuffle(...) # calling the shuffle method inside the random library

import random as rm # using an alias on the random library
rm.shuffle(...) # calling the shuffle method with the random alias

from random import shuffle, random # importing only some specific methods
shuffle(...) # calling the imported method

from random import shuffle as sf # importing only some specific methods and using an alias
sf(...) # calling the imported method

# /!\ NOT RECOMMENDED
import random * # importing everything from the random library
shuffle(...) # calling the imported method
```

## Files

- Modes
- write
- read
- with

### Modes

- w => will write and override any existing content
- a => will write and append to any existing content
- r => will read the content

### Write

```python
f = open('text.tx', mode='w') # will create a file with a the write mode enabled
f.write('Surfing rocks!!') # will write into the file
f.close() # will close the edition of the file
```

### Read

```python
f = open('text.tx', mode='r') # will create a file with a the write and read mode enabled
file_content = f.read() # will store what's in the file into a variable that can be used later
file_content = f.readlines() # will output a list of strings. Each string will be a line from the file
file_content = f.readline() # will output the first line of the file
f.close()
print(file_content) # will print the content of the file
```

### with

The **with** helper keyword can be used to help opening and closing the file:

```python
with open('text.tx', mode='w')as f: # the 'with' will open the file in 'write' mode
    f.write('Testing....') # Some string will be written
    # the 'with' method will close the file for us
user_input = input('whatever you want...') # the user will be prompted to enter a value
print('done...') # this will not be printed as long as the user doesnt enter a value
```

## Errors

Use the **try/except** statement to handle errors:

```python
try:
  ...some code to run
except IOError:
  print('Something went wrong')
```

## Object Oriented Programming (OOP)

Using Classes groups all the variables and methods to a given feature and hence makes the code cleaner.

- Define a Class
- Special methods
- Using a Constructor
- Private attribute
- Inheritance
- Static
- Property

### Define a Class

```python
class Avenger: # declare the Class
  name = 'Avenger' # this is a Class attribute, it will be shared accross all instances

avenger_01 = Avenger() # declaring an instance of the Class
print(avenger_01.name) # will print 'Avenger'
```

### Special methods

Special methods are built-in methods and have the following syntax: `__special_method_name__`  
The double underscores are also called: 'dunder'.

### Using a Constructor

```python
class Avenger: # declare the Class
   def __init__(self, custom_name): # this is the constructor method for a Class.
    self.name = custom_name # This is an instance attribute

avenger_01 = Avenger('Tony') # creating an instance of the Class and passing a value
print(avenger_01.name) # will print 'Tony'
```

### Private attribute

Use `__` to declare a private attribute:

```python
class Avenger: # declare the Class
   def __init__(self, custom_name): # this is the constructor method for a Class.
    self.__name = custom_name # This is an instance attribute

avenger_01 = Avenger('Tony') # creating an instance of the Class and passing a value
print(avenger_01.__name) # will result in an error
```

### Inheritance

Create a Class based on anthoer Class:

```python
# Base Class
class Vehicle:
  def __init__(self, starting_top_speed=100):
    top_speed = starting_top_speed

  def drive(self):
    print('I am driving like a maniac: like {} fast').format(self.top_speed)

# Inheritance
from vehicle import Vehicle # import the Vehicle Class from the vehicle.py file (no need to declare the file extension)

class Bus(Vehicle): # declare the Class with the base Class as parameter
    def __init__(self, starting_top_speed=100):
        super().__init__(starting_top_speed=starting_top_speed) # the 'super' method gives us access to the base Class contructor

    def honk(self):
      print('The bus is honking!!')

bus01 = Bus(233)
bus01.drive()
```

### Static ????

### Property

An **attribute** is always accessible.  
A **property** have a getter and a setter method:

```python
class MyMath:

  @property # this acts as a Getter
  def name(self):
    return self.__name

  @name.setter # this acts as a Setter
  def name(self, val):
    self.__name = val
```

## Modules

Use **Anaconda** to install packages.  
`https://www.anaconda.com/products/individual`

It uses a virtual environment that makes it possible to only use certain packages for a given project and to avoid installing them globally. It might be useful when working with a specific version of a package.
