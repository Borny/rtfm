# Python

- Data Types
- Variables
- Lists
- Functions
- Set
- Tuple
- Dictionary
- Comments and Doc Strings
- Loops
- Conditional

## Data Types

- Number
- Boolean
- String
- None

### Number

Numbers can be **integers** or **floats**: 1, 2, 4556, 4.345.  
Underscore can be used and will be removed to store the value:  
`some_number = 1_000_000 // 1000000`

- int() => will round down and convert a string to a number (if it can be converted)
  `ìnt('1') => 1`
  `ìnt(2.7) => 2`
  `ìnt(True) => 1`
  `ìnt(False) => 0`
  `ìnt('Whats up ?') => error`

- float() => will add a decimal to the number

### Boolean

**True** or **False**

### String

Any word surrounded by single or double quotes:  
'whatever you say' || "Okey dokey"  
Multiline text:  
'''
something written on
multiple lines
'''

- str() => will convert other type to string
  `str(456) => '456'`
- my_string[3] => will output the letter at index 3  
  `name = 'Spike'`  
  `name[2] // 'i'`
- replace() => will replace a letter  
  `name = 'Jet'`  
  `name.replace('e', "y') // 'Jyt'`

### None

**None** is equivalent to **Null** in Javascript

## Variables

Variables are declared using a name and the equal sign:  
`name = 1234`  
`some_name = 'and a value'` => use **underscore** for multiple words

Global scope vs Local scope

```python
name = 'Faye' # this variable is declared globally

def return_name():
  name = input('Your name ') # declared locally, only available inside the function

return_name()
```

## Lists

- append()
- pop()
- len()
- comprehension

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

List comprehension makes it possible to creaate a List based on another List:

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

## Functions

- About
- Arguments

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
places_climate = [('Hawaii' 'Hot'), ('Iceland', 'Freezing'), ('New Zealand','Moderate')]
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

- == // will check for equality
- != // will check for not equal
- > // greater than
- < // smaller than
- > = // greater or equal
- <= // smaller or equal
- is // will check for the same value and type
- in // is present in an Iterable
- not // will check for the opposite of **is** or **in**
- and // will check for another condition, all of them have to be met
- or // will check for another condition, only one of them needs to be met

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
