---
layout:     post
title:      Python For Dummies
date:       2021-05-30
categories: Programming
---

This is a transcript from my seminar on a Crash Course for Python3.

## Why Python?

- It is very **easy to learn** and **elegant to look at**, even if your code is not perfect.
- **No curly brackets** **or semicolon** bull.
- It has the **biggest programming community** by far. This means anything you want to build is already available open source, and the numerous libraries allow you to **make whatever you can dream of**.
- For the above reasons, competent Python programmers are in great demand right now. So **MONEEEYYYYY**.

![learn Python, python programming, python bootcamp, what is Python used for](https://codingnomads.co/wp-content/uploads/2019/06/PythonSalaryDaxx2019.png)

 

### Will This Talk Get Me $100k?

Well, probably not. But after the next 45 minutes, you will be able to read and write Python quite well and maybe do some data juggling with numpy and pandas.

**If you stick around until the end, I have something in store that will blow your mind.**

The flow will tentatively go as follows,

1. **Intro to interpreter**
2. **Hello World**
3. **Variables And Types**
4. **Operators**
5. **Lists**
6. **Loops**
7. **Conditions (if else)**
8. **Functions**
9. **Imports**
10. **Numpy Introduction**
11. **Pandas Introduction**

I've been told that there will be a separate session for numpy and pandas so we will not go too deep into that in this talk.

At the end of some sections, you will be given a problem to solve. We will not proceed until and unless everyone successfully solves it.

## Intro To The Interpreter

When you open up IDLE or type python3 in your terminal, you should see something that looks like the following if your python is properly installed.

**IDLE**:

![Screenshot 2020-11-18 at 1.40.55 PM](/savvy/assets/images/Screenshot 2020-11-18 at 2.02.23 PM.png)

**TERMINAL**:

![Screenshot 2020-11-18 at 1.41.46 PM](/savvy/assets/images/Screenshot 2020-11-18 at 1.41.46 PM.png)

This is called the interpreter. Interpreters execute code line by line.

## HELLO WORLD

Simply open up your interpreter and write

```python
print("Hello World!")
```

And press enter. You should see it printed on the next line.

![Screenshot 2020-11-18 at 1.43.39 PM](/savvy/assets/images/Screenshot 2020-11-18 at 1.43.39 PM.png)

ezpz Lets move on.

## VARIABLES AND TYPES

Python is called a statically typed language which means that you dont have to say beforehand what the data type a variable is. This makes it much more pleasing to look at.

![Screenshot 2020-11-18 at 1.52.15 PM](/savvy/assets/images/Screenshot 2020-11-18 at 1.52.15 PM.png)

Now you might ask, if data types dont exist how can I convert an integer to a decimal?

You would simply use `data_type_you_want(variable_name)` syntax. This is called **typecasting**.

![Screenshot 2020-11-18 at 2.00.20 PM](/savvy/assets/images/Screenshot 2020-11-18 at 2.00.20 PM.png)

## OPERATORS

The usual **addition (+)**, **subtraction (-)**, **multiplication (\*)** and **division (/)** are obviously present.

But apart from that we have the **modulo (%)** operator, used to find the remainder after division of two numbers. 

![Screenshot 2020-11-18 at 6.33.14 PM](/savvy/assets/images/Screenshot 2020-11-18 at 6.33.14 PM.png)

And the **power**(**) operator, to raise to a power.

![Screenshot 2020-11-18 at 6.36.06 PM](/savvy/assets/images/Screenshot 2020-11-18 at 6.36.06 PM.png)

## LISTS

`Lists` are very similar to arrays. They can contain any type of variable, and they can contain as many variables as you wish. `Lists` can also be iterated over in a very simple manner. Here is an example of how to build a list.

![Screenshot 2020-11-18 at 3.00.12 PM](/savvy/assets/images/Screenshot 2020-11-18 at 3.00.12 PM.png)

List elements can be **accessed using indexes** with square `[]` brackets . Lists are **indexed from 0** .

![Screenshot 2020-11-18 at 3.03.17 PM](/savvy/assets/images/Screenshot 2020-11-18 at 3.03.17 PM.png)

To add an element to the end of a list use the `append` function.

![Screenshot 2020-11-18 at 3.14.02 PM](/savvy/assets/images/Screenshot 2020-11-18 at 3.14.02 PM.png)

To remove an element from a list,

to remove by value use `remove`,

![Screenshot 2020-11-18 at 3.16.20 PM](/savvy/assets/images/Screenshot 2020-11-18 at 3.16.20 PM.png)

to remove by index use `del`,

![Screenshot 2020-11-18 at 3.17.42 PM](/savvy/assets/images/Screenshot 2020-11-18 at 3.17.42 PM.png)

## LOOPS

**NOTE:** 

Python3 **does not have curly brackets** to enclose units of code. It makes use of **blocks**.

A block is a area of code of written in the format of:

```python
block_head:
    1st block line
    2nd block line
    ...
```

Loops can run any number of commands as many times until a specified condition is valid.

In Python, there are two types of loops, for and while.

for loops iterate over a sequence,

![Screenshot 2020-11-18 at 6.05.18 PM](/savvy/assets/images/Screenshot 2020-11-18 at 6.05.18 PM.png)

 and while loops repeat as long as a certain boolean condition is valid. 

![Screenshot 2020-11-18 at 6.06.48 PM](/savvy/assets/images/Screenshot 2020-11-18 at 6.06.48 PM.png)

## Conditions

![coffee](/savvy/assets/images/coffee.png)

if-else statements are the pillars of programming. Its quite simple. if a condition checks out then we run some code but if not then we run something else.

![Screenshot 2020-11-18 at 9.25.55 PM](/savvy/assets/images/Screenshot 2020-11-18 at 9.25.55 PM.png)

We can also add more conditions using `and` and `or` operators between them. Open up your editor, lets write some conditions.

## FUNCTIONS

### What are Functions?

Functions are a convenient way to divide your code into useful blocks, allowing us to order our code, make it more readable, reuse it and save some time. Also functions are a key way to define interfaces so programmers can share their code.

### How do you write functions in Python?

Functions in python are defined using the block keyword "def", followed with the function's name as the block's name. For example:

```python
def my_function():
    print("Hello From My Function!")
```

Functions may also receive arguments (variables passed from the caller to the function). For example:

```python
def my_function_with_args(username, greeting):
    print(f"Hello, {username} , From My Function!, I wish you {greeting}")
```

Functions may return a value to the caller, using the keyword- 'return' . For example:

```python
def sum_two_numbers(a, b):
    return a + b
```

To call a function that has been defined, simply use `function_name(arguments)`,

```python
my_function_with_args("AUVSI","Happy Holidays")
```

## IMPORTS

Functions that are written in the same folder in different script or are installed through pip, can be imported directly into your program using `from module_name import function_name`.

```python
from random import random
print(random())
```

That gives your script access to the random function which prints a random number between 0 and 1.

 

## NOW FOR THE SURPRISE

I was really excited to talk about this on the sesh but sadly we went way overtime :( 

So lets say I made a script for a function that loops over a list like so,

![Screenshot 2020-11-18 at 9.21.05 PM](/savvy/assets/images/Screenshot 2020-11-18 at 9.21.05 PM.png)

Now, remove the colon at the start of the block and add an `end` at the end of **each block** and save this new script with a `.rb` extension. Now let me run this with the `ruby` interpreter.

![Screenshot 2020-11-18 at 9.24.18 PM](/savvy/assets/images/Screenshot 2020-11-18 at 9.24.18 PM.png)

Congratulations, you now not only know python, but also **ruby**! (which is if you recall from the top, the language with the most pay!)
