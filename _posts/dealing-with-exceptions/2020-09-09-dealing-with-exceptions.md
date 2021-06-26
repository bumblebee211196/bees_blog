---
title: Dealing with Exceptions
date: 2020-09-09 11:00:00 +05:30
tags: [Python]
---

Dealing with exceptions in python is straightforward. You would surround your code with `try-catch` blocks. The exception message to be captured/processed will be mentioned as a variable in the `except` block.

### 1. Catching Exceptions

Let's assume this is our block of code

```python
def fun():
    try:
        result = 1 / 0
    except ZeroDivisionError as error:
        print(error)

fun()
```
The outcome is the following
```
division by zero
```

### 2. Re-raising exceptions
In some cases, we want to capture the exception message and re-raise the exception as a new exception. We can achieve that in the following way

```python
def fun():
    try:
        result = 1 / 0
    except ZeroDivisionError as error:
        print(error)
        raise

fun()
```
The outcome is the following
```
Traceback (most recent call last):
  File "scratch.py", line 9, in <module>
    fun1()
  File "scratch.py", line 6, in fun1
    result = 1 / 0
ZeroDivisionError: division by zero
```

Adding the `raise` keyword raises a new exception from the exception message we just captured i.e., raise the exception that was last caught.

### 3. Re-raising with custom exceptions
We can also raise a custom exception from the last caught exception.

```python
def fun():
    try:
        result = 1 / 0
    except ZeroDivisionError as error:
        print(error)
        raise MyCustomException("Custom exception message") from error

fun()
```
The outcome is the following
```
Traceback (most recent call last):
  File "scratch.py", line 6, in fun1
    result = 1 / 0
ZeroDivisionError: division by zero

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "scratch.py", line 12, in <module>
    fun1()
  File "scratch.py", line 9, in fun1
    raise CustomException("Something wrong") from error
CustomException: Something wrong
```

The above technique is very much useful at situations where we want the user to see only the exceptions the module/package has raised.
