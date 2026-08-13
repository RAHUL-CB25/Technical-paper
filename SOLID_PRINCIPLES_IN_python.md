# SOLID Principles in Python

**By:** Rahul Raj
**Topic:** SOLID Design Principles with Code Samples

---

## Table of Contents

1. [Introduction](#introduction)
2. [S - Single Responsibility Principle](#s---single-responsibility-principle)
3. [O - Open/Closed Principle](#o---openclosed-principle)
4. [L - Liskov Substitution Principle](#l---liskov-substitution-principle)
5. [I - Interface Segregation Principle](#i---interface-segregation-principle)
6. [D - Dependency Inversion Principle](#d---dependency-inversion-principle)
7. [Summary Table](#summary-table)
8. [Conclusion](#conclusion)

---

## Introduction

**SOLID** is an acronym for five design principles introduced by **Robert C. Martin
(Uncle Bob)**. These principles help developers write code that is easier to
understand, extend, and maintain.

| Letter | Principle                          |
|--------|--------------------------------------|
| S      | Single Responsibility Principle      |
| O      | Open/Closed Principle                |
| L      | Liskov Substitution Principle        |
| I      | Interface Segregation Principle      |
| D      | Dependency Inversion Principle        |

Each section below explains the principle in simple words, then shows two versions
of the same code: **Before** (the problem) and **After** (the fix).

---

## S - Single Responsibility Principle

**Rule:** A class should have only one job. If a class does more than one thing,
it should be split into multiple classes.

**Why it matters:** If a class has many responsibilities, a change in one
responsibility can accidentally break another part of the class.

### Before (one class doing two jobs)

```python
class Report:
    def __init__(self, title, content):
        self.title = title
        self.content = content

    def generate_text(self):
        return f"{self.title}\n{self.content}"

    def save_to_file(self, filename):
        with open(filename, "w") as f:
            f.write(self.generate_text())
```

### After (each class has one job)

```python
class Report:
    def __init__(self, title, content):
        self.title = title
        self.content = content

    def generate_text(self):
        return f"{self.title}\n{self.content}"


class ReportSaver:
    @staticmethod
    def save_to_file(report, filename):
        with open(filename, "w") as f:
            f.write(report.generate_text())


report = Report("Sales Report", "Revenue increased by 20%")
ReportSaver.save_to_file(report, "sales.txt")
```

Now `Report` only handles report content, and `ReportSaver` only handles saving.
If the saving logic changes, only `ReportSaver` needs to change.

---

## O - Open/Closed Principle

**Rule:** A class should be open for extension but closed for modification. This
means you should be able to add new behavior without changing existing, already
tested code.

### Before (adding a new type means editing existing code)

```python
class DiscountCalculator:
    def calculate(self, customer_type, price):
        if customer_type == "regular":
            return price * 0.95
        elif customer_type == "premium":
            return price * 0.90
```

### After (new types are added as new classes)

```python
from abc import ABC, abstractmethod

class DiscountStrategy(ABC):
    @abstractmethod
    def apply_discount(self, price):
        pass

class RegularCustomerDiscount(DiscountStrategy):
    def apply_discount(self, price):
        return price * 0.95

class PremiumCustomerDiscount(DiscountStrategy):
    def apply_discount(self, price):
        return price * 0.90

class VipCustomerDiscount(DiscountStrategy):
    def apply_discount(self, price):
        return price * 0.80


class DiscountCalculator:
    def calculate(self, strategy: DiscountStrategy, price):
        return strategy.apply_discount(price)


calculator = DiscountCalculator()
print(calculator.calculate(VipCustomerDiscount(), 1000))
```

To support a new customer type, we simply create a new class. The existing,
already-tested classes are never touched.

---

## L - Liskov Substitution Principle

**Rule:** Objects of a subclass should be usable wherever the parent class is
expected, without causing errors or unexpected behavior.

### Before (subclass breaks the parent's promise)

```python
class Bird:
    def fly(self):
        print("Flying high!")

class Sparrow(Bird):
    def fly(self):
        print("Sparrow flying")

class Ostrich(Bird):
    def fly(self):
        raise Exception("Ostrich can't fly!")

def make_bird_fly(bird: Bird):
    bird.fly()

make_bird_fly(Ostrich())
```

`Ostrich` is a `Bird`, but it cannot honestly do what `Bird.fly()` promises. This
breaks the substitution rule.

### After (redesign so every subclass can honestly fulfil the contract)

```python
from abc import ABC, abstractmethod

class Bird(ABC):
    @abstractmethod
    def move(self):
        pass

class FlyingBird(Bird):
    def move(self):
        print("Flying high!")

class FlightlessBird(Bird):
    def move(self):
        print("Running fast!")

class Sparrow(FlyingBird):
    pass

class Ostrich(FlightlessBird):
    pass

def make_bird_move(bird: Bird):
    bird.move()

make_bird_move(Sparrow())
make_bird_move(Ostrich())
```

Now every subclass of `Bird` can safely replace `Bird` without breaking the program.

---

## I - Interface Segregation Principle

**Rule:** A class should not be forced to implement methods it does not need. It is
better to have several small, specific interfaces than one large, general one.

### Before (one big interface forces unused methods)

```python
from abc import ABC, abstractmethod

class Machine(ABC):
    @abstractmethod
    def print_doc(self):
        pass

    @abstractmethod
    def scan_doc(self):
        pass

    @abstractmethod
    def fax_doc(self):
        pass

class OldPrinter(Machine):
    def print_doc(self):
        print("Printing...")

    def scan_doc(self):
        raise NotImplementedError("This printer can't scan")

    def fax_doc(self):
        raise NotImplementedError("This printer can't fax")
```

`OldPrinter` is forced to define `scan_doc()` and `fax_doc()` even though it cannot
actually do either.

### After (split into small interfaces)

```python
from abc import ABC, abstractmethod

class Printer(ABC):
    @abstractmethod
    def print_doc(self):
        pass

class Scanner(ABC):
    @abstractmethod
    def scan_doc(self):
        pass

class Fax(ABC):
    @abstractmethod
    def fax_doc(self):
        pass


class OldPrinter(Printer):
    def print_doc(self):
        print("Printing...")


class AllInOnePrinter(Printer, Scanner, Fax):
    def print_doc(self):
        print("Printing...")

    def scan_doc(self):
        print("Scanning...")

    def fax_doc(self):
        print("Faxing...")
```

Each class now implements only the interfaces it actually supports.

---

## D - Dependency Inversion Principle

**Rule:** High-level code should not depend directly on low-level, specific classes.
Both should depend on a shared abstraction instead.

### Before (tightly coupled to one specific class)

```python
class MySQLDatabase:
    def save(self, data):
        print(f"Saving '{data}' to MySQL database")

class UserService:
    def __init__(self):
        self.db = MySQLDatabase()

    def register_user(self, name):
        self.db.save(name)
```

`UserService` can only ever work with `MySQLDatabase`. Switching to a different
database later means editing `UserService` itself.

### After (depends on an abstraction, not a specific class)

```python
from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def save(self, data):
        pass

class MySQLDatabase(Database):
    def save(self, data):
        print(f"Saving '{data}' to MySQL database")

class MongoDBDatabase(Database):
    def save(self, data):
        print(f"Saving '{data}' to MongoDB database")


class UserService:
    def __init__(self, db: Database):
        self.db = db

    def register_user(self, name):
        self.db.save(name)


service = UserService(MongoDBDatabase())
service.register_user("Ravi")
```

`UserService` now works with any class that follows the `Database` contract, so new
database types can be added without changing `UserService`.

---

## Summary Table

| Principle | Simple Rule | Common Python Tool |
|-----------|-------------|--------------------|
| S - SRP | One class should do one job | Split into smaller classes |
| O - OCP | Add new behavior without changing old code | Abstract classes, strategy pattern |
| L - LSP | A subclass must work anywhere the parent class is used | Careful abstraction design |
| I - ISP | Don't force a class to implement methods it doesn't need | Multiple small interfaces |
| D - DIP | Depend on an abstraction, not a specific class | Constructor injection |

---

## Conclusion

The SOLID principles are simple rules that guide how classes should be designed so
that code stays clean, easy to change, and easy to test. For a fresher developer,
practicing these principles on small examples like the ones above is a good way to
build habits that carry over into larger, real-world projects.

---

## References

1. Robert C. Martin (Uncle Bob), *SOLID Principles of Object Oriented and Agile Design* — https://www.youtube.com/watch?v=TMuno5RZNeE (Watch from 16:00)
2. Programming with Mosh, *Object-Oriented Programming / Design Principles Playlist* — https://www.youtube.com/playlist?list=PL6n9fhu94yhXjG1w2blMXUzyDrZ_eyOme
3. Python Software Foundation, *abc — Abstract Base Classes* — https://docs.python.org/3/library/abc.html
