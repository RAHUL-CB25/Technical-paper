# String Methods

Strings in Python are **ordered and immutable** collections of characters. The `str` class provides many built-in methods for working with text.

## Frequently Used String Methods

* `upper()` / `lower()` - converts text to uppercase or lowercase.
* `strip()` - removes spaces from the beginning and end.
* `split()` - converts a string into a list.
* `join()` - joins multiple strings into one string.
* `replace(old, new)` - replaces one text with another.
* `find(sub)` - returns the position of the text, or `-1` if not found.
* `index(sub)` - returns the position of the text, but gives an error if not found.
* `count(sub)` - counts how many times text appears.
* `startswith()` / `endswith()` - checks the beginning or ending of a string.
* `capitalize()` - makes the first character uppercase.
* `title()` - makes the first character of each word uppercase.
* `isalpha()` - checks if all characters are alphabets.
* `isdigit()` - checks if all characters are digits.
* `isalnum()` - checks if all characters are alphabets or numbers.
* `isspace()` - checks if all characters are spaces.

## Example

```python
name = "  rahul raj  "

clean_name = name.strip().title()

print(clean_name)
# Rahul Raj
``
text = "python is easy"

print(text.upper())
print(text.replace("easy", "powerful"))
print(text.split())
print(text.count("python"))
```
