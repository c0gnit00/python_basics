
### Function Definition Syntax

```python
def fib(n):    # write Fibonacci series less than n
```

* `def`: Used to define a function.
* `fib`: Name of the function.
* `(n)`: Parameter list. The function accepts one argument `n`.

---

### Docstrings (Documentation Strings)

```python
"""Print a Fibonacci series less than n."""
```

This is a **docstring**, a string literal placed as the first statement in a function to describe its purpose. Tools like `help()` or IDEs use it for documentation.

---

### Function Logic – Fibonacci Series

```python
a, b = 0, 1
while a < n:
    print(a, end=' ')
    a, b = b, a + b
```

* Starts with two values `a=0` and `b=1`.
* In each iteration:
  * Print `a`.
  * Then reassign: `a` gets the old value of `b`, and `b` becomes the sum of old `a + b`.
* This generates Fibonacci numbers less than `n`.

Example:

```python
fib(2000)
# Output: 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597
```

---

### Scope and Symbol Tables

> The execution of a function introduces a new **symbol table** for its local variables.

This means:

* Every time you call a function, a **new local scope** is created.
* Assignments (like `a = 0`) happen in the **local symbol table**.
* Python first looks for variable names in the **local**, then **enclosing**, then **global**, and finally **built-in** scopes.

---

### Argument Passing — Call by Object Reference

#### Are values passed by reference or by value in Python?

Python uses a model called:

> **“Pass-by-object-reference”** (also known as **“Call-by-sharing”**)

This means:

* **Everything in Python is an object**, including integers, strings, lists, etc.
* **Variables are references to objects**.
* When you pass a variable to a function, you're passing the **reference to the object**, **not the actual object itself**, **and not a copy of the reference either.**

But here's the **catch**:

> Whether a function can **modify** the passed-in object **depends on the object's type** (mutable or immutable).

---

#### Immutable types (like `int`, `float`, `str`, `tuple`)

You **can’t** change the value inside the function.
It behaves **as if passed by value**.

```python
def modify(x):
    x = x + 5
    print("Inside:", x)

a = 10
modify(a)
print("Outside:", a)
```

**Output:**

```
Inside: 15
Outside: 10
```

> `x` was pointing to a new object `15`, but that **doesn't change** `a` outside the function.

---

#### Mutable types (like `list`, `dict`, `set`, custom objects)

You **can** change the contents inside the function.
It behaves **as if passed by reference**.

```python
def modify_list(lst):
    lst.append(4)
    print("Inside:", lst)

my_list = [1, 2, 3]
modify_list(my_list)
print("Outside:", my_list)
```

**Output:**

```
Inside: [1, 2, 3, 4]
Outside: [1, 2, 3, 4]
```

> Since lists are mutable, the change is visible outside.

---

#### Important Clarification

If you **rebind a parameter** inside a function (i.e., assign it to a new object), the original reference outside the function is not affected:

```python
def change_list(lst):
    lst = [100, 200, 300]  # This creates a new local list
    print("Inside:", lst)

my_list = [1, 2, 3]
change_list(my_list)
print("Outside:", my_list)
```

**Output:**

```
Inside: [100, 200, 300]
Outside: [1, 2, 3]
```

> The function creates a new list and `lst` refers to that new list — but it **does not affect** `my_list` outside.

---

#### Final Rule

> You can **modify** mutable objects inside a function, but if you **reassign** the parameter inside the function, it won’t affect the original variable.

> Arguments are passed **using call by value** — but the value is always an **object reference**, not the object itself.

This means:

* **Immutable types** (like `int`, `str`, `tuple`) behave as if passed **by value**.
* **Mutable types** (like `list`, `dict`) behave as if passed **by reference**.

Thus, if a function modifies a **mutable argument**, that change is reflected outside the function.

---

### Return Value: `None`

In Python, **every function returns a value**, even if you don’t explicitly write a `return` statement.

#### Concept

In some programming languages like C or Java, there's a clear difference between:

* **Procedure**: a function that performs actions but doesn’t return a value (e.g., `void` functions in C/Java).
* **Function**: a block of code that returns a result.

In **Python**, there is **no such distinction**. Every function is a function—even if it returns nothing meaningful.

#### Example

```python
def fib(n):
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b

fib(0)             # Prints nothing
print(fib(0))      # Prints: None
```

##### What happens here?

* `fib(0)` doesn't return anything explicitly.
* So, **Python implicitly returns `None`**.
* When you call `print(fib(0))`, you actually print the return value — which is `None`.

---

#### Why this matters

* This consistent behavior simplifies Python’s model.
* You can rely on the fact that **every function call has a return value**, even if it’s just `None`.

---

### fib2(n): Returns a List Instead of Printing

```python
def fib2(n):
    result = []
    a, b = 0, 1
    while a < n:
        result.append(a)
        a, b = b, a + b
    return result
```

This version builds and **returns** the list instead of printing. For example:

```python
f100 = fib2(100)
print(f100)
# Output: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```
