# 25 Bad Python Habits

This page goes over 25 common bad habits that a new programmer commonly does when starting out with Python. While some of these habits can impact your code in a bad way, some will also work fine, albeit not as efficiently as the proper method.

This page is inspired from the video [25 Nooby Python Habits You Need to Ditch](https://www.youtube.com/watch?v=qUeud6DvOWI).

## 1. Manual String Formatting
``` python
def manual_str_formatting(name, subscribers):
    # Not great
    if subscribers > 100000:
        print("Wow " + name + "! you have " + str(subscribers) + " subscribers!")
    else:
        print("Lol " + name + " that's not many subs")

    # Better
    if subscribers > 100000:
        print(f"Wow {name}! you have {subscribers} subscribers!")
    else:
        print(f"Lol {name} that's not many subs")
```

Using `fstring`s are easier to write, less prone to errors, and easier to read.

## 2. Manually Closing a File
``` python
def manually_calling_close_on_a_file(filename):
    # Not great
    f = open(filename, "w")
    f.write("hello!\n")
    f.close()

    # Better - close automatic, even if exception
    with open(filename, "w") as f:
        f.write("hello!\n")
```

With the first method, if `f.write("hello!\n")` throws an exception, the file will never close. The resource manager will close the file even when an exception is thrown.

## 3. Using `try:` and `finally:` Instead of a Context Manager
``` python
def finally_instead_of_context_manager(host, port):
    # Not great
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    try:
        s.connect((host, port))
        s.sendall(b'Hello, world')
    finally:
        s.close()

    # Better - close even if exception
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(b'Hello, world')
```

In python, most resources that need to be closed have a context manager, use it!

## 4. Using a Bare `except:` Clause
``` python
def bare_except():
    # Not great
    while True:
        try:
            s = input("Input a number: ")
            x = int(s)
            break
        except:  # oops! can't CTRL-C to exit
            print("Not a number, try again")

    # Better
    while True:
        try:
            s = input("Input a number: ")
            x = int(s)
            break
        except Exception:
            print("Not a number, try again")

    # Even Better
    while True:
        try:
            s = input("Input a number: ")
            x = int(s)
            break
        except ValueError:  # Here, we catch the actual exception that's going to be thrown
            print("Not a number, try again")
```

In python, a bare except is usually going to catch something like hitting Ctrl+C, which is something you almost never want to do.

## 5. Thinking that `^` Means Exponentiation
``` python
def caret_and_exponentiation(x, p):
    y = x ^ p  # Bitwise xor of x and p, not exponentiation
    y = x ** p # Exponentiation
```

## 6. Use of Default Mutable Arguments
``` python
def mutable_default_arguments():
    # Not great
    def append(n, l=[]):
        l.append(n)
        return l

    l1 = append(0)  # [0]
    l2 = append(1)  # [0, 1]

    # Better
    def append(n, l=None):
        if l is None:
            l = []
        l.append(n)
        return l

    l1 = append(0)  # [0]
    l2 = append(1)  # [1]
```

Any use of default mutable arguments is to be avoided. Argument defaults are defined when the function is defined, not when it's run. In the first method, every and any call to the function `append` is sharing the same list!

If you want to define a default, set it to `None` and then check if it's None and set the default there.

## 7. Never Using Comprehensions, or Only Using List Comprehensions
``` python
def never_using_comprehensions():
    # Not great
    squares = {}
    for i in range(10):
        squares[i] = i * i

    # Better
    odd_squares = {i: i * i for i in range(10)}
```

A lot of code can be made shorter and clearer by using a comprehension. You can have dictionary, lists, set and even generator comprehensions (see below). You can learn more about comprehensions [here](https://www.geeksforgeeks.org/comprehensions-in-python/).

``` python
def example_comprehensions():
    dict_comp = {i: i * i for i in range(10)}
    list_comp = {x * x for x in range(10)}
    set_comp = {i%3 for i in range(10)}
    gen_comp = {2*x+5 for x in range(10)}
```

## 8. ALWAYS using comprehensions
``` python
def always_using_comprehensions(a, b, n):
    """matrix product of a, b of length n x n"""
    # Not always needed
    c = [
        sum(a[n * i + k] * b[n * k + j] for k in range(n))
        for i in range(n)
        for j in range(n)
    ]

    # More readable
    c = []
    for i in range(n):
        for j in range(n):
            ij_entry = sum(a[n * i + k] * b[n * k + j] for k in range(n))
            c.append(ij_entry)

    return 
```

Of course, you can flex with your comprehensions, but you don't need to turn every single loop into a comprehension. Think about what is more readable and reasonable to use.

## 9. Checking for a type using `==`
``` python
def checking_type_equality():
    Point = namedtuple('Point', ['x', 'y'])
    p = Point(1, 2)

    # Not great
    if type(p) == tuple:
        print("it's a tuple")
    else:
        print("it's not a tuple")

    # Better
    if isinstance(p, tuple):    # Probably meant to check if is instance of tuple
        print("it's a tuple")
    else:
        print("it's not a tuple")
```

While there are some rare cases where you do want to do this, but most of the time, this is not what you want. The reason is inheritance. In the example above, a namedTuple is a tuple so the `Point` class is a tuple, but it's not literally a tuple, it is a subclass

## 10. Using `==` to check for `None`, `True`, or `False`
``` python
def equality_for_singletons(x):
    # Not great
    if x == None:
        pass

    if x == True:
        pass

    if x == False:
        pass

    # Better
    if x is None:
        pass

    if x is True:
        pass

    if x is False:
        pass
```

Instead of equality, you should check for identity using the `is` comparison. This is what `==` was going to use anyway, so cut out the middle-man and use `is` directly.

## 11. Using an `if bool(x):` or `if len(x)` check
``` python
def checking_bool_or_len(x):
    # Not great
    if bool(x):
        pass

    if len(x) != 0:
        pass

    # Better - Expressions above are usually equivalent to this
    if x:
        pass
```

## 12. Using the `range(len(a))` idiom
``` python
def range_len_pattern():
    a = [1, 2, 3]
    # Not great
    for i in range(len(a)):
        v = a[i]
        ...

    # Better - Range loop
    for v in a:
        ...

    # Or if you wanted the index
    for i, v in enumerate(a):
        ...

    # Using i to sync between two things?
    b = [4, 5, 6]
    # Not great
    for i in range(len(b)):
        av = a[i]
        bv = b[i]
        ...

    # Better - Instead use zip
    for av, bv in zip(a, b):
        ...

    # If you still need the index
    for i, (av, bv) in enumerate(zip(a, b)):
        ...
```

## 13. Looping over the keys of a dictionary
``` python
def for_key_in_dict_keys():
    d = {"a": 1, "b": 2, "c": 3}
    for key in d.keys():
        ...

    # That's the default
    for key in d:
        ...

    # Or if you meant to make a copy of keys
    for key in list():
        ...
```

## 14. Not knowing about the dictionary items methods
``` python
def not_using_dict_items():
    d = {"a": 1, "b": 2, "c": 3}
    # Not great
    for key in d:
        val = d[key]
        ...

    # Better
    for key, val in d.items():
        ...
```

## 15. Not using tuple unpacking
``` python
def tuple_unpacking():
    x = 0
    y = 1

    tmp = x
    x = y
    y = tmp

    x, y = 0, 1
    x, y = y, x

    mytuple = 1, 2

    # Not great
    x = mytuple[0]
    y = mytuple[1]

    # Better
    x, y = mytuple
```

If you have a tuple and want to get both of it's elements out in seprate variables? Use tuple unpacking!

## 16. Creating your own index counter variable
``` python
def index_counter_variable():
    l = [1, 2, 3]

    # Not great
    i = 0
    for x in l:
        ...
        i += 1

    # Better
    for i, x in enumerate(l):
        ...
```

## 17. Using `time.time()` to time things
``` python
def timing_with_time():
    # Not great
    start = time.time()
    time.sleep(1)
    end = time.time()
    print(end - start)

    # Better - More accurate
    start = time.perf_counter()
    time.sleep(1)
    end = time.perf_counter()
    print(end - start)
```

`time.time()` is for telling you what time it currently is, and it's not as accurate as `time.perf_counter()`.

## 18. Not using the logging module
``` python
def print_vs_logging():
    # To be avoided
    print("debug info")
    print("just some info")
    print("bad error")

    # Better
    # in main
    level = logging.DEBUG
    fmt = '[%(levelname)s] %(asctime)s - %(message)s'
    logging.basicConfig(level=level, format=fmt)

    # wherever
    logging.debug("debug info")
    logging.info("just some info")
    logging.error("uh oh :(")
```

You can set up your logging format the way you want it. You can also set up logging to filter out messages that you are not interested in.

## 19. Using `shell = True` on any function in the subprocess library
``` python
def subprocess_with_shell_true():
    # Not great
    subprocess.run(["ls -l"], capture_output=True, shell=True)

    # Better
    subprocess.run(["ls", "-l"], capture_output=True)
```

`shell=True` is the source of a lot of security problems. The most common reason for doing this is to avoid putting your arguments into a list.

## 20. Doing maths or any kind of data analysis in Python
``` python
def not_using_numpy_pandas():
    # Not great - slow
    x = list(range(100))
    y = list(range(100))
    s = [a + b for a, b in zip(x, y)]

    # Better - faster
    x = np.arange(100)
    y = np.arange(100)
    s = x + y
```

Learn to use numpy arrays for math operations, and learn to use pandas for more general data analysis. They run C or C++ in the background and will perform operations much faster that in Python.

## 21. Using `import *` outside of an interactive session
``` python
from itertools import *

# who knows what variables are in scope now?

count()
```

`import *` usually litters your namespace with variables. Instead, just import the things you actually need.

## 22. Depending on a specific directory structure for your project
``` python
from mypackage.nearby_module import awesome_function


def main():
    awesome_function()


if __name__ == '__main__':
    main()
```

Relying on your folder structure to run code successfully makes your repo more fragile. Instead, take the time to create packages from the code you wrote and install it into your environment.

## 23. The common misconception that Python is not compiled
Python isn't compiled down to machine code. Instead, it is compiled down to byte code. That byte is then run by the interpreter.

You will commonly see these byte-code files in a `.pyc` file or a `__pycache__` file.

## 24. Not following PEP 8
PEP 8 is a style guide for writing python code. It is the standard style and makes things a lot easier to read. You can learn more about PEP 8 [here](https://pep8.org/).

## 25. Doing anything to do with Python 2
Python 2 hit its end of life years ago (to the point that it's not supported by modern operating systems anymore). The only reason for keeping Python 2 code around is only if you have millions (and nothing less) of lines of python 2 code.
