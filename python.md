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

