# Array (List) Methods in Python

Python does not use a separate array type like Java. In most cases, we use a list to store multiple values in an ordered and mutable collection.

Lists can be used to add, remove, search, and arrange elements.

## Common List Methods

| Method         | Use                                  |
| -------------- | ------------------------------------ |
| `append(x)`    | Adds one item at the end             |
| `extend(x)`    | Adds multiple items                  |
| `insert(i, x)` | Adds an item at a specific index     |
| `remove(x)`    | Removes the first matching item      |
| `pop(i)`       | Removes and returns an item          |
| `sort()`       | Sorts the list                       |
| `reverse()`    | Reverses the list                    |
| `index(x)`     | Finds the first index of an item     |
| `count(x)`     | Counts how many times an item occurs |
| `clear()`      | Removes all items                    |

## Example

```python
numbers = [4, 2, 9, 1]

numbers.append(7)
numbers.sort()

print(numbers)
# [1, 2, 4, 7, 9]


numbers = [10, 20, 30, 20]

numbers.append(40)
print(numbers)

numbers.remove(20)
print(numbers)

print(numbers.index(30))
print(numbers.count(20))

numbers.pop()
print(numbers)
```

