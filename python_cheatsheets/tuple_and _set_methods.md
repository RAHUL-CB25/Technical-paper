# Tuple Methods

A tuple is an **ordered and immutable** collection. Once created, its values cannot be changed.

## Frequently Used Tuple Methods

* `count(x)` - counts how many times a value appears.
* `index(x)` - returns the position of the first matching value.
* `len()` - returns the number of items.
* `min()` - returns the smallest value.
* `max()` - returns the largest value.
* `sum()` - returns the total of numeric values.
* `sorted()` - returns a sorted list.

## Example

```python
numbers = (10, 20, 10, 30)

print(numbers.count(10))
print(numbers.index(20))
print(len(numbers))
```
# Set Methods

A set is an **unordered and mutable** collection of unique values. Duplicate values are automatically removed.

## Frequently Used Set Methods

* `add(x)` - adds one value.
* `update(x)` - adds multiple values.
* `remove(x)` - removes a value and gives an error if it is missing.
* `discard(x)` - removes a value without an error if it is missing.
* `pop()` - removes an arbitrary value.
* `clear()` - removes all values.
* `union()` - combines two sets.
* `intersection()` - returns common values.
* `difference()` - returns values present only in the first set.
* `symmetric_difference()` - returns values that are not common.

## Example

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a.union(b))
print(a.intersection(b))
print(a.difference(b))
```

---



