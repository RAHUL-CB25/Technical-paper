# SOLID Principles in Python

## What is SOLID?

SOLID is a set of five principles used to write clean and maintainable object-oriented code.

| Letter | Principle             | Simple Meaning                             |
| ------ | --------------------- | ------------------------------------------ |
| S      | Single Responsibility | One class, one job                         |
| O      | Open/Closed           | Add new features without changing old code |
| L      | Liskov Substitution   | Child should work like parent              |
| I      | Interface Segregation | Don't force unnecessary methods            |
| D      | Dependency Inversion  | Depend on abstraction                      |

---
## 1. Single Responsibility Principle
A class should have one main responsibility.
### Problem

```python
class Report:
    def generate(self):
        return "Sales Report"

    def save(self, filename):
        with open(filename, "w") as file:
            file.write(self.generate())
```

The class is doing two jobs: generating and saving.

### Solution

```python
class Report:
    def generate(self):
        return "Sales Report"


class ReportSaver:
    def save(self, report, filename):
        with open(filename, "w") as file:
            file.write(report.generate())
```

Now each class has a separate job.

---

## 2. Open/Closed Principle

We should be able to add new behavior without changing existing code.

### Problem

```python
class Discount:
    def calculate(self, customer, price):
        if customer == "regular":
            return price * 0.95
        if customer == "premium":
            return price * 0.90
```

Every new customer type requires changing the class.

### Solution

```python
class RegularDiscount:
    def apply(self, price):
        return price * 0.95


class PremiumDiscount:
    def apply(self, price):
        return price * 0.90
```

For a new discount, create another class.

---

## 3. Liskov Substitution Principle

A child class should work correctly wherever the parent class is used.

### Problem

```python
class Bird:
    def fly(self):
        print("Flying")


class Ostrich(Bird):
    def fly(self):
        raise Exception("Cannot fly")
```

The child class breaks the expected behavior of the parent.

### Solution

```python
class Bird:
    def move(self):
        pass


class Sparrow(Bird):
    def move(self):
        print("Flying")


class Ostrich(Bird):
    def move(self):
        print("Running")
```

Now both classes follow the same basic contract.

---

## 4. Interface Segregation Principle

A class should not be forced to implement methods it does not need.

### Problem

```python
class Machine:
    def print(self):
        pass

    def scan(self):
        pass


class OldPrinter(Machine):
    def print(self):
        print("Printing")

    def scan(self):
        raise NotImplementedError
```

The old printer does not need scanning.

### Solution

```python
class Printer:
    def print(self):
        print("Printing")


class Scanner:
    def scan(self):
        print("Scanning")


class OldPrinter(Printer):
    pass
```

Each class uses only the functionality it needs.

---

## 5. Dependency Inversion Principle

A class should not depend directly on one specific implementation.

### Problem

```python
class MySQL:
    def save(self, data):
        print("Saving to MySQL")


class UserService:
    def __init__(self):
        self.db = MySQL()
```

UserService is tightly connected to MySQL.

### Solution

```python
class UserService:
    def __init__(self, db):
        self.db = db

    def register(self, name):
        self.db.save(name)


service = UserService(MySQL())
```

The database is passed from outside. This makes it easier to change the database later.

---
* Python Documentation: https://docs.python.org/3/library/abc.html
- Robert C. Martin (Uncle Bob) — SOLID Principles of Object Oriented and Agile Design: https://www.youtube.com/watch?v=TMuno5RZNeE

