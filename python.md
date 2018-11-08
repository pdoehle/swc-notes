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

- We have to be careful when modifying lists because of how Python treats lists in memory.

```python
odds = [1, 3, 5, 7]
primes = odds
primes.append(2)
print('primes:', primes)
print('odds:', odds)
```

- Since lists can be large and take up lots of memory, Python didn't copy a new list into `primes`. It labeled the same list in memory with two variable names. Changing the underlying list showed up in both variables.

- If we want a new list copied over, we must exlicitly tell Python to create another copy of the list using the `list()` function.

```python
odds = [1, 3, 5, 7]
primes = list(odds)
primes.append(2)
print('primes:', primes)
print('odds:', odds)
```

> Use a `for` loop to convert the string `hello` into a list of letters. Hint: You can create an empty list like this: `letters = [] `.

```python
letters = []
for char in 'hello':
    letters.append(char)
print(letters)
```

## Making Choices
- Often an analysis requires us to make decisions; for exampe, we may want our code to alert us if there happens to be an outlier in our data. Python gives us *conditionals* for just this kind of task.

- We can have Python make a decision using an `if` statement.

```python
num = 37
if num > 100:
    print('greater')
else:
    print('not greater')
print('done')
```

- Explain the anotomy of an `if` statement.

- With `if` and `else`, only one or the other is ever executed.

- Go ahead and try the code out with a few different values and see what happens.

- The `else` statement is not required. Python simply moves on with the program if the condition is false.

```python
num = 53
print('before conditional...')
if num > 100:
    print(num, ' is greater than 100')
print('...after conditional')
```

- We can chain several conditionals together using `elif` which stands for "else if."

```python
num = -3

if num > 0:
    print(num, 'is positive')
elif num == 0:
    print(num, 'is zero')
else:
    print(num, 'is negative')
```

- `==` is used to test equality, while `=` is used for assignment.

- We can make conditionals more complex by using `and` and `or`.

```python
if (1 > 0) and (-1 > 0):
    print('both parts are true')
else:
    print('at least one part is false')
```

- `and` requires both conditions to be true while `or` only requires one of the conditions to be true.

```python
if (1 < 0) or (-1 < 0):
    print('at least one test is true')
```

- In Python we refer to `True` and `False` values as *booleans*. `<` is an example of a boolean operator.

> Conditionals in Python can handle more than just strickly boolean values. Try the following code and explain the rules for whether or not a conditional statement is executed.

```python
if '':
    print('empty string is true')
if 'word':
    print('word is true')
if []:
    print('empty list is true')
if [1, 2, 3]:
    print('non-empty list is true')
if 0:
    print('zero is true')
if 1:
    print('one is true')
```

## Creating Functions
- Python gives us many functions, but we can also create our own.

- Functions are self-contained pieces of code.
  - Simplify our work, e.g, `sin()` or `cos()`.
  - Can be reused.
    - Our code is easier to write and easier to read.

- Let's define a function that converts fahrenheit to celsius.

```python
def fahr_to_celsius(temp):
    return ((temp - 32) * (5/9))
```

- Discuss the anatomy of a function.

- If Python didn't do anything, then all is well. This is a function definition. Now let's call our new function.

```python
fahr_to_celius(32)
```

- We can use our function exactly like the built-in Python functions.

```python
print('freezing point of water:', fahr_to_celius(32), 'C')
print('boiling point of water:', fahr_to_celius(212), 'C')
```

- Let's create another function that turns Celsius into Kelvin.

```python
def celsius_to_kelvin(temp_c):
    return temp_c + 273.15

print('freezing point of water in Kelvin:', celsius_to_kelvin(0.))
```

- Let's make one last function that converts from fahrenheit to Kelvin. Instead of using the formula, we can *compose* the functions we already have.

```python
def fahr_to_kelvin(temp_f):
    temp_c = fahr_to_celsius(temp_f)
    temp_k = celsius_to_kelvin(temp_c)
    return temp_k

print('boiling point of water in Kelvin:', fahr_to_kelvin(212.0))
```

- This is an example of how we compose larger programs. Different tasks are broken into smaller functions and the functions are composed together to make programming a difficult task more manageable.

- A good rule of thumb is to not have more than a few dozen lines of code in any given function. Otherwise reading it and creating it become quite difficult.

- This approach also makes collaboration easier as each collaborator can take a subtask and program their function. As long as the outputs are correct for the given inputs, other collaborators can simply use their function without knowing every detail.

### Testing and Documenting
- Since we don't want other people to worry about all the details in our function, it's important to test our functions to make sure they provide the right output.

- Let's practice this principal with a function we create to offset a dataset so that it's mean value shifts to a user-defined value.

```python
def offset_mean(data, target_mean_value):
    return (data - numpy.mean(data)) + target_mean_value
```

- We can try this on our inflammation data, but we don't know what the results should look like. Let's create an array of zeros and test it on that.

```python
z = numpy.zeros((2,2))
print('original z\n', z)
print('offset mean\n', offset_mean(z, 3))
```

- This is looking good. Let's test it on our inflammation data.

```python
data = numpy.loadtxt(fname='inflammation-01.csv', delimiter=',')
print(offset_mean(data, 0))
```

- The default screen output didn't help much. Let's run some statistical tests to reassure ourselves.

```python
offset_data = offset_mean(data, 0)
print('original min, mean, and max are:', 
      numpy.min(data), 
      numpy.mean(data), 
      numpy.max(data))
print('min, mean, and max of offset data are:', 
      numpy.min(offset_data), 
      numpy.mean(offset_data), 
      numpy.max(offset_data))
```

- The minimum inflammation in the original was zero and the mean was 6.14875. A new minimum of -6.14875 seems like our function is working.

- The mean is not zero, but it's within a resaonable amount for roundoff error.

- It's likely our function is working.

- It's important to think about a process for testing the results of your function before coding it.

- Let's document our function so others (and ourselves in 6 months) can use it.

- One strategy is to use comments:

```python
# offset_mean(data, target_mean_value):
# return a new array containing the original data with its mean offset to match the desired value.
def offset_mean(data, target_mean_value):
    return (data - numpy.mean(data)) + target_mean_value
```

- With functions we have an even better tool for documentation.

```python
def offset_mean(data, target_mean_value):
    '''Return a new array containing the original data
       with its mean offset to match the desired value.
    Example: offset_mean([1, 2, 3], 0) => [-1, 0, 1]'''
    return (data - numpy.mean(data)) + target_mean_value

offset_mean?
```

- `'''` indicates a multi-line comment. You can write as many lines after this marker as you want and Python will ignore them until the next `'''`.

- When you look up a help entry for a function, it looks for a *docstring* at the beginning of the function.

- This allows us to write documentation that can be used by the help functionality in Jupyter Notebook.

### Defining Defaults

- The last piece of functions we need to focus on is parameters.

- Recall from earlier when we read in our data using the NumPy `loadtxt()` function.

```python
numpy.loadtxt('inflammation-01.csv', delimiter=',')
```


