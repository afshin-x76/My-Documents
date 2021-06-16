# Builtin Functions

#### 1400/3/24

##### Best practice in 

##### all(iterable)

return True if all the iterable is True

```
a = [True, True] \  all(a) --> True
b = [True, False] \ all(b) -->False
```

##### any(iterable)

Return True if any element of iterable is True

##### ascii(object)

as **repr()** return a string containing a printable representaion of `object`

```
a = [True, False] \ ascii(a) --> '[True, False]'
ascii(45) --> '45'
```

##### bin(int)

Convert an integer to binary string, prefixed with '0b'

```
bin(4) --> '0b100'
bin(3.5) --> TypeError

```

##### bool([x])

return a boolean value

##### breakpoint(*args, **kws)

drops us into a debugger at the call site

##### callable(object)

return True if object is callable, when __call__ is defined in class
it will retunr True

```
class A:
	def __call__(self):
    	pass

b = A() && callable(b) --> True

```

##### chr(i)

return the string representing a character whos unicode code point to integer

```
chr(65) --> 'A' && chr(66542) --> '\U000103ee' && chr(40) --> '('
```

##### @classmethod --> decorator

transform a method(*default is object method*) to class method.

it receives a class as implicit first argument. can access to class attrs with cls prefix. when you change something in class method, it effects on all class instances.


```
class A:
    first = 12
    @classmethod
    def test(cls):
        cls.first = 16
        print(cls.first)
c = A() && c.first --> 12
b = A() && b.test() && b.first --> 16
A.first --> 16
c.first --> 16
```

##### compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)

best example in [programize](https://www.programiz.com/python-programming/methods/built-in/compile)

best practice in [w3school](https://www.w3schools.com/python/ref_func_compile.asp)

##### complex([real[, imag]])

Return a complex number with the value real + imag*1j or convert a string or number to a complex number.

best practice in [programize](https://www.programiz.com/python-programming/methods/built-in/complex)

##### delattr(object, name)

The function deletes the named attribute.

```
class A:
	a=15
    b=14
a = A()
a.a --> 15
delattr(A, 'a')
a.a
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'A' object has no attribute 'a'
```

##### getattr(object, name[, default])

returns the value of the named attribute of an object.

```
class A:
	a=15
    b=14
a = A()
getattr(a, 'a') --> 15
getattr(A, 'a') --> 15
```

##### setattr(object, name, value)

set name and value for object
```
class A:
    a=1
    b=2

setattr(A, "c", 3) && A.c --> 3
b=A()
setattr(b, "d", 4) && b.d --> 4

>>> c=A() 
>>> c.d
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'A' object has no attribute 'd'

>>> A.d
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: type object 'A' has no attribute 'd'
>>> 

```

##### divmod(a, b)

takes 2 number and return a pair of numbers consisting of their quotient and remainder. For integers, the result is the same as (a // b, a % b).

```
divmod(8,3) --> (2, 2)
```

##### enumerate(iterable, start=0)

return an enumerate object, *itrable* must be an iterable object

##### eval(expression[, globals[, locals]])

The arguments are a string and optional globals and locals. If provided, globals must be a dictionary. If provided, locals can be any mapping object.

The expression is evaluated as a Python expression

```
def test_in_mine():
	return "hello"
print(eval(test(), {"test": test_in_mine})) --> "hello"
has access to test_in_mine and builtins
```
if you pass dictionary empty, it means that only has access to builtins

##### exec(object[, globals[, locals]])

The exec() method executes the dynamically created statement, which is either a string or a code object.

```
program = 'a = 5\nb=10\nprint("Sum =", a+b)'
exec(program) --> sum = 15
```
so similar to eval. you can restrict like eval.

##### filter(function, iterable)

construct an iterator(loop) on each of elements of iterable and will filter the iterable base on function return on that element.

return a filter object that is iterable and you can pass it to your iterable like(list(), tuple(),...)
True: keep the element, False: ignore the element

```
def my_filter(num):
	if num > 0:
    	return True
	return False
my_list = [-22,18 , 14, 0, -1, 23, 2]
my_filtered_list =  list(filter(my_filter, my_list)) --> [18, 14, 23, 2]
```
##### format(value[, format_spec])

retunr value in the specific format

```
print(format(123, "d")) --> 123
print(format(123.4567898, "f")) --> 123.456790
print(format(12, "b")) --> 1100
```

##### globals()

returns the dictionary of the current global symbol table

dir() --> will print only names
globals() --> will print those names with their valuse

##### hasattr(object, name)

retunrs True if object has attribute with name

##### hash(object)

The hash() method returns the hash value of an object if it has one. nut numerics

Hash values are just integers that are used to compare dictionary keys during a dictionary lookup quickly.


##### help([object])

Invoke the built-in help system. If no argument is given, the interactive help. system starts on the interpreter console.

```
help(print)

Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.

```

##### hex(x)

Convert an integer number to a lowercase hexadecimal string prefixed with “0x”.
```
hex(255) --> '0xff'
hex(-42) --> '-0x2a'
```
##### id(object)

Return the “identity” of an object. This is an integer which is guaranteed to be unique and constant for this object during its lifetime.

CPython implementation detail: This is the address of the object in memory.

##### input([prompt])

##### isinstance(object, classinfo)

Return True if the object argument is an instance of the classinfo argument, or of a (direct, indirect or virtual) subclass thereof.

```
class A:
    pass

class B(A):
    pass

class C(B):
    pass
    
a = C()
print(isinstance(a, A)) --> True
```

##### issubclass(class, classinfo)

Return True if class is a subclass (direct, indirect or virtual) of classinfo.

```
class A:
    pass

class B(A):
    pass

class C(B):
    pass

print(issubclass(C, A)) --> True
```

##### iter(object[, sentinel])

returns an iterator for the given object.

##### len(s)

Return the length (the number of items) of an object.

##### locals()

Update and return a dictionary representing the current local symbol table.Note that at the module level, locals() and globals() are the same dictionary.

##### map(function, iterable, ...)

Return an iterator that applies function to every item of iterable, yielding the results.

```
def multiply(num):
	return num * num
    
list(map(multiply, [1, 4, 9])) --> [1, 4, 9]
```

##### max(itrable)

Return the largest item in an iterable or the largest of two or more arguments.

##### min(iterable)

##### next(iterator)

Retrieve the next item from the iterator by calling its __next__() method.

##### oct(x)

Convert an integer number to an octal string prefixed with “0o”.

##### open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

Open file and return a corresponding file object. If the file cannot be opened, an OSError is raised.

mode = r, w, x, b, a, t, +

##### ord(c)

Given a string representing one Unicode character, return an integer representing the Unicode code point of that character.

##### pow(base, exp[, mod])

The pow() function returns the power of a number.

pow(x, y) is equal to `x**y`

pow(x, y, z) is equal to `x**y % z`

##### repr(object)

Return a string containing a printable representation of an object. 

##### reversed(seq)

Return a reverse iterator. seq must be an object which has a __reversed__() method or supports the sequence protocol(the __len__() method and the __getitem__() method with integer arguments starting at 0).

##### round(number[, ndigits])

Return number rounded to ndigits precision after the decimal point. If ndigits is omitted or is None, it returns the nearest integer to its input.

```
round(10) --> 10
round(10.4) --> 10
round(10.5) --> 10
round(10.6) --> 11
```

##### sorted(iterable, *, key=None, reverse=False)

Return a new sorted list from the items in iterable.

for examples see [programize](https://www.programiz.com/python-programming/methods/built-in/sorted)

##### @staticmethod

transform a method to static method.

it know nothing about class and object. can be called from class and object

```
class A:
    @staticmethod
    def test(name, last):
        full = name + " " + last
        return full

print(A.test("afshin", "kheiry")) # call it with class
print(A().test("afshin", "kheiry")) # call it with object
```

##### sum(iterable, /, start=0)

adds the items of an iterable and returns the sum. item of iterable should be numbers.
```
numbers = [2.5, 3, 4, -5]

numbers_sum = sum(numbers) --> 4.5

numbers_sum = sum(numbers, 10) --> 14.5
```

##### vars([object])

Return the __dict__ attribute for a module, class, instance, or any other object with a __dict__ attribute.

When you call dir(), the keys are displayed, but when you call vars, their values are also displayed.

##### zip(*iterables)

Make an iterator that aggregates elements from each of the iterables. 

The iterator stops when the shortest iterable is finished.

```
a = [1, 2, 3, 4]
b = ['a', 'b', 'c']
list(zip(a)) --> [(1,), (2,), (3,), (4,)]
list(zip(a, b)) --> [(1, 'a'), (2, 'b'), (3, 'c')] # last argument of a has been omitted
b.append('d') && list(zip(a, b)) --> [(1, 'a'), (2, 'b'), (3, 'c'), (4, 'd')]
c = ['f'] && list(zip(a, b, c)) --> [(1, 'a', 'f')]
```
you can unzip with * operator

`a, b = zip(*<zip object>) `













