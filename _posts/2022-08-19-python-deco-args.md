---
title: "Python decorator with arguments"
categories:
  - blog
tags:
  - python
---

In this article we will create a Python decorator that accepts arguments.

Python decorators are great way to extend/change the behaviour of selected code without modifying it.

# First example

Creating the decorator function and our primary function to be decorated:

```python
from functools import wraps

def my_decorator(message):
    def wrapper(func):
        @wraps(func)
        def inner(*args, **kwargs):
            print(message)
            return func(*args, **kwargs)
        return inner
    return wrapper

@my_decorator("Message 1")
def my_func(message):
    print(message)
```

And now when we call the just defined function `my_func`:

```python
>>> my_func("Message 2")
Message 1
Message 2
```

## Second example

Now we do something crazier as we want to also do something **after** the decorated function call.

I like using `try` with `finally` as it is executed after the `try` block. Nice and clean.

```python
from functools import wraps

def my_decorator(msg_before, msg_after):
    def wrapper(func):
        @wraps(func)
        def inner(*args, **kwargs):
            print(msg_before)
            try:
                return func(*args, **kwargs)
            finally:
                print(msg_after)
        return inner
    return wrapper

@my_decorator("Message before", "Message after")
def my_func(message):
    print(message)
```

And the output looks like this:

```python
>>> my_func("Message from function")
Message before
Message from function
Message after
```

## Decorating a class method

Decorating class methods is not much different than doing so to functions.
Let's look at the example.
We `print()` the `inner()` functions `*args` to see what they consist of.

```python
from functools import wraps

def my_decorator(message):
    def wrapper(func):
        @wraps(func)
        def inner(*args, **kwargs):
            print(args)
            return func(*args, **kwargs)
        return inner
    return wrapper

class MyClass:
    def __init__(self):
        self.value = 0

    @my_decorator("Hello")
    def print_value(self):
        print(self.value)
```

```python
>>> obj = MyClass()
>>> obj.print_value()
(<__main__.MyClass object at 0x000002C38F2E7DC0>,)
0
```

As we can see, the first argument of a class method is the class instance itself. Which makes sense as the methods have `self` as 
the first argument.

### Modifying the class with decorator

Now we modify the class within the decorator just for fun.
We change the `value` variable of the `MyClass` inside the decorator from 0 to 1000 and print the value.

```python
from functools import wraps

def my_decorator(message):
    def wrapper(func):
        @wraps(func)
        def inner(*args, **kwargs):
            args[0].value = 1000  # Remember, this is the class instance when a class method is decorated
            return func(*args, **kwargs)
        return inner
    return wrapper

class MyClass:
    def __init__(self):
        self.value = 0

    @my_decorator("Hello")
    def print_value(self):
        print(self.value)
```

```python
>>> obj = MyClass()
>>> obj.print_value()
1000
```

# Conclusion

Python decorators are great when they are utilized properly. They can be used in various ways and that is
a topic for another day. 

Have fun!