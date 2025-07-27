
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
