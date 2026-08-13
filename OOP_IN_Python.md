# Object-Oriented Programming (OOP) in Python

**By:** Rahul Raj
**Topic:** OOP Concepts in Python with Code Examples

---

## Table of Contents

1. [Introduction](#introduction)
2. [Class vs Object](#class-vs-object)
3. [Instance Variables vs Class Variables](#instance-variables-vs-class-variables)
4. [The Four Pillars of OOP](#the-four-pillars-of-oop)
   - [1. Encapsulation](#1-encapsulation)
   - [2. Inheritance](#2-inheritance)
     - [Types of Inheritance](#types-of-inheritance)
     - [The Diamond Problem](#the-diamond-problem)
     - [Python's Solution: MRO](#pythons-solution-mro-method-resolution-order)
   - [3. Polymorphism](#3-polymorphism)
   - [4. Abstraction](#4-abstraction)
5. [Conclusion](#conclusion)

---

## Introduction

**Object-Oriented Programming (OOP)** is a programming paradigm based on the concept of
**objects**, which bundle together **data (attributes)** and **behavior (methods)**.

Python is a fully object-oriented language and supports all major OOP concepts:

| Concept        | Purpose                                              |
|-----------------|-------------------------------------------------------|
| Class           | Blueprint for creating objects                       |
| Object          | Instance of a class                                  |
| Encapsulation   | Hides internal state, exposes controlled access      |
| Inheritance     | Reuses code from a parent class                      |
| Polymorphism    | Same interface, different implementations            |
| Abstraction     | Hides implementation details, shows only essentials  |

---

## Class vs Object

A **class** is a blueprint/template. An **object** is a real instance created from that
blueprint, with its own copy of data.

```python
class Student:              # Class = Blueprint
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks

s1 = Student("Anita", 89)   # Object 1
s2 = Student("Ravi", 76)    # Object 2

print(s1.name, s1.marks)
print(s2.name, s2.marks)
```

---

## Instance Variables vs Class Variables

This distinction confuses a lot of beginners, so it's worth covering separately before
diving into the four pillars.

### Instance Variable

- Declared **inside the constructor** (`__init__`) using `self`.
- **Unique to each object** — every object gets its own separate copy.
- Changing it on one object does **not** affect other objects.

### Class Variable

- Declared **directly inside the class**, outside any method.
- **Shared by all objects** of that class — there is only one copy in memory.
- Useful for values that should be common across every instance (constants, counters,
  configuration).

```python
class Employee:
    company_name = "TechCorp"     # class variable - shared by ALL employees
    employee_count = 0            # class variable used as a counter

    def __init__(self, name, salary):
        self.name = name          # instance variable - unique to each employee
        self.salary = salary      # instance variable - unique to each employee
        Employee.employee_count += 1   # updates the shared class variable

    def show_details(self):
        print(f"{self.name} works at {Employee.company_name}, salary: {self.salary}")


e1 = Employee("Anita", 50000)
e2 = Employee("Ravi", 60000)

e1.show_details()   # Anita works at TechCorp, salary: 50000
e2.show_details()   # Ravi works at TechCorp, salary: 60000

print(Employee.employee_count)   # 2 -> shared across all objects

# Changing a class variable through the class affects everyone
Employee.company_name = "NewTechCorp"
e1.show_details()   # Anita works at NewTechCorp
e2.show_details()   # Ravi works at NewTechCorp

# But assigning via an instance creates a NEW instance variable
# that only shadows the class variable for that one object
e1.company_name = "Freelance"
print(e1.company_name)   # Freelance (instance variable, only for e1)
print(e2.company_name)   # NewTechCorp (still using the class variable)
```

| Aspect              | Instance Variable                     | Class Variable                          |
|---------------------|-----------------------------------------|--------------------------------------------|
| Declared where      | Inside `__init__` using `self.var`      | Directly inside the class body              |
| Memory              | Separate copy per object                | Single shared copy for all objects          |
| Access              | `self.var` or `object.var`              | `ClassName.var` (or `self.var` to read)     |
| Typical use case     | Data unique to each object (name, id)   | Shared constants, counters, configuration   |

---

## The Four Pillars of OOP

### 1. Encapsulation

Encapsulation means **binding data and methods together** and **restricting direct access**
to some of an object's components using access modifiers.

- `public` → accessible everywhere (default)
- `_protected` → single underscore (convention only, still accessible)
- `__private` → double underscore (name mangling, not directly accessible from outside)

Below is a small **ATM-style bank account program** that demonstrates encapsulation:
the PIN and balance are kept **private**, and can only be read or changed through the
class's own controlled methods — never directly from outside the class.

```python
class BankAccount:
    """A simple ATM-style account demonstrating encapsulation."""

    def __init__(self, holder_name, pin, balance=0):
        self.holder_name = holder_name    # public attribute
        self.__pin = pin                  # private attribute - hidden from outside
        self.__balance = balance          # private attribute - hidden from outside

    # ---- Controlled access to private data ----
    def verify_pin(self, entered_pin):
        return self.__pin == entered_pin

    def get_balance(self):
        return self.__balance

    def deposit(self, amount):
        if amount <= 0:
            print("Invalid deposit amount.")
            return
        self.__balance += amount
        print(f"Deposited {amount}. New balance: {self.__balance}")

    def withdraw(self, amount):
        if amount <= 0:
            print("Invalid withdrawal amount.")
        elif amount > self.__balance:
            print("Insufficient balance!")
        else:
            self.__balance -= amount
            print(f"Withdrew {amount}. Remaining balance: {self.__balance}")

    def change_pin(self, old_pin, new_pin):
        if self.verify_pin(old_pin):
            self.__pin = new_pin
            print("PIN changed successfully.")
        else:
            print("Incorrect old PIN. PIN not changed.")


account = BankAccount("Ravi", pin="1234", balance=1000)

account.deposit(500)              # Deposited 500. New balance: 1500
account.withdraw(200)             # Withdrew 200. Remaining balance: 1300
print(account.get_balance())      # 1300 - read only through a method

account.change_pin("1234", "4321")

# print(account.__balance)        # AttributeError - not accessible directly
# print(account.__pin)            # AttributeError - not accessible directly
```

Here, `__balance` and `__pin` cannot be modified directly from outside the class —
every change must go through `deposit()`, `withdraw()`, or `change_pin()`, which
enforce validation rules (no negative deposits, no overdrawing, correct old PIN
required). This is the core idea of encapsulation: **protect data by controlling how
it is accessed and modified.**

---

### 2. Inheritance

Inheritance allows a class (**child**) to reuse properties and methods of another class
(**parent**), promoting code reuse.

```python
class Vehicle:
    def __init__(self, brand):
        self.brand = brand

    def start(self):
        print(f"{self.brand} vehicle is starting...")


class Car(Vehicle):          # Car inherits from Vehicle
    def __init__(self, brand, model):
        super().__init__(brand)
        self.model = model

    def display(self):
        print(f"{self.brand} {self.model}")


car = Car("Toyota", "Innova")
car.start()      # inherited method
car.display()    # own method
```

### Types of Inheritance

**1. Single** — one child, one parent.

```python
class Animal:
    def eat(self):
        print("Eating...")

class Dog(Animal):
    def bark(self):
        print("Barking...")

Dog().eat()
Dog().bark()
```

**2. Multilevel** — a chain: grandparent → parent → child.

```python
class Animal:
    def eat(self):
        print("Eating...")

class Dog(Animal):
    def bark(self):
        print("Barking...")

class Puppy(Dog):
    def weep(self):
        print("Weeping...")

Puppy().eat()    # from Animal
Puppy().bark()   # from Dog
Puppy().weep()   # own method
```

**3. Hierarchical** — one parent, multiple children.

```python
class Animal:
    def eat(self):
        print("Eating...")

class Dog(Animal):
    def bark(self):
        print("Barking...")

class Cat(Animal):
    def meow(self):
        print("Meowing...")

Dog().eat()
Cat().eat()
```

**4. Multiple** — one child, multiple parents.

```python
class Father:
    def skills(self):
        print("Gardening")

class Mother:
    def skills(self):
        print("Cooking")

class Child(Father, Mother):
    pass

Child().skills()   # Gardening -> resolved using MRO (left-to-right)
```

**5. Hybrid** — a combination of two or more of the above types in the same design
(for example, hierarchical + multiple together). This is exactly the shape that
causes the diamond problem, explained next.

---

### The Diamond Problem

The diamond problem happens in **multiple inheritance** when two parent classes
both inherit from the same grandparent class, and a child class inherits from both
parents. The class hierarchy looks like a diamond shape:

```
        A
       / \
      B   C
       \ /
        D
```

If both `B` and `C` override a method from `A`, it becomes unclear which version
`D` should use.

```python
class A:
    def greet(self):
        print("Hello from A")

class B(A):
    def greet(self):
        print("Hello from B")

class C(A):
    def greet(self):
        print("Hello from C")

class D(B, C):
    pass

D().greet()   # Which greet() runs - B's or C's?
```

### Python's Solution: MRO (Method Resolution Order)

Python solves the diamond problem using an algorithm called **C3 Linearization**,
which produces a fixed, predictable search order called the **MRO**. Python looks
at parent classes strictly **left to right**, and never checks a class twice.

```python
print(D.mro())
# [<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>]

D().greet()   # Hello from B  -> B is checked before C
```

Because `D(B, C)` lists `B` before `C`, Python finds `greet()` in `B` first and stops
there — `A`'s version is never reached unless neither `B` nor `C` defines it. You can
always check the exact order using `ClassName.mro()` or `help(ClassName)`.

---

### 3. Polymorphism

Polymorphism means **"many forms"** — the same interface (method name/operator) can
behave differently depending on the object or data type it works with. In Python this
shows up in three common ways: **duck typing**, **method overriding**, and
**operator overloading**.

#### a) Duck Typing

Python does **not check an object's type** before calling a method — it only checks
whether the object **has the method being called**. This is Python's famous saying:
*"If it walks like a duck and quacks like a duck, it's treated as a duck."* Unrelated
classes (no shared parent) can be used interchangeably as long as they support the
same method.

```python
class Duck:
    def sound(self):
        print("Quack quack!")

class Car:
    def sound(self):
        print("Vroom vroom!")

class Person:
    def sound(self):
        print("I'm imitating a sound!")

def make_it_sound(thing):
    # No type checking - Python just calls .sound() on whatever is passed in
    thing.sound()

for item in [Duck(), Car(), Person()]:
    make_it_sound(item)
```

None of `Duck`, `Car`, or `Person` share a common parent class, yet `make_it_sound()`
works with all of them because each one simply **has** a `sound()` method. This is
polymorphism achieved purely through duck typing.

#### b) Method Overriding (runtime polymorphism)

A child class provides its **own implementation** of a method that is already defined
in its parent class. The correct version is chosen automatically at runtime, based on
the actual object type.

```python
class Shape:
    def area(self):
        return 0

class Rectangle(Shape):
    def __init__(self, l, w):
        self.l, self.w = l, w

    def area(self):              # overrides Shape.area()
        return self.l * self.w

class Circle(Shape):
    def __init__(self, r):
        self.r = r

    def area(self):              # overrides Shape.area()
        return 3.14 * self.r * self.r

for shape in [Rectangle(4, 5), Circle(3)]:
    print(shape.area())          # same method call, different result per object
```

#### c) Operator Overloading

Python lets you redefine how built-in operators (`+`, `-`, `==`, `<`, `str()`, etc.)
behave for objects of your own classes, using special **dunder (double-underscore)
methods** such as `__add__`, `__eq__`, and `__str__`.

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        # Overloads the '+' operator for Point objects
        return Point(self.x + other.x, self.y + other.y)

    def __eq__(self, other):
        # Overloads the '==' operator for Point objects
        return self.x == other.x and self.y == other.y

    def __str__(self):
        # Overloads how print(point) displays the object
        return f"Point({self.x}, {self.y})"


p1 = Point(2, 3)
p2 = Point(4, 1)

p3 = p1 + p2          # calls p1.__add__(p2)
print(p3)             # Point(6, 4)

print(p1 == Point(2, 3))   # True -> calls __eq__
```

Without operator overloading, `p1 + p2` would raise a `TypeError`, since Python does
not know how to "add" two custom objects by default.

---

### 4. Abstraction

Abstraction hides complex implementation details and shows only the necessary features.
In Python, it is achieved using the `abc` module (Abstract Base Classes).

```python
from abc import ABC, abstractmethod

class Payment(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

class CreditCardPayment(Payment):
    def pay(self, amount):
        print(f"Paid {amount} using Credit Card")

class UpiPayment(Payment):
    def pay(self, amount):
        print(f"Paid {amount} using UPI")

# Payment()  # Error: Can't instantiate an abstract class
payment = UpiPayment()
payment.pay(250)
```

---

## Conclusion

Object-Oriented Programming makes code **modular, reusable, and easier to maintain**.

- **Instance variables** hold data unique to each object, while **class variables**
  hold data shared across all objects.
- **Encapsulation** protects an object's internal state (as shown in the ATM-style
  `BankAccount` example) by exposing only controlled, validated access.
- **Inheritance** allows child classes to reuse and extend parent class behavior, and
  comes in several forms (single, multilevel, hierarchical, multiple, hybrid). Python
  resolves ambiguity in multiple/hybrid inheritance (the diamond problem) using MRO.
- **Polymorphism** lets the same interface behave differently — via **duck typing**
  (no shared parent needed), **method overriding** (child redefines a parent method),
  and **operator overloading** (redefining how built-in operators work on custom
  objects).
- **Abstraction** hides implementation details behind a clean, simple contract.



---
