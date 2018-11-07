# Python (Day I)

## Setup
- Download [data](http://swcarpentry.github.io/python-novice-inflammation/data/python-novice-inflammation-data.zip).
- Download [code](http://swcarpentry.github.io/python-novice-inflammation/code/python-novice-inflammation-code.zip)
- Create a folder on your desktop called `swc-python`.
- Move the above downloaded folders to `swc-python` and unzip them.
- Launch Jupyter notebook and navigate to `swc-python/python-novice-inflammation-data/data` folder within the file navigator.
- Start a new Python 3.0 notebook and give it a conenient name.

## Intro to Jupyter Notebook, iPython, and Python Interpreter

## Analyzing Patient Data
- Any Python interpreter can be used as a calculator.

```python
3 + 5 * 4
```

- To do more useful things, we need variables.

```python
weight_kg = 60
```

```python
weight_kg 
```

- Rules for variable names:
  - Can include letters, digits, and underscores
  - Cannot start with a digit
  - Case sensative

### Data Types
- Every object in Python has a *data type*.

- Three common data types:
  - Integer numbers
  - Floating point numbers
  - Strings

- To change `weight_kg` from an integer to floating-point value, we can do the following:

```python
weight_kg = 60.0
```

- To create a string, we need to use quotes (why).

```python
weight_kg_text = 'weight in kilograms:'
```

- `print()` displays the value in a variable.

- Functions in Python take inputs, do something, and provide an output.
  - Recipe = code in the funcion, ingredients = arguments.
- To *call* a function, type the function name and put the inputs in the parenthesis.

```python
print(weight_kg)
```

- `print()` takes multiple *arguments*.

```python
print(weight_kg_text, weight_kg)
```

- We can do arithmetic with variables inside the `print` function.

```python
print('weight in pounds:', 2.2 * weight_kg)
```

- This did not change the value of `weight_kg`.

```python
print(weight_kg)
```

- To update a variable, we need to explicitly update the variable's value.

```python
weight_kg = 65.0
print('weight in kilograms is now:', weight_kg)
```

### Loading data into Python
- One of the great things about both R and Python is the large amount of *libraries* developed by the respective communities.

- A library is a piece of code that extends the base functions of Python.

- To start playing with our data, we are going to use the NumPy library.
  - NumPy is a scientific computing library that provides all kinds of advanced math functions.
  - Particularly Linear Algebra capabilities.
  - Parts of it are written in C, so it is fast!

- To gain access to the functionality in NumPy, we begin by importing the library.

```python
import numpy
```

- NumPy allows us to load in our data and begin our analysis.

```python
numpy.loadtxt(fname='inflammation-01.csv', delimiter=',')
```

- `loadtxt()` is a function within NumPy that loads our CSV file.
  - filename
  - delimiter

- The `.` is common notation in Python that indicates that one object belongs to another.

- `1.` saves space by not including the decimal values. 

- We printed our data to the screen, but we did not save it into memory.

- We need to use a variable for this.

```python
data = numpy.loadtxt(fname='inflammation-01.csv', delimiter=',')
print(data)
```

> What type is the data type of our new `data` variable?

```python
print(type(data))
```

- This is a new data type for us.

- An `ndarray` is how NumPy represents an N-deimenstional array.

- For our particular data, the rows are the individual patients and the columns are their arthritis inflammation measurement for each day.

- The `type()` function only tells us we have an `ndarray`, but it doesn't tell us what type of objects are in the `ndarray`.

```python
print(data.dtype)
```

- To find out the array's dimensions, use the following:

```python
data.shape
```
- 60 rows, 40 columns.

- `shape` is an *attribute* of `data`.
  - Not a function, so no `()` at the end.
  - Use of `.` notation since it belongs to `data`.

- To pull a single number from the array, we use indexing.
  - Indexing with NumPy is ordered by row, then column.
  - 0-indexing

```python
rst value in data:', data[0, 0])
print('middle value in data:', data[30, 20])
```

- Slicing allows us to select whole sections.

```python
print(data[0:4, 0:10])
```

- We get rows 0, 1, 2, 3 with this slicing.
  - "Start at index 0 and go up to, but not including, index 4."

- Our slice does not have to start with 0.

```python
print(data[5:10, 0:10])
```

- If we don't include one of the numbers in the slice, Python automatically goes to the end of the axis in that direction.

```python
small = data[:3, 36:]
print('small is:')
print(small)
```

> What happens if we take of both numbers from the slice?

```python
small = data[:3, :]
print('small is:')
print(small)
```

- One of the nice things about NumPy is it allows us to do math operations on all the values in an array at once.

```python
doubledata = data * 2.0
```

```python
print('original:')
print(data[:3, 36:])
print('doubledata:')
print(doubledata[:3, 36:])
```

- We can add matrices that have the same dimensions.

```python
tripledata = doubledata + data
```

```python
print('tripledata:')
print(tripledata[:3, 36:])
```

- NumPy also gives us standard linear algebra operations, but that won't be the focus of today.

```python
numpy.multiply(small, small)
```

- We can do more complex things with our data, like finding the average inflammation for all patients on all days.

```python
print(numpy.mean(data))
```

- `mean()` takes an array as input. There are many functions that also do these kind of array operations.

```python
maxval = numpy.max(data)
minval = numpy.min(data)
stdval = numpy.std(data)

print('maximum inflammation:', maxval)
print('minimum inflammation:', minval)
print('standard deviation', stdval)
```
- When we do statistical analysis, we're usually more interested in questions like "What is the maximum inflammation for a single patient?"

```python
patient_0 = data[0, :]
print('maximum inflammation for patent 0:', patient_0.max())
```

- We can condense this by writing it as one line.

```python
print('maximum inflammation for patient 2:', numpy.max(data[2, :]))
```

- Most array functions take an axis parameter.

- To go across rows, use axis 0. To go across columns, use axis 1.

```python
print(numpy.mean(data, axis=0))
```

- To check ourselves, let's look at the shape of the results.

```python
print(numpy.mean(data, axis=0).shape)
```

- This is the average daily inflammation per patient.

- Going across columns, we get the following:

```python
print(numpy.mean(data, axis=1))
```

- Average inflammation per day for every patient.

## Repeating Actions with Loops
- Advantage of loops: automation.

- Suppose we want to print each letter in a word.

- One approach:

```python
word = 'lead'

print(word[0])
print(word[1])
print(word[2])
print(word[3])
```

> What's wrong with this approach?

- Doesn't scale.

- Fragile:

```python
word = 'tin'

print(word[0])
print(word[1])
print(word[2])
print(word[3])
```

- A better approach is to use a `for` loop.

``python
word = 'oxygen'
for char in word:
    print(char)
```

- Explain the anatomy of a `for` loop.

- Discuss indenting conventions in Python.

- You can call the loop variable anything you want, but you shouldn't.

> What do we expect the results to be for the following code?

```python
letter = 'z'
for letter in 'abc':
    print(letter)
print('after the loop, letter is', letter)
```

- A loop variable is used to record the progress of a loop. It still exists after the loop finishes.

- As we use loops more and more, you will find the `len()` function helpful.

```python
print(len('aeiou'))
```

> Exponentiation is built into Python: `print(5 ** 3)`. Write a loop that calculates the same result as `5 ** 3` using multiplication (and without exponentiation).

- Hint: a good function to use would be `range()`.

```python
for i in range(3):
    print(i)
```

```python
for i in range(3, 6):
    print(i)
```

```python
for i in range(3, 21, 3):
    print(i)
```

> Now you're ready for the challenge (put up slide).



```python
result = 1
for exponent in range(3):
    result = result * 5
print(result)
```

## Storing Multiple Values in Lists
- Lists allow us to store multipe values and treat them like one object.

- You make lists in Python by putting items in square brackets.

```python
odds = [1, 3, 5, 7]
print('adds are:', odds)
```

- Similarly to arrays, we can index items in lists.

```python
print('first and last:', odds[0], odds[-1])
```

- If we loop over a list, the loop variable is assigned elements one at a time.

```python
for number in odds:
    print(number)
```

- Items in lists can be changed, but characters in strings cannot.

```python
names = ['Curie', 'Darwing', 'Turing']  # Typo in Darwin's name
print('names is originally:', names)
names[1] = 'Darwin'  # Correct the name
print('final value of names:', names)
```

```python
name = 'Darwin'
name[0] = 'd'
```

- Anything after `#` is a comment (for human eyes only).

- Comments are important:
  - Collaborating
  - Yourself six months later

- Lists can contain any Python object, including lists.

```python
cupboard = [['pepper', 'zucchini', 'onion'], 
    ['cabbage', 'lettuce', 'garlic'],
    ['apple', 'pear', 'banana']]
```

> Can you print out the first list?

```python
print(cupboard[0])
```

> Can you print out just `'pepper'`?

```python
print(cupboard[0][0])
```

- Lists can contain different data types.

```python
sample_ages = [10, 12.5, 'Unkown']
print(sample_ages)
```

- Python gives us many functions for changing the contents of lists.

```python
odds.append(11)
print('odds after adding a value:', odds)
```

```python
del odds[0]
print('odds after removing the first element:', odds)
```

```python
odds.reverse()
print('odds after reversing:', odds)
```


