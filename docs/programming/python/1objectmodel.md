# 1. The Object Model

This section details the underpinnings of the Python language. It's possible to be a talented Python programmer without knowing any of the details presented in this section, but know them provides a fundamental understanding of the language.

Essentially, everything in python is an object. Every variable you make is an object, every operation you execute is between objects. Seriously. `int`, `float`, functions, and anything else you can thing of: all objects that inherit from python's base object class, unsurprisingly named `object`. What does this mean? In python, primitive data types are in fact not very primitive at all, and are instead feature rich in comparison to their counterparts in lower level languages. 

Let's explore this a little bit by just looking at an integer. We'll create an `int`, then see what attributes it has by calling the `dir` function. When `dir` is called on an object, it recursively searches all the attributes of the argument and it's parents or base classes.
```
>>> n = 1
>>> dir(n)
['__abs__', '__add__', '__and__', '__bool__', '__ceil__', '__class__', '__delattr__', '__dir__', '__divmod__', '__doc__', '__eq__', '__float__', '__floor__', '__floordiv__', '__format__', '__ge__', '__getattribute__', '__getnewargs__', '__gt__', '__hash__', '__index__', '__init__', '__init_subclass__', '__int__', '__invert__', '__le__', '__lshift__', '__lt__', '__mod__', '__mul__', '__ne__', '__neg__', '__new__', '__or__', '__pos__', '__pow__', '__radd__', '__rand__', '__rdivmod__', '__reduce__', '__reduce_ex__', '__repr__', '__rfloordiv__', '__rlshift__', '__rmod__', '__rmul__', '__ror__', '__round__', '__rpow__', '__rrshift__', '__rshift__', '__rsub__', '__rtruediv__', '__rxor__', '__setattr__', '__sizeof__', '__str__', '__sub__', '__subclasshook__', '__truediv__', '__trunc__', '__xor__', 'bit_length', 'conjugate', 'denominator', 'from_bytes', 'imag', 'numerator', 'real', 'to_bytes']
```

Wow! There's a lot more to an integer than just a value. In lower level languages, like C, C++, or Java, primitive types refer to sections of memory, designated to contain a specifc value that can be directly manipulted in assembly code or even transistor level logic in CPUs. Python sits at a much higher abstraction level. Though python is slower than the aforementioned languages, the programmer doesn't have to deal with problems like overflow, underflow, or invalid type casting as all of this is abstracted away through objects. Some the attributes listed through `dir(int)` are class specific to `int`, and some are inherited from `object`. Let's see what attributes `object` by calling `dir` on the static type itself.
```
>>> dir(object)
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
```

By the end of this note, you should understand what most of the different attributes listed above do. But, with so many different types of objects we have to have some way of discerning between different types.

!!! note "NotImplemented"
    Python has one special object run globally called `NotImplemented`. It can be used when object's don't implement some of the default binary function attributes given to them by the `object` class. `type(NotImplemented) = <class 'NotImplementedType'>`.


## Typing
Python provides a few built-in functions to help distinguish between objects of different types.

`type(arg)` : the `type` function returns a Type Object representing the type or class of its argument. For example:
```
>>> type(int)
<class 'type'>
>>> type(type(int))
<class 'type'>
```
Though this function returns the type of an object, there are built-in functions more specifically designed for comparing the types of two differnet objects.

The first is the `isinstance(object, classInfo)` function. It takes in an object (can be static type or instance) as the first argument, and a tuple of types as the second parameter. For example:
```
>>> n = 1.0
>>> isinstance(n, int)
False
>>> isinstance(n, (int, float))
True
```

`issubclass(class, classInfo)` is used less, but is useful for determining if a given class is a sub class of another. For example, `issubclass(int, object)` gives `True`.

### None
The `None` object represents, well, nothing. `None` is globally unique and is the only possible value of objects of type `None`.

## How objects store values
### `__dict__`
In Python, objects store all of their attributes in dictionary or dictionary like objects. The `vars` function lists all of the attributes in an objects `__dict__`. The result of a `vars` call differs if the argument is an object type or an object instance. When called on an object type, for example `vars(int)`, the output is very similar to that of dir (except it won't look at base classes), but it returns a protected dictionary called `mappingproxy` that contains attributes and their values pertinate to the static or class level attributes of a type. Calling `vars` on an instance returns the attributes pertinate to the instance. Here's an example to demonstrate:

```
>>> class Test(object):
...     static_val = 0
...     def test(self):
...         pass
... 
>>> t = Test()
>>> t.inst_val = 1
>>> vars(Test)
mappingproxy({'__module__': '__main__', 'static_val': 0, 'test': <function Test.test at 0x7f072c48d730>, '__dict__': <attribute '__dict__' of 'Test' objects>, '__weakref__': <attribute '__weakref__' of 'Test' objects>, '__doc__': None})
>>> vars(t)
{'inst_val': 1}
```

!!! note
    calling vars is essentially equivalent to accessing the `__dict__` attribute of an object type or an object instance). It's worth noting that some object instances, like those of  type `int` do not actually have a `__dict__` attribute. This really only seems to apply to primitive-like or built-in types.

In fact, because Python objects store their values using the `__dict__` attribute, we can actually create very basic objects using the `type` function. By passing more parameters to the `type` function, it actually creates a new type object. The following are two identical ways of creating the same object.

```
>>> class X:
...     a = 1
...
>>> X = type('X', (object,), dict(a=1))
```

### Getters and Setters.
Python additionally has a set of built in set of methods for getting, setting, and deleting attributes. The `object` base class has the following getter and setter methods which are wrapped by built-in python functions.

| Object Method | Built-in Function | Description|
|-------------------------------------|---------------------------------|---------------------------------------|
| `object.__getattribute__('name')`   | `getattr(object, 'name')`        | Gets attribute                        |
| `object.__setattr__('name', value)` | `setattr(object, 'name', value)` | Sets attribute to value               |
| `object.__delattr('name')`          | `delattr(object, 'name')`        | Deletes attribute                     |
| N/A                                 | `hasattr(object, 'name')`        | Checks if an object has an attribute. |

## Mutable vs Immutable Objects

In Python, objects fit into one of two categories: mutable, or immutable. Immutable objects have to be copied in order to change, whereas mutable objects do not. Upon instantiation, every object in Python is assigned a unique ID that often corresponds to it's location in memory and can be accessed via the `id(obj)` method. Immutable objects derive their concepet of uniqueness from the values they contian, rather than their instantiation in memory. This means that if we change the value of an immutable object held by a variable, we are actually creating a new object and changing the variable to point at the new object. Here's an example:

```
>>> a = 0; id(a)
10968768
>>> b = 1; id(b)
10968800
>>> a +=1; id(a)
10968800
>>> c = "hi"; id(c); d = "hi"; id(d)
140191817812880
140191817812880
```
Notice how on each assignment, the ID of each variable completely changes as it points to a new object. Primitive like types are immutable because of the speed benefits an immutable object offers. As primitive objects are not large or complex, we do not incur a large cost when copying or re-instantiating them upon changing a value. Additionally, _passing immutable types to functions is always safe_, as we can't actually modify the underlying object is a variable is pointing to. 

!!! note
    ID's will not always be the same for the "same" immutable object. For example, if we execute `a = "hi"; b = "h"; b += "i"`, `id(a)` and `id(b)` will not be the same. This is because we don't recheck memory to determine if the same string has already been used and instead just allocate new space for the new string. 

__The following are Immutable in Python__: int, float, complex, string, tuple, frozen set, bytes

Every other object is mutable. Mutable objects are not copied and are instead modified in place. This means that operations on mutable types can be unsafe when passed to functions as the function is able to change the underlying object. For example,

```
>>> a = [1]; id(a)
140191817820232
>>> b = [1, 2]; id(b)
140191817818952
>>> a.append(2); id(a)
140191817820232
```

### is vs ==
The python `is` operator simply compares the `id` of the two objects, or does an integer comparison `id(a) == id(b)`. The `==` operator on the other hand, calls the special `__eq__` comparison operator defined between objects. Unless overrided, this defaults to simply doing the same id comparison as `is`. Thus, unless the `__eq__` method is specified, `==` comparisons between custom objects in Python won't behave as expected.


### A Tricky Example

In Python, tuples are a basic immutable data type that represent a fixed length of fixed values. Let's look at a complicated example using tuples to drive mutability vs immutability home.

```
>>> a, b = (0, [1, 2]) , (0, [1, 2])
>>> a == b; id(a) == id(b)
True
False
```

What happened here? Even though the tuples look identical, they are actually different! This is because the list created for `a` and the list created for `b` are actually distinct, different objects as they are mutable. When we check `a == b`, the `__eq__` function is called recursively on all the members of the two tuples, thus showing they are equal, but the IDs of the two tuples are different because they were created with different list objects. If we instantiated them with the same list, they would have the same id. `l = [1, 2]; id( (0, l) ) == id( (0, l) )` yeilds True.

## Type Casting In Python
One consequence of all of the object model is that Python really does not have "casting" in the traditional sense. When you convert a `int` to a `float` by running something along the lines of `float(n)`, a specific verison of the `float` constructor is actually called that takes in an argument. A new object is then created based off of the parameter. Thus, when you type cast, you are really just initializing a new object. 

## Special Object Functions and Operators

In order to enable consistent usage across all of the different objects in Python, they all utilize the same special methods denoted with two underscores on either side `__func__`. Overriding these objects in your own implementations can lead to neat and concise code for complex functions. They can be overriden by defining the `__func__(self, [args])` function within an object. These functions are often refered to as "magic" functions. Ommited from this list are the getter and setter attributes discussed previously in this section and other attributes that are more specific to different types of data structures.

| Attribute      | Built-In Accesor          | Descripton                                                                                                               |
|----------------|---------------------------|--------------------------------------------------------------------------------------------------------------------------|
| `__new__`      | N/A                       | Called when creating a new instance of an object                                                                         |
| `__init__`     | N/A                       | Called when instantiating a new instance of an object, after `__new__`                                                   |
| `__class__`    | `type(obj)`               | Returns the type of a given object.                                                                                      |
| `__str__`      | str(obj)                  | Converts object to a string. This is called when printed.                                                                |
| `__repr__`     | repr(obj)                 | Returns a representation of the object used in the python shell.                                                         |
| `__doc__`      | `help(obj)                | Gives documentation for the given type.                                                                                  |
| `__hash__`     | `hash(obj)`               | Custom hash function for the object.                                                                                     |
| `__get__` | N/A | Invoked every time an object is accessed. Used by function objects to make them callable. Similar definition for `__set__`|
| `__eq__`       | `==`                      | Provides comparison binary operator. Other comaprison operators are `__lt__`, `__le__`, `__ne__`, `__ge__`, `__gt__`     |
| `__add__`      | `add(a, b), +`            | Overrides math functionality. Others are `__sub__`, `__mul__`, `__abs__`, `__floordiv__`, `__truediv__`, `__pow__`, etc. |
| `__getitem__`  | `object[ind]`             | Returns value of an object at a certain index                                                                            |
| `__and__`      | `and`                     | Returns logical and. Other logical operators are `__or__`, `__not__`, `__xor__`, `__lshift__`, etc.                      |
| `__contains__` | `val in obj`              | Checks to see if an object contains a value using `in` keyword.                                                          |
| `__next__`     | `next(obj)`               | yeilds next value of an object                                                                                           |
| `__yield__`    | `iter(obj), for _ in obj` | returns an iterator over the values of the object                                                                        |

You can find more operator attributes at this link: https://docs.python.org/3/library/operator.html

## Sources and Further Reading
https://docs.python.org/3.7/reference/datamodel.html
https://docs.python.org/3/library/functions.html
https://docs.python.org/3/library/constants.html
https://docs.python.org/3/library/stdtypes.html