# Python

## Table of Contents

- [Python](#python)
  - [Table of Contents](#table-of-contents)
  - [Introduction to Python](#introduction-to-python)
    - [What is Python?](#what-is-python)
    - [Features of Python](#features-of-python)
    - [Applications of Python](#applications-of-python)
    - [How Python Works](#how-python-works)
    - [Python vs JavaScript — High-Level Comparison](#python-vs-javascript--high-level-comparison)
  - [Setting Up Python](#setting-up-python)
    - [Installing Python](#installing-python)
    - [pip — Package Manager](#pip--package-manager)
    - [Virtual Environments](#virtual-environments)
    - [pyenv — Python Version Manager](#pyenv--python-version-manager)
    - [Running Python](#running-python)
  - [Data Types](#data-types)
    - [Numeric Types](#numeric-types)
    - [Boolean](#boolean)
    - [String](#string)
    - [NoneType](#nonetype)
    - [Sequence Types](#sequence-types)
    - [Mapping Type](#mapping-type)
    - [Set Types](#set-types)
    - [Type Conversion](#type-conversion)
  - [Variables and Type Checking](#variables-and-type-checking)
    - [Variable Assignment](#variable-assignment)
    - [Multiple Assignment](#multiple-assignment)
    - [type() and isinstance()](#type-and-isinstance)
    - [Dynamic vs Static Typing](#dynamic-vs-static-typing)
    - [Constants Convention](#constants-convention)
  - [Operators](#operators)
    - [Arithmetic Operators](#arithmetic-operators)
    - [Assignment Operators](#assignment-operators)
    - [Comparison Operators](#comparison-operators)
    - [Logical Operators](#logical-operators)
    - [Identity Operators](#identity-operators)
    - [Membership Operators](#membership-operators)
    - [Bitwise Operators](#bitwise-operators)
    - [Ternary (Conditional) Expression](#ternary-conditional-expression)
    - [Walrus Operator](#walrus-operator)
  - [Truthy and Falsy](#truthy-and-falsy)
    - [Falsy Values](#falsy-values)
    - [Truthy Values](#truthy-values)
    - [Short-Circuiting](#short-circuiting)
  - [Strings](#strings)
    - [String Basics](#string-basics)
    - [f-Strings (Template Literals Equivalent)](#f-strings-template-literals-equivalent)
    - [Multiline Strings](#multiline-strings)
    - [String Methods](#string-methods)
    - [String Slicing](#string-slicing)
  - [Control Flow](#control-flow)
    - [if / elif / else](#if--elif--else)
    - [match / case (Structural Pattern Matching)](#match--case-structural-pattern-matching)
  - [Loops](#loops)
    - [for Loop](#for-loop)
    - [while Loop](#while-loop)
    - [break, continue, pass](#break-continue-pass)
    - [else on Loops](#else-on-loops)
    - [range()](#range)
    - [enumerate()](#enumerate)
    - [zip()](#zip)
  - [Functions](#functions)
    - [Defining Functions](#defining-functions)
    - [Default Parameters](#default-parameters)
    - [*args and **kwargs (Rest / Spread Equivalent)](#args-and-kwargs-rest--spread-equivalent)
    - [Keyword-Only and Positional-Only Arguments](#keyword-only-and-positional-only-arguments)
    - [Lambda Functions (Arrow Function Equivalent)](#lambda-functions-arrow-function-equivalent)
    - [Closures](#closures)
    - [IIFE Equivalent](#iife-equivalent)
    - [Higher-Order Functions](#higher-order-functions)
    - [Recursion](#recursion)
  - [Decorators](#decorators)
    - [What is a Decorator?](#what-is-a-decorator)
    - [Function Decorators](#function-decorators)
    - [Decorator with Arguments](#decorator-with-arguments)
    - [Class Decorators](#class-decorators)
    - [functools.wraps](#functoolswraps)
    - [Built-in Decorators](#built-in-decorators)
  - [Data Structures](#data-structures)
    - [Lists (Array Equivalent)](#lists-array-equivalent)
    - [Tuples](#tuples)
    - [Sets](#sets)
    - [Dictionaries (Object / Map Equivalent)](#dictionaries-object--map-equivalent)
    - [collections Module](#collections-module)
  - [Comprehensions](#comprehensions)
    - [List Comprehensions](#list-comprehensions)
    - [Dictionary Comprehensions](#dictionary-comprehensions)
    - [Set Comprehensions](#set-comprehensions)
    - [Generator Expressions](#generator-expressions)
  - [Unpacking (Destructuring Equivalent)](#unpacking-destructuring-equivalent)
    - [Tuple / List Unpacking](#tuple--list-unpacking)
    - [Extended Unpacking (Spread Equivalent)](#extended-unpacking-spread-equivalent)
    - [Dictionary Unpacking](#dictionary-unpacking)
    - [Swapping Variables](#swapping-variables)
  - [Modules and Packages](#modules-and-packages)
    - [Importing Modules](#importing-modules)
    - [Creating Your Own Module](#creating-your-own-module)
    - [Packages](#packages)
    - [__name__ == "__main__"](#name--main)
    - [Relative Imports](#relative-imports)
  - [Object-Oriented Programming](#object-oriented-programming)
    - [Classes and Objects](#classes-and-objects)
    - [__init__ and Instance Attributes](#__init__-and-instance-attributes)
    - [Instance, Class, and Static Methods](#instance-class-and-static-methods)
    - [Properties — Getters and Setters](#properties--getters-and-setters)
    - [Inheritance](#inheritance)
    - [Multiple Inheritance and MRO](#multiple-inheritance-and-mro)
    - [Mixins](#mixins)
    - [Dunder (Magic) Methods](#dunder-magic-methods)
    - [Abstract Classes](#abstract-classes)
    - [Dataclasses](#dataclasses)
    - [__slots__](#slots)
  - [Exception Handling](#exception-handling)
    - [try / except / else / finally](#try--except--else--finally)
    - [Built-in Exception Hierarchy](#built-in-exception-hierarchy)
    - [Custom Exceptions](#custom-exceptions)
    - [Raising Exceptions](#raising-exceptions)
    - [Context-Aware Error Handling Patterns](#context-aware-error-handling-patterns)
  - [Type Hints (TypeScript Equivalent)](#type-hints-typescript-equivalent)
    - [Basic Type Hints](#basic-type-hints)
    - [Optional and Union](#optional-and-union)
    - [Collections with Type Hints](#collections-with-type-hints)
    - [Callable Types](#callable-types)
    - [TypeVar and Generics](#typevar-and-generics)
    - [TypedDict](#typeddict)
    - [Protocol (Interface Equivalent)](#protocol-interface-equivalent)
    - [Literal, Final, ClassVar](#literal-final-classvar)
    - [TYPE_CHECKING Guard](#type_checking-guard)
  - [Iterators and Generators](#iterators-and-generators)
    - [Iterators](#iterators)
    - [Generators](#generators)
    - [Generator Expressions](#generator-expressions-1)
    - [yield from](#yield-from)
    - [Infinite Generators](#infinite-generators)
  - [Context Managers](#context-managers)
    - [with Statement](#with-statement)
    - [Custom Context Manager — Class](#custom-context-manager--class)
    - [Custom Context Manager — contextlib](#custom-context-manager--contextlib)
  - [File I/O](#file-io)
    - [Reading Files](#reading-files)
    - [Writing Files](#writing-files)
    - [pathlib — Modern File Paths](#pathlib--modern-file-paths)
    - [JSON Files](#json-files)
    - [CSV Files](#csv-files)
  - [Async / Await (asyncio)](#async--await-asyncio)
    - [Synchronous vs Asynchronous](#synchronous-vs-asynchronous)
    - [Coroutines and async def](#coroutines-and-async-def)
    - [asyncio.run and event loop](#asynciorun-and-event-loop)
    - [await and Chaining](#await-and-chaining)
    - [asyncio.gather — Parallel Coroutines](#asynciogather--parallel-coroutines)
    - [Async Generators and Comprehensions](#async-generators-and-comprehensions)
    - [asyncio Tasks](#asyncio-tasks)
    - [Async Context Managers](#async-context-managers)
  - [Functional Programming](#functional-programming)
    - [map, filter, zip, reduce](#map-filter-zip-reduce)
    - [functools Module](#functools-module)
    - [itertools Module](#itertools-module)
    - [Partial Functions](#partial-functions)
  - [Standard Library Highlights](#standard-library-highlights)
    - [os and sys](#os-and-sys)
    - [datetime](#datetime)
    - [re — Regular Expressions](#re--regular-expressions)
    - [json](#json)
    - [collections](#collections)
    - [enum](#enum)
    - [typing](#typing)
    - [copy](#copy)
    - [random](#random)
    - [math](#math)
  - [Concurrency and Parallelism](#concurrency-and-parallelism)
    - [The GIL (Global Interpreter Lock)](#the-gil-global-interpreter-lock)
    - [threading](#threading)
    - [multiprocessing](#multiprocessing)
    - [concurrent.futures](#concurrentfutures)
    - [asyncio vs threading vs multiprocessing](#asyncio-vs-threading-vs-multiprocessing)
  - [Advanced Python](#advanced-python)
    - [Descriptors](#descriptors)
    - [Metaclasses](#metaclasses)
    - [Memory Management and Garbage Collection](#memory-management-and-garbage-collection)
    - [WeakRef](#weakref)
    - [Python Data Model Summary](#python-data-model-summary)
  - [Copying Objects](#copying-objects)
    - [Shallow Copy](#shallow-copy)
    - [Deep Copy](#deep-copy)
  - [Python vs JavaScript — Feature Map](#python-vs-javascript--feature-map)

---

## Introduction to Python

### What is Python?

Python is a **high-level, interpreted, dynamically typed, general-purpose programming language** created by Guido van Rossum and first released in 1991. It emphasises code readability through significant indentation and clean syntax.

```
Python = Readability + Versatility + Huge Ecosystem
```

Python is both **imperative** (step-by-step instructions) and supports **declarative / functional** programming styles.

### Features of Python

| Feature | Detail |
|---|---|
| Interpreted | Executed line-by-line; no separate compile step for the developer |
| Dynamically typed | Variable types are resolved at runtime |
| Strongly typed | Unlike JS, Python **does not** silently coerce types (`"5" + 5` raises `TypeError`) |
| Multi-paradigm | Supports procedural, OOP, and functional styles |
| Garbage collected | Automatic memory management via reference counting + cyclic GC |
| Cross-platform | Runs on Windows, macOS, Linux, embedded systems |
| Batteries included | Extensive standard library |
| Open source | Free to use and distribute |

### Applications of Python

- **Web development** — Django, FastAPI, Flask
- **Data science & ML** — NumPy, Pandas, TensorFlow, PyTorch, scikit-learn
- **Scripting & automation** — system scripts, file processing, task automation
- **DevOps & infrastructure** — Ansible, AWS CDK, tool development
- **Scientific computing** — SciPy, Matplotlib
- **Desktop GUIs** — Tkinter, PyQt
- **Cybersecurity** — penetration testing tools, exploits
- **Game development** — Pygame

### How Python Works

```
.py source  →  CPython bytecode compiler  →  .pyc bytecode  →  Python VM (interpreter)
```

- **CPython** is the reference (default) implementation written in C
- Other implementations: **PyPy** (JIT-compiled, faster), **Jython** (JVM), **MicroPython** (microcontrollers)
- Python files: `.py` (source), `.pyc` (compiled bytecode, auto-cached in `__pycache__/`)

### Python vs JavaScript — High-Level Comparison

| Aspect | Python | JavaScript |
|---|---|---|
| Primary use | Backend, data science, scripting | Frontend, backend (Node.js) |
| Typing | Dynamic + strong | Dynamic + weak (coerces types) |
| Whitespace | Significant (indentation = blocks) | Braces `{}` |
| `null` equivalent | `None` | `null` / `undefined` |
| Template strings | f-strings `f"Hello {name}"` | Template literals `` `Hello ${name}` `` |
| Arrow functions | Lambda (limited) / named `def` | `() => {}` |
| Array equivalent | `list` | `Array` |
| Object equivalent | `dict` | Plain object `{}` / `Map` |
| Async model | `asyncio` (cooperative, event loop) | Event loop (browser / Node.js) |
| Package manager | `pip` | `npm` / `yarn` / `pnpm` |

---

## Setting Up Python

### Installing Python

```bash
# Download from python.org, or use a package manager:

# macOS (Homebrew)
brew install python@3.12

# Ubuntu / Debian
sudo apt install python3 python3-pip

# Windows — use the official installer from python.org
# or Windows Store: python3
```

Always install **Python 3.x** (Python 2 is EOL since 2020).

Verify:
```bash
python3 --version   # or: python --version on Windows
```

### pip — Package Manager

`pip` is Python's standard package manager (equivalent to `npm`).

```bash
pip install requests          # install a package
pip install requests==2.31.0  # specific version
pip uninstall requests        # uninstall
pip list                      # list installed packages
pip show requests             # show package info
pip freeze > requirements.txt # export dependencies
pip install -r requirements.txt  # install from file
pip install --upgrade pip     # upgrade pip itself
```

### Virtual Environments

A virtual environment isolates project dependencies (equivalent to `node_modules` per project, but explicit).

```bash
# Create a virtual environment
python3 -m venv .venv

# Activate
# macOS / Linux:
source .venv/bin/activate
# Windows (PowerShell):
.venv\Scripts\Activate.ps1
# Windows (cmd):
.venv\Scripts\activate.bat

# Deactivate
deactivate

# Install packages into the venv (always activate first)
pip install flask
```

> **Best practice:** Always use a virtual environment per project. Add `.venv/` to `.gitignore`.

### pyenv — Python Version Manager

`pyenv` manages multiple Python versions side-by-side (equivalent to `nvm` for Node.js).

```bash
# Install (macOS / Linux — see pyenv docs for Windows: pyenv-win)
curl https://pyenv.run | bash

# List available versions
pyenv install --list

# Install a specific version
pyenv install 3.12.3

# Set global version
pyenv global 3.12.3

# Set local version (per directory, writes .python-version)
pyenv local 3.11.8
```

### Running Python

```bash
python3 script.py          # run a script
python3 -c "print('hi')"  # one-liner
python3 -m module_name     # run a module as script (-m venv, -m http.server, etc.)
python3                    # open interactive REPL (Ctrl+D to exit)
```

---

## Data Types

Python's built-in types cover all common use cases. Unlike JavaScript, every value has a single, clear type.

### Numeric Types

```python
# int — arbitrary precision integer
age = 30
big = 10_000_000       # underscores for readability
binary = 0b1010        # 10
hexadecimal = 0xFF     # 255

# float — IEEE 754 double precision
price = 19.99
scientific = 1.5e-3    # 0.0015

# complex
z = 3 + 4j
print(z.real, z.imag)  # 3.0  4.0

# Arithmetic always promotes to the "wider" type:
print(type(3 / 2))     # <class 'float'>  — true division
print(type(3 // 2))    # <class 'int'>    — floor division
```

### Boolean

```python
# bool is a subclass of int in Python
active = True
inactive = False

print(int(True))   # 1
print(int(False))  # 0
print(True + True) # 2  (not recommended, but valid)
```

### String

Strings are **immutable** sequences of Unicode characters.

```python
name = "Alice"
name = 'Alice'      # both work
name = """Alice"""  # also valid
```

See the [Strings](#strings) section for full coverage.

### NoneType

`None` is Python's equivalent of `null` (and loosely `undefined`). It is a singleton.

```python
result = None

# Check for None — always use 'is', not ==
if result is None:
    print("no value")

if result is not None:
    print("has value")
```

> `None` is falsy. `== None` works but `is None` is idiomatic Python.

### Sequence Types

```python
# list — mutable, ordered, allows duplicates
fruits = ["apple", "banana", "cherry"]

# tuple — immutable, ordered, allows duplicates
coordinates = (10, 20)
single = (42,)       # trailing comma required for 1-element tuple

# range — lazy sequence of integers
r = range(10)        # 0..9
r = range(1, 11)     # 1..10
r = range(0, 10, 2)  # 0,2,4,6,8
```

### Mapping Type

```python
# dict — mutable, ordered (insertion order, Python 3.7+), key-value pairs
person = {"name": "Alice", "age": 30}
person["email"] = "alice@example.com"  # add / update

# Keys must be hashable (str, int, tuple, frozenset — NOT list or dict)
```

### Set Types

```python
# set — mutable, unordered, unique elements
colors = {"red", "green", "blue"}
colors.add("yellow")

# frozenset — immutable set (hashable, can be a dict key)
fs = frozenset(["a", "b", "c"])
```

### Type Conversion

```python
int("42")          # 42
int(3.9)           # 3  (truncates, not rounds)
float("3.14")      # 3.14
str(100)           # "100"
bool(0)            # False
bool("hello")      # True
list((1, 2, 3))    # [1, 2, 3]
tuple([1, 2, 3])   # (1, 2, 3)
set([1, 2, 2, 3])  # {1, 2, 3}
dict([("a", 1)])   # {"a": 1}

# Careful: Python does NOT implicitly coerce
"5" + 5            # TypeError! (unlike JS which gives "55")
```

---

## Variables and Type Checking

### Variable Assignment

```python
# No keyword needed — just assign
name = "Alice"
age = 30
is_active = True

# Python infers the type at runtime
x = 10
x = "now a string"  # valid, variables can change type
```

### Multiple Assignment

```python
a = b = c = 0        # all point to same object
x, y, z = 1, 2, 3   # tuple unpacking
a, b = b, a          # swap
```

### type() and isinstance()

```python
x = 42
print(type(x))           # <class 'int'>
print(type(x).__name__)  # 'int'

# isinstance checks against a type or tuple of types (preferred)
isinstance(x, int)         # True
isinstance(x, (int, float)) # True — is it int OR float?

# type() is strict (no inheritance)
isinstance(True, int)   # True  — bool is subclass of int
type(True) is int       # False — type(True) is bool, not int
type(True) is bool      # True
```

### Dynamic vs Static Typing

Python is **dynamically typed** at runtime, but supports optional **static type hints** checked by external tools like `mypy`.

```python
# No type hints — runs fine, no IDE support
def add(a, b):
    return a + b

# With type hints — same runtime behaviour, but mypy/pyright catches misuse
def add(a: int, b: int) -> int:
    return a + b

add(1, 2)       # fine
add("1", "2")   # mypy error (but runs at runtime!)
```

See the [Type Hints](#type-hints-typescript-equivalent) section for full coverage.

### Constants Convention

Python has no built-in `const`. By convention, constants are `ALL_CAPS` module-level names:

```python
MAX_RETRIES = 3
PI = 3.14159
BASE_URL = "https://api.example.com"
```

Use `Final` from `typing` for static-analysis enforcement:

```python
from typing import Final

MAX: Final = 100
MAX = 200  # mypy error
```

---

## Operators

### Arithmetic Operators

```python
10 + 3   # 13  — addition
10 - 3   # 7   — subtraction
10 * 3   # 30  — multiplication
10 / 3   # 3.3333... — true division (always float)
10 // 3  # 3   — floor division (integer result)
10 % 3   # 1   — modulo (remainder)
10 ** 3  # 1000 — exponentiation (JS uses Math.pow or **)
-10 // 3 # -4  — floor rounds toward negative infinity
```

### Assignment Operators

```python
x = 10
x += 5   # 15
x -= 3   # 12
x *= 2   # 24
x /= 4   # 6.0  (becomes float!)
x //= 2  # 3.0
x %= 2   # 1.0
x **= 3  # 1.0
```

### Comparison Operators

```python
5 == 5    # True
5 != 4    # True
5 > 3     # True
5 < 3     # False
5 >= 5    # True
5 <= 4    # False

# Python allows chaining (JS does not)
1 < 2 < 3      # True  (reads naturally)
1 < 2 > 0      # True
```

### Logical Operators

Python uses **English words** instead of symbols (`&&`, `||`, `!`):

```python
True and False   # False  (JS: &&)
True or False    # True   (JS: ||)
not True         # False  (JS: !)

# and / or return the actual value, not just True/False (short-circuits)
0 or "default"   # "default"
"hello" and 42   # 42
```

### Identity Operators

```python
x is y      # True if x and y are the SAME object (same memory address)
x is not y  # True if different objects

# Use 'is' for None checks (not ==)
result = None
result is None      # True (correct)
result == None      # True (works, but not idiomatic)
```

> `is` tests object identity (like `===` for object references in JS). `==` tests equality (value).

### Membership Operators

```python
"a" in ["a", "b", "c"]       # True
"x" not in ["a", "b", "c"]   # True
"ell" in "hello"              # True  — works on strings too
"key" in {"key": 1}           # True  — checks dict keys
```

### Bitwise Operators

```python
5 & 3    # 1  — AND
5 | 3    # 7  — OR
5 ^ 3    # 6  — XOR
~5       # -6 — NOT
5 << 1   # 10 — left shift
5 >> 1   # 2  — right shift
```

### Ternary (Conditional) Expression

```python
# Python syntax: value_if_true if condition else value_if_false
age = 20
label = "adult" if age >= 18 else "minor"

# JS equivalent: age >= 18 ? "adult" : "minor"
```

### Walrus Operator

The walrus operator `:=` (Python 3.8+) assigns AND returns a value — useful in conditions:

```python
# Without walrus
data = get_data()
if data:
    process(data)

# With walrus — assign and test in one expression
if data := get_data():
    process(data)

# Useful in while loops
while chunk := file.read(8192):
    process(chunk)
```

---

## Truthy and Falsy

Python has a clear set of falsy values. Everything else is truthy.

### Falsy Values

```python
False
None
0          # int zero
0.0        # float zero
0j         # complex zero
""         # empty string
[]         # empty list
()         # empty tuple
{}         # empty dict
set()      # empty set
b""        # empty bytes
range(0)   # empty range
```

Any object whose `__bool__` returns `False` or `__len__` returns `0`.

### Truthy Values

```python
True
1               # any non-zero number
"hello"         # any non-empty string
[1, 2]          # any non-empty list
{"a": 1}        # any non-empty dict
# etc.
```

```python
# Practical use
items = []
if not items:
    print("empty list")

name = ""
display = name or "Anonymous"   # "Anonymous"
```

### Short-Circuiting

Python's `and` / `or` short-circuit and **return the deciding value**, not a boolean:

```python
# OR — returns first truthy value, or last value
"" or 0 or "hello" or 42    # "hello"
"" or 0 or False            # False   (exhausted, return last)

# AND — returns first falsy value, or last value
"hi" and 42 and "world"     # "world" (all truthy → return last)
"hi" and 0 and "world"      # 0       (found falsy → stop)

# Practical patterns
config = user_config or default_config    # fallback
name = user.name and user.name.upper()   # safe attribute access
```

---

## Strings

### String Basics

```python
s = "Hello, World!"
s = 'Hello, World!'          # equivalent
s = """Multi
line"""                      # triple quotes (also used for docstrings)

# Strings are immutable — you cannot do s[0] = "h"
len(s)                       # 13
s[0]                         # 'H'
s[-1]                        # '!'  (negative indexing)
```

### f-Strings (Template Literals Equivalent)

f-strings (Python 3.6+) are the modern, preferred way to embed expressions in strings:

```python
name = "Alice"
age = 30

# f-string (JS equivalent: `Hello, ${name}!`)
greeting = f"Hello, {name}! You are {age} years old."

# Expressions inside {}
print(f"Next year: {age + 1}")
print(f"Upper: {name.upper()}")
print(f"Pi: {3.14159:.2f}")        # formatting specifier

# Debug shorthand (Python 3.8+)
x = 42
print(f"{x = }")    # prints: x = 42
```

Older alternatives (less preferred):
```python
"Hello, %s!" % name                      # % formatting (legacy)
"Hello, {}!".format(name)               # .format() method
"Hello, {name}!".format(name=name)      # named
```

### Multiline Strings

```python
text = """
This is a
multiline string.
"""

# Or use \ to avoid the newline
sql = (
    "SELECT *"
    " FROM users"
    " WHERE active = 1"
)  # string literal concatenation — no + needed
```

### String Methods

```python
s = "  Hello, World!  "

s.strip()           # "Hello, World!"  — removes leading/trailing whitespace
s.lstrip()          # removes leading only
s.rstrip()          # removes trailing only

s.lower()           # "  hello, world!  "
s.upper()           # "  HELLO, WORLD!  "
s.title()           # "  Hello, World!  "

s.replace("World", "Python")  # "  Hello, Python!  "

s.split(",")        # ['  Hello', ' World!  ']
",".join(["a","b","c"])  # "a,b,c"

s.startswith("  He")   # True
s.endswith("!  ")      # True

s.find("World")     # 9  (-1 if not found)
s.count("l")        # 3

s.strip().center(20, "-")  # "--Hello, World!---"

"42".zfill(5)       # "00042"
"hello".capitalize()  # "Hello"
"Hello World".split()   # ['Hello', 'World'] — splits on any whitespace by default
```

### String Slicing

```python
s = "Hello, World!"
#    0123456789...

s[0:5]    # "Hello"   — start:stop (stop exclusive)
s[7:]     # "World!"  — to end
s[:5]     # "Hello"   — from beginning
s[-6:]    # "orld!"   — from 6th from end
s[::2]    # every other character
s[::-1]   # "!dlroW ,olleH" — reversed
```

---

## Control Flow

### if / elif / else

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "F"

print(f"Grade: {grade}")
```

Python uses **indentation** (typically 4 spaces) to define blocks — no braces.

### match / case (Structural Pattern Matching)

Python 3.10+ — similar to switch/case but far more powerful:

```python
command = "quit"

match command:
    case "quit":
        print("Quitting")
    case "help":
        print("Showing help")
    case _:         # wildcard / default
        print("Unknown command")

# Matching with conditions (guards)
point = (1, 0)
match point:
    case (0, 0):
        print("Origin")
    case (x, 0):
        print(f"On x-axis at {x}")
    case (0, y):
        print(f"On y-axis at {y}")
    case (x, y):
        print(f"Point at ({x}, {y})")

# Matching types
def classify(obj):
    match obj:
        case int():
            return "integer"
        case str():
            return "string"
        case list():
            return "list"
        case _:
            return "unknown"
```

---

## Loops

### for Loop

Python's `for` loop always iterates over an **iterable** — it has no traditional 3-part `for(;;)` form:

```python
# Over a list
for fruit in ["apple", "banana", "cherry"]:
    print(fruit)

# Over a string
for char in "hello":
    print(char)

# Over a range
for i in range(5):       # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 6):    # 1, 2, 3, 4, 5
    print(i)
```

### while Loop

```python
count = 0
while count < 5:
    print(count)
    count += 1

# Infinite loop with break
while True:
    data = input("Enter: ")
    if data == "quit":
        break
    print(f"You said: {data}")
```

### break, continue, pass

```python
for i in range(10):
    if i == 3:
        continue    # skip to next iteration
    if i == 7:
        break       # exit loop entirely
    print(i)

# pass — no-op placeholder (needed where a statement is syntactically required)
for i in range(5):
    pass    # do nothing — valid empty loop
```

### else on Loops

Python loops can have an `else` block that runs **only if the loop completed without a `break`**:

```python
for item in items:
    if item == target:
        print("Found!")
        break
else:
    print("Not found")  # runs only if no break
```

### range()

```python
range(stop)           # 0 to stop-1
range(start, stop)    # start to stop-1
range(start, stop, step)  # with step

list(range(5))           # [0, 1, 2, 3, 4]
list(range(2, 10, 3))    # [2, 5, 8]
list(range(10, 0, -2))   # [10, 8, 6, 4, 2]  — countdown
```

### enumerate()

Equivalent to `array.forEach((item, index) => ...)`:

```python
fruits = ["apple", "banana", "cherry"]

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")

# Start index at 1
for i, fruit in enumerate(fruits, start=1):
    print(f"{i}. {fruit}")
```

### zip()

Combine multiple iterables in lockstep:

```python
names = ["Alice", "Bob", "Charlie"]
scores = [95, 87, 92]

for name, score in zip(names, scores):
    print(f"{name}: {score}")

# Convert to list of tuples
pairs = list(zip(names, scores))
# [("Alice", 95), ("Bob", 87), ("Charlie", 92)]

# zip stops at shortest — use itertools.zip_longest to pad
```

---

## Functions

### Defining Functions

```python
def greet(name):
    """Return a greeting string."""   # docstring
    return f"Hello, {name}!"

result = greet("Alice")   # "Hello, Alice!"

# Functions return None by default if no return statement
def do_nothing():
    pass

print(do_nothing())   # None
```

### Default Parameters

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Alice")              # "Hello, Alice!"
greet("Bob", "Hi")          # "Hi, Bob!"
greet("Eve", greeting="Hey")  # "Hey, Eve!"  — keyword argument

# GOTCHA: mutable defaults are shared across calls!
def bad_append(item, lst=[]):   # DON'T do this
    lst.append(item)
    return lst

# CORRECT — use None as sentinel
def good_append(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst
```

### *args and **kwargs (Rest / Spread Equivalent)

```python
# *args — variable positional arguments (like JS rest: ...args)
def sum_all(*args):
    return sum(args)

sum_all(1, 2, 3, 4)   # 10

# **kwargs — variable keyword arguments
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=30, city="NYC")

# Combining all parameter types
def mixed(a, b, *args, keyword_only, **kwargs):
    pass

# Spread / unpack when calling (like JS spread: ...array)
nums = [1, 2, 3]
print(*nums)           # 1 2 3  — unpacks list as positional args

config = {"sep": ", ", "end": "!\n"}
print("a", "b", "c", **config)   # unpacks dict as keyword args
```

### Keyword-Only and Positional-Only Arguments

```python
# Keyword-only: parameters after * must be passed by name
def draw(x, y, *, color="black", width=1):
    pass

draw(10, 20, color="red")     # fine
draw(10, 20, "red")           # TypeError — color is keyword-only

# Positional-only: parameters before / must be passed positionally
def distance(x, y, /):
    return (x**2 + y**2) ** 0.5

distance(3, 4)          # fine
distance(x=3, y=4)      # TypeError — positional-only

# Combined
def full(pos_only, /, normal, *, kw_only):
    pass
```

### Lambda Functions (Arrow Function Equivalent)

Lambdas are single-expression anonymous functions. Unlike JS arrow functions, they cannot contain statements.

```python
# lambda arguments: expression
square = lambda x: x ** 2
square(5)   # 25

add = lambda a, b: a + b
add(3, 4)   # 7

# Common use — passing as argument
numbers = [3, 1, 4, 1, 5]
numbers.sort(key=lambda x: -x)   # sort descending

# For multi-line logic, always use a def instead
```

### Closures

A closure is a function that **captures variables from its enclosing scope** — identical concept to JavaScript closures:

```python
def make_counter(start=0):
    count = start

    def increment():
        nonlocal count    # needed to modify enclosing variable
        count += 1
        return count

    return increment

counter = make_counter()
counter()   # 1
counter()   # 2
counter()   # 3

# Another closure
def multiplier(factor):
    return lambda x: x * factor

double = multiplier(2)
triple = multiplier(3)
double(5)   # 10
triple(5)   # 15
```

> `nonlocal` is Python's way to write to an enclosing scope variable (JS just assigns — no keyword needed because JS doesn't need it due to scoping rules).

### IIFE Equivalent

Python has no built-in IIFE syntax, but the same pattern is achievable:

```python
# IIFE via immediate lambda call (rare in practice)
result = (lambda x: x * 2)(21)   # 42

# More Pythonic: just use a regular function or block
result = (lambda: 42)()

# Or simply use a local scope with a function
def _():
    x = "local"
    return x

result = _()
```

### Higher-Order Functions

```python
def apply(func, value):
    return func(value)

apply(str.upper, "hello")   # "HELLO"
apply(len, [1, 2, 3])       # 3

# Functions as return values
def power_of(n):
    def pow(base):
        return base ** n
    return pow

square = power_of(2)
cube = power_of(3)
square(4)   # 16
cube(3)     # 27
```

### Recursion

```python
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

factorial(5)   # 120

# Fibonacci with memoization
from functools import lru_cache

@lru_cache(maxsize=None)
def fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

fib(50)   # fast
```

> Python's default recursion limit is 1000. Increase with `sys.setrecursionlimit(n)` if needed.

---

## Decorators

Decorators are Python's equivalent of TypeScript decorators — they wrap functions or classes to add behaviour.

### What is a Decorator?

A decorator is a **callable that takes a function and returns a new function** (or the same one, modified). The `@decorator` syntax is syntactic sugar:

```python
@decorator
def my_func():
    pass

# Equivalent to:
def my_func():
    pass
my_func = decorator(my_func)
```

### Function Decorators

```python
def log(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Done {func.__name__}")
        return result
    return wrapper

@log
def add(a, b):
    return a + b

add(2, 3)
# Calling add
# Done add
# 5
```

### Decorator with Arguments

```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hello():
    print("Hello!")

say_hello()
# Hello!
# Hello!
# Hello!
```

### Class Decorators

```python
def singleton(cls):
    instances = {}
    def get_instance(*args, **kwargs):
        if cls not in instances:
            instances[cls] = cls(*args, **kwargs)
        return instances[cls]
    return get_instance

@singleton
class Database:
    def __init__(self):
        print("Connecting...")

db1 = Database()   # Connecting...
db2 = Database()   # (no output — same instance)
db1 is db2         # True
```

### functools.wraps

Always use `@functools.wraps(func)` inside wrapper functions to preserve the original function's metadata (`__name__`, `__doc__`, etc.):

```python
from functools import wraps

def log(func):
    @wraps(func)         # preserves func.__name__, func.__doc__
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

@log
def my_func():
    """My function docstring."""
    pass

my_func.__name__   # "my_func"  (not "wrapper")
my_func.__doc__    # "My function docstring."
```

### Built-in Decorators

```python
class MyClass:
    class_var = 0

    @staticmethod
    def static_method(x):       # no self or cls
        return x * 2

    @classmethod
    def class_method(cls):      # receives class, not instance
        cls.class_var += 1

    @property
    def computed(self):         # acts as getter — call without ()
        return self._value * 2
```

---

## Data Structures

### Lists (Array Equivalent)

```python
# Creating
nums = [1, 2, 3, 4, 5]
empty = []
mixed = [1, "hello", True, None]

# Indexing and slicing (same as strings)
nums[0]       # 1
nums[-1]      # 5
nums[1:3]     # [2, 3]
nums[::-1]    # [5, 4, 3, 2, 1]  — reversed

# Modifying
nums.append(6)          # add to end       [1,2,3,4,5,6]
nums.insert(0, 0)       # insert at index  [0,1,2,3,4,5,6]
nums.extend([7, 8])     # append multiple  [0,...,8]
nums.pop()              # remove & return last
nums.pop(0)             # remove & return at index
nums.remove(3)          # remove first occurrence of value
del nums[0]             # delete by index

# Searching
3 in nums               # True / False
nums.index(4)           # first index of 4
nums.count(2)           # count occurrences

# Sorting
nums.sort()             # in-place sort
nums.sort(reverse=True)
sorted(nums)            # returns new sorted list (original unchanged)
sorted(words, key=str.lower)  # custom key

# Other
len(nums)               # length
nums.reverse()          # in-place reverse
nums.copy()             # shallow copy
nums.clear()            # empty the list
```

### Tuples

Tuples are **immutable** lists. Use them for fixed collections (coordinates, RGB, records).

```python
point = (3, 4)
rgb = (255, 128, 0)
single = (42,)     # trailing comma required

# Access
point[0]           # 3
x, y = point       # unpacking

# Tuples are hashable — can be dict keys / set members
d = {(0, 0): "origin"}

# Named tuples — tuple with named fields
from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(3, 4)
p.x, p.y   # 3, 4
```

### Sets

```python
# Creating
colors = {"red", "green", "blue"}
colors = set(["red", "green", "blue"])  # from list
empty_set = set()   # NOT {} — that's an empty dict!

# Operations
colors.add("yellow")
colors.discard("red")    # remove if present (no error if missing)
colors.remove("red")     # remove — raises KeyError if missing

# Set math
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
a | b          # {1,2,3,4,5,6} — union
a & b          # {3,4}         — intersection
a - b          # {1,2}         — difference (in a not in b)
a ^ b          # {1,2,5,6}     — symmetric difference

# Membership (O(1) lookup)
"red" in colors
"purple" not in colors
```

### Dictionaries (Object / Map Equivalent)

```python
# Creating
person = {"name": "Alice", "age": 30}
person = dict(name="Alice", age=30)  # keyword style
empty = {}

# Access
person["name"]              # "Alice" — KeyError if missing
person.get("email")         # None    — safe access
person.get("email", "N/A")  # "N/A"  — with default

# Modifying
person["email"] = "alice@example.com"    # add / update
del person["age"]                         # delete key
person.pop("age", None)                   # pop with default
person.update({"age": 31, "city": "NYC"}) # merge/update multiple

# Iterating
for key in person:              # iterate keys
    pass
for value in person.values():   # values
    pass
for key, value in person.items():  # key-value pairs
    pass

# Useful methods
person.keys()     # dict_keys([...])
person.values()   # dict_values([...])
person.items()    # dict_items([...])
"name" in person  # check key existence

# Merging dicts (Python 3.9+)
merged = {**dict1, **dict2}    # spread operator equivalent
merged = dict1 | dict2         # union operator (3.9+)
dict1 |= dict2                 # in-place merge (3.9+)

# defaultdict — auto-initializes missing keys
from collections import defaultdict
counts = defaultdict(int)
counts["a"] += 1    # no KeyError
```

### collections Module

```python
from collections import Counter, deque, OrderedDict, defaultdict, namedtuple

# Counter — count occurrences
words = ["apple", "banana", "apple", "cherry"]
c = Counter(words)
# Counter({'apple': 2, 'banana': 1, 'cherry': 1})
c.most_common(2)   # [('apple', 2), ('banana', 1)]

# deque — double-ended queue (efficient append/prepend)
dq = deque([1, 2, 3])
dq.appendleft(0)   # [0, 1, 2, 3]
dq.popleft()       # 0, deque is [1, 2, 3]
dq.rotate(1)       # [3, 1, 2]

# deque with maxlen — circular buffer
recent = deque(maxlen=3)
for i in range(6):
    recent.append(i)
# deque([3, 4, 5], maxlen=3)
```

---

## Comprehensions

Comprehensions are Python's powerful, concise syntax for building collections (like `map` + `filter` combined).

### List Comprehensions

```python
# Basic form: [expression for item in iterable]
squares = [x**2 for x in range(10)]
# [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# With condition (filter): [expression for item in iterable if condition]
evens = [x for x in range(20) if x % 2 == 0]

# Equivalent to:
evens = list(filter(lambda x: x % 2 == 0, range(20)))

# Nested loops
matrix = [[i * j for j in range(1, 4)] for i in range(1, 4)]
# [[1,2,3],[2,4,6],[3,6,9]]

flat = [cell for row in matrix for cell in row]
# [1, 2, 3, 2, 4, 6, 3, 6, 9]
```

### Dictionary Comprehensions

```python
# {key: value for item in iterable}
squares = {x: x**2 for x in range(6)}
# {0:0, 1:1, 2:4, 3:9, 4:16, 5:25}

# Invert a dict
original = {"a": 1, "b": 2, "c": 3}
inverted = {v: k for k, v in original.items()}
# {1: "a", 2: "b", 3: "c"}

# With condition
filtered = {k: v for k, v in original.items() if v > 1}
# {"b": 2, "c": 3}
```

### Set Comprehensions

```python
# {expression for item in iterable}
unique_lengths = {len(word) for word in ["hi", "hello", "hey", "world"]}
# {2, 5}
```

### Generator Expressions

Like list comprehensions but **lazy** — compute values on demand without building the full list in memory:

```python
# Parentheses instead of brackets
gen = (x**2 for x in range(1_000_000))  # no memory spike
next(gen)    # 0
next(gen)    # 1

# Often passed directly to functions that accept iterables
total = sum(x**2 for x in range(100))
any(x > 90 for x in scores)
all(x > 0 for x in numbers)
```

---

## Unpacking (Destructuring Equivalent)

### Tuple / List Unpacking

```python
# Basic unpacking (like JS array destructuring)
x, y = (3, 4)
a, b, c = [1, 2, 3]

# Swap (no temp variable needed)
a, b = b, a

# Ignore values with _
_, second, _ = (1, 2, 3)
```

### Extended Unpacking (Spread Equivalent)

```python
# * captures the "rest"
first, *rest = [1, 2, 3, 4, 5]
# first=1, rest=[2,3,4,5]

*head, last = [1, 2, 3, 4, 5]
# head=[1,2,3,4], last=5

first, *middle, last = [1, 2, 3, 4, 5]
# first=1, middle=[2,3,4], last=5

# JS equivalent: const [first, ...rest] = [1, 2, 3, 4, 5]
```

### Dictionary Unpacking

```python
config = {"host": "localhost", "port": 8080}

# Spread into another dict (JS: {...config, ...overrides})
overrides = {"port": 9090}
merged = {**config, **overrides}
# {"host": "localhost", "port": 9090}

# Unpack as function keyword arguments
def connect(host, port):
    pass

connect(**config)   # equivalent to connect(host="localhost", port=8080)
```

### Swapping Variables

```python
a, b = 1, 2
a, b = b, a   # swap without temp variable
```

---

## Modules and Packages

Python's module system is the equivalent of JS ES Modules (`import` / `export`).

### Importing Modules

```python
import math                       # import whole module
math.pi                           # 3.14159...
math.sqrt(16)                     # 4.0

from math import pi, sqrt         # named imports (like JS: import { pi, sqrt } from 'math')
pi                                # 3.14159...

from math import pi as PI         # alias import
from math import *                # import all (avoid — pollutes namespace)

import os.path                    # sub-module
os.path.join("a", "b")
```

### Creating Your Own Module

Any `.py` file is a module:

```python
# utils.py
def greet(name):
    return f"Hello, {name}!"

PI = 3.14159
```

```python
# main.py
import utils
utils.greet("Alice")

from utils import greet, PI
greet("Bob")
```

### Packages

A **package** is a directory with an `__init__.py` file (can be empty):

```
mypackage/
    __init__.py
    math_utils.py
    string_utils.py
```

```python
from mypackage import math_utils
from mypackage.string_utils import slugify
```

### __name__ == "__main__"

Guards code that should only run when the file is executed directly (not imported):

```python
# script.py
def main():
    print("Running main!")

if __name__ == "__main__":
    main()
```

```bash
python script.py      # prints "Running main!"
# import script       # does NOT print (name is "script", not "__main__")
```

### Relative Imports

Inside a package, use `.` for relative imports:

```python
# mypackage/math_utils.py
from . import string_utils          # sibling module
from .string_utils import slugify   # named import from sibling
from .. import other_pkg            # parent package
```

---

## Object-Oriented Programming

### Classes and Objects

```python
class Dog:
    # Class variable (shared by all instances)
    species = "Canis lupus familiaris"

    # Constructor
    def __init__(self, name, age):
        # Instance variables
        self.name = name
        self.age = age

    # Instance method
    def bark(self):
        return f"{self.name} says: Woof!"

    def __repr__(self):
        return f"Dog(name={self.name!r}, age={self.age})"

# Instantiation
buddy = Dog("Buddy", 3)
rex = Dog("Rex", 5)

buddy.bark()         # "Buddy says: Woof!"
Dog.species          # "Canis lupus familiaris"
buddy.species        # also accessible on instances
```

### __init__ and Instance Attributes

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance   # _ prefix = "private by convention"

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Amount must be positive")
        self._balance += amount

    def withdraw(self, amount):
        if amount > self._balance:
            raise ValueError("Insufficient funds")
        self._balance -= amount
        return amount

    def __str__(self):
        return f"Account({self.owner}: ${self._balance})"
```

### Instance, Class, and Static Methods

```python
class Temperature:
    _unit = "Celsius"

    def __init__(self, degrees):
        self.degrees = degrees

    def to_fahrenheit(self):          # instance method — has self
        return self.degrees * 9/5 + 32

    @classmethod
    def get_unit(cls):                # class method — has cls (the class)
        return cls._unit

    @classmethod
    def from_fahrenheit(cls, f):      # alternative constructor pattern
        return cls((f - 32) * 5/9)

    @staticmethod
    def is_valid(degrees):            # static method — no self or cls
        return degrees >= -273.15

t = Temperature(100)
t.to_fahrenheit()                    # 212.0
Temperature.get_unit()               # "Celsius"
t2 = Temperature.from_fahrenheit(212)  # Temperature(100)
Temperature.is_valid(-300)           # False
```

### Properties — Getters and Setters

Python's `@property` is cleaner than JS getters/setters syntax:

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):           # getter — access as attribute: c.radius
        return self._radius

    @radius.setter
    def radius(self, value):    # setter — c.radius = 5
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value

    @radius.deleter
    def radius(self):           # deleter — del c.radius
        del self._radius

    @property
    def area(self):             # computed, read-only property
        import math
        return math.pi * self._radius ** 2

c = Circle(5)
c.radius         # 5   — calls getter
c.radius = 10   # calls setter
c.area           # 314.159...
```

### Inheritance

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def speak(self):
        raise NotImplementedError("Subclasses must implement speak()")

    def __repr__(self):
        return f"{type(self).__name__}({self.name!r})"

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

# super() — call parent method (equivalent to JS super())
class PoliceDog(Dog):
    def __init__(self, name, badge):
        super().__init__(name)   # call Dog.__init__ (which calls Animal.__init__)
        self.badge = badge

    def speak(self):
        return f"[Badge {self.badge}] {super().speak()}"

pd = PoliceDog("Rex", "K9-001")
pd.speak()   # "[Badge K9-001] Woof!"

# isinstance and issubclass
isinstance(pd, PoliceDog)   # True
isinstance(pd, Dog)         # True
isinstance(pd, Animal)      # True
issubclass(Dog, Animal)     # True
```

### Multiple Inheritance and MRO

Python supports multiple inheritance. The **Method Resolution Order (MRO)** determines which method is called:

```python
class A:
    def greet(self): return "A"

class B(A):
    def greet(self): return "B"

class C(A):
    def greet(self): return "C"

class D(B, C):   # inherits from B and C
    pass

D().greet()        # "B" — MRO: D → B → C → A
D.__mro__          # (D, B, C, A, object)
```

Python uses the **C3 linearization** algorithm to determine MRO — no ambiguity.

### Mixins

Mixins are reusable classes that add functionality without being used standalone:

```python
class JSONMixin:
    def to_json(self):
        import json
        return json.dumps(self.__dict__)

class LogMixin:
    def log(self, message):
        print(f"[{type(self).__name__}] {message}")

class User(LogMixin, JSONMixin):
    def __init__(self, name, email):
        self.name = name
        self.email = email

u = User("Alice", "alice@example.com")
u.to_json()    # '{"name": "Alice", "email": "alice@example.com"}'
u.log("Created")  # [User] Created
```

### Dunder (Magic) Methods

Dunder methods (double underscore) let you define how objects behave with operators and built-ins:

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):           # repr(v) — developer string
        return f"Vector({self.x}, {self.y})"

    def __str__(self):            # str(v) / print(v) — user string
        return f"({self.x}, {self.y})"

    def __add__(self, other):     # v1 + v2
        return Vector(self.x + other.x, self.y + other.y)

    def __sub__(self, other):     # v1 - v2
        return Vector(self.x - other.x, self.y - other.y)

    def __mul__(self, scalar):    # v * 3
        return Vector(self.x * scalar, self.y * scalar)

    def __rmul__(self, scalar):   # 3 * v (reversed)
        return self.__mul__(scalar)

    def __eq__(self, other):      # v1 == v2
        return self.x == other.x and self.y == other.y

    def __len__(self):            # len(v)
        return 2

    def __getitem__(self, index): # v[0], v[1]
        return (self.x, self.y)[index]

    def __iter__(self):           # for coord in v:
        yield self.x
        yield self.y

    def __abs__(self):            # abs(v)
        return (self.x**2 + self.y**2) ** 0.5

    def __bool__(self):           # if v:
        return self.x != 0 or self.y != 0

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v1 + v2    # Vector(4, 6)
abs(v1)    # 2.236
```

**Common dunder methods reference:**

| Method | Triggered by |
|---|---|
| `__init__` | `MyClass()` |
| `__repr__` | `repr(obj)`, REPL display |
| `__str__` | `str(obj)`, `print(obj)` |
| `__len__` | `len(obj)` |
| `__getitem__` | `obj[key]` |
| `__setitem__` | `obj[key] = val` |
| `__delitem__` | `del obj[key]` |
| `__contains__` | `item in obj` |
| `__iter__` | `for x in obj` |
| `__next__` | `next(obj)` |
| `__call__` | `obj()` |
| `__enter__` / `__exit__` | `with obj:` |
| `__add__`, `__sub__`, etc. | `+`, `-`, etc. |
| `__eq__`, `__lt__`, etc. | `==`, `<`, etc. |
| `__hash__` | `hash(obj)`, use in sets/dicts |
| `__bool__` | `bool(obj)`, `if obj:` |

### Abstract Classes

Python uses the `abc` module to define abstract base classes (like TypeScript interfaces/abstract classes):

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> float:
        pass

    @abstractmethod
    def perimeter(self) -> float:
        pass

    def describe(self):           # concrete method
        return f"Area: {self.area():.2f}, Perimeter: {self.perimeter():.2f}"

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w = w
        self.h = h

    def area(self):
        return self.w * self.h

    def perimeter(self):
        return 2 * (self.w + self.h)

# Shape()          # TypeError — can't instantiate abstract class
r = Rectangle(4, 5)
r.area()            # 20
r.describe()        # "Area: 20.00, Perimeter: 18.00"
```

### Dataclasses

`@dataclass` auto-generates `__init__`, `__repr__`, `__eq__`, and more:

```python
from dataclasses import dataclass, field

@dataclass
class Point:
    x: float
    y: float

p = Point(3.0, 4.0)
print(p)       # Point(x=3.0, y=4.0)
p == Point(3.0, 4.0)   # True

@dataclass(order=True, frozen=True)  # immutable + comparable
class Config:
    host: str = "localhost"
    port: int = 8080
    tags: list = field(default_factory=list)  # mutable default

@dataclass
class Employee:
    name: str
    department: str
    salary: float = 0.0

    def __post_init__(self):   # called after __init__
        if self.salary < 0:
            raise ValueError("Salary cannot be negative")
```

### __slots__

`__slots__` restricts instance attributes and reduces memory (no `__dict__` per instance):

```python
class Point:
    __slots__ = ("x", "y")

    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(1, 2)
p.z = 3    # AttributeError — z not in __slots__
```

---

## Exception Handling

### try / except / else / finally

```python
try:
    result = 10 / int(input("Enter divisor: "))
except ValueError:
    print("That's not a number!")
except ZeroDivisionError:
    print("Can't divide by zero!")
except (TypeError, ArithmeticError) as e:
    print(f"Math error: {e}")
except Exception as e:
    print(f"Unexpected: {e}")
else:
    # Runs only if NO exception was raised in try
    print(f"Result: {result}")
finally:
    # Always runs — cleanup (like JS finally)
    print("Done")
```

### Built-in Exception Hierarchy

```
BaseException
├── SystemExit
├── KeyboardInterrupt
└── Exception
    ├── TypeError
    ├── ValueError
    ├── AttributeError
    ├── NameError
    ├── IndexError
    ├── KeyError
    ├── FileNotFoundError  (subclass of OSError)
    ├── PermissionError    (subclass of OSError)
    ├── RuntimeError
    ├── StopIteration
    ├── NotImplementedError
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   └── OverflowError
    └── OSError
        ├── FileNotFoundError
        └── PermissionError
```

### Custom Exceptions

```python
class AppError(Exception):
    """Base class for application errors."""

class ValidationError(AppError):
    def __init__(self, field, message):
        self.field = field
        super().__init__(f"Validation error on '{field}': {message}")

class NotFoundError(AppError):
    def __init__(self, resource, id):
        super().__init__(f"{resource} with id {id!r} not found")

# Usage
try:
    raise ValidationError("email", "invalid format")
except ValidationError as e:
    print(e.field)   # "email"
    print(e)         # "Validation error on 'email': invalid format"
```

### Raising Exceptions

```python
def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("b cannot be zero")
    return a / b

# Re-raise with context
try:
    data = load_file("missing.json")
except FileNotFoundError as e:
    raise RuntimeError("Config file missing") from e
    # "from e" chains the original exception (shown in traceback)

# Suppress exception context (raise from None)
try:
    risky()
except Exception:
    raise CustomError("Failed") from None
```

### Context-Aware Error Handling Patterns

```python
# Result pattern (avoiding exceptions for expected failures)
def safe_divide(a, b):
    if b == 0:
        return None, "Division by zero"
    return a / b, None

result, error = safe_divide(10, 0)
if error:
    print(f"Error: {error}")

# Suppress specific exceptions
from contextlib import suppress

with suppress(FileNotFoundError):
    os.remove("maybe_missing.txt")
```

---

## Type Hints (TypeScript Equivalent)

Python's type hints (PEP 484+) are the static-typing equivalent of TypeScript. They don't affect runtime behaviour but are checked by tools like `mypy` and `pyright`.

### Basic Type Hints

```python
name: str = "Alice"
age: int = 30
score: float = 9.5
active: bool = True

def greet(name: str) -> str:
    return f"Hello, {name}!"

def increment(n: int) -> int:
    return n + 1

def nothing() -> None:
    pass
```

### Optional and Union

```python
from typing import Optional, Union   # Python 3.9 and earlier

# Optional[X] is equivalent to Union[X, None]
def find_user(id: int) -> Optional[str]:
    return "Alice" if id == 1 else None

# Union — accepts multiple types
def process(value: Union[int, str]) -> str:
    return str(value)

# Python 3.10+ shorthand (X | Y)
def greet(name: str | None = None) -> str:
    return f"Hello, {name or 'stranger'}!"

def process(value: int | str) -> str:
    return str(value)
```

### Collections with Type Hints

```python
from typing import List, Dict, Tuple, Set   # Python 3.8 and earlier

def average(nums: List[float]) -> float:
    return sum(nums) / len(nums)

def lookup(data: Dict[str, int]) -> None:
    pass

# Python 3.9+ — use built-in types directly (no import needed)
def average(nums: list[float]) -> float:
    return sum(nums) / len(nums)

def lookup(data: dict[str, int]) -> None:
    pass

def parse(coords: tuple[int, int]) -> None:
    pass

def unique(items: set[str]) -> None:
    pass
```

### Callable Types

```python
from typing import Callable

# Callable[[arg_types], return_type]
def apply(func: Callable[[int], int], value: int) -> int:
    return func(value)

# Any callable
from typing import Callable
def run(callback: Callable[..., None]) -> None:
    callback()
```

### TypeVar and Generics

```python
from typing import TypeVar, Generic

T = TypeVar("T")

# Generic function
def first(items: list[T]) -> T:
    return items[0]

first([1, 2, 3])      # returns int
first(["a", "b"])     # returns str

# Generic class
class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: list[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()

stack: Stack[int] = Stack()
stack.push(1)
stack.pop()   # int
```

### TypedDict

TypedDict defines the shape of a dictionary (equivalent to TypeScript interface for objects):

```python
from typing import TypedDict

class UserDict(TypedDict):
    name: str
    age: int
    email: str

# With optional fields
class PartialUser(TypedDict, total=False):
    name: str
    age: int

def process_user(user: UserDict) -> str:
    return user["name"]

user: UserDict = {"name": "Alice", "age": 30, "email": "alice@example.com"}
```

### Protocol (Interface Equivalent)

`Protocol` defines structural subtyping — similar to TypeScript interfaces (duck typing with type checking):

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None: ...

class Circle:
    def draw(self) -> None:
        print("Drawing circle")

class Square:
    def draw(self) -> None:
        print("Drawing square")

def render(shape: Drawable) -> None:
    shape.draw()

# Both Circle and Square satisfy Drawable
# No explicit inheritance needed — structural!
render(Circle())
render(Square())
```

### Literal, Final, ClassVar

```python
from typing import Literal, Final, ClassVar

# Literal — restrict to specific values (like TypeScript string unions)
def set_direction(d: Literal["left", "right", "up", "down"]) -> None:
    pass

set_direction("left")   # fine
set_direction("north")  # mypy error

# Final — value cannot be reassigned
MAX: Final = 100

# ClassVar — marks a class-level variable (not instance)
class Config:
    debug: ClassVar[bool] = False
```

### TYPE_CHECKING Guard

Avoid circular imports — import types only during type checking:

```python
from __future__ import annotations  # makes all annotations strings (lazy evaluation)
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from mymodule import HeavyClass   # only imported by type checkers, not at runtime

def process(obj: "HeavyClass") -> None:
    pass
```

---

## Iterators and Generators

### Iterators

An object is **iterable** if it implements `__iter__`. An **iterator** additionally implements `__next__`.

```python
class CountUp:
    def __init__(self, start, stop):
        self.current = start
        self.stop = stop

    def __iter__(self):
        return self        # iterator is its own iterable

    def __next__(self):
        if self.current >= self.stop:
            raise StopIteration
        val = self.current
        self.current += 1
        return val

for n in CountUp(1, 5):
    print(n)   # 1 2 3 4

# Built-in iter() and next()
it = iter([10, 20, 30])
next(it)   # 10
next(it)   # 20
next(it)   # 30
next(it)   # StopIteration
next(it, "default")  # "default" — safe default
```

### Generators

Generators are **functions that yield values lazily** — the simplest way to create iterators. Equivalent to JavaScript generators (`function*` / `yield`):

```python
def count_up(start, stop):
    current = start
    while current < stop:
        yield current        # pause and return value
        current += 1         # resume here on next()

gen = count_up(1, 5)
next(gen)   # 1
next(gen)   # 2

for n in count_up(1, 5):
    print(n)   # 1 2 3 4

# Generator state is preserved between yields
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
[next(fib) for _ in range(8)]   # [0, 1, 1, 2, 3, 5, 8, 13]
```

### Generator Expressions

```python
# Lazy version of list comprehension
gen = (x**2 for x in range(1000000))  # computed on demand
total = sum(x**2 for x in range(100)) # no intermediate list
```

### yield from

`yield from` delegates to a sub-generator (equivalent to JS `yield*`):

```python
def chain(*iterables):
    for it in iterables:
        yield from it   # delegates all values from it

list(chain([1, 2], [3, 4], [5]))   # [1, 2, 3, 4, 5]

# Recursive generator
def flatten(nested):
    for item in nested:
        if isinstance(item, list):
            yield from flatten(item)
        else:
            yield item

list(flatten([1, [2, [3, 4]], 5]))   # [1, 2, 3, 4, 5]
```

### Infinite Generators

```python
import itertools

# itertools provides ready-made infinite iterators
count = itertools.count(start=0, step=1)   # 0, 1, 2, 3...
cycle = itertools.cycle([1, 2, 3])         # 1, 2, 3, 1, 2, 3...
repeat = itertools.repeat(42, times=3)     # 42, 42, 42

# Take first N from infinite generator
import itertools
first_10 = list(itertools.islice(itertools.count(), 10))
# [0, 1, 2, ..., 9]
```

---

## Context Managers

Context managers handle setup/teardown automatically via the `with` statement — cleaner than try/finally.

### with Statement

```python
# File handling — file is automatically closed
with open("file.txt") as f:
    data = f.read()
# f is closed here, even if an exception occurred

# Multiple context managers
with open("input.txt") as fin, open("output.txt", "w") as fout:
    fout.write(fin.read())

# Database connection example
with db.connect() as conn:
    conn.execute("SELECT 1")
# conn.close() called automatically
```

### Custom Context Manager — Class

Implement `__enter__` and `__exit__`:

```python
class Timer:
    import time

    def __enter__(self):
        import time
        self.start = time.perf_counter()
        return self        # value bound to 'as' variable

    def __exit__(self, exc_type, exc_val, exc_tb):
        import time
        elapsed = time.perf_counter() - self.start
        print(f"Elapsed: {elapsed:.4f}s")
        return False       # False = don't suppress exceptions

with Timer() as t:
    result = sum(range(1_000_000))
# prints: Elapsed: 0.0123s
```

### Custom Context Manager — contextlib

```python
from contextlib import contextmanager

@contextmanager
def timer():
    import time
    start = time.perf_counter()
    try:
        yield              # code inside 'with' runs here
    finally:
        print(f"Elapsed: {time.perf_counter() - start:.4f}s")

with timer():
    result = sum(range(1_000_000))

# Suppress specific exception
from contextlib import suppress
with suppress(FileNotFoundError):
    os.remove("maybe_missing.txt")
```

---

## File I/O

### Reading Files

```python
# Read entire file as string
with open("file.txt") as f:
    content = f.read()

# Read line by line (memory efficient for large files)
with open("file.txt") as f:
    for line in f:
        print(line.strip())

# Read all lines into a list
with open("file.txt") as f:
    lines = f.readlines()     # includes \n
    lines = f.read().splitlines()  # strips \n

# Specify encoding (always explicit for non-ASCII)
with open("file.txt", encoding="utf-8") as f:
    content = f.read()
```

### Writing Files

```python
# Write (creates or overwrites)
with open("output.txt", "w") as f:
    f.write("Hello, World!\n")

# Append
with open("log.txt", "a") as f:
    f.write("New log entry\n")

# Write multiple lines
lines = ["line 1\n", "line 2\n", "line 3\n"]
with open("output.txt", "w") as f:
    f.writelines(lines)

# Binary mode
with open("image.png", "rb") as f:
    data = f.read()
```

### pathlib — Modern File Paths

`pathlib.Path` is the modern, OOP approach to file paths (Python 3.4+):

```python
from pathlib import Path

# Creating paths — works on any OS
p = Path("data") / "users" / "alice.json"
p = Path.home() / "Documents" / "notes.txt"
p = Path.cwd()          # current working directory

# Path operations
p.exists()              # True/False
p.is_file()
p.is_dir()
p.suffix                # ".json"
p.stem                  # "alice"
p.name                  # "alice.json"
p.parent                # Path("data/users")
p.parts                 # ("data", "users", "alice.json")

# File operations
p.read_text(encoding="utf-8")           # read file as string
p.write_text("content", encoding="utf-8")  # write file
p.read_bytes()                          # read binary

# Directory operations
Path("mydir").mkdir(parents=True, exist_ok=True)  # like mkdir -p
list(Path(".").iterdir())               # list directory
list(Path(".").glob("**/*.py"))         # recursive glob
Path("old.txt").rename("new.txt")
Path("file.txt").unlink()               # delete file
```

### JSON Files

```python
import json

# Read JSON
with open("data.json", encoding="utf-8") as f:
    data = json.load(f)     # file → Python object

# Write JSON
with open("data.json", "w", encoding="utf-8") as f:
    json.dump(data, f, indent=2, ensure_ascii=False)

# String conversion
text = json.dumps({"name": "Alice"}, indent=2)  # dict → string
obj = json.loads(text)                           # string → dict
```

### CSV Files

```python
import csv

# Read CSV
with open("data.csv", newline="", encoding="utf-8") as f:
    reader = csv.DictReader(f)    # rows as dicts using header row
    for row in reader:
        print(row["name"], row["age"])

# Write CSV
with open("output.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["name", "age"])
    writer.writeheader()
    writer.writerow({"name": "Alice", "age": 30})
```

---

## Async / Await (asyncio)

Python's `asyncio` module provides cooperative concurrency — similar to JavaScript's async/await built on the event loop.

### Synchronous vs Asynchronous

```python
# Synchronous — blocks until complete
import time

def fetch_sync(url):
    time.sleep(2)   # simulate network request
    return f"data from {url}"

# Sequential — takes 4 seconds total
r1 = fetch_sync("url1")
r2 = fetch_sync("url2")

# Asynchronous — yields control while waiting
import asyncio

async def fetch_async(url):
    await asyncio.sleep(2)   # non-blocking sleep
    return f"data from {url}"

# Concurrent — takes ~2 seconds total
async def main():
    r1, r2 = await asyncio.gather(
        fetch_async("url1"),
        fetch_async("url2")
    )
```

### Coroutines and async def

```python
# A coroutine is defined with 'async def'
async def greet(name: str) -> str:
    return f"Hello, {name}!"

# Calling an async function returns a coroutine object, not the result
coro = greet("Alice")   # coroutine object
# To execute, you must await it or run it in an event loop
```

### asyncio.run and event loop

```python
import asyncio

async def main():
    result = await greet("Alice")
    print(result)

# Python 3.7+ — run a coroutine from synchronous code
asyncio.run(main())

# Or inside Jupyter/IPython where a loop is already running:
# await main()
```

### await and Chaining

```python
import asyncio

async def step1():
    await asyncio.sleep(1)
    return "step1 done"

async def step2(input_data: str):
    await asyncio.sleep(1)
    return f"step2 processed: {input_data}"

async def pipeline():
    r1 = await step1()
    r2 = await step2(r1)
    print(r2)

asyncio.run(pipeline())
```

### asyncio.gather — Parallel Coroutines

```python
import asyncio

async def task(name: str, delay: float) -> str:
    await asyncio.sleep(delay)
    return f"{name} done"

async def main():
    # Run concurrently — total time = max(1, 2, 3) = 3s, not 6s
    results = await asyncio.gather(
        task("A", 1),
        task("B", 2),
        task("C", 3),
    )
    print(results)   # ['A done', 'B done', 'C done']

asyncio.run(main())
```

### Async Generators and Comprehensions

```python
async def async_range(n):
    for i in range(n):
        await asyncio.sleep(0.1)
        yield i

async def main():
    async for value in async_range(5):
        print(value)

    # Async comprehension
    values = [v async for v in async_range(5)]
```

### asyncio Tasks

```python
import asyncio

async def background_task():
    await asyncio.sleep(3)
    print("Background done")

async def main():
    # Create task — runs concurrently without awaiting immediately
    task = asyncio.create_task(background_task())

    # Do other work while task runs in background
    await asyncio.sleep(1)
    print("Main work done")

    # Await task completion
    await task

asyncio.run(main())
```

### Async Context Managers

```python
import asyncio

class AsyncDB:
    async def __aenter__(self):
        print("Connecting...")
        await asyncio.sleep(0.1)
        return self

    async def __aexit__(self, *args):
        print("Disconnecting...")

    async def query(self, sql):
        await asyncio.sleep(0.1)
        return []

async def main():
    async with AsyncDB() as db:
        rows = await db.query("SELECT 1")
```

---

## Functional Programming

### map, filter, zip, reduce

```python
numbers = [1, 2, 3, 4, 5]

# map — apply function to each element
doubled = list(map(lambda x: x * 2, numbers))
# [2, 4, 6, 8, 10]
# Modern equivalent: [x * 2 for x in numbers]

# filter — keep elements where function returns True
evens = list(filter(lambda x: x % 2 == 0, numbers))
# [2, 4]
# Modern equivalent: [x for x in numbers if x % 2 == 0]

# zip — combine iterables
names = ["Alice", "Bob"]
scores = [95, 87]
paired = list(zip(names, scores))
# [("Alice", 95), ("Bob", 87)]

# reduce — fold/accumulate (not built-in — use functools)
from functools import reduce
total = reduce(lambda acc, x: acc + x, numbers, 0)
# 15
```

### functools Module

```python
from functools import (
    lru_cache,     # memoization decorator
    partial,       # partial application
    reduce,        # fold
    wraps,         # preserve decorator metadata
    cache,         # unbounded cache (Python 3.9+)
    total_ordering # complete comparison methods from __eq__ and one of __lt__/__gt__
)

# lru_cache — memoize function results (like a built-in memoize)
@lru_cache(maxsize=128)
def expensive(n):
    return sum(range(n))

expensive(1000)   # computed
expensive(1000)   # from cache

expensive.cache_info()   # CacheInfo(hits=1, misses=1, ...)
expensive.cache_clear()  # clear cache

# partial — fix some arguments
def power(base, exp):
    return base ** exp

square = partial(power, exp=2)
cube = partial(power, exp=3)
square(5)   # 25
cube(3)     # 27
```

### itertools Module

```python
import itertools

# chain — concatenate iterables
list(itertools.chain([1, 2], [3, 4], [5]))
# [1, 2, 3, 4, 5]

# islice — lazy slicing
list(itertools.islice(range(100), 5, 15))  # [5..14]

# product — cartesian product
list(itertools.product("AB", repeat=2))
# [('A','A'),('A','B'),('B','A'),('B','B')]

# combinations and permutations
list(itertools.combinations([1, 2, 3], 2))
# [(1,2),(1,3),(2,3)]

list(itertools.permutations([1, 2, 3], 2))
# [(1,2),(1,3),(2,1),(2,3),(3,1),(3,2)]

# groupby — group consecutive elements
data = sorted([{"type": "a", "v": 1}, {"type": "b", "v": 2}, {"type": "a", "v": 3}], key=lambda x: x["type"])
for key, group in itertools.groupby(data, key=lambda x: x["type"]):
    print(key, list(group))

# accumulate — running totals
list(itertools.accumulate([1, 2, 3, 4, 5]))
# [1, 3, 6, 10, 15]
```

### Partial Functions

```python
from functools import partial

def log(level, message):
    print(f"[{level}] {message}")

info = partial(log, "INFO")
error = partial(log, "ERROR")

info("Server started")    # [INFO] Server started
error("Connection failed")  # [ERROR] Connection failed
```

---

## Standard Library Highlights

### os and sys

```python
import os
import sys

# os — operating system interface
os.getcwd()                # current directory
os.listdir(".")            # list directory contents
os.makedirs("a/b/c", exist_ok=True)  # mkdir -p
os.path.join("a", "b")    # platform-safe path join
os.path.exists("file.txt")
os.path.abspath("file.txt")
os.environ.get("HOME")     # environment variable
os.getenv("API_KEY", "default")  # with default
os.rename("old.txt", "new.txt")
os.remove("file.txt")

# sys — Python runtime info
sys.argv                   # command-line arguments
sys.version                # Python version string
sys.path                   # module search paths
sys.exit(0)                # exit with code
sys.stdin / sys.stdout / sys.stderr
```

### datetime

```python
from datetime import datetime, date, time, timedelta

now = datetime.now()                    # local datetime
utc = datetime.utcnow()                # UTC (deprecated in 3.12: use datetime.now(UTC))
today = date.today()

# Formatting
now.strftime("%Y-%m-%d %H:%M:%S")      # "2024-01-15 14:30:00"
datetime.strptime("2024-01-15", "%Y-%m-%d")  # parse string to datetime

# Arithmetic
tomorrow = today + timedelta(days=1)
delta = datetime(2025, 1, 1) - datetime.now()
delta.days        # days remaining
delta.total_seconds()

# ISO format
now.isoformat()   # "2024-01-15T14:30:00.123456"
datetime.fromisoformat("2024-01-15T14:30:00")
```

### re — Regular Expressions

```python
import re

text = "Hello, my email is alice@example.com"

# Search — find first match anywhere
m = re.search(r"\b[\w.]+@[\w.]+\.\w+\b", text)
if m:
    print(m.group())    # "alice@example.com"
    print(m.start(), m.end())

# Findall — all matches
re.findall(r"\d+", "I have 3 cats and 2 dogs")   # ["3", "2"]

# Match — match at beginning of string
re.match(r"\d+", "123 abc")    # Match object
re.match(r"\d+", "abc 123")    # None

# Sub — replace
re.sub(r"\d+", "NUM", "I have 3 cats")   # "I have NUM cats"

# Compile for reuse
pattern = re.compile(r"(\w+)@(\w+)\.(\w+)")
m = pattern.search(text)
m.groups()    # ("alice", "example", "com")

# Flags
re.search(r"hello", text, re.IGNORECASE)
```

### json

```python
import json

# Serialize (Python → JSON string)
data = {"name": "Alice", "scores": [95, 87, 92]}
json_str = json.dumps(data)                   # compact
json_str = json.dumps(data, indent=2)         # pretty
json_str = json.dumps(data, sort_keys=True)   # sorted keys

# Deserialize (JSON string → Python)
obj = json.loads(json_str)

# Custom serialization
class UserEncoder(json.JSONEncoder):
    def default(self, obj):
        if hasattr(obj, "__dict__"):
            return obj.__dict__
        return super().default(obj)

json.dumps(my_object, cls=UserEncoder)
```

### collections

```python
from collections import (
    Counter,       # element frequency counter
    defaultdict,   # dict with default value factory
    OrderedDict,   # ordered dict (mostly redundant in Python 3.7+)
    deque,         # double-ended queue
    namedtuple,    # tuple with named fields
    ChainMap,      # multiple dicts as one view
)

# Counter
c = Counter("abracadabra")
c.most_common(3)   # [('a', 5), ('b', 2), ('r', 2)]
c["a"]             # 5

# defaultdict
graph = defaultdict(list)
graph["A"].append("B")   # no KeyError

# ChainMap — lookup across multiple dicts
defaults = {"color": "blue", "size": "medium"}
overrides = {"color": "red"}
config = ChainMap(overrides, defaults)
config["color"]   # "red"
config["size"]    # "medium"
```

### enum

```python
from enum import Enum, auto, IntEnum, Flag

class Direction(Enum):
    NORTH = "N"
    SOUTH = "S"
    EAST = "E"
    WEST = "W"

d = Direction.NORTH
d.name    # "NORTH"
d.value   # "N"
Direction("N")        # Direction.NORTH
Direction["NORTH"]    # Direction.NORTH

# auto() — auto-assign values
class Color(Enum):
    RED = auto()   # 1
    GREEN = auto() # 2
    BLUE = auto()  # 3

# IntEnum — int-compatible
class Status(IntEnum):
    OK = 200
    NOT_FOUND = 404

# Flags — bitmask enum
class Permission(Flag):
    READ = auto()
    WRITE = auto()
    EXECUTE = auto()
    ALL = READ | WRITE | EXECUTE

p = Permission.READ | Permission.WRITE
Permission.READ in p   # True
```

### typing

```python
from typing import (
    Any, Union, Optional, Literal,
    List, Dict, Tuple, Set,          # deprecated in 3.9+
    Callable, Iterator, Generator,
    TypeVar, Generic, Protocol,
    overload, cast, TYPE_CHECKING,
    ClassVar, Final,
    TypedDict, NamedTuple,
)

# overload — multiple call signatures
from typing import overload

@overload
def process(x: int) -> int: ...
@overload
def process(x: str) -> str: ...

def process(x):
    if isinstance(x, int):
        return x * 2
    return x.upper()
```

### copy

```python
import copy

original = [[1, 2], [3, 4]]

# Shallow copy — new outer container, shared inner objects
shallow = copy.copy(original)
shallow[0].append(99)
original[0]   # [1, 2, 99] — shared!

# Deep copy — fully independent recursive copy
deep = copy.deepcopy(original)
deep[0].append(99)
original[0]   # [1, 2] — unaffected
```

### random

```python
import random

random.random()           # float in [0.0, 1.0)
random.randint(1, 10)     # int in [1, 10]
random.choice([1,2,3])    # random element
random.choices([1,2,3], weights=[1,2,3], k=5)  # weighted, with replacement
random.sample([1,2,3,4,5], k=3)  # without replacement
random.shuffle([1,2,3])   # in-place shuffle
random.seed(42)           # reproducible results
```

### math

```python
import math

math.pi          # 3.14159...
math.e           # 2.71828...
math.inf         # infinity
math.nan         # NaN

math.sqrt(16)    # 4.0
math.ceil(4.2)   # 5
math.floor(4.9)  # 4
math.abs(-5)     # use built-in abs(-5) instead
math.log(100, 10)  # 2.0 — log base 10
math.log2(8)     # 3.0
math.log(math.e)   # 1.0 — natural log
math.factorial(5)  # 120
math.gcd(12, 8)    # 4
math.isnan(math.nan)   # True
math.isinf(math.inf)   # True
```

---

## Concurrency and Parallelism

### The GIL (Global Interpreter Lock)

The **GIL** is a mutex in CPython that allows only **one thread to execute Python bytecode at a time**.

| Implication | Detail |
|---|---|
| CPU-bound threads | Won't run in parallel — GIL prevents it |
| I/O-bound threads | **Do** run concurrently — GIL is released during I/O waits |
| `asyncio` | Single-threaded cooperative concurrency — not affected by GIL |
| `multiprocessing` | Spawns separate processes — each has its own GIL |

> Python 3.13 introduces **optional GIL disabling** (PEP 703), but this is experimental.

### threading

```python
import threading
import time

def worker(name, delay):
    time.sleep(delay)
    print(f"{name} done")

# I/O-bound tasks benefit from threading
t1 = threading.Thread(target=worker, args=("Task1", 1))
t2 = threading.Thread(target=worker, args=("Task2", 2))

t1.start()
t2.start()

t1.join()   # wait for t1 to finish
t2.join()

# Thread-safe data with Lock
lock = threading.Lock()
counter = 0

def increment():
    global counter
    with lock:
        counter += 1

threads = [threading.Thread(target=increment) for _ in range(100)]
for t in threads: t.start()
for t in threads: t.join()
print(counter)   # 100
```

### multiprocessing

```python
from multiprocessing import Pool, Process
import os

def cpu_bound_task(n):
    return sum(i * i for i in range(n))

# Pool — parallel map across processes
with Pool(processes=4) as pool:
    results = pool.map(cpu_bound_task, [10**6, 10**6, 10**6, 10**6])

print(results)

# Single process
def worker(name):
    print(f"{name} running in PID {os.getpid()}")

p = Process(target=worker, args=("Worker",))
p.start()
p.join()
```

### concurrent.futures

High-level interface for both threads and processes:

```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

urls = ["http://example.com", "http://python.org"]

# Thread pool (I/O-bound — e.g., HTTP requests)
with ThreadPoolExecutor(max_workers=5) as executor:
    futures = [executor.submit(fetch, url) for url in urls]
    results = [f.result() for f in futures]

# Process pool (CPU-bound)
with ProcessPoolExecutor() as executor:
    results = list(executor.map(cpu_bound_task, [10**6] * 4))

# as_completed — process results as they arrive
from concurrent.futures import as_completed

with ThreadPoolExecutor(max_workers=5) as executor:
    futures = {executor.submit(fetch, url): url for url in urls}
    for future in as_completed(futures):
        url = futures[future]
        try:
            data = future.result()
        except Exception as e:
            print(f"{url} failed: {e}")
```

### asyncio vs threading vs multiprocessing

| Scenario | Best Choice |
|---|---|
| Many I/O-bound tasks (HTTP, DB, files) | `asyncio` |
| I/O-bound with synchronous libraries | `threading` |
| CPU-bound computation | `multiprocessing` |
| Mixed (CPU + I/O) | `asyncio` + `ProcessPoolExecutor` |

---

## Advanced Python

### Descriptors

A descriptor is any object that defines `__get__`, `__set__`, or `__delete__`. Properties are built on descriptors:

```python
class Validator:
    def __set_name__(self, owner, name):
        self.name = name
        self.private = f"_{name}"

    def __get__(self, obj, objtype=None):
        if obj is None:
            return self
        return getattr(obj, self.private, None)

    def __set__(self, obj, value):
        if not isinstance(value, int):
            raise TypeError(f"{self.name} must be int")
        if value < 0:
            raise ValueError(f"{self.name} must be >= 0")
        setattr(obj, self.private, value)

class Product:
    price = Validator()
    quantity = Validator()

    def __init__(self, price, quantity):
        self.price = price
        self.quantity = quantity

p = Product(10, 5)
p.price = -1   # ValueError
```

### Metaclasses

A metaclass is the **class of a class**. `type` is the default metaclass. Metaclasses let you customise class creation:

```python
# type(name, bases, dict) — dynamic class creation
MyClass = type("MyClass", (object,), {"x": 1, "greet": lambda self: "hi"})

# Custom metaclass — runs when the class is defined
class SingletonMeta(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    def __init__(self, url):
        self.url = url

db1 = Database("postgres://...")
db2 = Database("postgres://...")
db1 is db2   # True
```

### Memory Management and Garbage Collection

```python
import gc
import sys

# Reference counting — primary mechanism
x = [1, 2, 3]
sys.getrefcount(x)    # count of references + 1 (for getrefcount itself)

# Cyclic garbage collector — handles circular references
gc.collect()           # force collection
gc.get_count()         # (gen0, gen1, gen2) counts

# Memory size
sys.getsizeof([1, 2, 3])   # bytes used by object

# del removes the name binding (may not free immediately)
x = [1, 2, 3]
del x    # x is unbound; list freed if refcount drops to 0
```

### WeakRef

Weak references don't prevent garbage collection:

```python
import weakref

class Cache:
    pass

cache = Cache()
weak = weakref.ref(cache)

weak()    # Cache object
del cache
weak()    # None — object was collected

# WeakValueDictionary — values auto-removed when collected
wd = weakref.WeakValueDictionary()
```

### Python Data Model Summary

Python's data model defines how objects interact with language constructs. Key areas:

| Area | Mechanism |
|---|---|
| Object identity | `id(obj)`, `is` operator |
| Object truth | `__bool__`, `__len__` |
| Comparison | `__eq__`, `__lt__`, `__le__`, `__gt__`, `__ge__` |
| Hashing | `__hash__` (required for dict keys / set members) |
| String representation | `__repr__`, `__str__`, `__format__` |
| Container protocol | `__len__`, `__getitem__`, `__setitem__`, `__contains__`, `__iter__` |
| Numeric protocol | `__add__`, `__mul__`, `__sub__`, `__truediv__`, `__floordiv__`, etc. |
| Callable | `__call__` |
| Context manager | `__enter__`, `__exit__` |
| Attribute access | `__getattr__`, `__setattr__`, `__getattribute__` |
| Class creation | `__init_subclass__`, `__class_getitem__` |

---

## Copying Objects

### Shallow Copy

Creates a new object but doesn't recursively copy nested objects:

```python
import copy

original = [1, [2, 3], {"a": 4}]

# Various shallow copy methods
shallow1 = original.copy()        # list method
shallow2 = original[:]            # slice
shallow3 = list(original)         # constructor
shallow4 = copy.copy(original)    # explicit

# Mutation of nested objects is shared
shallow1[1].append(99)
original[1]   # [2, 3, 99] — affected!
```

### Deep Copy

Recursively copies all nested objects:

```python
import copy

original = [1, [2, 3], {"a": 4}]
deep = copy.deepcopy(original)

deep[1].append(99)
original[1]   # [2, 3] — unaffected

# Custom deepcopy
class MyObj:
    def __deepcopy__(self, memo):
        new = MyObj()
        memo[id(self)] = new
        # custom deep copy logic
        return new
```

---

## Python vs JavaScript — Feature Map

Quick reference for developers who know JavaScript and are learning Python:

| JS / TS Concept | Python Equivalent |
|---|---|
| `let x = 5` | `x = 5` |
| `const X = 5` | `X = 5` (convention) or `Final[int]` |
| `undefined` / `null` | `None` |
| `typeof x` | `type(x)` |
| `x instanceof Foo` | `isinstance(x, Foo)` |
| Template literals `` `Hi ${name}` `` | f-strings `f"Hi {name}"` |
| `===` (strict equal) | `==` (Python `==` is strict for primitives) |
| `&&` / `\|\|` / `!` | `and` / `or` / `not` |
| `??` (nullish coalescing) | `x if x is not None else default` or `x or default` |
| `?.` (optional chaining) | `getattr(obj, "attr", None)` or `obj and obj.attr` |
| `...spread` | `*iterable` / `**dict` |
| `[...rest]` | `*rest` in unpacking |
| Arrow function `x => x*2` | Lambda `lambda x: x*2` or `def` |
| `Array` | `list` |
| `{}` object / `Map` | `dict` |
| `Set` | `set` |
| `Array.from()` | `list()` |
| `arr.map(fn)` | `[fn(x) for x in arr]` or `map(fn, arr)` |
| `arr.filter(fn)` | `[x for x in arr if fn(x)]` or `filter(fn, arr)` |
| `arr.reduce(fn, init)` | `functools.reduce(fn, arr, init)` |
| `arr.forEach(fn)` | `for x in arr: fn(x)` |
| `arr.find(fn)` | `next((x for x in arr if fn(x)), None)` |
| `arr.some(fn)` | `any(fn(x) for x in arr)` |
| `arr.every(fn)` | `all(fn(x) for x in arr)` |
| `arr.includes(x)` | `x in arr` |
| `arr.flat()` | `[x for sub in arr for x in sub]` |
| `Object.keys()` | `dict.keys()` |
| `Object.values()` | `dict.values()` |
| `Object.entries()` | `dict.items()` |
| `{...a, ...b}` | `{**a, **b}` |
| `JSON.stringify` | `json.dumps()` |
| `JSON.parse` | `json.loads()` |
| `Promise` | `asyncio.Future` / coroutine |
| `async/await` | `async def` / `await` |
| `Promise.all()` | `asyncio.gather()` |
| `try/catch/finally` | `try/except/finally` |
| `throw new Error()` | `raise ValueError()` |
| `class` / `extends` | `class` / `class Child(Parent)` |
| `constructor` | `__init__` |
| `super()` | `super()` |
| `static` | `@staticmethod` |
| getter / setter | `@property` / `@x.setter` |
| TypeScript interface | `Protocol` (structural) or `ABC` (nominal) |
| TypeScript generics | `TypeVar` + `Generic` |
| TypeScript `type X = ...` | `TypeAlias` / `type X = ...` (3.12+) |
| TypeScript enum | `Enum` from `enum` module |
| TypeScript decorators | Python decorators `@decorator` |
| `import`/`export` | `import`/`from x import y` |
| `require()` | `import` |
| `module.exports` | module-level names (all public by default) |
| `npm install` | `pip install` |
| `package.json` | `requirements.txt` / `pyproject.toml` |
| `node_modules` | virtual environment (`.venv`) |
| `for...of` | `for x in iterable` |
| `for...in` | `for key in dict` |
| Generator function `function*` | `def` with `yield` |
| `Symbol` | (no direct equivalent; use `object()` for unique sentinels) |
| WeakMap / WeakSet | `weakref.WeakValueDictionary` / `weakref.WeakSet` |
| Proxy | (no direct equivalent; use `__getattr__` / descriptors) |
