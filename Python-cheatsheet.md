# Python Cheat Sheet

| **1. List / Array** | **2. Dictionary** |
|---|---|
| Python's dynamic, ordered and mutable array. | Stores data as `key : value`. Keys are unique. |
| `append(x)` → add one item | `get(key)` → get value safely |
| `extend(x)` → add multiple items | `keys()` → all keys |
| `insert(i, x)` → add at index | `values()` → all values |
| `remove(x)` → remove first matching value | `items()` → key-value pairs |
| `pop(i)` → remove and return item | `update()` → add/update values |
| `sort()` → sort the list | `pop(key)` → remove key |
| `reverse()` → reverse the list | `popitem()` → remove last pair |
| `index(x)` → get first index | `setdefault()` → get or add default |
| `count(x)` → count occurrences | `clear()` → remove all items |
| `clear()` → remove all items | `"name" in d` → check key |
| `len(list)` → number of items | `len(d)` → number of pairs |
| **Remember:** `append` = one, `extend` = many | **Remember:** `update` = add/update |

---

| **3. Tuple** | **4. String** |
|---|---|
| **Ordered • Immutable • Duplicates allowed** | **Ordered • Immutable** |
| `count(x)` → count occurrences | `upper()` → uppercase |
| `index(x)` → first index | `lower()` → lowercase |
| `len(t)` → length | `title()` → capitalize each word |
| `min(t)` → smallest value | `capitalize()` → capitalize first character |
| `max(t)` → largest value | `strip()` → remove spaces from both ends |
| `sum(t)` → total | `lstrip()` / `rstrip()` → left/right spaces |
| `sorted(t)` → returns a list | `split()` → string to list |
| Cannot be changed after creation | `join()` → iterable to string |
| | `replace(a, b)` → replace text |
| | `find(x)` → position, `-1` if absent |
| | `index(x)` → position, error if absent |
| | `count(x)` → count occurrences |
| | `startswith(x)` → check beginning |
| | `endswith(x)` → check ending |
| | `isalpha()` → alphabets only |
| | `isdigit()` → digits only |
| | `isalnum()` → alphabets + digits |
| | `isspace()` → whitespace only |

---

| **5. Set** | **6. Built-in Functions** |
|---|---|
| **Unique • Mutable** | `len()` → length |
| `add(x)` → add one | `type()` → data type |
| `update(x)` → add many | `isinstance()` → type check |
| `remove(x)` → remove, error if absent | `sum()` → total |
| `discard(x)` → remove, no error | `min()` / `max()` → smallest/largest |
| `pop()` → remove arbitrary item | `abs()` → absolute value |
| `clear()` → remove all | `round()` → round number |
| `union()` → combine sets | `sorted()` → new sorted list |
| `intersection()` → common items | `reversed()` → reverse iterator |
| `difference()` → first set only | `enumerate()` → index + value |
| `symmetric_difference()` → in either, not both | `zip()` → combine iterables |
| `issubset()` → check subset | `map()` → apply function |
| `issuperset()` → check superset | `filter()` → filter values |
| `isdisjoint()` → no common items | `any()` → at least one true |
| `a \| b` → union | `all()` → all true |
| `a & b` → intersection | `range()` → number sequence |
| `a - b` → difference | |

---

| **7. Functions** | **8. OOP** |
|---|---|
| ```python<br>def add(a, b):<br>    return a + b<br>``` | **Class** → blueprint |
| `*args` → tuple | **Object** → instance |
| `**kwargs` → dictionary | **Encapsulation** → controls data access |
| `lambda` → anonymous function | **Inheritance** → code reuse |
| `return` → sends value back | **Polymorphism** → different behavior |
| | **Abstraction** → hides implementation |
| ```python<br>list(map(lambda x: x * 2, data))<br>``` | `self.name` → public |
| ```python<br>list(filter(lambda x: x % 2 == 0, data))<br>``` | `self._name` → protected convention |
| | `self.__name` → private |
| | `@classmethod` → class method |
| | `@staticmethod` → static method |
| | `super()` → call parent class |
| | `ABC` → abstract base class |
| | `@abstractmethod` → abstract method |

---

| **9. Advanced Python** | **10. DSA / Collections** |
|---|---|
| **Comprehension** → short collection creation | `Counter` → frequency |
| **Iterator** → works with `next()` | `defaultdict` → default value |
| **Generator** → uses `yield`, lazy values | `deque` → queue / FIFO |
| **Decorator** → changes function behavior | `list` → stack / LIFO |
| **Exception** → handles runtime errors | `heapq` → min heap / priority queue |
| `[x*x for x in nums]` | `bisect` → binary search |
| `{x*x for x in nums}` | |
| `{x:x*x for x in nums}` | |
| `[x for x in nums if x % 2 == 0]` | |

### Exception Handling

```python
try:
    ...
except ValueError:
    ...
else:
    ...
finally:
    ...