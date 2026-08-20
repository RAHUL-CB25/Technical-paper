# Dictionary Methods

A dictionary in Python stores data as **key-value pairs**. It is mutable, so values can be added, changed, or removed after creation.
## Frequently Used Dictionary Methods

- `get(key)` - gets the value; returns `None` if the key is not found.
* `keys()` - returns all keys.
* `values()` - returns all values.
* `items()` - returns all key-value pairs.
* `update()` - adds new values or updates existing values.
* `pop(key)` - removes a key and returns its value.
* `popitem()` - removes and returns the last key-value pair.
* `setdefault(key, value)` - gets the value of a key or adds it if it does not exist.
* `clear()` - removes all items from the dictionary.
* `copy()` - creates a copy of the dictionary.

## Example

```python id="g2x8pd"
student = {
    "name": "Rahul",
    "age": 22,
    "course": "Python"
}

print(student.get("name"))

student["age"] = 23
student.update({"city": "Bengaluru"})

print(student)

student = {
    "name": "Rahul",
    "age": 22,
    "course": "Python"
}

print(student.keys())
print(student.values())
print(student.items())

student.pop("age")

print(student)
```

