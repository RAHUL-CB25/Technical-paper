# Python Cheat Sheet

Quick reference for the built-in types and tools you use every day.

## 1. List / Array

Python's dynamic, ordered, and mutable array.

| Method | Description |
|---|---|
| `append(x)` | add one item |
| `extend(x)` | add multiple items |
| `insert(i, x)` | add at index |
| `remove(x)` | remove first matching value |
| `pop(i)` | remove and return item |
| `sort()` | sort the list |
| `reverse()` | reverse the list |
| `index(x)` | get first index |
| `count(x)` | count occurrences |
| `clear()` | remove all items |
| `len(list)` | number of items |

Remember: `append` adds one item, `extend` adds many.

## 2. Dictionary

Stores data as `key : value` pairs. Keys are unique.

| Method | Description |
|---|---|
| `get(key)` | get value safely |
| `keys()` | all keys |
| `values()` | all values |
| `items()` | key-value pairs |
| `update()` | add/update values |
| `pop(key)` | remove key |
| `popitem()` | remove last pair |
| `setdefault()` | get or add default |
| `clear()` | remove all items |
| `"name" in d` | check key |
| `len(d)` | number of pairs |

Remember: `update` adds or updates values.

## 3. Tuple

Ordered, immutable, duplicates allowed.

| Method | Description |
|---|---|
| `count(x)` | count occurrences |
| `index(x)` | first index |
| `len(t)` | length |
| `min(t)` | smallest value |
| `max(t)` | largest value |
| `sum(t)` | total |
| `sorted(t)` | returns a list |

A tuple cannot be changed after creation.

## 4. String

Ordered and immutable.

| Method | Description |
|---|---|
| `upper()` | uppercase |
| `lower()` | lowercase |
| `title()` | capitalize each word |
| `capitalize()` | capitalize first character |
| `strip()` | remove spaces from both ends |
| `lstrip()` / `rstrip()` | left/right spaces |
| `split()` | string to list |
| `join()` | iterable to string |
| `replace(a, b)` | replace text |
| `find(x)` | position, -1 if absent |
| `index(x)` | position, error if absent |
| `count(x)` | count occurrences |
| `startswith(x)` | check beginning |
| `endswith(x)` | check ending |
| `isalpha()` | alphabets only |
| `isdigit()` | digits only |
| `isalnum()` | alphabets + digits |
| `isspace()` | whitespace only |

## 5. Set

Unique and mutable.

| Method | Description |
|---|---|
| `add(x)` | add one |
| `update(x)` | add many |
| `remove(x)` | remove, error if absent |
| `discard(x)` | remove, no error |
| `pop()` | remove arbitrary item |
| `clear()` | remove all |
| `union()` | combine sets |
| `intersection()` | common items |
| `difference()` | first set only |
| `symmetric_difference()` | in either, not both |
| `issubset()` | check subset |
| `issuperset()` | check superset |
| `isdisjoint()` | no common items |
| `a \| b` | union |
| `a & b` | intersection |
| `a - b` | difference |

## 6. Built-in Functions

Handy on their own, not tied to one type.

| Function | Description |
|---|---|
| `len()` | length |
| `type()` | data type |
| `isinstance()` | type check |
| `sum()` | total |
| `min()` / `max()` | smallest/largest |
| `abs()` | absolute value |
| `round()` | round number |
| `sorted()` | new sorted list |
| `reversed()` | reverse iterator |
| `enumerate()` | index + value |
| `zip()` | combine iterables |
| `map()` | apply function |
| `filter()` | filter values |
| `any()` | at least one true |
| `all()` | all true |
| `range()` | number sequence |

## 7. Functions

```python
def add(a, b):
    return a + b
```

| Concept | Description |
|---|---|
| `*args` | tuple of extra positional args |
| `**kwargs` | dict of extra keyword args |
| `lambda` | anonymous function |
| `return` | sends value back |

```python
list(map(lambda x: x * 2, data))     # double every item
list(filter(lambda x: x % 2 == 0, data))  # keep even items
```

## 8. OOP

Class is a blueprint, object is an instance.

| Concept | Description |
|---|---|
| Encapsulation | controls data access |
| Inheritance | code reuse |
| Polymorphism | different behavior |
| Abstraction | hides implementation |
| `self.name` | public |
| `self._name` | protected (convention) |
| `self.__name` | private |
| `@classmethod` | class method |
| `@staticmethod` | static method |
| `super()` | call parent class |
| `ABC` | abstract base class |
| `@abstractmethod` | abstract method |

## 9. Advanced Python

| Concept | Description |
|---|---|
| Comprehension | short collection creation |
| Iterator | works with `next()` |
| Generator | uses `yield`, lazy values |
| Decorator | changes function behavior |
| Exception | handles runtime errors |

```python
[x*x for x in nums]          # list comprehension
{x*x for x in nums}          # set comprehension
{x: x*x for x in nums}       # dict comprehension
[x for x in nums if x % 2 == 0]  # filtered comprehension
```

## 10. DSA / Collections

| Tool | Description |
|---|---|
| `Counter` | frequency counts |
| `defaultdict` | gives a default value |
| `deque` | queue / FIFO |
| `list` | stack / LIFO |
| `heapq` | min heap / priority queue |
| `bisect` | binary search |

## Exception Handling

```python
try:
    ...
except ValueError:
    ...
else:
    ...
finally:
    ...
```