# String Formatting in Python: A Practical Guide

Python provides multiple ways to format strings, numbers, and objects for clear, concise, and flexible output. This guide systematically covers all main string formatting techniques, focusing on practical examples and their use cases.

---

## 1. String Formatting Methods Overview

### 1.1. Old-Style `%` Formatting

```python
name = "Alice"
age = 30
print("Name: %s, Age: %d" % (name, age))
# Output: Name: Alice, Age: 30
```

### 1.2. `str.format()` Method

```python
print("Name: {}, Age: {}".format(name, age))
print("Name: {0}, Age: {1}".format(name, age))
print("Name: {n}, Age: {a}".format(n=name, a=age))
```

### 1.3. f-Strings (Python 3.6+)

```python
print(f"Name: {name}, Age: {age}")
```

---

## 2. Format Specifiers for Strings

### 2.1. Width and Alignment

```python
text = "Hi"
print("{:10}".format(text))      # 'Hi        '  (right-padded)
print("{:<10}".format(text))     # 'Hi        '
print("{:>10}".format(text))     # '        Hi'
print("{:^10}".format(text))     # '    Hi    '
```

### 2.2. Truncating Long Strings

```python
long_text = "HelloWorld"
print("{:.5}".format(long_text))   # 'Hello'
```

---

## 3. Formatting Numbers

### 3.1. Integers

```python
num = 42
print("{:d}".format(num))      # '42'
print("{:5d}".format(num))     # '   42' (width 5, right-aligned)
print("{:05d}".format(num))    # '00042' (zero-padded)
print("{:+d}".format(num))     # '+42' (explicit sign)
print("{:<5d}".format(num))    # '42   ' (left-aligned)
print("{:^5d}".format(num))    # ' 42  ' (center-aligned)
print("{:b}".format(num))      # '101010' (binary)
print("{:x}".format(num))      # '2a' (hex)
print("{:#x}".format(num))     # '0x2a' (hex with prefix)
```

### 3.2. Floating-Point Numbers

**Fixed-point (`f`):**

```python
x = 123.456789
print("{:.2f}".format(x))      # '123.46' (2 decimals, rounded)
print("{:10.2f}".format(x))    # '    123.46' (width 10, 2 decimals)
print("{:+.1f}".format(x))     # '+123.5' (1 decimal, with sign)
```

**Scientific Notation (`e`):**

```python
print("{:.2e}".format(x))      # '1.23e+02'
print("{:10.1e}".format(x))    # '  1.2e+02'
```

---

## 4. The `'g'` Format Specifier (General Format for Floats)

### 4.1. Behavior

- Chooses fixed-point or scientific notation automatically
- Controls total number of **significant digits**
- Removes trailing zeros (unless `#` used)
- Default precision: 6 significant digits

### 4.2. Examples

```python
x = 12345.6789
print("{:.3g}".format(x))         # '1.23e+04' (scientific, 3 sig. digits)
print("{:.6g}".format(1.230000))  # '1.23'
print("{:#.6g}".format(1.230000)) # '1.23000' (force trailing zeros)
print("{:g}".format(123456789.0)) # '1.23457e+08' (default 6 sig. digits)
print("{:.3g}".format(0.00012345)) # '0.000123' (fixed-point)
print("{:.3g}".format(0.000012345)) # '1.23e-05' (scientific)
print("{:g}".format(float('inf')))  # 'inf'
print("{:G}".format(float('inf')))  # 'INF'
```

**Notation Switch Rule:**
- Uses fixed-point if exponent is between `-4` and `precision`
- Otherwise uses scientific notation

---

## 5. Formatting Other Objects

### 5.1. Using `str()` and `repr()`

```python
class Point:
    def __init__(self, x, y):
        self.x, self.y = x, y
    def __str__(self):
        return f"Point({self.x}, {self.y})"
    def __repr__(self):
        return f"Point<{self.x}, {self.y}>"

p = Point(3, 4)
print(str(p))   # 'Point(3, 4)'
print(repr(p))  # 'Point<3, 4>'
```

### 5.2. Format with `__format__`

```python
class Money:
    def __init__(self, amount):
        self.amount = amount
    def __format__(self, spec):
        return f"${self.amount:,.2f}"

print("{:}".format(Money(1234.5)))   # '$1,234.50'
```

---

## 6. Comparison Table of Main Numeric Format Specifiers

| Specifier | Use-case         | Example              | Output         |
|-----------|------------------|----------------------|---------------|
| `d`       | Integer          | `"{:d}".format(42)`  | `'42'`        |
| `b`       | Binary           | `"{:b}".format(10)`  | `'1010'`      |
| `x`       | Hex lowercase    | `"{:x}".format(15)`  | `'f'`         |
| `f`       | Fixed-point      | `"{:.2f}".format(1.5)`| `'1.50'`     |
| `e`       | Scientific       | `"{:.1e}".format(10)` | `'1.0e+01'`  |
| `g`       | General float    | `"{:.2g}".format(0.01234)` | `'0.012'` |
| `%`       | Percentage       | `"{:.1%}".format(0.5)`| `'50.0%'`    |

---

## 7. Quick Reference: Syntax for `str.format()` and f-strings

```python
# General
"{field_name:format_spec}".format(value)
f"{value:format_spec}"

# Examples
"{:>10}".format('abc')      # right-align, width 10
f"{3.14159:.2f}"            # 2 decimals
f"{n:04d}"                  # zero-pad integer (width 4)
```

---

## 8. Further Reading

- [Format Specification Mini-Language](https://docs.python.org/3/library/string.html#format-specification-mini-language)
- [PEP 498: f-Strings](https://peps.python.org/pep-0498/)

---
