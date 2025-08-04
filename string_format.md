# Python Format String:

**General form:**
```
{ [field_name] [!conversion] [:format_spec] }
```
- All components are optional except the braces.
- Used in `str.format()`, f-strings, and `format()`.

---

## 1. `field_name` — Selecting What to Format

### Theory & Rules

- **Purpose:** Specifies the data to insert.
- **Can be:**
  - Positional index (e.g. `{0}`, `{1}`)
  - Keyword (e.g. `{name}`)
  - Attribute lookup (e.g. `{0.x}`, `{user.name}`)
  - Index lookup (e.g. `{0[2]}`, `{d[key]}`)
  - Chained combinations (e.g. `{0[users][2].name}`)

### 1.1. Positional Arguments

```python
print("{0} {1}".format("foo", "bar"))      # 'foo bar'
print("{1} {0}".format("foo", "bar"))      # 'bar foo'
```

### 1.2. Automatic Numbering

```python
print("{} {} {}".format("a", "b", "c"))    # 'a b c'
# Note: Cannot mix manual and automatic numbering in the same string
```

### 1.3. Keyword Arguments

```python
print("{first} {second}".format(first="a", second="b"))  # 'a b'
```

### 1.4. Attribute Lookup

```python
class Point:
    def __init__(self, x, y):
        self.x, self.y = x, y
p = Point(10, 20)
print("{0.x}, {0.y}".format(p))             # '10, 20'
```

### 1.5. Index Lookup

```python
lst = ['a', 'b', 'c']
print("{0[2]}".format(lst))                 # 'c'

d = {'x': 10, 'y': 99}
print("{0[x]}".format(d))                   # '10'
```

### 1.6. Chained Lookups

```python
data = {'users': [{'name': 'Alice'}, {'name': 'Bob'}]}
print("{0[users][1][name]}".format(data))   # 'Bob'
```

### 1.7. f-String Expressions

f-strings allow any valid Python expression:
```python
user = {'score': 42}
print(f"{user['score'] + 1}")               # '43'
```

### 1.8. Limitations & Notes

- **No quoting for keys:** `{d['key']}` is invalid, use a variable or simple key.
- **Mixing types:** Can chain attributes and indexes, e.g. `{0.attr[index]}`.
- **Custom objects:** Attributes must exist; will raise AttributeError/KeyError otherwise.

---

## 2. `!conversion` — Forcing Value Conversion Before Formatting

### Theory & Rules

- **Purpose:** Force a specific string representation before `format_spec` is applied.
- **Applies to:** All types (string, int, float, object, etc.)

### 2.1. Supported Conversions

| Conversion | Method called     | Result                              |
|------------|------------------|-------------------------------------|
| `!s`       | `str(value)`     | Human-readable string               |
| `!r`       | `repr(value)`    | Programmer/debug view (may have '') |
| `!a`       | `ascii(value)`   | Like `repr()`, escapes non-ASCII    |

### 2.2. Examples with Built-ins

```python
val = 'café'
print("{!s}".format(val))   # 'café'
print("{!r}".format(val))   # "'café'"
print("{!a}".format(val))   # "'caf\\xe9'"
```

### 2.3. Examples with Custom Types

```python
class Obj:
    def __str__(self):  return "as_str"
    def __repr__(self): return "as_repr"
o = Obj()
print("{!s}".format(o))    # 'as_str'
print("{!r}".format(o))    # 'as_repr'
```

### 2.4. f-Strings and Conversion

```python
num = 12.3
print(f"{num!r}")          # '12.3'
print(f"{num!s}")          # '12.3'
print(f"{num!a}")          # '12.3'
```

### 2.5. Conversion is Before Formatting

```python
x = 3.14159
print("{!r:.2f}".format(x))  # ERROR: repr(x) returns a str, can't format as float
# Conversion happens first!
```

---

## 3. `:format_spec` — Mini-Language for Formatting

### Theory & Rules

- **Purpose:** Controls width, alignment, padding, precision, type, etc.
- **Applies to:** Strings, numbers, custom objects with `__format__`.

### 3.1. Syntax

```
[[fill]align][sign][#][0][width][grouping][.precision][type]
```

#### - **fill**: Any character for padding (default space).
#### - **align**:  
  - `<` : Left (default for strings)
  - `>` : Right (default for numbers)
  - `^` : Center
  - `=` : Sign comes first, padding after (numbers)

#### - **sign**:  
  - `+` : Always show (+/-)
  - `-` : Only negative (default)
  - ` ` : Space for positive, - for negative

#### - **#**: Alternate form (prefixes for bin/oct/hex, forces decimal point for floats)

#### - **0**: Pad with zeros (numbers, sign-aware)

#### - **width**: Minimum width (integer)

#### - **grouping**:  
  - `,` : thousands separator
  - `_` : underscores

#### - **.precision**:  
  - Strings: max chars
  - Floats: digits after decimal or significant digits

#### - **type**: Presentation type (see below)

----

### 3.2. Detailed Examples for Each Specifier

#### 3.2.1. fill & align

```python
print("{:<10}".format("left"))      # 'left      '  (left aligned)
print("{:>10}".format("right"))     # '     right'  (right aligned)
print("{:^10}".format("center"))    # '  center  '  (center)
print("{:*^10}".format("wow"))      # '***wow****'  (center, fill with '*')
print("{:->12}".format("hello"))    # '-------hello' (right, fill with '-')
print("{:0=8d}".format(-42))        # '-0000042'   (sign, then padding with zeros)
```

#### 3.2.2. sign

```python
print("{:+d}".format(42))        # '+42' (always show sign)
print("{: d}".format(42))        # ' 42' (space for positive, - for negative)
print("{:-d}".format(-42))       # '-42' (only negative, default)
```

#### 3.2.3. #

```python
print("{:#b}".format(5))         # '0b101' (binary prefix)
print("{:#o}".format(8))         # '0o10' (octal prefix)
print("{:#x}".format(255))       # '0xff' (hex prefix)
print("{:#.0f}".format(3.0))     # '3.'   (force decimal point for float)
print("{:#.3g}".format(1.2))     # '1.20' (keep trailing zeros in general format)
```

#### 3.2.4. 0 (zero padding)

```python
print("{:05d}".format(42))       # '00042'
print("{:08.2f}".format(3.14))   # '00003.14'
print("{:0=6d}".format(-42))     # '-000042'
```

#### 3.2.5. width

```python
print("{:8}".format("cat"))      # 'cat     '
print("{:8d}".format(123))       # '     123'
print("{:<8}".format("cat"))     # 'cat     '
```

#### 3.2.6. grouping

```python
print("{:,}".format(1234567))   # '1,234,567'
print("{:_}".format(1234567))   # '1_234_567'
print("{:,.2f}".format(12345.678)) # '12,345.68'
```

#### 3.2.7. .precision

```python
print("{:.2f}".format(3.14159))   # '3.14' (float, 2 decimals)
print("{:.3}".format("abcdef"))   # 'abc' (string, 3 chars)
print("{:.4g}".format(12345.6789))# '1.235e+04' (float, 4 significant digits)
```

#### 3.2.8. type

- **Integers:** d (decimal), b (binary), o (octal), x/X (hex), n (locale), c (char)
- **Floats:** f/F (fixed), e/E (scientific), g/G (general), n (locale), % (percent)
- **Strings:** s (default)

| Example                | Output         | Description                       |
|------------------------|---------------|-----------------------------------|
| `"{:d}".format(255)`   | '255'         | Decimal integer                   |
| `"{:b}".format(10)`    | '1010'        | Binary                            |
| `"{:o}".format(8)`     | '10'          | Octal                             |
| `"{:x}".format(255)`   | 'ff'          | Hexadecimal (lower)               |
| `"{:X}".format(255)`   | 'FF'          | Hexadecimal (upper)               |
| `"{:c}".format(65)`    | 'A'           | Unicode character                 |
| `"{:.2f}".format(3.14)`| '3.14'        | 2 decimal float                   |
| `"{:.2e}".format(3.14)`| '3.14e+00'    | Scientific notation               |
| `"{:.2%}".format(0.123)`| '12.30%'     | Percentage                        |

---

### 3.3. Nested Format Specs

You can use variables for width or precision:

```python
w, p = 10, 3
num = 123.45678
print("{0:>{w}.{p}f}".format(num, w=w, p=p))  # '   123.457'
```

---

### 3.4. Custom Types

```python
class Money:
    def __init__(self, amt): self.amt = amt
    def __format__(self, spec):
        if spec == "$":
            return f"${self.amt:,.2f}"
        return format(self.amt, spec)
print("{:}".format(Money(1234.5)))   # '$1,234.50'
print("{:$}".format(Money(1234.5)))  # '$1,234.50'
```

---

## 4. Comprehensive Examples Using All Three: `{field_name!conversion:format_spec}`

### Example 1: Formatting a Complex Object with All Parts

```python
class User:
    def __init__(self, name, score): self.name, self.score = name, score
    def __repr__(self): return f"<User {self.name!r} ({self.score})>"
    def __str__(self):  return self.name

user = User("Alice", 9876.543)
# Show repr, right-align, width 30
print("{0!r:>30}".format(user))
# Output: '         <User \'Alice\' (9876.543)>'
```

### Example 2: Formatting a Dictionary Value as Debug String

```python
data = {'city': 'München', 'pop': 1471508}
# Show ascii (escapes non-ascii), pad with '*', width 20, center
print("{0[city]!a:*^20}".format(data))
# Output: '***\'M\\xfcnchen\'*****'
```

---


## Examples

### Strings

#### Width and Alignment

```python
text = "Hi"
print("{:10}".format(text))      # 'Hi        '  (right-padded)
print("{:<10}".format(text))     # 'Hi        '
print("{:>10}".format(text))     # '        Hi'
print("{:^10}".format(text))     # '    Hi    '
```

#### Truncating Long Strings

```python
long_text = "HelloWorld"
print("{:.5}".format(long_text))   # 'Hello'
```

---

### Integers

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

### Floating-Point Numbers

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

###  The `'g'` Format Specifier (General Format for Floats)

#### Behavior

- Chooses fixed-point or scientific notation automatically
- Controls total number of **significant digits**
- Removes trailing zeros (unless `#` used)
- Default precision: 6 significant digits

#### Examples

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

### Formatting Other Objects

#### Using `str()` and `repr()`

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

#### Format with `__format__`

```python
class Money:
    def __init__(self, amount):
        self.amount = amount
    def __format__(self, spec):
        return f"${self.amount:,.2f}"

print("{:}".format(Money(1234.5)))   # '$1,234.50'
```

---

### Comparison Table of Main Numeric Format Specifiers

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

### Syntax for `str.format()` and f-strings

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

## Troubleshooting & Gotchas

- **Order of operations:** conversion is applied first, then formatting.
- **Type mismatch:** after forced conversion, the format_spec must be valid for the new type (e.g. can't apply `.2f` to a string).
- **Mixing numbering:** Don't mix auto and manual field numbering.
- **f-strings:** support full expressions in field_name, but `str.format()` does not.

---

## References

- [Format Specification Mini-Language (docs)](https://docs.python.org/3/library/string.html#format-specification-mini-language)
- [PEP 498 — f-Strings](https://peps.python.org/pep-0498/)
- [str.format() docs](https://docs.python.org/3/library/stdtypes.html#str.format)

---
