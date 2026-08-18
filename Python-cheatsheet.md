# Python Cheat Sheet

<table>
<tr>
<td width="50%" valign="top">

## 1. List / Array

Lists are Python's dynamic arrays. They are ordered and mutable.

- `append(x)` → add one item
- `extend(x)` → add multiple items
- `insert(i, x)` → add at index
- `remove(x)` → remove first matching value
- `pop(i)` → remove and return item
- `sort()` → sort the list
- `reverse()` → reverse the list
- `index(x)` → get first index
- `count(x)` → count occurrences
- `clear()` → remove all items
- `len(list)` → number of items

**Remember:**  
`append` = one | `extend` = many  
`remove` = value | `pop` = index  
`sort` = changes list | `sorted` = new list

---

## 2. Dictionary

Stores data as `key : value`. Keys are unique.

- `get(key)` → get value safely
- `keys()` → all keys
- `values()` → all values
- `items()` → key-value pairs
- `update()` → add/update values
- `pop(key)` → remove key
- `popitem()` → remove last pair
- `setdefault()` → get or add default
- `clear()` → remove all

```python
d = {"name": "Rahul", "age": 22}

d.get("name")
d.keys()
d.values()
d.items()
d.update({"age": 23})
d.pop("age")
```

`"name" in d` → check key  
`len(d)` → number of pairs

---

## 3. Tuple

**Ordered • Immutable • Duplicates allowed**

- `count(x)` → count occurrences
- `index(x)` → first index

```python
len(t)
min(t)
max(t)
sum(t)
sorted(t)       # returns a list
```

A tuple cannot be changed after creation.

</td>

<td width="50%" valign="top">

## 4. String

Strings are ordered and immutable.

- `upper()` → uppercase
- `lower()` → lowercase
- `title()` → capitalize each word
- `capitalize()` → capitalize first character
- `strip()` → remove spaces from both ends
- `lstrip()` / `rstrip()` → remove left / right spaces
- `split()` → string to list
- `join()` → iterable to string
- `replace(a,b)` → replace text
- `find(x)` → position, `-1` if absent
- `index(x)` → position, error if absent
- `count(x)` → count occurrences
- `startswith(x)` → check beginning
- `endswith(x)` → check ending
- `isalpha()` → alphabets only
- `isdigit()` → digits only
- `isalnum()` → alphabets + digits
- `isspace()` → whitespace only

**`find()` vs `index()`**  
`find()` → `-1` if missing  
`index()` → raises error if missing

---

## 5. Set

**Unique • Mutable**

- `add(x)` → add one
- `update(x)` → add many
- `remove(x)` → remove; error if absent
- `discard(x)` → remove; no error
- `pop()` → remove arbitrary item
- `clear()` → remove all
- `union()` → combine
- `intersection()` → common items
- `difference()` → first set only
- `symmetric_difference()` → in either, not both
- `issubset()` → check subset
- `issuperset()` → check superset
- `isdisjoint()` → no common items

```python
a | b     # union
a & b     # intersection
a - b     # difference
a ^ b     # symmetric difference
```

---

## 6. Built-in Functions

`len()` → length  
`type()` → data type  
`isinstance()` → type check  
`sum()` → total  
`min()` / `max()` → smallest / largest  
`abs()` → absolute value  
`round()` → round number  
`sorted()` → new sorted list  
`reversed()` → reverse iterator  
`enumerate()` → index + value  
`zip()` → combine iterables  
`map()` → apply function  
`filter()` → filter values  
`any()` → at least one true  
`all()` → all true  
`range()` → number sequence

</td>
</tr>

<tr>
<td width="50%" valign="top">

## 7. Functions

```python
def add(a, b):
    return a + b

def test(*args):
    pass

def test(**kwargs):
    pass

lambda x: x * x
```

```python
list(map(lambda x: x * 2, data))
list(filter(lambda x: x % 2 == 0, data))
```

`*args` → tuple  
`**kwargs` → dictionary  
`lambda` → anonymous function

---

## 8. OOP

**Class** → blueprint  
**Object** → instance  
**Encapsulation** → controls data access  
**Inheritance** → code reuse  
**Polymorphism** → different behavior  
**Abstraction** → hides implementation

### Method Types

```python
def show(self):
    pass

@classmethod
def show(cls):
    pass

@staticmethod
def add(a, b):
    return a + b
```

### Access

```python
self.name       # public
self._name      # protected convention
self.__name     # private
```

### Inheritance

```python
class Child(Parent):
    def __init__(self):
        super().__init__()
```

### Abstraction

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    @abstractmethod
    def start(self):
        pass
```

</td>

<td width="50%" valign="top">

## 9. Advanced Python

**Comprehension** → short collection creation  
**Iterator** → works with `next()`  
**Generator** → uses `yield`, lazy values  
**Decorator** → changes function behavior  
**Exception** → handles runtime errors

```python
[x*x for x in nums]
{x*x for x in nums}
{x:x*x for x in nums}
[x for x in nums if x % 2 == 0]
```

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

```python
def numbers():
    for i in range(5):
        yield i
```

---

## 10. DSA / Collections

```python
from collections import Counter, defaultdict, deque
```

```python
Counter(arr)
Counter(arr).most_common()

d = defaultdict(list)
d["a"].append(1)

q = deque()
q.append(x)
q.popleft()

stack = []
stack.append(x)
stack.pop()
```

`Counter` → frequency  
`defaultdict` → default value  
`deque` → queue / FIFO  
`list` → stack / LIFO

### Heap

```python
import heapq

heapq.heappush(heap, x)
heapq.heappop(heap)
heapq.heapify(arr)
heapq.nlargest(k, arr)
heapq.nsmallest(k, arr)
```

`heapq` → min heap / priority queue

### Binary Search

```python
import bisect

bisect.bisect_left(arr, x)
bisect.bisect_right(arr, x)
```

---

## 11. SOLID

**S — Single Responsibility**  
One class, one main responsibility.

**O — Open/Closed**  
Open for extension, closed for modification.

**L — Liskov Substitution**  
Child should safely replace parent.

**I — Interface Segregation**  
Don't force unused methods.

**D — Dependency Inversion**  
Depend on abstractions, not concrete classes.

</td>
</tr>
</table>




```
