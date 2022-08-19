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

# Conclusion

Decorators are great when they are utilized properly. They can be used in various ways and that is
a topic for another day. 

Have fun!