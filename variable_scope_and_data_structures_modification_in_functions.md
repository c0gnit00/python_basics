## What is a "Symbol Table"?

A **symbol table** is just a mapping of variable names to their values. Python maintains different symbol tables for:

1. Local scope
2. Enclosing functions
3. Global scope
4. Built-in names

---

## Scope Resolution Order (LEGB Rule)

When you reference a variable, Python searches for its value in this order:

| Level            | Description                                                       |
| ---------------- | ----------------------------------------------------------------- |
| L: Local         | Inside the current function                                       |
| E: Enclosing     | Inside any enclosing (outer) functions, if it's a nested function |
| G: Global        | At the top level of the file or module                            |
| B: Built-in      | Python’s built-in names like `len`, `print`, `range`, etc.        |

---

## Example 1: Local vs Global Variable Reference

```python
x = 10  # global variable

def my_func():
    print(x)  # refers to global x

my_func()
```

This works fine because we are **only referencing** the variable `x`, not assigning to it.

---

## Example 2: Assigning a Value to a Global Variable Inside a Function

```python
x = 10

def my_func():
    x = x + 5  # Error: UnboundLocalError
    print(x)

my_func()
```

**Why error?** Because Python sees `x = x + 5` and assumes that **`x` is a local variable**. But then it tries to evaluate `x` before it's defined locally.

---

## Fix with `global` keyword

```python
x = 10

def my_func():
    global x
    x = x + 5  # Now it works
    print(x)

my_func()  # Output: 15
```

`global x` tells Python:

> "Don’t create a new local variable `x`. Use the global one."

---

## Example 3: `nonlocal` in Nested Functions

```python
def outer():
    x = 5
    def inner():
        nonlocal x
        x += 1
        print(x)
    inner()
    print(x)

outer()
```

* `x` is **not global**; it lives in the `outer()` function.
* `inner()` is a **nested function**, and it wants to **modify `x` in `outer()`**, not create a new local one.
* So we use `nonlocal x` to access the `x` from the enclosing scope.

---

## Summary

* **Variable assignments inside a function create local variables**, unless you explicitly say otherwise.
* **Variable references (e.g., just using the variable)** follow the **LEGB rule**: Local → Enclosing → Global → Built-in.
* Use:

  * `global var_name` — to modify global variables inside a function
  * `nonlocal var_name` — to modify variables in an enclosing function

---

## Are values passed by reference or by value in Python?

Python uses a model called:

> **“Pass-by-object-reference”** (also known as **“Call-by-sharing”**)

This means:

* **Everything in Python is an object**, including integers, strings, lists, etc.
* **Variables are references to objects**.
* When you pass a variable to a function, you're passing the **reference to the object**, **not the actual object itself**, **and not a copy of the reference either.**

But here's the **catch**:

> Whether a function can **modify** the passed-in object **depends on the object's type** (mutable or immutable).

---

### Immutable types (like `int`, `float`, `str`, `tuple`)

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

### Mutable types (like `list`, `dict`, `set`, custom objects)

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

## Important Clarification

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

## Final Rule

> You can **modify** mutable objects inside a function, but if you **reassign** the parameter inside the function, it won’t affect the original variable.
