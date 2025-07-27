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
