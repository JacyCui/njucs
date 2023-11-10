# Structure and Interpretation of Computer Programs

Jacy（家才）



[TOC]

## Lecture 01 Course Introduction

> Computer Science is no more about computers than astronomy is about telescopes.

- Introduction to Programing
- Managing Complexity: Mastering Abstraction; Programing Paradigms

**This is a challenging course that will demand a lot from you.**

![image-20210110153911333](C:\Users\23051\AppData\Roaming\Typora\typora-user-images\image-20210110153911333.png)

- Ask for help if you are stuck and make a good effort on all of the homework.

- Do not share code.

### Expressions

#### description

> An expression describes a computation and evaluates to a value.
>

#### types of expressions

- **Primitive Expressions**:  `2`(numbers); `"hello"`(strings); `add`(names)
- **Arithmetic Expressions**: `1 + 2`; `15 // 3`
- **Call Expressions**: `add(3, 4)`; `max(add(2, 3), 5*min(-1, 4))`

#### evaluating call expressions

1. **Evaluate**

- evaluate the operator subexpression
- evaluate each operand subexpression

2. **Apply**

- apply the value of the operator subexpression to the values of the operand subexpression

### Values

#### description

- Programs manipulate values


- Values represent different types of data


#### types of data

- **Integers**: `2`; `44`; `-3`
- **Strings**:`"hello"`; `"cs61a"`
- **Floats**: `3.14`; `4.5`; `-2.0`
- **Booleans**: `True`; `False`

> Expressions evaluate to values in one or more steps



## Lecture 02 Names & Functions

### Names

#### description

- Values can be assigned to names to make referring to them easier.
- A name can only be bound to a single value.

- One way to introduce a new name in a program is with an **assignment statement**.

#### statement

> Statements affect the program, but do not evaluate to values.

#### assignment

```python
x = 1 + 2*3 - 4//5
```

- Names are bound to values in an **environment**

To execute an assignment statement:

1. **Evaluate** the expression to the right of =.
2. **Bind** the value of the expression to the name to the left of = **in the current environment**.

### Functions

#### description

- Functions allow us to abstract away entire expressions and sequences of computation

- They take in some input (known as their arguments) and transform it into an output (the return value)

- We can create functions using **def statements**. Their input is given in a function call, and their output is given by a return statement.

#### defining functions

```python
from math import pi
def volum(r): #def <name>(<parameters>): -Function signature
    return (4/3) * pi * (r**3) #return <return expression> -Function body
```

- Function **signature** indicates name and number of arguments
- Function **body** defines the computation performed when the function is applied
- **Def statements** are a type of assignment that **bind** names to function values

##### Procedure for calling/applying user-defined functions (for now)

1. Create a new **environment frame** (we call it **local frame**, included in **global frame**)

2. Bind the function's **parameters**(formal arguments) to its **arguments**(actual arguments) in that frame

3. **Execute** the body of the function in the new environment

```python
from operator import mul #built-in function
def square(x):
    return mul(x, x) #user-defined function
y = square(-2)
```

#### Some built-in functions

```python
from math import pi #pi = Π = 3.1415926......
from math import e #e = 2.71828......
from math import exp #exp(x) = e ^ x
from math import expm1 #expm1(x) = e ^ x - 1
from math import log #log(x,y) = lnx/lny; log(x) = lnx
from math import log10(x) #log10(x) = lgx
from math import log1p(x) #log1p(x) = ln(1+x)
from math import ceil #ceil(x) = [x] + 1
from math import floor #floor(x) = [x]
from math import pow #pow(x, y) = x ^ y
from math import sqrt #sqrt(x) = the square root of x
from math import fabs #fabs(x) return the absolute of x and the return number is a float
from math import factorial #factorial(x) = x!
from math import hypot #hypot(x, y) = sqrt(x*x + y*y)
from math import sin
from math import asin
from math import cos
from math import acos
from math import tan
from math import atan
```

```python
abs(x) #abs(x) = |x| even if x is complex
complex(x, y) #create a complex with x to be real and y to be imaginative; y is not necessary
pow(x, y) #pow(x, y) = x ^ y
round(x, y) #round x to y decimal places, you don't have to input y if y = 0
input([prompt])
print(...)
```

### Summary

- Programs consist of **statements**, or instructions for the computer, containing **expressions**, which describe computation and evaluate to **values**.

- Values can be assigned to **names** to avoid repeating computations.

- An **assignment statement** assigns the value of an expression to a name in the current **environment**.

- **Functions** encapsulate a series of statements that maps **arguments** to a **return value**.

- A **def** **statement** creates a function object with certain **parameters** and a **body** and binds it to a name **in the current environment.**
- A **call expression** applies the value of its **operator**, a function, to the value(s) of its **operand**(s), some arguments.



## Lecture 03 Control

### None

> None indicates that nothing is returned.

- The special value **None** represents nothing in Python.
- A function that does not explicitly return a value will return **None**.
- Careful: **None** is not displayed by the interpreter as the value of an expression.

```python
>>> def does_not_return_square(x):
...     x * x #No return
...
>>> does_not_return_square(4) #None value is not displayed
>>> sixteen = does_not_return_square(4) #The name sixteen is now bound to the value None
>>> sixteen + 4
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'

```

### Pure Functions & Non-pure Functions

- Pure Functions: just return values
- Non-pure Functions: have side effects
  - A side effect isn't a value; it's anything that happens as a consequence of calling a function.

> `print(-2)`  Python displays the output `-2` , but this function return none.

- Nested Expressions with Print

```python
>>> print(print(1), print(2))
1
2
None None
```

### Control

#### Conditional statements (if statements)

```python
if <conditional expression>:
		<suite of statements> #Always start with if clause
elif <conditional expression>:
    	<suite of statements> #Zero or more elif clauses
else:
    	<suite of statements> #Zero or one else clause, always at the end
```

> If the expression evaluates to true or the header is an else, execute the suite and skip the remaining headers

- an **if** example

```python
def abs(x):
    """Return the absolute value of x."""
    if x < 0: 
        return -x
    elif x == 0:
        return 0
    else:
        return x
    #There are two boolean contexts in this assignment statement.
```

#### Boolean Contexts

> **Boolean context** is any place where an expression is evaluated to check if it's a **True value** or a **False value**.

- False values in Python: `False`, `None`, `0`, ` `(empty strings)
- True values: everything else

##### Boolean Expressions

- `<exp1> and <exp2> and <exp3> and ...`
  - Evaluate to the first false value.
  - If none are false, evaluate to the last expression
- `<exp1> or <exp2> or <exp3> or ...`
  - Evaluate to the first true value.
  - If none are true, evaluates to the last expression
- `not <exp>`

> When using `and` or `or` , there could be short-circuiting. Because usually python won't evaluate all the expressions contained.

### Iteration

#### While statements

```python
i, total = 0, 0
while i < 3:
    i = i + 1
    total = total + i
```

- **Execution Rule for While Statements:**

1. Evaluate the header’s expression

2. If it is a true value, execute the (whole) suite, then return to step 1

##### The Fibonacci Sequence (example)

```python
def fib(n):
    """Compute the nth Fibonacci number, for N >= 1"""
    pred, curr = 0, 1
    k = 1
    while k < n:
        pred, curr = curr, pred + curr
        k = k + 1
	return curr
```

> The next Fibonacci number is the sum of the current one and its predecessor.

### Summary

- **print** vs **None**
  - None represents when an expressions doesn’t evaluate to a value
  - Print displays values in the interpreter
- **Control**
  - Allow for the interpreter to selectively or repetitively execute parts of a program
- **Iteration**
  - A particular variant of control which is based in the idea of repeating operations to compute a value



## Lecture 04  Higher-Order Functions

### Descriptions

- Functions are **first-class**, meaning they can be manipulated as values
- A **higher-order function** is:
  - A function that takes a function as an argument
  - and/or
  - A function that takes a function as a return value

### Generalization

#### generalizing patterns with arguments

```python
from math import pi, sqrt

def area_square(r):
    assert r > 0, 'The input length must be positive' #assert <boolean expression>, '<words printed when the value of the boolean expression is false>'
    return r * r

def area_circle(r):
    assert r > 0, 'The input length must be positive'
    return pi * r * r

def area_hexagon(r):
    assert r > 0, 'The input length must be positive'
    return 3 * sqrt(3) * r * r / 2
```

- Finding common structure allows for shared implementation

```python
from math imort pi, sqrt

def area(r, shape_constant): #using arguement to generalize similar patterns
    assert r > 0, 'The input length must be positive'
    return r * r * shape_constant #a kind of superior abstraction

def area_square(r):
    return area(r, 1)

def area_circle(r):
    return area(r, pi)

def area_hexagon(r):
    return area(r, 3 * sqrt(3) / 2)
```

#### generalizing over-computational processes

```python
def sum_naturals(n):
    i, total = 1, 0
    while i <= n:
        total, i = total + i, i + 1
    return total

def sum_cubes(n):
    i, total = 1, 0
    while i <= n:
        total, i = total + pow(i, 3), i + 1
    return total
```

- The common structure among functions may be a computational process, rather than a number.

```python
def summation(n, term): #here term is a parameter that will be bound to a function
	""" Sum the first n terms of a sequence
	
	>>> summation(5, cube)
	225
	>>> summation(5, identity) #The cube function is passed as an argument value
	15
	"""
    total, k = 0, 1
    while k <= n:
        total, k = total + term(k), k + 1 #The function bound to term gets called here
    return total

def identity(k):
    return k

def cube(k): #function of a single argument
    return pow(k, 3)

def sum_naturals(n):
    return summation(n, identity)

def sum_cubes(n):
    return summation(n, cube)
```

#### functions as return values

> Functions defined within other function bodies are bound to names in a local frame

```python
def make_adder(n): #A function that returns a function
    """Return a function that takes one argument k and returns k + n.
    
    >>> add_three = make_adder(3) #The name add_three is bound to a function
    >>> add_three(4)
    7
    """
    def adder(k): #A def statement within annother def statement
        return k + n #Can refer to names in the enclosing function
    return adder
```

- the concept of scope

#### call expressions as operator

```python
>>> make_adder(1)(2)
3
```

> make_adder(1) : An call expression that evaluates to a function
>
> make_adder(1)(2): An nested call expression that eventually evalutes to the outside function's return value

### Summary

- Higher-order-function: any function that either accepts a function as an argument and/or returns a function
- Why are these useful?
  - Generalize over different form of computation
  - Helps remove repetitive segments of code
- We saw nested functions(closures) can access variables in outer function through static scoping.

> A more complex example
>
> ```python
> def make_adder(n):
>     def adder(k):
>         return k + n
>     return adder
> 
> def square(x):
>     return x * x
> 
> 
> def compose1(f, g):
>     def h(x):
>         return f(g(x))
>     return h
> ```
>
> ```python
> >>> compose1(square, make_adder(2))(3)
> 25
> ```



## Lecture 05 Environment Diagrams

### Environment Diagrams

#### What ?

- A visual tool to keep track of bindings and state of a computer program
- It can be applied to similar languages aside of python

#### Why?

- conceptual

  - understand **why** programs work the way they do
  - confidently **predict** how a program will behave

- helpful for debugging

  - when you're really stuck,

    diagramming code > staring at lines of cod

#### What have we seen so far

- Assignment Statement
- Def Statement
- Call Expressions

#### Terminology: Frames

- A **frame** keeps track of variable-to-value bindings
  - Every call expression has a corresponding frame
- **Global**, a.k.a. the global frame, is the starting frame
  - It doesn't correspond to a specific call expression
- **Parent frames**
  - The parent of a function is the frame in which it was defined
  - If you can't find a variable in the current frame, you check it's parent, and so on. If you can't find the variable, **NameError**

> **Review: Evaluation Order**
>
> Remember to evaluate the operator, then the operand(s), then apply the operator onto the operand(s)
>
> The environment diagram should reflect Python's evaluation.

#### Variable Lookup

- Lookup name in the current frame
- Lookup name in the parent frame, its parent frame, etc..
- Stop at the global frame
- If not found, an error is thrown

> Important: There was no lookup done in f1 since the parent of f2 was Global 

#### Evaluation vs Apply

> A new frame is created only when an application happens. Evaluation will not create a new frame.

### Lambda Expressions

> Expressions that evaluate to functions!

```python
>>> square = lambda x: x * x # A function with parameter x that returns the value of x * x
>>> square
<function <lambda> ... >
>>> square(4)
16
>>> x = square(5)
>>> x
25
```

#### Lambda Expressions vs def Statements

```python
square = lambda x: x * x
```

```python
def square(x):
    return x * x
```

- Both create a function with the same behavior
- The parent frame of each function is the frame in which they were defined
- Both bind the function to the same name
- Only the **def** statement gives the function an intrinsic name
  - you need to use an assignment statement to bind the function created by lambda expression to a name in order to refer to it later 

### Summary

- **Environment Diagrams** formalize the evaluation procedure for Python

  - Understanding them will help you think deeply about how the code that you are writing actually works

- **Lambda** functions are similar to functions defined with **def**, but are **nameless**

- A **Higher Order Function** is a function that either takes in functions as an argument (shown earlier) and/or returns a function as a return value (will see soon)



## Lecture 06 Recursion

> **Recursion** is useful for solving problems with a naturally repeating structure - they are defined in terms of themselves.
>
> It requires you to find patterns of smaller problems, and to define the smallest problem possible.

### Recursive Functions

- A function is called **recursive** if the body of that function calls itself, either directly or indirectly
- This implies that executing the body of a recursive function may require **applying that function multiple times**
- Recursion is inherently tied to functional abstraction

### Structure of a Recursive Function

- One or more **base cases**, usually the smallest input
- One or more ways of **reducing the problem**, and then **solving the smaller problem using recursion**
- One or more ways of **using the solution to each smaller problem** to solve our larger problem.

```python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)
```

### How to Trust Functional Abstraction

Verifying the correctness of recursive functions

1. Verify that the base cases work as expected
2. For each larger case, verify that it works by **assuming the smaller recursive calls are correct**

### Some Examples

#### Count Up

> Let’s implement a recursive function to print the numbers from 1 to `n`. Assume `n` is positive.

```python
def count_up(n):
	if n == 1: 
		print(1)
	else:
		count_up(n-1)
		print(n)
```

#### Sum Digits

> Let’s implement a recursive function to sum all the digits of `n`. Assume `n` is positive.

```python
def sum_digits(n):
     if n < 10:
             return n
     return sum_digits(n // 10) + (n % 10)
```

#### Cascade

> Print out a cascading tree of a positive integer n.

```python
def cascade(n):
    if n < 10:
        print(n)
    else:
        print(n)
        cascade(n // 10)
        print(n)
#等价形式
def cascade(n):
    print(n)
    if n >= 10:    
        cascade(n // 10)
        print(n)
```

#### Counting Partitions

> Count the number of ways to give out `n`(>0) pieces of chocolate if nobody can have more than `m`(>0) pieces.

**Ideas: (take count_part(6, 4) as an example)**

- Find simpler instances of the problem
- Explore two possibilities:
  - Use a 4
  - Don't use a 4
- Solve two simpler problems:
  - count_part(2, 4)
  - count_part(6, 3)
- Sum up the results of these smaller problems!

**How do we know we're done?**

- If `n` is negative, then we cannot get to a valid partition
- If `n` is 0, then we have arrived at a valid partition
- If the largest piece we can use is 0, then we cannot get to a valid partition

```python
 def count_part(n, m):
    if n == 0:
        return 1
    elif n < 0:
        return 0
    elif m == 0:
        return 0
    else:
        with_m = count_part(n-m, m)
        without_m = count_part(n, m-1)
        return with_m + without_m
```

### Summary

- **Recursive functions** are functions that call themselves in their body one or more times
  - This allows us to break the problem down into smaller pieces
  - Using functional abstraction, we do not have to worry about how those smaller problems are solved

- A recursive function has a **base case** to define its smallest problem, and one or more recursive calls
  - If we know the base case is correct, and that we get the correct solution assuming the recursive calls work, then we know the function is correct

- Evaluating recursive calls follow the same rules we’ve talked about so far
- **Recursion** has three main components
  - **Base case/s**: The simplest form of the problem
  - **Recursive call/s**: Smaller version of the problem
  - Use the solution to the smaller version of the problem to arrive at the solution to the original problem
- **Tree recursion** makes multiple recursive calls and explores different choices
- Use doctests and your own examples to help you figure out the simplest forms and how to make the problem smaller



## Lecture 07 Sequences & Data Abstraction

### Sequence

> A **sequence** is an ordered collection of values.
>
> - **strings**: sequence of characters
> - **lists**: sequence of values of any data type

#### Sequence Abstraction

- All sequences have **finite length**.

- Each element in a sequence has a discrete integer **index**.
- Sequences share common behaviors based on the shared trait of having a **finite length** and **indexed elements**(**Remember that the elements are indexed from `0`**).
  - **Retrieve** an element at a particular position
  - Create a copy of a **subsequence**
  - **Check** for membership
  - **Concatenate** two sequences together
  - ......

##### What can you do with sequences?

> **Get item:** get the **ith** element `<seq>[i]`
>
> ```python
> >>> lst = [1, 2, 3, 4, 5]
> >>> lst[2]
> 3
> >>> lst[-1] #When the index is negative, ‘-’means from right to left.
> 5
> >>> lst[-2]
> 4
> >>> "cs61a"[3]
> '1'
> ```
>
> **Check membership:** check if the value of <expr> is in <seq>  `<expr> in <seq>`
>
> ```python
> >>> 3 in [1, 2, 3, 4, 5]
> True
> >>> 'z' in "socks"
> False
> >>> 2 + 4 in [7, 6, 5, 4, 3]
> True
> ```
>
> **Slice a subsequence:** create a copy of the sequence from **i(included)** to **j(unincluded)**  `<seq>[i:j:skip]`
>
> ```python
> >>> lst = [1, 2, 3, 4, 5]
> >>> lst[1:4]
> [2, 3, 4]
> >>> "lolololololol"[3::2]
> 'ooooo'
> ```
>
> **Concatenate:** combine two sequences into a single sequence  `<s1> + <s2>`
>
> ```python
> >>> [1, 2, 3] + [4, 5]
> [1, 2, 3, 4, 5]
> >>> "hello "  + "world"
> "hello world"
> >>> [-1] + [0] + [1]
> [-1, 0, 1]
> ```

#### Sequence Processing

##### Iterating through sequences

> You can use a **for statement** to iterate through the elements of a sequence:
>
> ​		`for <name> in <seq>:`
>
> ​				`<body>`

Rules for execution:

For each element in `<seq>`:

1. Bind it to `<name>`  **in current frame**

2. Execute `<body>`

```python
i = 0
for elem in [8, 9, 10]:
    print(i, ":", elem)
    i += 1
```

#####  Range

> The **range** function creates a sequence containing the values within a specified range.
>
> `range(<start>, <end>, <skip> )`
>
> Creates a range object from <start> (inclusive) to <end> (exclusive), skipping every <skip> element, which is useful for looping.
>
> You may just input one number using range, and when this happens, start = 0 and skip = 1, what you input is the <end>  (exclusive).

```python
>>> for e in range(1, 8, 2):
...     print(e)
1
3
5
7  
```

```python
>>> lst = [8, 9, 10]
>>> for i in range(len(lst)):
...     print(i, ":", lst[i])
0: 8
1: 9
2: 10
```

##### List Comprehensions

> You can **create out a list out of a sequence** using a list comprehension:
>
> ​		`[<expr> for <name> in <seq> if <cond>]`
>
> It is similar to a call expression, which will create a local frame and do the execution in the local frame. So, binding to <name>  will not change the bindings in its parent frames.
>
> `if <cond>` doesn't have to be there when it is true all the time.
>
> ```python
> >>> [x ** 2 for x in [1, 2, 3]]
> [1, 4, 9]
> >>> [c + “0” for c in "cs61a"]
> ['c0', 's0', '60', '10', 'a0']
> >>> [e for e in "skate" if e > "m"]
> ['s', 't']
> >>> [[e, e+1] for e in [1, 2, 3]]
> [[1, 2], [2, 3], [3, 4]]
> ```

**Rules for execution**

1. Create an empty result list that will be the value of the list comprehension 

2. For each element in <seq>:
   - Bind that element to <name> in the a newly created frame
   - If <cond> evaluates to a true value, then add the value of <expr> to the result list

### Data Abstraction

#### Description

- **Compound values** combine other values together
  - A date: a year, a month, and a day
  - A geographic position: latitude and longitude

- **Data abstraction** lets us manipulate compound values as units

- Isolate two parts of any program that uses data:
  - How data are **represented** (as parts)
  - How data are **manipulated** (as units)

- Data abstraction: A methodology by which functions enforce an abstraction barrier between **representation** and **use**

#### Rational Numbers (example)

##### Constructor and Selectors

- A rational number equals to a numerator dividing a denominator

- Exact representation as fractions includes a pair of integers. However,  as soon as division occurs, the exact representation may be lost! 

- Assume we can compose and decompose rational numbers:

```python
rational(n, d) #returns a rational number x
numer(x) #returns the numerator of x
denom(x) #returns the denominator of x
```

> Here we call rational(n, d) **Constructor** and call numer(x) and demon(x) **Selectors**

##### Rational Numbers Arithmetic Implementation

```python
def mul_rational(x, y):
    return rational(numer(x) * numer(y), denom(x) * denom(y))

def add_rational(x, y):
    nx, dx = numer(x), denom(x)
    ny, dy = numer(y), denom(y)
    return rational(nx * dy + ny * dx, dx * dy)

def print_rational(x):
    print(numer(x), '/', denom(x))
    
def rationals_are_equal(x, y):
    return numer(x) * denom(y) == numer(y) * denom(x)
```

- These functions implement an abstract representation for rational numbers and their calculations.

##### Representing Rational Numbers

```python
def rational(n, d):
    """A representation of the rational number N/D."""
    return [n, d] #Construct a list

def numer(x):
    """Return the numerator of rational number X."""
    return x[0]

def denom(x):
    """Return the denominator of rational number X."""
    return x[1] #Slect item from a list
```

*alternative way*

```python
def rational(n, d):
    def slect(name):
        if name == 'n':
            return n
        elif name == 'd':
            return d
    return slect

def numer(x):
    return x('n')

def denom(x):
    return x('d')
```

> In many programming languages, functions can return functions. And when a function returns a function, the returning function keeps a reference to all the variables that it needs to execute, which are declared in the parent function.
>
> **That’s exactly what a closure is. It is a bucket of references to variables a function needs to execute which are declared outside its scope.** 

##### Reducing to Lowest Terms

```python
from fractions import gcd #Greatest Common Divisor

def rational(n, d):
    """A representation of the rational number N/D."""
    g = gcd(n, d) # Always has the sign of d
    return [n//g, d//g]
```

### Dictionaries

> Dictionaries are unordered collections of key-value pairs.
>
> Each value is bound to a key in a dictionary.

```python
message_dict = {
    'name' : 'cuijiacai',
    'age' : '18',
    'sex' : 'male',
    'weight' : '63',
    'height' : '171'}
```

Dictionary keys do have two **restrictions**:

- A key of a dictionary **cannot be** a list or a dictionary (or any mutable type)
  - This first restriction is tied to Python's underlying implementation of dictionaries
- Two **keys cannot be equal**; There can be at most one value for a given key
  - The second restriction is part of the dictionary abstraction

> If you want to associate multiple values with a key, store them all in a sequence value

#### Manipulation for dictionaries

- **Get item:**

  ```python
  >>> <dictionary-name>[<key>]
  <value>
  ```

- **Modify:**

  ```python
  >>> <dictionary-name>[<key>] = <value>
  ```

  > If the key exists in the dictionary, this assignment statement will change the value corresponding to this key to the new value to the right of =.
  >
  > If the key doesn't exist in the dictionary, this assignment statement will create a new key-value bond in the dictionary.

  ```python
  >>> <dictionary-name>.pop(<key>)
  value
  ```

  > The method `pop` will delete the key-value bond in the dictionary and return the value corresponding to the deleted bond.

  > We can still use the built-in function `len` to get the number of key-value bonds in the dictionary.

- **Compose:**

  ```python
  >>> <dictionary1>.update(<dictionary2>)
  ```

  > The method update will compose two dictionaries together. However, if the key-value bonds in dictionary2 exist in dictionary1, they will replace them.

- **Travel:**

  ```python
  for key, value in <dictionary>.items():
      #iterate over each key-value bond
  for key in <dictionary>.keys():
      #iterate over each key
  for value in <dictionary>.values():
      #iterate over each value 
  ```



## Lecture 08 Trees

### Box-and-Pointer Notation

#### The closure property of data types

- A method for combining data values satisfies the **closure property** if:
  - The result of combination can itself be combined using the same method
- Closure is powerful because it permits us to create **hierarchical structures**
- Hierarchical structures are made up of parts, which themselves are made up of parts, and so on

> Lists can contain lists as elements (in addition to anything else)

#### Box-and-pointer notation in environment diagrams

> - Lists are represented as a row of index-labeled adjacent boxes, one per element
>
> - Each box either **contains a primitive value** or **points to a compound value**

#### Slicing

> Slicing creates new values

```python
digits = [1, 8, 2, 8]
start = digits[:1]
middle = digits[1:3]
end = digits[3:]
full = digits[:]
```

### Processing Container Values

#### Sequence aggregation

> Several built-in functions take iterable arguments and aggregate them into a value

- `sum(iterable[,start])`
  - Return the sum of an iterable of numbers (NOT strings) plus the value of parameter 'start' (which defaults to 0). 
  - When the iterable is empty, return start.
- `max(iterable[,key = func])` ; `max(a, b, c, ...[, key = func])`
  - With a single iterable argument, return its largest item.
  - With two or more arguments, return the largest argument.
- `all(iterable)`
  - Return True if bool(x) is True for all values x in the iterable.
  - If the iterable is empty, return True.

### Tree Abstraction

#### Recursive description (wooden trees)

- A **tree** has a root and a list of **branches**
- Each branch is a **tree**
- A tree with zero branches is called a **leaf**

#### Relative description (family trees)

- Each location in a tree is called a **node**
- Each **node** has a **label value**
- One node can be the **parent/child** of another

> People often refer to values by their locations: "each parent is the sum of its children"

### Implementing the Tree Abstraction

> A tree has a label value and a list of branches

```python
def tree(lable, branches = []):
    for branch in branches:
        assert is_tree(branch) #Verifies the tree definition
    return [label] + list(branches) #Creates a list from a sequence of branches

def label(tree):
    return tree[0]

def branches(tree):
    return tree[1:]

def is_tree(tree):
    if type(tree) != list or len(tree) < 1: #Verifies that tree is bound to a list
        return False
    for branch in branches(tree):
        if not is_tree(branch):
            return False
    return True

def is_leaf(tree):
    return not branches(tree)
```

### Tree Processing

#### Tree processing uses recursion

- Processing a leaf is often the base case of a tree processing function
- The recursive case typically makes a recursive call on each branch, then aggregates

```python
def count_leaves(t):
    """Count the leaves of a tree."""
    if is_leaf(t):
        return 1
    else:
        branch_counts = [count_leaves(b) for b in branches(t)]
        return sum(branch_counts)
```

> Hint: If you **sum** a list of lists, you get a list containing the elements of those lists

```python
def leaves(tree):
    """Return a list containing the leaves of tree.
    
    >>> leaves(fib_tree(5))
    [1, 0, 1, 0, 1, 1, 0, 1]
    """
    if is_leaf(tree):
        return [label(tree)]
    return sum([leaves (b) for b in branches(tree)],[])
```

#### Creating trees

> A funtion that creates a tree from another tree is typically also recursive

```python
def increment_leaves(t):
    """Return a tree like t but with leaf values incremented."""
    if is_leaf(t):
        return tree(label(t) + 1)
    else:
        bs = [increment_leaves(b) for b in branches(t)]
        return tree(label(t), bs)
```

```python
def increment(t):
    """Return a tree like t but with all node values incremented."""
    return tree(label(t) + 1, [increment(b) for b in branches(t)])
```

#### Printing trees

```python
def print_tree(t, indent = 0):
    """Print a representation of this tree in which each node is indented by 
    two spaces times its depth from the root.
    
    >>> print_tree(tree(1))
    1
    >>> print_tree(tree(1,[tree(2)]))
    1
      2
    """
    print('  ' * indent + str(label(t)))
    for b in branches(t):
        print_tree(b, indent + 1)
```



## Lecture 09 Mutable Values

### Objects

#### Descriptions

- Objects represent information
- They consist of data and behavior, bundled together to create abstractions
- Objects can represent things, but also properties, interactions and processes
- A type of object is called a class; **classes** are first-class values in Python
- Object-oriented programming:
  - A metaphor for organizing large programs
  - Special syntax that can improve the composition of programs
- In Python, every value is an object
  - All **objects** have **attributes**
  - A lot of data manipulation happens through object **methods**
  - Functions do one thing; objects do many related things

### Example: Strings

#### Representing Strings: the ASCII Standard

> American Standard Code for information interchange.
>
> ASCII Code Chart

- Layout was chosen to support sorting by character code
- Rows indexed 2-5 are a useful 6-bit (64 element) subset
- Control characters were designed for transmission

#### Representing Strings: the Unicode Standard

- 137,994 characters in Unicode 12.1
- 150 scripts (organized)
- Enumeration of character properties, such as case
- Supports bidirectional display order
- A canonical name for every character

### Mutation Operations

#### Some objects can change

- Only objects of mutable types can change: lists & dictionaries

#### Mutation can happen within a function call

> A function can change the value of any object in its scope

```Python
def mystery(s):
    s.pop()
    s.pop() #s.pop() means leaving out the last element in the sequence s and returning it
    
def another_mystery():
    four.pop()
    four.pop()
```

```python
>>> four = [1, 2, 3, 4]
>>> len(four)
4
>>> mystery(four)
>>> len(four)
2

>>> four = [1, 2, 3, 4]
>>> len(four)
4
>>> another_mystery() #No arguments!
>>> len(four)
2
```

### Tuples

#### Tuples are immutable sequences

> Immutable values are protected from mutation

```python
>>> turtle = (1, 2, 3)
>>> ooze() #ooze can change turtle's binding
>>> turtle
(1, 2, 3)

>>> turtle = [1, 2, 3]
>>> ooze()
>>> turtle
['Anything could be here']
```

- The value of an expression can change because of changes in names or objects

Name change:

```python
>>> x = 2
>>> x + x
4
>>> x = 3
>>> x + x
6
```

Object mutation:

```python
>>> x = [1, 2]
>>> x + x
[1, 2, 1, 2]
>>> x.append(3)
>>> x + x
[1, 2, 3, 1, 2, 3]
```

- An immutable sequence may still change if it *contains* a mutable value as an element

```python
>>> s = ([1, 2], 3)
>>> s[0] = 5
ERROR
>>> s = ([1, 2], 3)
>>> s[0][0] = 4
>>> s
([4, 2], 3)
```

### Mutation

#### Sameness and change

- As long as we never modify objects, a compound object is just the totality of its pieces
- A rational number is just its numerator and denomenator
- This view is no longer valid in the presence of change
- A compound data object has an "identity" in addition to the pieces of which it is composed
- A list is still "the same" list even if we change its contents
- Conversely, we could have two lists that happen to have the same contents, but are different

```python
>>> a = [10]
>>> b = a
>>> a == b
True
>>> a
[10, 20]
>>> b
[10, 20]
>>>a == b
True
```

```python
>>> a = [10]
>>> b = [10]
>>> a == b
True
>>> b.append(20)
>>> a
[10]
>>> b
[10, 20]
>>> a == b
False
```

#### Identity operators

> Identical objects are always equal values

- Identity `<exp0> is <exp1>` evaluates to True if both <exp0> and <exp1> evaluates to the same object.
- Equality `<exp0> == <exp1>` evaluates to True if both <exp0> and <eap1> evaluates to equal values.

#### Mutable default arguments are dangerous

> A default argument value is part of a function value, not generated by a call

```python
>>> def f(s = []):
... 	s.append(3)
... 	return len(s)
...
>>> f()
1
>>> f()
2
>>> f()
3
# Each time the function is called, s is bound to the same value!
```

### Lists

#### List in environment diagrams

Assume that before each example below we execute:

s = [2, 3]

t = [5, 6]

| Operation                                                    | **Example**   | **Result**          |
| :----------------------------------------------------------- | ------------- | ------------------- |
| **append** adds one element to a list                        | `s.append(t)` | s ~ [2, 3, [5, 6]]  |
|                                                              | `t = 0`       | t ~ 0               |
| **extend** adds all elements in one list to another list     | `s.extend(t)` | s ~ [2, 3, 5, 6]    |
|                                                              | `t[1] = 0`    | t ~ [5, 0]          |
| **addition** & **slicing** create new lists containing existing elements | `a = s + [t]` | s ~ [2, 3]          |
|                                                              | `b = a[1:]`   | t ~ [5, 0]          |
|                                                              | `a[1] = 9`    | a ~ [2, 9, [5, 0]]  |
|                                                              | `b[1][1] = 0` | b ~ [3, [5, 0]]     |
| The **list** function also creates a new list containing existing elements | t = list(s)   | s ~ [2, 0]          |
|                                                              | s[1] = 0      | t ~  [2, 3]         |
| **slice assignment** replaces a slice with new values        | s[0:0] = t    | s ~ [5, 6, 2, 5, 6] |
|                                                              | s[3:] = t     | t ~ [5, 0]          |
|                                                              | t[1] = 0      |                     |
| **pop** removes & returns the last element                   | t  = s.pop()  | s ~ [2]             |
|                                                              |               | t ~ 3               |
| **remove** removes the first element equal to the argument   | t.extend(t)   | s ~ [2, 3]          |
|                                                              | t.remove(5)   | t ~ [6, 5, 6]       |
| **slice assignment** can remove elements from a list by assigning [] to a slice. | s[:1] = []    | s ~ [3]             |
|                                                              | t[0:2] = []   | t ~ []              |

> Only method pop will return value, other methods will return None.

#### Lists in lists in lists in environment diagrams

> Remember that when a compound value os called, it always uses a reference



## Lecture 10 Mutable Functions & Growth

### Review: Mutable Values

#### Identity vs. equality in environment diagrams

- Review: For assignment statements, evaluate the rignt side, then assign to the left
- Copying: When creating a copy, copy exactly what's "in the box".

#### Mutating Within Functions

```python
>>> def mystery(lst): #mutative function
... 	lst.pop()
... 	lst.pop()
>>> four = [4, 4, 4, 4]
>>> mystery(four)
>>> four
[4, 4]
```

### Mutable Functions I

#### Functions with behavior that changes over time

- An immutable function returns the same value when called with the same input
- A mutable function returns different values when called with the same input

> Let's model a bank account that has a balance of $100; each time we do the same thing `withdraw(25)`, but we get different return values of money left.

#### Persistent Local State Using Environments

- All calls to the same function have the same parent
- We can get some techniques to make changes to the parent frame in a local frame

#### Reminder: local assignment

> Assignment binds name(s) to value(s) in the first frame of the current environment.

**Execution rule for assignment statements:**

1. Evaluate all expressions right of =, from left to right
2. Bind the names on the left to the resulting values in the **current frame**

#### Non-local assignment & Persistent local state

```python
def make_withdraw(balance):
    """Return a withdraw function with a starting balance."""
    def withdraw(amount):
        nonlocal balance #Declare the name "balance" nonlocal at the top of the body of the function in which it is re-assigned
        if amount > balance:
            return 'Insufficient funds'
        balance -= amount #Re-bind balance in the first non-local frame in which it was bound previously
        return balance
    return withdraw
```

### Mutable Functions II

#### The effect of nonlocal statements

`nonlocal <name>, <name>, ...`

- **Effect**: Future assignments to that name change its pre-existing binding in the **first non-local frame** of the **current environment**(an "enclosing scope") in which that name is bound.

> From the Python 3 language reference:
>
> Names listed in a nonlocal statement must refer to pre-existing bindings in an enclosing scope.
>
> Names listed in a nonlocal statement must not collide with pre-existing bindings in the **local scope**(current frame).

#### The many meanings of assignment statements

`x = 2`

| Status                                                       | Effect                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| No nonlocal statement and  "x" is not bound locally          | Create a new binding from name "x" to object 2 in the first frame of the current environment |
| No nonlocal statement and "x" is bound locally               | Re-bind name "x" to object 2 in the first frame of the current environment |
| nonlocal x and "x" is bound in a non-local frame             | Re-bind "x" to 2 in the first non-local frame of the current environment in which "x" is bound |
| nonlocal x and "x" is not bound in a non-local frame         | SyntaxError: no binding for nonlocal 'x' found               |
| nonlocal x and "x" is bound in a non-local frame and "x" also bound locally | SyntaxError: name 'x' is parameter and nonlocal              |

#### Python particulars

> Python pre-computes which frame contains each name before executing the body of a function.
>
> Within the body of a function, all instances of a name must refer to the same frame.

#### Mutable Values  & Persistent Local State

Mutable values can be changed without a nonlocal statement.

```python
def make_withsraw_list(balance):
    b = [balance] #Name bound outside of withdraw def
    def withdraw(amount):
        if amount > b[0]:
            return 'Insufficient funds'
        b[0] = b[0] - amount #Element assignment of a list
        return b[0]
    return withdraw
```

### Multiple Mutable Functions

#### Referential transparency, Lost

- Expressions are **referentially transparent** if substituing an expression with its value does not change the meaning of a program.

```python
mul(add(x, mul(4, 6)), add(3, 5))
mul(add(2, 24), add(3, 5))
mul(26, add(3, 5))
#The expressions above are actually the same. 
```

- Mutation operations violate the condition of referential transparency because they do more than just return a value; **they change the environment**.

#### Summary

- Nonlocal allows you to modify a binding in a parent frame, instead of just looking it up.
- Don't need a nonlocal statement to mutate a value.
- A variable declared nonlocal must:
  - Exist in a parent frame(other than the global frame)
  - Not exist in the current frame

### Program Performance

#### Measuring Performance

- Different functions run in different amounts of time
  - Different implementations of the same program can also run in different amounts of time
- How do we measure this?
  - Amount of time taken to run once
  - Average time taken to run
  - Average across a bunch of different computers
  - Number of operations

#### Improving number of operations

- Getting better performance usually requires clever thought
  - Some tools can be used to speed up programs(Ex: memoization, up next)
  - Other times, need to have a different approach or incorporate some insight

### Counting

#### How to count calls

> Can't always rely onn having a program to count calls

- For an iteration function: step through the program, and identify how many times you need to go through the loop before exiting
- For a recursive function: draw out an environment diagram/call tree

After you do this for a few example inputs, you can find patterns to estimate the number of steps for other inputs.

#### "Patterns" of Growth

- There are common patterns for how functions grow
- Because these patterns often aren't linear, you often can't use just one input to compare programs
- Instead, you have to identify the overall pattern
  - Constant Growth
  - Linear Growth
  - Logarithmic Growth
  - Exponential Growth



## Lecture 11  Iterators & Generators

### Iterators

#### Definitions

- **Lazy evalution** - Delays evaluation of an expression until its value is needed
- **Iterable** - An object capable of returning its members one at a time.
  - Examples include all sequences(lists, strings, tuples) and some non-sequence types(dictionaries).
- **Iterator** - An object that provides sequential access to values, one by one.
  - All iterators are iterables. Not all iterables are iterators.
- Metaphor: Iterables are books & Iterators are bookmarks.

#### How do we cerate iterators?

> A container can provide an iterator that provides access to its elements in order.

- **iter**(iterable): Return an iterator over the elements of an iterable value
  - This method creates a bookmark from a book starting at the front.
- **next**(iterator): Return the next element in an iterator
  - Returns the current page and moves the bookmark to the next page.
  - The iterator remembers where you left off. If the current page is the end of the book, error.

```python
>>> s = [3, 4, 5] #the book
>>> t = iter(s) #create bookmark t
>>> next(t) #move bookmark t
3
>>> next(t) #move bookmark t
4
>>> u = iter(s) #create bookmark u
>>> next(u) #move bookmark u
3
>>> next(t) #move bookmerk t
5
>>> next(u) #move bookmark u
4
```

### Dictionary Iteration

> An iterable value is any value that can be passed to **iter** to produce an iterator.
>
> An iterator is returned from **iter** and can be passed to **next**; all iterators are mutable.

#### Views of dictionary

A dictionary, its keys, its values, and its items are all iterable values

- The order of items in a dictionary is the order in which they were added(Python3.6+)
- Historically, items appeared in an arbitrary order(Python 3.5 and earlier)

### For Statements

> Iterator is also iterable, which can be used in for statements, but only once.

### Built-In Iterator Functions

- Many built-in Python sequence operations return iterators that compute results lasily
  - **map**(func, iterable): Iterate over func(x) for x in iterable
  - **filter**(func, iterable): Iterate over x in iterable if func(x)
  - **zip**(first_iter, second_iter): Iterate over co_indexed(x, y) pairs
  - **reversed**(sequence): Iterate over x in a sequence inreverse order
- To view the contents of an iterator, place the resulting elements into a container
  - **list**(iterable): Create a list containing all x in iterable
  - **tuple**(iterable): Create a tuple containing all x in iterable
  - **sorted**(iterable): Create a sorted list containing x in iterable

### Exceptions / Errors

#### Today's topic: handling errors

Sometimes, computer programs behave in non-standard ways

- A function receives an argument value of an improper type
- Some resource (such as a file) is not available
- A network connection is lost in the middle of data transmission

#### Raise Exceptions

> Exceptions are raised with a raise statement: `raise <expression>`
>
> <expression> must be an Exception, which is created like so:
>
> E.g. TypeError('Error message')

- TypeError - A function was passed the wrong number/type of argument
- NameError - A name wasn't found
- KeyError - A key wasn't found in a dictionary
- RuntimeError - Catch-all for troubles during interpretation

#### Try statements

> Try statements handle exceptions
>
> ```python
> try:
>     <try suite>
> except <exception class> as <name>:
>     <except suit>
> ```

Execution rule

- The <try suite> is executed first
- If, during the course of executing the <try suite>, an exception is raised that is not handled otherwise, then the <except suite> is executed with <name> bound to the exeception.

```python
>>>try:
... x = 1/0
... except ZeroDivisionError as e:
...	print('Except a', type(e))
...	x = 0
Except a <class ‘ZeroDivisionError’>
>>> x
0
```

#### Back to iterators - The for statement

> When executing a for statement, iter returns an iterator and next provides each item:

```python
>>> counts = [1, 2, 3]
>>> for item in counts:... 	print(item)
1
2
3
```

```python
>>> counts = [1, 2, 3]
>>> items = iter(counts)
>>> try:
...   while True:
... 	item = next(items)
...	print(item)
... except StopIteration: #StopIteration is raised whenever next is called on an empty iterator
...		pass
1
2
3
```

The two code squares above are equivalent!

### Generators

#### Definitions and rules

- Some definitions:
  - **Generator**: An iterator created automatically by calling a generator function.
  - **Generator function**: A function that contains the keyword **yield** anywhere in the body.
- When a generator function is called, it returns a generator **instead of** going into the body of the function. The only way to go into the body of  a generator function is by calling next on the returned generator.
- Yielding values are the same as returning values except yield remembers where it left off.

#### Generators and generator functions

``` python
>>> def plus_minus(x):
... 	yield x
... 	yield -x
...
>>> t = plus_minus(3)
>>> next(t)
3
>>> next(t)
-3
>>> t
<generator object plus_minus ...>
```

- We are allowed to call next on generators because generators are a type of iterator.
- Calling next on a generator goes into the function and evaluates to the first yield statement. 
- The next time we call next on that generator, it resumes where it left off(just like calling next on any iterator!)
- Once the generator hits on a return statement, it raises a StopIteration.

#### Generators to Represent Infinite Sequences

Iterators are used to represent infinite sequences. In this course, when we ask you to write an iterator, we want you to write a generator.

```python
>>> def naturals():
...		x = 0
...		while True:
...			yield x
...			x += 1

>>> nats = naturals()
>>> next(nats)
0
>>> next(nats)
1
>>> nats1, nats2 = naturals(), naturals()
>>> [next(nats1) * next(nats2) for _ in range(5)]
[0, 1, 4, 9, 16] # Squares the first 5 natural numbers
```

#### Generators can yield from iterators

> A `yield from` statement yields all values from an iterable.

```python
def a_then_b(a, b):
    for x in a:
        yield x
    for x in b:
        yield x
```

```python
def a_then_b(a, b):
    yield from a
    yield from b
```

The above two code squares are equivalent.

More example:

```python
def countdown(k):
    if k == 0:
        yield 'Blast off'
    else:
        yield k
        yield from countdown(k - 1)
```

### Summary

- Iterators (bookmarks) are used to iterate over iterables (books).
  - We use the iter method to turn iterables into iterators and we use the next method to get the next element.

- Exceptions can be raised and handled.
- Generators are how we implement iterators in this course and use yield statements. 
  - We can use yield from to yield multiple values from an iterable.



## Lecture 12 Object-Oriented Programming

### Why OOP?

- A (hopefully) more intuitive way of representing data.
- A common method for organizing programs.
- Formally split "global state" and "local state" for every object.

### Classes

#### Description

- Every object is an **instance** of a class.
- A class is a type or category of objects.
- A class serves as a blueprint for its instances.

> Some terminology:
>
> Class - Instance - Attributes
>
> ​                            - Behaviors

#### The class statement

> Class Statements defines the blueprint.
>
> `class <name> :  <suite>`
>
> Assignment & def statements in the Class Statement create class attributes.

Before we start implementing our methods,  we need t talk about how to create Accounts.

Ideal: All bank accounts have a balance and an account holder. These are not shared across Accounts.

When a class is called:

1. A new instance of that class is created.
2. The `__init__` method of a class is called with the new object as its first argument (named self), along with additional arguments provided in the call expression.

```python
class Account:
    max_withdrawal = 10
    
    def __init__(self, account_holder): #__init__ is called the contructor
        """Create an instance of the Account class"""
        self.balance = 0
        self.holder = account_holder
        
    def deposit(self, amount):
        """Deposit ammount to the amount."""
        self.balance += amount
        return self.balance
    
    def withdraw(self, amount):
        """Subtracts amount from the account."""
        if amount > self.max_withdraw or amount > self.balance:
            return "Can't withdraw this amount"
        self.balance -= amount
        return self.balance
```

#### Terminology: Attributes, Functions, and Methods

- All objects have **attributes**, which are name_value pairs.
- **Classes are objects** too, so they have attributes.

```python
>>> type(tom_account) #We define class to define objects: type(my_object) -> MyClass
<class '__main__.Account'>

>>> type(Account) #type is the metaclass in Python
<class 'type'>
```

> As classes are objects in Python, we use what to define "class objects"?
>
> We use **metaclass** to define classes: type(MyClass) -> MetaClass
>
> `my_object = MyClass()`  `MyClass = MetaClass()`
>
> ```python
> ACGN = type('ACGN', (tuple for parent classes), {dic for attribute pairs})
> print(ACGN)
> type(ACGN)
> ```

- **Instance attribute**: attribute of an instance
- **Class attribute**: attribute of the class of an instance

### Object Identity

- Every object that is an instance of a user-defined class has a unique identity.
- Identity operators "is" and "is not" test if two expressions evaluate to the same object(the arrows point to the same place)
- Binding an object to a new name using assignment does not create a new object.

#### Dot Expressions

> You can access class or instance attributes with dot notation. `<expression>.<name>`
>
> The <expression> can be any valid Python expression that evaluates to a **class** or instance. The <name> must be an **attribute** or a **method**. 
>
> ```python
> >>> tiff_account.max_withdrawal
> ```

To evaluate a dot expression:

1. Evaluate the <expression> to the left of the dot, which yields the object of the dot expression
2. <name> is matched against the instance attributes of that object; if an attribute with that name exists, its value is returned
3. If not, <name> is looked up in the class, which yields a class attribute or a method
4. The corresponding attribute is returned or corresponding method is called.

> Remember that 'evaluate' is not the same as 'execute'.
>
> If we bind the <object.attribute> to a value and the attribute doesn't exist in the instance/class, it will be created **just** in the instance/class.

### Methods and Functions

> **Methods** are functions defined in the suite of a class statement.

However, methods that are accessed through an instance will be bound methods. **Bound methods** couple together a function and the object on which that method will be invoked. This means that when we invoke bound methods, the instance is automatically passed in as the first argument.

```python
>>> a = Account("Jacy")
>>> Account.deposit
<function>
>>> a.deposit
<bound method>
```

#### Invoking methods

> We can call class methods in two ways: as a bound method and as a function.

- Invoking class methods as a bound method:
  - Bound methods are accessed through the instance and implicitly pass the instance object in as the first argument of the method.
  - `<instance>.<mathod_name>(<arguments>)`
- Invoking class methods as functions:
  - We can use the class name to directly call a method. These follow our typical function call rules and nothing is implicitly passed in.
  - `<class_name>.<mathod_name>(<instance>,<argument>)`

### Attributes

#### Accessing attributes

There are built-in functions that can help us access attributes.

- Using **getattr**, we can look up an attribute using a string instead.
  - `getattr(<expression>, <attribute_name(string)>)`
  - `getattr(a, 'balance')` is the same as `a.balance`
  - `getattr(Account, 'balance')` is the same as `Account.balance`
- Using **hasattr**, we can check if an attribute exists.
  - `hasattr(<expression>, <attribute_name(string)>)`
  - `hasattr(a, 'balance')` returns True
  - `hasattr(Account, 'balance')` returns False

#### Assigning attributes

We saw this before, but let's formalize the rules for assigning/reassigning instance and class attributes.

`<expression>.<name> = <value>`

Change attributes for the **object of that dot expression**.

- **If the expression evaluates to an instance**: then assignment sets an instance attribute, even if it exists in the class.
- **If the expression evaluates to a class**: then assignment sets a class attribute.

### Summary

- Object-oriented programming is another way (paradigm) to organize and reason about programs
- The Python **class** statement allows us to create user-defined data types that can be used just like built-in data types
  - Class attributes are variables shared across instances
  - Instance attributes are unique to each instance
  - Two ways to invoke methods, implicitly and explicitly.



## Lecture 13 Inheritance

### Methods and Functions

*Python distinguishes between:*

- **Functions**, which we have been creating since the beginning of the course, and
- **Bound Methods**, which couple together a function and the object on which that method will be invoked

> Object + Function = Bound Method

```python
>>> type(Account.deposit)
<class 'function'>
>>> type(tom_account.deposit)
<class 'method'>

>>> Account.deposit(tom_account, 1001) #Function: all arguments within parentheses
1001
>>> tom_account.deposit(1004) #Method: One object before the dot and other arguments whthin parentheses
2005
```

### Python Object System

- Functions are objects.
- Bound methods are also objects: a function that has its first parameter "self" already bound to an instance
- Dot expressions evaluate to bound methods for class attributes that are functions `<instance>.<method_name>`

### Looking Up Attributes by Name

`<expression>.<name>`

To evaluate a dot expression:

1. Evaluate the **expression** to the left of the dot, which yields the object of the dot expression
2. **name** is matched against the instance attributes of that object; if an attribute with that name exists, its value is returned
3. If not, **name** is looked up in the class, which yields a class attribute value
4. That value is returned unless it is a function, in which case a bound method is returned instead

> Class attributes are "shared" across all instances of a class because they are attributes of the class, not the instance.

### Assignment to Attributes

Assignment statements with a dot expression on their left-hand side affect attributes for the object of that dot expression

- If the object is an instance, then assignment sets an instance attribute
- If the object is a class, then assignment sets a class attribute

> `tom_account.interest = 0.08`
>
> `tom_account` evaluates to an object, but the name 'interest' is not looked up.
>
> Attribute assignment statement adds or modifies the attribute named 'interest' of tom_account

### Inheritance

- Inheritance is a technique for relating classes together

- A common use: Two similar classes differ in their degree of specialization
- The specialized class may have the same attributes as the general class, along with some special-case behavior

> `class <name>(<Base Class>):`
>
> ​	`<suite>`

- Conceptually, the new subclass inherits attributes of its base class

- The subclass may override certain inherited attributes
- Using inheritance, we implement a subclass by specifying its differences from the base class.

#### Inheritance Example

A **CheckingAccount** is a specialized type of **Account**

```python
>>> ch = CheckingAccount
>>> ch.interest #Lower interest rate for checking accounts
0.01
>>> ch.deposit(20) #Deposits are the same
20
>>> ch.withdraw(5) #Withdrawals incur a $1 fee
14
```

Most behavior is shared with the base class Account

```python
class CheckingAccount(Account):
    """A bank account that charges for withdrawls."""
    withdraw_fee = 1
    interest = 0.01
    def withdraw(self, amount):
        return super().withdraw(amount + self.withdraw_fee)
    #or
    	return Account.withdraw(self, amount + self.withdraw_fee)
```

#### Looking Up Attribute Names on Classes

> Base class attributes aren't copied into subclasses!

To look up a name in a class:

1. If it names an attribute in the class, return the attribute value.
2. Otherwise, look up the name in the base class, if there is one.

```python
>>> ch = CheckingAccount('Tom') # Calls Account.__init__
>>> ch.interest #Found in CheckingAccount
0.01
>>> ch.deposit(20) #Found in Account
20
>>> ch.withdraw(5) #Found in CheckingAccount
14
```

### Object-Oriented Design

#### Designing for Inheritance

- Don't repeat yourself; use existing implementations
- Attributes that have been overridden are still accessible via class objects
- Look up attributes on instances whenever possible

```python
class CheckingAccount(Account):
    """A bank account that charges for """
    withdraw_fee = 1
    interest = 0.01
    def withdraw(self, amount):
        return Account.withdraw(self, amount + self.withdraw_fee)
               #Attribute look-up on base class 
               #Preferred to CheckingAccount.withdraw_fee to allow for specialized accounts.
```

> Assume in the future, a subclass GreenCheckingAccount whose interest is 0.01 but withdraw_fee is only 0.23

#### Inheritance: Use it Carefully

> Inheritance helps code reuse but **NOT** for code reuse!

- Disadvantages of inheritance
  - Breaks encapsulation
    - Inheritance forces the developer of the subclass to know about the internals of the superclass
  - Unnecessary cost for inheritance maintenance

#### Composition

Colloquially, composition means

> If you want to reuse some behavior, put that behavior in a class, create an object of that class, **include** it as an attribute, and call its methods when the behavior is needed.

- Composition does not break encapsulation, and does not affect the types(all public interfaces remain unchanged)
- No need to involve in possibly complex hierarchy, and easy to understand and implement

#### Inheritance vs Composition

Guidance to choose inheritance or composition

- By conceptual difference
  - Inheritance represents **"is-a"** relationship
    - e.g. A checking account is a specific type of account.
  - Composition represents **"has-a"** relationhip
    - e.g. a bank has a collection of bank accounts it manages
- By practical need
  - If type B wants to **expose all public public methods** of type A (B can be used wherever A is expected), favors **inheritance**
  - If type B needs **only parts of behaviors** exposed by type A, favors **composition**

> Implementing **composition** means we need to **wrap the delegation logic** (delegated to the composed object) into certain methods, in which case **inheritance**'s "**direct reuse**" seems more convenient.
>
> Do we have some approach to somewhat take the advantage of both inheritance and composition?

#### Mixin

Mixin is a class that contains methods for use by other classes without having to be the parent class of those other classes, and without having to use delegation to a composed object.

- Mixin is usually considered as "**included**" rather than "inherited"
- But unlike composition (as also an "**included**" approach), the mixin-ed methods appear to be inherited(no object delegation)
- Unlike Python, some languages such as Ruby and Scala, has language support to enforce to enforce the syntax/semantics of Mixin
  - Mixin is called **module** in Ruby, and **trait** in Scala
- Mixin is usually considered as a mean for multiple inheritance

### Multiple Inheritance

```python
class SavingAccount(Account):
    deposit_fee = 2
    def deposit(self, amount):
        return Account.deposit(self, amount - self.deposit_fee)
```

- A class may inherit from multiple base classes in Python

- CleverBank marketing executive has an idea:
  - Low interest rate of 1%
  - A $1 fee for withdrawals
  - A $2 fee for deposits
  - A free dollar when you open your account

```python
class CleverAccount(CheckingAccount, SavingAccount):
    def __init__(self, account_holder):
        self.holder = account_holder
        self.balance = 1 #A free dollar!
```

```python
>>> tom = CleverAccount('Tom')
>>> tom.balance #Instance attribute
1
>>> tom.deposit(20) #SavingAccount
19
>>> tem.withdraw(5)
13
```

#### Diamond Problem

- Method Resolution Order(MRO)?
- C3 Linearization Algorithm for method resolution while doing multiple inheritance



## Lecture 14 Special Methods

### Outline

- Polymorphism
- `Polymorphism Functions(__str__, __repr__)`
- `Operator Overloading (+ and __add__)`
- More **Special Methods**

### Polymorphism

- Ad Hoc Polymorphism

  - e.g. Overloading:    `foo(int) { xxx }`   `foo(string) {xxx xxx xx}`

- Parametric Polymorphism

  - e.g. Generic functions:

    ```c
    Template <typename T>
    T foo(T x, T y){
        return (x > y) ? x : y;
    }
    foo<int>(3, 7);
    foo<char>('h', 'k');
    ```

- Inclusion Polymorphism

  - Subtypes and inheritance

> Next, we introduce two instances of **ad hoc polymorphism** to help illustrate some important **special methods** in Python:
>
> `polymorphic function (__str__, __repr__)`
>
> `operator overloading (__add__)`

### String Representations

- An object value should behave like the kind of data it is meant to represent

- For instance,  by producing a string representation of itself

- String are important: they represent language and programs

- In Python, all objects produce two string representations:

  - The **str** is legible to humans
  - The **repr** is legible to the python interpreter

  > The **str** and **repr** strings are often the same, but not always

#### The repr string for an object

- repr:

  - string representation of Python object. For most object types, **eval** will convert it back to that object, eval(repr(obj)) == obj

    ```python
    >>> 2e3
    2000.0
    >>> repr(2e3)
    '2000.0'
    >>> eval(repr(2e3))
    2000.0
    ```

  - The result of calling **repr** on a value is what Python outputs in an interactive session

    ```python
    >>> min
    <built-in function min>
    ```
    
    ```python
    >>> repr(min)
    <built-in function min>
    ```

#### The str string for an object

- Human interpretable string are useful as well:

  ```python
  >>> from fractions import Fraction
  >>> half = Fraction(1, 2)
  >>> repr(half)
  'Fraction(1, 2)'
  >>> str(half)
  '1/2'
  ```

- The result of calling **str** on the value of an expression is what Python prints using the **print** function:

  ```python
  >>> print(half)
  1/2
  ```

```python
>>> import datetime
>>> now = datetime.datetime.now()
>>> now
datetime.datetime(2020, 9, 14, 10, 36, 46, 832676)
>>> repr(now)
'datetime.datetime(2020, 9, 14, 10, 36, 46, 832676)'
>>> str(now)
'2020-09-14 10:36:46.823676'
```

> **repr** is to be unambiguous
>
> **str** is to be readable

### Polymorphic Functions

- Polimorphic function: 
  - A function that applies to many (ploy) different forms (morph) of data

> **Polymorphic functions** behave differently depending on the types of the arguments come in, while **parametric function** execute the same code for arguments of any admissible types.

- **str** and **repr** are both polimophic: 
  
  - They apply to any object and do not have much logic, and thay defer to the object (comes in) to decide what to do
  
    - **repe** invokes a zero-argument method `__repr__` on its argument
  
      ```python
      >>> half.__repr__()
      'Function(1, 2)'
      ```
  
    - **str** invokes a zero-argument method `__str__` on its argument
  
      ```python
      >>> half.__str__()
      '1/2'
      ```

#### Implementing repr and str

- The behavior of **repr** is slightly more complicated than invoking `__repr__` on its argument:
  - An instance attribute called `__repr__` is ignored! Only class attributes are found
- The behavior of **str** is also complicated:
  - An instance attribute called `__str__` is ignored
  - If no `__str__` attribute is found, uses **repr** string

### Operator Overloading

>  Operator overloading is to give the operator extended meaning beyond its predefined operational meaning.

e.g. adding instances of user-definded classes invokes `__add__` method

```python
>>> Ratio(1, 3) + Ratio(1, 6)
Ratio(1, 2)
>>> Ratio(1, 3).__add__(Ratio(1, 6))
Ratio(1, 2)
```

- Operator '+' is **overloaded** by `__add__` method when '+' is used to add user-defined objects
- A + B, different behaviors of this adding expression may exhibit, depending on the types of the operands (A or B). Thus we say **operator overloading** is a kind of **polymorphism**.

### Summary

> Certain names are special because they have built-in behaviors
>
> These names always start and end with two underscores

| `__init__`     | Method invoked automatically when an object is constructed  |
| -------------- | ----------------------------------------------------------- |
| `__repr/str__` | Method invoked to display an object as a Python expression  |
| `__add/radd__` | Method invoked to add one object to another                 |
| `__float__`    | Method invoked to convert an object to a float(real number) |



## Lecture 15 Linked Lists & Trees

### Linked Lists

- A simple but powerful data structure
- Can be used to implement other data structures, e.g. stack, queues
- Fast insertions/deletions, etc.

#### Linked list definition

A Linked List either:

- empty
- Composed of a first element and the rest of the **linked list**
  - rest contains a pointer to a linked list
  - List terminated with an empty linked list

> Linked lists are sequences where each value is the first element of a pair.

#### Creating linked lists

We'll define a linked list recursively by making a constructor that takes in a first and rest value.

##### Link class

```python
class Link:
    empty = () #You should not assume the representation here. It could be 'I'm empty'
    def __init__(self, first, rest=empty): #Rest defaults to the empty list
        assert rest is Link.empty or isinstance(rest, Link)
        self.first = first #.first gives elements in the list
        self.rest = rest #.rest traverses
```

> Feel free to use environment diagrams to help you understand the structure.
>

### Processing Linked Lists

#### sum

> Goal: Given a linked list, return the sum of all elements in the linked list

```python
def sum(linked_list): #the linked_list here is not nested
    if linked_list is Link.empty:
        return 0
    return linked_list.first + sum(linked_list.rest)
```

#### display_link 

> Goal: Given a linked list, return a string representing the elements in the linked list

```python
def display_link(link):
    result = ''
    while link is not Link.empty:
        if isinstance(link.first, Link):
            elem = display_link(link.first) #used to process nested link list
        else:
            elem = str(link.first)
        result += ' ' + elem
        link = link.rest
    return '<' + result + ' >'
```

#### map

> Goal: Given a linked list, and a one argument function, return a new linked list obtained from applying the function to each element of the linked list

```python
def map(f, link):
    if link is Link.empty:
        return Link.empty
    return Link(f(link.first), map(f, link.rest))
```

### Mutating Linked Lists

#### map

> Goal: Given a linked list and a one argument function, mutate the linked list by applying f to each element.

```python
def map(lnk, f): #using iteration
    """
    >>> lnk = Link(1, Link(2, Link(3)))
    >>> map(lnk, lambda x:x * 2)
    >>> display_link(lnk)
    '<2, 4, 6>'
    """
    if lnk is Link.empty:
        return
    lnk.first = f(lnk.first)
    map(lnk.rest, f)
```

```python
def map(lnk, f): #using iteration
    while lnk is not Link.empty:
        lnk.first = f(lnk.first)
        lnk = lnk.rest #Note that the original lnk will not shrink
```

### Why Linked Lists?

> Insert element at index 1
>
> Total number of operations = the length of the list minus 1

```python
inserted_elem = Link(2)
inserted_elem.rest = lnk.rest
lnk.rest = inserted_elem
```

### Tree Class

> A Tree is a label and a list of branches; each branch is a Tree

```python
class Tree:
    def __init__(self, label, branches=[]):
        self.label = label
        for branch in branches:
            assert isinstance(branch, Tree)
        self.branches = branches
```



## Lecture 16 Scheme - Elementary

> Scheme is a Dialect of Lisp

### Scheme Expressions

Scheme programs consist entirely of two types of expressions.

- **Atomic expressions** (*Atoms: primitive values that cannot be broken up into smaller parts*)
  
- **Self-evaluating:** numbers, booleans
  
  `3, 5.5, -10, #t, #f`
  
- **Symbols:** names bound to values
  
  `+, modulo, list, x, foo, hello-world`
  
- **Combinations**

  `(<operator> <operand1><operand2>...)`

  - A combination is either a **call expression** or a **special form expression**.

#### Call expressions

Syntax:`(<operator> <operand1><operand2>...)`

> A call expression applies a procedure to some arguments.

How to evaluate call expressions:

1. Evaluate the operator to get a procedure.
2. Evaluate all operands left to right to get the arguments.
3. Apply the procedure to the arguments.

> Call expressions include an operator and 0 or more operands in the parentheses.

```scheme
>(quotient 10 2)
5
>(quotient (+ 8 7) 5)
3
>( + (* 3
        (+ (* 2 4)
           (+ 3 5))
     (+ (- 10 7)
        6)))
```

> "quotient" names Scheme's built-in integer division procedure(i.e function)
>
> Combinations can span multiple lines (spacing doesn't matter)

#### Special form expressions

##### Assigning values to names

The define special form assigns a value to a name: `(define <name><expr>)`

**How to evaluate:**

1. Evaluate the given expression
2. Bind the resulting value to the given name in the current frame.
3. Return the names as a symbol.

```scheme
scm> (define x (+ 3 4))
x
scm> x
7
scm> (define x (+ x 5))
x
scm> x
12
```

##### Control flow

The if special form allows us to evaluate an expression based on a condition: `(if <predicate> <if-true><if-false>)`

**How to evaluate:**

1. Evaluate the `<predicate>`.
2. If `<predicate>` evaluates to anything but `#f`, evaluate `<if-true>` and return the value. Otherwise, evaluate `<if-false>` if provided and return the value.

> `#f` is the only Falsy value in Scheme.

```scheme
scm> (if #t 3 5)
3
scm> (if 0 (+ 1 0)(/ 1 0))
1
scm> (if (> 10 1)(* 5 6))
30
scm> (if (not 4) 1 (if #f 5 6))
6
```

##### Define functions with names

The second version of define is a shorthand for creating a function with a name: `(define (<name> <param1><param2>...)<body>)`

**How to evaluate:**

1. Create a lambda procedure with the given parameters and body.
2. Bind it to the given name in the current frame.
3. Return the function name as a symbol.

```scheme
scm> (define (square x) (* x x))
square
scm> square
(lambda (x) (*x x))
scm> (square 4)
16
scm> (square 10)
100
```

##### Lambda Expressions

The lambda special form returns a lambda procedure. `(lambda (<param1><param2>...)<body>)`

**How to evaluate:**

1. Create a lambda procedure with the given parameters and body.

   > The body expression is evaluated when the lambda procedure is applied.

2. Return the lambda procedure.

```scheme
scm> (lambda (x) (* x x))
(lambda(x) (* x x) 5)
scm> (lambda(x) (* x x) 5)
25
scm> (define square (lambda (x) (* x x)))
square
scm> (square 4)
16
```

**Two equivalent expressions:**

`(define （plus4 x) (+ x 4))`

`(define plus4 (lambda (x) (+ x 4)))`

**An operator can be a call expression too:**

`((lambda (x y z) (+ x y (square z))) 1 2 3)`

> Evaluates to the x + y + z^2 procedure.

### Examples

#### Factorial

Recall the factorial function, which takes in an integer `n` and computes the product of all the integers from 1 to `n`.

Let's try to write it in Scheme!

Scheme has no special form that allows for iteration, so we have to use recursion.

**Ideas:**

1. Base case: if n is 0 or 1, just return 1
2. Recursive case: Return the factorial of the previous number multiplied by n
3. Use the if special form to capture our two cases:`if <pred><if-true><if-false>)`

```scheme
(define (fact x)
    (if (<= x 1) 
        1
        (* x (fact (- x 1)))))
```

> Combinations can be split across multiple lines.
>
> No explicit return statement!

#### Counting up

Let's write a function `count-up` that takes in an integer n and prints all the integers from 1 to n.

**Ideas:**

1. We need to keep track of the current element, k. k starts at 1.
2. Since we have to use recursion, we can write a helper function to keep track of k.
3. Print k at the beginning of every call and only make a recursive call to print more numbers if k is less than n.

```scheme
(define (count-up n)
  (define (counter k)
    (print k)
    (if (< k n)
        (counter (+ k 1))))
  (counter 1))
```

> If there is more than one expression in the body, the function returns the value of the last expression.

### Summary

- Scheme programs consist only of expressions, all of which can be categorized into either **atomic expressions** or **combinations**.
- Combinations are either **call expressions** or **special form expressions**, and they differ in the value of the operator.
- Scheme call expressions are evaluated just like they are in Python, but each special form has its own rules of evaluation.
- The special forms we learned today are `if`, `define`, and `lambda`.
- Writing some procedures in Scheme will require recursion; there is no special form for iteration.



## Lecture 17 Scheme - Further

### Pairs and Lists

#### Pairs

- Pairs are created using the `cons` expression in Scheme
- `car` selects the first element in a pair
- `cdr` selects the second element in a pair
- The second element of a pair must be another pair, or `nil`(empty)

```scheme
scm> (define x (cons 1 (cons 3 nil)))
x
scm> x
(1 3)
scm> (car x)
1
scm> (cdr x)
(3)
```

> The pairs in scheme is quite similar to linked lists in python.

#### Lists

- The only type of sequence in Scheme is the linked list
  - We can create these with pairs using multiple `cons` expression
- `nil` represents the empty list

```scheme
scm> (cons 1 (cons 2 nil))
(1 2)
scm> (define x (cons 1 (cons 2  nil)))
x
scm> x
(1 2)
scm> (car x)
1
scm> (cdr x)
(2)
scm> (cons 1 (cons 2 (cons 3 (cons 4 nil))))
(1 2 3 4)
```

#### Symbolic programming

- Symbols normally refer to values; how do we refer to symbols?

```scheme
> (define a 1)
> (define b 2)
> (list a b)
(1 2) 
```

> No sign of "a" and "b" in the resulting value.

- Quotation is used to refer to symbols directly in Lisp.

```scheme
> (list 'a 'b)
(a b)
> (list 'a b)
(a 2)
```

> Short for (quota a), (quota b):
>
> Special form to indicate that the expression itself is the value.

- Quotation can also be applied to combinations to form lists.

```scheme
> '(a b c)
(a b c)
> (car '(a b c))
a
> (cdr '(a b c))
(b c)
```

### Tail Recursion

#### Recursion versus iteration in python

```python
def rfactorial(n):
    if n == 0:
        return 1
    return n * rfactorial(n - 1)

def ifactorial(n):
    total = 1
    while n > 0:
        total *= n
        n -= 1
    return total
```

| Multiplication Operations | Frames? |
| ------------------------- | ------- |
| n                         | n       |
| n                         | 1       |

#### Tail recursion

- We say an expression is in a **tail context** if it is evaluated as the last step in the function call
  - That means nothing is evaluated/applied after it is evaluated
- Function calls in a tail context are called **tail calls**
- If the tail call calls the function itself, we say that function is **tail recursive**
  - If a language supports tail call optimization, a tail recursive function will only ever open a constant number of frames

**Identifying tail contexts**

> An expression is in a tail context only if **it is the last thing evaluated** in every possible scenario (no other action is performed afterwards)

#### Recursive frames

```scheme
(define (fact n) 
  (if (= n 0) 
      1 
      (* n (fact (- n 1)))))
```

> We need to keep these frames open because the last step in the function is to multiply `n` with the result of the recursive call.

#### Tail calls

```scheme
(define (fact n)
  (define (fact-tail n result)
    (if (<= n 1)
        result
        (fact-tail (- n 1) (* n result))))
  (fact-tail n 1))
```

> Number of the frames the same regardless of input size!

**A python version:**

```python
def fact(n):
    def fact_tail(n, result):
        if n <= 1:
            return result
        return fact_tail(n - 1, n * result)
    return fact_tail(n, 1)
```

#### Writing Tail Recursive Functions

1. Identify recursive calls that are not in a tail context. Tail contexts are:
   - The last body subexpression in a *lambda* (a function)
   - The consequent and alternative in a tail context of *if*
   - The last sub-expression in a tail context *and, or, begin, or let*
2. Create a helper function with arguments to accumulate the computation that prevents it from being tail recursive

##### Example: length of linked list

> Goal: Write a function that takes in a list and returns the length of the list. Make sure it is tail recursive

A conventional version:

```scheme
(define (length lst)
  (if (null? lst)
      0
      (+ 1 (length (cdr lst)))))
```

**Tail recursive version**

```scheme
(define (length-tail lst)
  (define (helper lst result)
    if (null? list)
    	result
        (helper (cdr lst) (+ 1 result)))
  (helper n 1))
```



## Lecture 18 Interpreters

### Translation

- Problem:
  - Computers can only understand one language, binary (0s and 1s)
  - Human can't really write a program using only 0s and 1s (not quickly anyways)

- Solution:
  - Programming languages
  - Languages like Python, Java, C, etc are *translated* to 0s and 1s
- This translation step comes in a couple forms:
  - Compiled (pre-translated) - translate all at once and run later
  - Interpreted (translated on-the-fly) - translated while the program is running

> We'll focus on interpreted languages.

### Interpreters

- An **interpreter** does 3 things:
  - **Reads** input from user in a specific programming language
  - Translates input to be computer readable and **evaluates** the result
  - **Prints** the result for the user
- There are two languages involved:
  - **Implemented language:** this is the language the user types in
  - **Implementation language:** this is the language interpreter is implemented in

> **Implemented Language** is translated into the **Implementation Language**.

#### Read-Eval-Print Loop (REPL)

`input string` - **Read** - `expression` - **Eval** - `value` **Print** - `output string` - `loop` - `input string` - ......

```python
while True:
    exp = read()
    val = eval(exp)
    print(val)
```

### Read

#### Reading input

- **Lexical Analysis (Lexer):** Turning the input into a collection of tokens
  - A token: single input of the input string, e.g. literals, names, keywords, delimiters
- **Syntactic Analysis (Parser):** Turning tokens into a representation of the expression in the implementing language
  - The exact "representation" depends on the type of expression
  - Types of Scheme Expressions: self-evaluating expressions, symbols, call expressions, special form expressions.

`input string` - **Lexer** - `tokens` - **Parser** - `representation of the expression`

#### Representing scheme primitive expressions

- **Self-Evaluating expressions** *(booleans and numbers)*
  - Use Python booleans and Python numbers
- **Symbols**
  - Use Python strings

#### Representing combinations

`(<operator> <operand1> <operand2> ...)`

> **Combinations** are just Scheme lists containing an operator and operands.

```scheme
scm> (define expr '(+ 2 3)) ;Create the expression (+ 2 3)
expr
scm> (eval expr) ;Evaluate the expression
5
scm> (car expr) ;Get the operator
+
scm> (cdr expr) ;Get the operands
(2 3)
```

```python
>>> expr = ['+', 2, 3] #Representation of (+ 2 3)
>>> expr[0] #Get the operator
'+'
>>> expr[1:] #Get the operands
[2, 3]
```

> Works, but isn't an exact representation of Scheme lists.

#### Python Pairs

To accurately represent Scheme combinations as linked lists, let's write a `Pair` class in Python!

```python
class Pair:
    def __init__(self, first, second):
        self.first = first
        self.second = second
    
    def __repr__(self):
        return 'Pair({0}, {1})'.format(self.first, self.second)

class nil:
    def __repr__(self):
        return 'nil'

nil = nil() #There is one instance of nil. No other instances can be created.
```

```python
>>> expr = Pair('+', Pair(2, Pair(3, nil))) #Represent (+ 2 3)
>>> expr.first #Get the operator
'+'
>>> expr.second #Get the operands
Pair(2, Pair(3, nil))
```

#### Reading Combinations

`'(+ 2 3)' ` - **Lexer** - `['(', '+', 2, 3, ')']` - **Parser** - `Pair('+', Pair(2, Pair(3, nil)))`

`(define (f) 3)` - **Lexer** - `['(', 'define', '(', 'f', ')', 3, ')']` - **Parser** - `Pair('define', Pair((Pair('f', nil)), Pair(3, nil)))`

#### Special case: quote

- Recall that the quota special form can be invoked in two ways:

  - `(quote <expr>)`

    ```scheme
    scm> 'hello
    hello
    scm> '(1 2 3 4)
    (1 2 3 4)
    ```

  - `'<expr>`

    ```scheme
    scm> (quote hello)
    hello
    scm> (quote (1 2 3 4))
    (1 2 3 4)
    ```

- The special `'` syntax gets converted by the reader into a `quote` expression, which is a list with 2 elements: `Pair('quote', Pair(<exp>, nil))`

### Eval

#### Evaluating expression

- Rules for evaluating an expression depends on the expression's type.
- Types of scheme expression: self-evaluating expressions, symbols, call expressions, special form expressions

`expression representation` - **Eval** - `value`

> Eval takes in one argument besides the expression itself: the current environment.

#### Frames and environments

> When evaluating expressions, the current **environment** consists of the current frame, its parent frame, and all its ancestor frames until the global Frame.

```scheme
(define (make-adder x)
  (lambda (y) (x + y)))

(define add-three (make-adder 3))

(add-three 5)

(add-three 10)
```

#### Frames in our interpreter

- Frames are represented in our interpreter as instances of the **Frame** class
- Each **Frame** instance has two instance attributes:
  - **bindings**: a dictionary that binds Scheme symbols (Python strings) to Scheme values
  - **parent**: the parent frame, another **Frame** instance
- The evaluator needs to know the current environment, given as a single **Frame** instance, in order to look up names in expressions.

#### Evaluating primitive expressions

- **Self-evaluating expressions**: These expressions evaluate to themselves.
- **Symbols**:
  1. Look in the current frame for the symbol. If it is found, return the value bound to it.
  2. If it is not found in the current frame, look in the parent frame. If it is not found in the parent frame, look in its parent frame, and so on.
  3. If the global frame is reached and the name is not found, raise a **Scheme Error**.

`4.65` - **Eval** - `4.65`

`+` - **Eval** - `BuiltinProcedure(scheme_add)`

#### Evaluating combinations

`(<operator> <operand1> <operand2> ...)`

> The operator of a combination tells us whether it is a special form expression or a call expression.

- If the operator is a symbol and is found in the dictionary of special forms, the combination is a special form.

  - Each special form has special rules for evaluation.

- Otherwise, the combination is a call expression.

  - **Step 1.** Evaluate the operator to get a procedure.

  - **Step 2.** Evaluate all of the operands from left to right.

    > First two steps are recursive calls to eval.

  - **Step 3.** Apply the procedure to the values of the operands. 

    > How does apply work?

#### Types of procedures

- A **built-in procedure** is a procedure that is predefined in our Scheme interpreter, e.g. `+`, `list`, `modulo`, etc.
  - Each built-in procedure has a corresponding Python function that performs the appropriate operation.
  - In our interpreter -- instances of the `BuiltinProcedure` class
- A **user-defined procedure** is a procedure defined by the user, either with a lambda expression or a define expression.
  - Each user-defined procedure has
    1. a list of formal parameters
    2. a body (which is a scheme list)
    3. a parent frame.
  - In our interpreter -- instances of the `LambdaProcedure` class

##### Built-in procedures

- Applying built-in procedures:
  - Call the Python function that implements the built-in procedure on the arguments.

> **operator:** expr.first
>
> **operands:** expr.second

##### User-defined Procedures

- Applying user-defined procedures:
  - **Step 1.** Open a new frame whose parent is the parent frame of the procedure being applied.
  - **Step 2.** Bind the formal parameters of the procedure to the arguments in the new frame.
  - **Step 3.** Evaluate the body of the procedure in the new frame.

#### The evaluator

The evaluator consists of two *mutually-recursive* components:

**Evaluator:**

- **Eval**
  - Base Cases:
    - Self-evaluating expressions
    - Look up values bound to symbols
  - Racursive Cases:
    - **Eval(operator), Eval(o)** for each operand **o** 
    - **Apply(proc, args)**
    - **Eval(expr)** for expression **expr** in body of special form
- **Apply**
  - Base Cases:
    - Built-in proocedures
  - Recursive Cases:
    - **Eval(body)** of user-defined procedures

##### Counting eval/apply calls: built-in procedures

> How many calls to eval and apply are made in order to evaluate this expression?
>
> example: `(+ 2 (* 4 1) 5)`

```python
eval(Pair('+',
     Pair(2,
     Pair(Pair('*', Pair(4, Pair(1, nil))),
     Pair(5, nil)))))
	eval('+')
    eval(2)
    eval(Pair('*', Pair(4, Pair(1, nil))))
    	eval('*')
        eval(4)
        eval(1)
        apply(BuiltinProc(scheme_mul), [4, 1])
    eval(5)
    apply(BuiltinProc(scheme_add), [2, 4, 5])
#calls: eval-8, apply-2
```

##### Counting eval/apply calls: user-defined procedures

> How many calls to eval and apply are made in order to evaluate the second expression? (Assume the define expression has alredy been evaluated.)
>
> example: `(define (f x) (+ x 1))`
>
> ​				`(* (f 3) 2)`

```python
eval(Pair('*',
     Pair(Pair('f', Pair(3, nil)),
     Pair(2, nil))))
	eval('*')
    eval(Pair('f', Pair(3, nil)))
    	eval('f')
        eval(3)
        apply(lambda, 3)
        	eval(Pair('+', Pair('x', Pair(1, nil))))
            	eval('+')
                eval('x')
                eval(1)
                apply(BuiltinProc(scheme_add), [3, 1])
   	eval(2)
    apply(BuiltinProc(scheme_mul), [4, 2])
#calls: eval-10, apply-3
```

## Lecture 19 Macros

### Review: Representing Expressions

- In Scheme, we can create lists that "look like" combinations
  - In fact, in Scheme, expressions *are* lists (or primitive values)
- Quoting prevents evaluation of an expression
- Calling eval on an unevaluated expression will evaluate that value

```scheme
scm> '(+ 1 2)
(+ 1 2)
scm> (eval '(+ 1 2))
3
scm> (list 'quotient 10 2)
(quotient 10 2)
scm> (eval (list 'quotient 10 2))
5
```

### Expressions in Scheme

#### Expressions as data

> Recall: programs are composed of expressions, but manipulate values or data
>
> In Scheme, **expressions** are either primitive expressions or **lists**, which means they're also a form of **data**!

This means we can:

- Assign expressions to variables
- Pass expressions into functions
- Create & return new expressions within functions

#### Begin

> **begin** is a special form takes in any numbers of expressions, evaluates them in order, and evaluates to the value of the final expression.

```scheme
scm> (begin 3 2 1)
1
scm> (begin (define x 2) (define x (+ x 1)) x)
3
```

#### Let

```scheme
(let ((symbol1 expr1) ;Each symbol is bound to the value of the expression in parallel
      (symbol2 expr2) ;The bindings only exists when evaluating the body
      ...)
  	  body) ;Evaluate to the value of the body using the binding
```

```scheme
scm> (let ((x 2)
           (y 3))
       	   (+ x y))
5
```

### Macros

#### Example: Double

> Let's write a procedure double. We want it to evaluate whatever expression we pass in twice.

```scheme
scm> (double (print 2))
2
2
```

Issues:

- How do we prevent evaluation of the input?
- How do we easily get the intended behavior?

#### Macros

- Macros are a more convenient way to transform or create expressions
- The `define-macro` special form will create a macro procedure
- Macros take in and return expressions, which are then evaluated **in place of** the call to the macro.

```scheme
(define-macro (twice expr) ;Here expr is a piece of code that hasn't been evaluated
              (list 'begin expr expr)) ;Returns a piece of code that then gets evaluated
```

```scheme
scm> (twice (print 2)) ;Equivalent to:(begin (print 2) (print 2))
2
2
```

#### Evaluating macros

- Recall evaluation procedure used for regular call expressions:
  1. Evaluate the operator sub-expression, which evaluates to a regular procedure.
  2. Evaluate the operand expressions in order.
  3. Apply the procedure to the evaluated operands.

- Macros, on the other hand, do the following:
  1. Evaluate the operator sub-expression, which evaluates to a macro procedure.
  2. Apply the macro procedure to the operand expressions **without evaluating them first**.
  3. Evaluate the expression returned by the macro procedure in the frame the macro was called in.

#### Writing macros

- Because macros take in and return expressions, when writing macros you should think about:
  1. What types of expressions you'll take in
  2. What expression has equivalent behavior to your macro
- Consider a macro **add-to** which should take in a symbol and an expression, and increment the value of the variable by the expression.

```scheme
(define-macro (add-to var expr) (list 'define var (list '+ var expr)))
```

```scheme
scm> (define x 1)
scm> (add-to x (+ 1 2))
x
scm> x
4
```

#### For Macro

> Scheme doesn't have `for` loops,  but thanks to macros, we can add them.

```scheme
scm> (for x in '(1 2 3 4) do (* x x))
(1 4 9 6)
scm> (map (lambda(x) (* x x)) '(1 2 3 4))
(1 4 9 16)
```

```scheme
(define-macro (for sym in vals do expr) 
              (list 'map (list 'lambda (list sym) expr) vals))
```

### Quasi-Quotation

> Quasi-quotation allows you to have some parts of a list be read literally and some parts be evaluated.
>
> It's especially useful for constructing code in macros.

```scheme
(define-macro (for sym vals expr)
              (list 'map (list 'lambda (list sym) expr) vals))
```

```scheme
(define-macro (for sym vals expr)
              `(map (lambda (,sym) ,expr) ,vals))
;` short for quasiquote
;, short for unquote
```

Write the twice and add-to macros using quasi-quotes

```scheme
(define-macro (twice expr)
              `(begin ,expr ,expr))
(define-macro (add-to sym expr)
              `(define ,sym (+ ,sym ,expr)))
```



## Lecture 20 Streams

### Lazy Evaluation in Scheme

> Streams are similar to lists, except that the tail of a stream is not evaluated until we ask to do it. This allows streams to be used to represent infinitely long lists.

#### Lazy evaluation

- In Python, iterators and generators allow for **lazy evaluation**

```python
def ints(first):
    while True:
        yield first
        first += 1
```

```python
>>> s = ints(1)
>>> next(s)
1
>>> next(s)
2
```

- Scheme doesn't have iterators. How about a list?

```scheme
(define (ins first)
  (cons first (ints (+ first 1))))
```

```scheme
scm> (ints 1)
maximum recursion depth exceeded
```

> Second argument to `cons` is **always evaluated**.

#### Streams

> Instead of iterators, Scheme uses **streams**.

```scheme
(define (ints first)
  (cons-stream first
               (ints (+ first 1))))
```

```scheme
scm> (ints 1)
(1 . #[promise (not forced)]) ;Lazy evaluation, just like iterators in Python
```

- **Stream**: (linked) list whose **rest** is lazily evaluated
  - A **promise** to compute
- **cdr** returns the rest of a list
  - For normal lists, the rest is another list
  - The rest of a stream is **a promise to compute the list**.
- **cdr-stream** forces Scheme to compute the rest

```scheme
scm> (define s (cons-stream 1 (cons-stream 2 nil))) ;Remember, a stream is just a regular Scheme pair whose second element is a promise
s
scm> s
(1 . #[promise (not forced)])
scm> (car s)
1
scm> (cdr s)
#[promise (not forced)]
scm> (cdr-stream s)
(2 . #[promise (not forced)])
scm> (cdr-stream (cdr-stream s))
()
```

#### Promises: delay

- Promise: an object that delays evaluation of an expression

  - The **delay** special form creates promises

  ```scheme
  scm> (print 5) ;(print 5) is immediately evaluated
  5
  scm> (delay (print 5)) ;(print 5) is not evaluated yet
  #[promise (not forced)]
  ```

#### Promises: force

- The **delay** special form creates promises
- The **force** procedure evaluates the expression inside the promise

```scheme
scm> (define x (delay (print 5))) ;(print 5) is not evaluated yet
x
scm> x
#[promise (not forced)]
scm> (force x) ;Evaluates (print 5)
5
;Error in our interpreter
scm> x
#[promise (forced)]
```

> **cons-stream** and **cdr-stream** are syntactic sugar. Achieve the same effect with **delay** and **force**

```scheme
scm> (define s (cons-stream 1 nil))
s
scm> s
(1 . #[promise (not forced)])
scm> (cdr-stream s)
()
```

```scheme
scm> (define s (cons 1 (delay nil)))
s
scm> s
(1 . #[promise (not forced)])
scm> (force (cdr s))
()
```

#### Recursively defined streams - Constant stream

> Let's start with the constant stream. A constant stream is an infinitely long stream with a number repreated.

```scheme
(define (constant-stream i)
  (cons-stream i (constant-stream i)))
```

```scheme
scm> (define ones (constant-stream 1))
scm> (car ones)
1
scm> (car (cdr-stream ones))
1
```

> Let's define the naturals stream which is an infinitely long stream with the natural numbers starting at start.

```scheme
(define (nats start)
  (cons-stream start (nats (+ start 1))))
```

```scheme
scm> (define s (nats 0))
scm> (car s)
0
scm> (car (cdr-stream s))
1
scm> (car (cdr-stream (cdr-stream s)))
2
```

> Let's write a function that will add two infinite streams together and return a new stream.

```scheme
(define (add-stream s1 s2)
  (cons-stream (+ (car s1) (car s1))
               (add-stream (cdr-stream s1) (cdr-stream s2))))
```

> Let's use the ones stream we've defined before and our new add-stream function to define the ints stream. This is the same as (nats 1). How do we do this?

```scheme
(define ints (cons-stream 1 (add-stream ints ones)))
;We can use infinite streams to build other infinite streams. This is the power of lazy evalluation, our current stream stays one step ahead of itself!
```

##### Examples: map-stream

- Implement `(map-stream fn s)`:
  - `fn` is a one-argument function
  - `s` is a stream
- Returns a new stream with `fn` applied to elements of `s`

```scheme
(define (map-stream fn s)
  (if (null? s) nil
  (cons-stream (fn (car s)) (map-stream fn (cdr-stream s)))))
```

##### Examples: stream-to-list

- Implement `(stream-to-list s num-elements)`:
  - `s` is a stream
  - `num-elements` is a non-negative integer
- Returns a Scheme list containing the first `num-elements` elements of `s`

```scheme
scm> (stream-to-list (ints 1) 10)
(1 2 3 4 5 6 7 8 9 10)
```

```scheme
(define (stream-to-list s num-elements)
  (if (or (= num-elements 0) (null? s)) nil
      (cons (car s)
            (stream-to-list (cdr-stream s) (- num-elements 1)))))
```



## Lecture 21 SQL I

### Declarative Programming

#### Programming paradigms

- Up till now, we've been focused (primarily) on **imperative programming**.

- Impreactive program contain explicit instructions to tell the computer **how** to accomplish something. The interpreter then executes those instructions.
- Now, we'll learn about **declarative programming**, where we can just tell the computer **what** we want, instead of how we want it done. The interpreter then figures out how to accomplish that.
- Declarative programs are often specialized to perform a specific task, because they allow for repretitive computation to be abstracted away and for the interpreter to optimize its execution.

### SQL

#### SQL & Database Languages

> SQL is an example of a (declarative) language with interacts with a **database management systems (DBMS)** in order to make data processing easier and faster.

It collects records into **tables**, or a collection of rows with a value for each column.

| Latitude | Longitude | Name        |
| -------- | --------- | ----------- |
| 38       | 112       | Berkeley    |
| 42       | 71        | Cambridge   |
| 45       | 93        | Minneapolis |

- A **row** has a value for each column.
- A **column** has a name and a type.
- A **table** has columns and rows.

#### Tables in SQL (Sqlite, a small SQL database engine)

```sql
CREATE TABLE cities AS
	SELECT 38 AS latitude, 122 AS longtitude, "Berkeley" AS name UNION
	SELECT 42,			   71,                "Cambridge"		 UNION
	SELECT 45,			   93,   			  "Minneapolis";
```

```sql
SELECT "west coast" AS region, name FROM cities WHERE longtitude >= 115 UNION
SELECT "other", 			   name FROM cities WHERE longtitude < 115;
```

**Cities**:

| Latitude | Longtitude | Name        |
| -------- | ---------- | ----------- |
| 38       | 112        | Berkeley    |
| 42       | 71         | Canbridge   |
| 45       | 93         | Minneapolis |

| Region     | Name        |
| ---------- | ----------- |
| west coast | Berkeley    |
| other      | Minneapolis |
| other      | Cambridge   |

#### SQL Basics

> The SQL Language varies across implementations but we will look at some shared concepts.

- A `SELECT` statement creates a new table, either from scratch or by taking information from an existing table
- A `CREATE TABLE` statement gives a global name to a table.
- Lots of other statements exist: `DELETE`, `INSERT`, `UPDATE` etc...
- Most of the important action is in the `SELECT` statement.

#### Selecting value lierals

- A `SELECT` statement always includes a comma-separated list of column descriptions.

- A column description is an expression, optionally followed by `AS` and a column name.
  - `SELECT [expression] AS [name], [expression] AS [name], ... ;`
- `SELECT`ing literals `CREATE`s a one-row table.
- The `UNION` of two `SELECT` statements is a table containing the rows of both of their results.

#### Naming Tables

> SQL is often used as an interactive language.

- The result of a `SELECT` statement is displayed to the user, but not stored.
- A `CREATE TABLE` statement gives the result a name.
  - `CREATE TABLE [name] AS [SELECT statements];`

```sql
CREATE TABLE parents AS
	SELECT "delano" AS parent,  "herbert" AS child UNION
	SELECT "abraham",			"barack"		   UNION
	SELECT "abraham",			"clinton"		   UNION
	SELECT "fillmore",			"abraham"		   UNION
	SELECT "fillmore",			"delano"		   UNION
	SELECT "eisenhower",		"fillmore";
```

**Parents:**

| Parents    | Child    |
| ---------- | -------- |
| delano     | herbert  |
| abraham    | barack   |
| abraham    | clinton  |
| fillmore   | abraham  |
| fillmore   | delano   |
| eisenhower | fillmore |

### Selecting From Tables

#### SELECT statements project existing tables

- A `SELECT` statement can specify an input table using a `FROM` clause.
- A subset of the rows of the input table can be selected using a `WHERE` clause.
- Can declare the order of the ramaining rows using an `ORDER BY` clause. Otherwise, no order.
- Column descriptions determine how each input row is projected to a result row:
  - `SELECT [columns] FROM [table] WHERE [condition] ORDER BY [order] [ASC/DESC] LIMIT[number];`

```sql
sqlite> SELECT * FROM parents ORDER BY parents DESC; --Only use ASC/DESC IF there's ORDER BY
sqlite> SELECT child FROM parents WHERE parent = "abraham";
sqlite> SELECT parent FROM parents WHERE parent > child;
--WHERE and ORDER BY are optional
```

#### Arithmetic in SELECT statements

- In a `SELECCT` expression, column names evaluate to row values.
- Arithmetic expressions can combine row values and constants.

```sql
CREATE TABLE restaurant AS
	SELECT 101 AS ttable, 2 AS single, 2 AS couple UNION
	SELECT 102		   , 0			, 3			  UNION
	SELECT 103		   , 3			, 1;
```

```sql
sqlite> SELECT ttable, single + 2 * couple AS total From restaurant;
```

| table | total |
| ----- | ----- |
| 101   | 6     |
| 102   | 6     |
| 103   | 5     |

Given a table ints that describes how to sum powers of 2 from various integers.

```sql
CREATE TABLE ints AS
	SELECT "zero" AS word, 0 AS one, 0 AS two, 0 AS four, 0 AS eight UNION
	SELECT "one"		 , 1	   , 0		 , 0		, 0			 UNION
	SELECT "two"		 , 0	   , 2		 , 0		, 0			 UNION
	SELECT "three"		 , 1	   , 2		 , 0		, 0			 UNION
	SELECT "four"		 , 0	   , 0		 , 4		, 0			 UNION
	SELECT "five"		 , 1	   , 0		 , 4		, 0			 UNION
	SELECT "six"		 , 0	   , 2		 , 4		, 0			 UNION
	SELECT "seven"		 , 1	   , 2		 , 4		, 0			 UNION
	SELECT "eight"		 , 0	   , 0		 , 0		, 8			 UNION
	SELECT "nine"		 , 1	   , 0		 , 0		, 8;
```

1. Write a `SELECT` statement for a two-column table of the word and the value for each integer.

```sql
sqlite> SELECT word, one + two + four + eight AS value FROM ints; 
```

2. Write a `SELECT` statement for the word names of the powers of two.

```sql
sqlite> SELECT word  FROM ints WHERE one + two + four + eight = 1 OR one + two + four + eight = 2 OR one + two + four + eight = 4 OR one + two + four + eight = 8;
--The code above has syntax error but can convey the logic.
```

### Joinning Tables

#### An example:

```sql
CREATE TABLE dogs AS
	SELECT "abraham" AS name, "long" AS fur UNION
	SELECT "barack"			, "short" 		UNION
	SELECT "clinton"		, "long"		UNION
	SELECT "delano"			, "long"		UNION
	SELECT "esisenhower"	, "short"		UNION
	SELECT "fillmore"		, "curly"		UNION
	SELECT "grover"			, "short"		UNION
	SELECT "herbert"		, "curly";
```

> How can we select the parents of curly furred dogs?

#### Joining tables

- Two tables A & B are joined by a comma to yield all combinations of a row from A & a row from B.

  ```sql
  SELECT * FROM parents, dogs;
  ```

- Selects all combinations of rows from both tables. We only want the rows for curly haired dogs.

  ```sql
  SELECT * FROM parents, dogs WHERE fur = "curly";
  ```

- This filters the 56 rows to now only have rows where the fur is curly. But this has rows that have nothing to do with each other. We only care about rows where the two dogs match.

  ```sql
  SELECT * FROM parents, dogs WHERE child = name AND fur = "curly";
  ```

- The condition on which the tables are joined on is called the join condition.

  ```sql
  SELECT parent FROM parents, dogs WHERE child = name AND fur = "curly";
  ```

#### Joining a table with itself

- Two tables may share a column name; dot expressions and aliases disambiguate column values.

`SELECT [columns] FROM [table] WHERE [condition] ORDER BY [order];`

> [table] is a comma-separated list of table names with optional aliases.

**Select all pairs of siblings. No duplicates.**

| First   | Second  |
| ------- | ------- |
| barack  | clinton |
| abraham | delano  |
| abraham | grover  |
| delano  | grover  |

#### Aliasing

```sql
SELECT * FROM parents, parents;
--This doesn't work because the tables share a column. Lets fix that!
```

```sql
SELECT * FROM parents AS a, parents AS b;
--This works because SQL can tell the columns in the two tables apart.
--Let's now only keep rows where the children share a parent.
```

```sql
SELECT * FROM parents AS a, parents AS b WHERE a.parent = b.parent;
--We need to get rid of duplicates because pairs of siblings appear twice.
```

```sql
--We can do this by enforcing an arbitrary ordering, a.child < b.child alphabetically.
--Then we get the two columns we want.
SELECT a.child AS first, b.child AS second FROM parents AS a, parents AS b WHERE a.parent = b.parent AND a.child < b.child;
```

### String Expressions

> String values can be combined to form longer strings.

```sql
sqlite> SELECT "hello," || " world";
hello, world
sqlite> SELECT name || " dog" FROM dogs;
abraham dog
barack dog
clinton dog
delano dog
eisenhower dog
fillmore dog
grover dog
herbert dog
```



## Lecture 22 SQL II

### Review of Tables and Join

#### Table

> A table stores data. It consists of ...
>
> - a fixed number of **columns**.
> - data entries stored in **rows**.

- To make a table in SQL, use a `CREATE TABLE` statement:

```sql
CREATE TABLE [name] AS ...;
```

- To create rows of data, `UNION` together `SELECT` statements:

```sql
SELECT [expr] AS [name], [expr] AS [name], ... UNION
SELECT [expr] AS [name], [expr] AS [name], ... UNION
SELECT [expr] AS [name], [expr] AS [name], ... ;
```

- To create rows of data from existing tables, use a `SELECT` statement with a `FROM` clause:

```sql
SELECT [columns] FROM [table] WHERE [condition] ORDER BY [order] [ASC/ DESC] LIMIT [number];
```

#### Join

> Given multiple tables, we can join them together by specifying their names, separated by commas, in the `FROM` clause of a `SELECT` statement.

```sql
SELECT * FROM table1, table2;
```

>  When we join two tables, we get a new table with one row for each combination of rows from the original tables.

| parent  | child   |
| ------- | ------- |
| abraham | barack  |
| abraham | clinton |

| name    | fur   |
| ------- | ----- |
| abraham | long  |
| barack  | short |
| clinton | long  |

| parent  | child   | name    | fur   |
| ------- | ------- | ------- | ----- |
| abraham | barack  | abraham | long  |
| abraham | barack  | barack  | short |
| abraham | barack  | clinton | long  |
| abraham | clinton | abraham | long  |
| abraham | clinton | barack  | short |
| abraham | clinton | clinton | long  |

##### Check your understanding

> Table **songs**:
>
> name | artist |album
>
> Table **albums**:
>
> name |artist | release_year
>
> Table **artists**:
>
> name | first_year_active

1. Write an SQL query that outputs the first 10 artists who became active after 2015.

   ```sql
   SELECT name FROM artists 
   	WHERE first_year_active > 2015 LIMIT 10;
   ```

2. Write an SQL query that outputs the names and artists of songs that were released in 2010 ordered by the first year active of the artist.

   ```sql
   SELECT s.name, s.artist 
   	FROM songs AS s, artists AS ar, albums AS al
       WHERE album = al.name AND s.artist = ar.name
       	AND release_year = 2010
       	ORDER BY first_year_active;
   ```

### Aggregation

#### Single row operations

##### Single-table queries

> So far, our SQL statements have referred to the values in a single row at a time.

Write a query that outputs the name of dogs that either have long fur or are named Grover.

```sql
SELECT name FROM dogs WHERE fur = 'long' OR name = 'grover'
```

##### Join

Write a query that outputs the names and fur types of all of fillmore's children.

```sql
SELECT name, fur FROM dogs, parents WHERE parent = 'fillmore' And name = child;
```

#### Aggregation

> **Aggregation** is the process of doing operations on *groups of rows* instead of just a single row.

SQL provides **aggregate functions** whose return values can be used as entries in a column.

table **dogs**

| name       | fur   | age  |
| ---------- | ----- | ---- |
| delano     | long  | 10   |
| eisenhower | short | 7    |
| fillmore   | curly | 8    |
| grover     | short | 2    |
| herbert    | curly | 4    |

*out put the average age of all dogs:*

```sql
SELECT AVG(age) AS ave_age FROM dogs;
```

*output:*

| ave_age |
| ------- |
| 6.2     |

*output the total number of rows:*

```sql
SELECT COUNT(*) AS count FROM dogs;
```

*output:*

| count |
|----------|
| 5         |

#### Aggregate function

| Aggregation function | Return value                              |
| -------------------- | ----------------------------------------- |
| `MAX([columns])`     | The maximum value in the given column(s)  |
| `MIN([columns])`     | The minimum value in the given column(s)  |
| `AVG([columns])`     | The average value in the given column     |
| `COUNT([column])`    | The number of values in the given column  |
| `SUM([column])`      | The sum of the values in the given column |

table **dogs**

| name       | fur   | age  |
| ---------- | ----- | ---- |
| eisenhower | short | 7    |
| delano     | long  | 10   |
| grover     | short | 2    |

*output the sum of ages of all dogs:*

```sql
SELECT SUM(age) AS sum_age FROM dogs;
```

*output the name thet comes first alphabetically:*

```sql
SELECT MIN(name) AS min_name FROM dogs;
```

#### Groups

> By default, aggregation id performed over all the rows of the table.

- We can specify that we want to group rows based on values in a particular column using the `GROUP BY` clause in a `SELECT` statement.

table **dogs**

| name       | fur   | age  |
| ---------- | ----- | ---- |
| eisenhower | short | 7    |
| delano     | long  | 10   |
| grover     | short | 2    |
| fillmore   | curly | 8    |
| herbert    | curly | 4    |

Write a query that finds out the average age of dogs for each fur type.

```sql
SELECT fur, AVG(age) AS avg_age FROM dogs GROUP BY fur;
```

*output:*

| fur   | avg_age |
| ----- | ------- |
| short | 4.5     |
| long  | 10      |
| curly | 6       |

#### More on group by

> You can `GROUP BU` any valid SQL expression, which includes using multiple column names and operators.

```sql
SELECT [columns] FROM [table] WHERE [condition]
	GROUP BY [expression]
	ORDER BY [order] [ASC/ DESC]
	LIMIT [number];
```

- A single group consists of all rows for which `[expression]` **evaluates to the same value**.
- The output table will have **one row** per group.

##### Check your understanding

table **dogs** 

| name |  fur |  age |
| ---------|-------|---------  |
|abraham |long |9 |
|herbert |curly| 4 |
|fillmore |curly |8 |
|delano |long |10 |
|eisenhower |short |3|

table **parents**

|parent| child|
|----------|--------- |
|delano| herbert|
|fillmore| abraham|
|fillmore| delano|
|eisenhower |fillmore|

1. Write a query that outputs a table containing the average age of each parent's children.

   ```sql
   SELECT parent, AVG(age) AS avg_age
   	FROM dogs, parents 
   	WHERE name = child 
   	GROUP BY parent;
   ```

   *output:*

   | parent     | avg_age |
   | ---------- | ------- |
   | delano     | 4       |
   | fillmore   | 9.5     |
   | eisenhower | 8       |

2. Write a query that outputs a table with 2 rows: one with the number of dogs of even ages and the other with the number of dogs of odd ages (ignore order).

   > Remember that you can **GROUP BY** expressions containing operators!

   ```sql
   SELECT COUNT(*) AS count
   	FROM dogs
   	GROUP BY age % 2 = 0;
   ```
   
   *output:*
   
   | count |
   | ---------|
   |2|
   |3|

#### Filtering Groups

> We know how to filter individual rows using the `WHERE` clause.
>
> To filter groups, use the `HAVING [condition]` clause!

table **dogs**

| name       | fur   | age  |
| ---------- | ----- | ---- |
| abraham    | long  | 9    |
| herbert    | curly | 4    |
| fillmore   | curly | 8    |
| delano     | long  | 10   |
| eisenhower | short | 3    |

Write a query that finds the average age of dogs for each fur type if there are more than one dogs with that fur type.

```sql
SELECT fur, AVG(age) AS avg_age
	FROM dogs GROUP BY fur
	HAVING COUNT(*) > 1;
```

*output:*

| fur   | avg_age |
| ----- | ------- |
| long  | 9.5     |
| curly | 6       |

##### Check your understanding

 table **dogs** 

| name       | fur   | age  |
| ---------- | ----- | ---- |
| abraham    | long  | 9    |
| herbert    | curly | 4    |
| fillmore   | curly | 8    |
| delano     | long  | 10   |
| eisenhower | short | 3    |

table **parents**

| parent     | child    |
| ---------- | -------- |
| delano     | herbert  |
| fillmore   | abraham  |
| fillmore   | delano   |
| eisenhower | fillmore |

Write a query that outputs the average age of each parent's children if that parent's youngest child is at least 5.

```sql
SELECT parent, AVG(age) AS avg_age
	FROM dogs, parents
	WHERE name = child
	GROUP BY parent
	HAVING MIN(age) >= 5;
```

*output:*

| parent     | avg_age |
| ---------- | ------- |
| fillmore   | 9.5     |
| eisenhower | 8       |

### Mutating Tables

#### Databases

> In real databases, it's common practice to initialize empty tables and add rows as new data is introduced.

#### Create / remove tables

- To create an empty table, use the `CREATE TABLE` statement, specifying the table name and column names (and possible default values):

  ```sql
  CREATE TABLE [name]([columns]);
  ```

  ```sql
  CREATE TABLE parents(parent, child);
  ```

  ```sql
  CREATE TABLE dogs(name, fur, phrase DEFAULT 'woof');
  --When adding rows, if no value is provided for the third column, the default value will be used.
  ```

- To remove a table from our database, use the `DROP TABLE` statement:

  ```sql
  DROP TABLE [IF EXISTS] [name];
  ```

  ```sql
  DROP TABLE dogs;
  ```

  ```sql
  DROP TABLE IF EXISTS parents;
  ```

#### Inserting records

- To insert rows into a table:

```sql
INSERT INTO [table]([columns]) VALUES ([values]), ([values]);
```

| name     | fur   | phrase |
| -------- | ----- | ------ |
| fillmore | curly | woof   |
| delano   | long  | hi!    |
|          | short | bark   |

```sql
CREATE TABLE dogs(name, fur, phrase DEFAULT 'woof');
INSERT INTO dogs(name, fur) VALUES('fillmore', 'curly');
INSERT INTO dogs VALUES('delano', 'long', 'hi!');
INSERT INTO dogs(fur, phrase) VALUES('curly', 'bark');
```

#### Updating Records

- To update existing entries in a table:

  ```sql
  UPDATE [table] SET [column] = [expression] WHERE [condition];
  ```

- To delete existing rows in a table:

  ```sql
  DELETE FROM [table] WHERE [condition];
  ```

| name     | fur   | phrase |
| -------- | ----- | ------ |
| fillmore | curly | WOOF   |
| delano   | short | hi!    |
|          | short | bark   |

```sql
UPDATE dogs SET phrase = 'WOOF' WHERE fur = 'curly';
DELETE FROM dogs WHERE fur = 'curly' and phrase = 'WOOF';
UPDATE dogs SET fur = 'short';
```

#### Summary

- Cerate empty table

  ```sql
  CREATE TABLE [name]([columns]);
  ```

  - using default values

    ```sql
    CREATE TABLE [name](..., [column] DEFAULT [value], ...);
    ```

- Remove table from database

  ```sql
  DROP TABLE [IF EXISTS] [name];
  ```

- Inserting records (new row)

  ```sql
  INSERT INTO [table]([columns]) VALUES([values]), ([values]);
  ```

  ```sql
  INSERT INTO [table] VALUES(..., [values (one for each column)], ...);
  ```

- Updating records (existing row):

  ```sql
  UPDATE [table] SET [column] = [expression] WHERE [condition];
  ```

  ```sql
  DELETE FROM [table] WHERE [condition];
  ```

### Summary

- We can use **aggregate function** to perform operations on a set of rows rather than on individual rows.
- To specify an expression by which to group rows, use the `GROUP BY` clause.
- To filter groups based on a condition over the whole group, use the `HAVING` clause.
- In real database, we commonly initialize empty tables and insert, update, or remove records over time.











