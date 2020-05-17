# 3. Functions

In python, all functions are actually just objects. Thus as a result, they can be passed as parameters just as any other type, stor__ed in other objects/data structures, or assinged to variables. This makes using functions in Python extremely versetile. 

Functions are made callable as they define the special `__call__` method. Any object that implements this method will become callable itself.

## *args and **kwargs
The `*` operator in python is often overlooked, but can be extremely useful. It unpacks an iterable and essentially replaces it with a comma seperated list of its content inline. This can be seen in the following example.

```
>>> int.__eq__(*[1, 2])
False
```
`int.__eq__` is the implementation of the binary equality function between integer objects, and thus accepts two integers. The `*` unpacks the arguments so the line is equivalent to `int.__eq__(1, 2)`.

`*args` allows you to pass a variable number of arguments to a python function. For example, let's write an add function that accepts any number of objects to add.
```
>>> def add(*args):
...     total = 0
...     for arg in args:
...         total += arg
...     return total 
>>> add(1, 4, -1, 7)
11
```
**kwargs unpacks keyword arguments. It is given in the form of a dictionary of string keys mapping from the name of the keyword argument to it's value. Here's an example to demonstrate this.
```
>>> def print_kwargs(**kwargs):
...     for arg_name, arg_val in kwargs.items():
...         print(arg_name, arg_val)
... 
>>> print_kwargs(a=10, b=2)
a 10
b 2
```

If you are using `*args` and `**kwargs`, required parameters must be specified first followed by `args` then `kwargs`: `def fn(req_arg1, req_arg2, *args, **kwargs)` 

You can use unpacking to call any function as well. Let's say we have a function with the following header: `def fn(a1, a2, kw1=0, kw2=True, kw3=[])`. We could define the variables `args = ("a1", "a2")` and `kwargs = {"kw1" : 0}`, then call `fn(*args, **kwargs)`.

## Return
By default, Python functions that don't explicitly return a value will return the `None` object.

Additionally, when returning multiple values, they are implicitly wrapped in a tuple that you can use as a tuple, or implicitly unpack.
```
>>> def fn(): return 0, 1
... 
>>> fn()
(0, 1)
>>> a, b = fn()
>>> print(a, b)
0 1
```

## Typing
Python 3.5 adding the `typing` module that allows for type hinting within functions. You can read the full documentation [here](https://docs.python.org/3/library/typing.html). The typing module will perform checks on function arguments are return types to verify that they are behaving as desired. High quality code can leverage typing to improve readability and correctness.

Within function signatures, a colon `:` following an argument indicuates its expected type and a an arrow `->` after the parenthetical indicates the expected return type. 
```
def append_and_return(items: list, item : int) -> list:
    items.append(item)
    return items

def get(map: dict, key: str) -> int:
    return map[key]
```

### Type Aliases
The typing module includes support for aliases that can be leveraged for more complex type verification. Aliases for all of the common python data types already exist inclduing `List`, `Dict`, `Tuple`, and `Set`. Objects from the `typing` module have a bracket operator that allows you to indicate what type of items an object might hold.
```
from typing import List, Dict, Tuple, Set
def dot(vec_1: List[float], vec_2: List[float]) -> float:
    # implementation
```
We can also rewrite the same signature using a type alias. We create a new `typing` object that further defines the signature. 
```
Vector = List[float]
def dot(vec_1: Vector, vec_2: Vector) -> float:
    # implementation
```
Both of the above examples are identical. Here's a more complex example taken from the Python documentation linked earlier.
```
ConnectionOptions = Dict[str, str]
Address = Tuple[str, int]
Server = Tuple[Address, ConnectionOptions]

def broadcast_message(message: str, servers: Sequence[Server]) -> None:
```




## Decorators

## Function Caching

## Lambdas