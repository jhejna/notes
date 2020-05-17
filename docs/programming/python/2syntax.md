# 2. Syntax

This section covers some finer details of python's syntax, along with a few extra tid-bits.

## Scoping
In python, scopes are defined by `dict` like namespaces, similar to those found in objects. Within objects (and thus within functions), different namespaces are applied than the global python namespace. When python looks up the value attributed to a variable name, it first looks up the name in it's immediate scope by checking the namespace of the object the function is in. If it doesn't find the given variable there, it proceeds down the call stack searching for it, until it finds the variable or reaches the global scope and can't find it.
```
x = "global"
def fn1():
    x = "local"
    print(x)
def fn2():
    print(x)
print(x)    # global
fn1()       # local
fn2()       # global
```
You can access the local namespace by calling the `locals()` function, or the global namespace by calling the `globals()` function, both of which return key attribute dictionaries.

### nonlocal
The nonlocal keyword in python can be used in nested functions to make a given variable refer to one that is used in a different scope. This essentially binds the variable in local scope to the variable in the next higher level scope where it is defined.
```
def outer():
	x = "outer"
	def inner():
		nonlocal x
		x = "inner"
		print("From Inner:", x)
	inner()
	print("From Outer:", x)
outer()
```
This code yields the following result:
```
From Inner: inner
From Outer: inner
```
This is because the variable `x` is set to refer to the namespace of `outer` with the `nonlocal` keyword. Thus, `x` gets set to "inner" in the namespace of outer. If `nonlocal` was not present, we would instead get the expected result of inner from inner and outer from outer.

### global
While `nonlocal` can refer to external scopes, the keyword `global` can be used to make variables refer to their versions in the global namespace. The `global` key word makes a given variable refer to the global scope. Be careful though the `global` and `nonlocal` descriptors only take effect in a given scope. The following example demonstrates this.

```
x = "global"
def outer():
	x = "outer"
	def inner():
		global x
		x = "inner"
		print("From Inner:", x)
	inner()
	print("From Outer:", x)
outer()
print("From Global:", x)
```
The result of running this is:
```
From Inner: inner
From Outer: outer
From Global: inner
```
Note that in the scope of `outer` the variable is unchanged. This is because the binding of `global` only takes effect within the scope it is used. As expected, in `inner`, `x` will refer to the global variable and change it to "inner".

## Variable Naming
Python has no strict for scoping variables as private or public. Instead, naming conventions are used in order to indicate a variables type of access.

Single underscores as prefixes, like `_variable` are used to indicate private attributes of an object.

In messy situations with inheritance, double underscores, like `__variable` can be used. At runtime, these varaible names have the class type prepended along with underscores, to form a name like `__classname__variable`. This can be used to automatically prevent name conflicts between parent and sub classes.

Double underscores on either side of a variable name, like `__variable__` refer to attributes that are directly used by the python interpreter. Changing or overriding these functions is not garunteed to be safe in general.

## Ternary Operator

Most people don't know about it, but python actually has a ternary operator. It is of the following form:
```
true_result if condition else false_result
```

The same effect can be achieved with tuples, though the first method is preferable as it is more readable.
```
(true_result, false_result)[condition]
```
This works because `True` casts to the integer value 1 while `False` casts to the integer value 0.


## The Python Debugger

pdb is amazing. Not many people use it. It basically allows you to insert yourself in the middle of python's repl loop and step through any part of execution and manipulate variables at will. I've found it to be super useful. You can use it at any point by just importing `pdb`, then running `pdb.set_trace()`. A lot of Python programmers debug with print statements, and I find pdb to be much faster sometimes. Using pdb is straight forward and you can find it's documentation  [here](https://docs.python.org/3/library/pdb.html).



## `for` and `else`
One relatively unknown part of basic python control flow is the `for` and `else` combination. You can include an `else` clause at the end of for loops in python that will execute if no `break` is called. When searching through iteratbles, this can act as an ending "catch-all" if no matching item is found. THe official Python documentation has this example that finds all the prime numbers between 2 and 9 inclusive.
```
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print( n, 'equals', x, '*', n/x)
            break
    else:
        # loop fell through without finding a factor
        print(n, 'is a prime number')
```
If a number `n` doesn't have any factors, we will never hit the `break` and thus the `else` clause will be invoked.

## `del`
Python handles garbage collection automatically, so you don't need to give memory usage or space allocation much thought. However, python still provides teh `del` keyword, which can be used to delete objects and free the space in memory they consumed. Just specify `del obj`. One feature of note is that the `del` operator works on indices. For example, `del lst[3]` will delete the third item of the list `lst`.