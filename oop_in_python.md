# Object-Oriented Programming (OOP) in Python
## Class and Object
A class is a blueprint for creating objects. An object is an instance of a class.
```python
class Student:
    def __init__(self, name, marks):
        self.name = name
        self.marks = marks
    def show(self):
        print(self.name, self.marks)
s1 = Student("Rahul", 85)
s2 = Student("Amit", 75)
s1.show()
s2.show()
```
---
## Instance and Class Variables
An instance variable belongs to one object. A class variable is shared by objects of the class.
```python
class Employee:
    company = "TechCorp"
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
e1 = Employee("Rahul", 50000)
e2 = Employee("Amit", 60000)
print(e1.name)
print(e2.company)
```
| Instance Variable | Class Variable |
|---|---|
 Belongs to an object | Belongs to the class |
Uses `self` | Written inside the class |
| Different for objects | Usually shared |
| `name`, `salary` | `company`, counter |
---
# Four Pillars of OOP
The four main pillars are:
1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction
## 1. Encapsulation
Encapsulation means keeping data and methods together and controlling access to data.
Python uses normal attributes,  _name  as a protected convention, and  `__name`  for name mangling.
```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
    def get_balance(self):
        return self.__balance
account = BankAccount(1000)
account.deposit(500)
print(account.get_balance())
```
---
# 2. Inheritance
Inheritance allows a child class to reuse code from a parent class.
```python
class Vehicle:
    def start(self):
        print("Vehicle started")
class Car(Vehicle):
    def display(self):
        print("This is a car")
car = Car()
car.start()
car.display()
```
super() can be used when we want to call a parent class method.
---
# Types of Inheritance
### Single
One parent and one child.
### Multilevel
Inheritance happens in a chain.
### Hierarchical
Multiple children have the same parent.
### Multiple
One child has more than one parent.
### Hybrid
A combination of two or more inheritance types.
# Diamond Problem and MRO
The diamond problem can happen with multiple inheritance.


```text
      A
     / \
    B   C
     \ /
      D
```
Both `B` and `C` inherit from `A`, while `D` inherits from both.
MRO means Method Resolution Order. It tells Python the order used to search for a method.

---
# 3. Polymorphism
Polymorphism means "many forms". The same method or interface can work differently for different objects.
## Duck Typing
Python often cares about what an object can do instead of its exact type.
```python
class Duck:
    def sound(self):
        print("Quack")
class Dog:
    def sound(self):
        print("Bark")
def make_sound(animal):
    animal.sound()
make_sound(Duck())
make_sound(Dog())
```
Both objects have `sound()`, so the function works with both.
## Method Overriding
A child class can provide its own version of a parent method.
```python
class Animal:
    def sound(self):
        print("Some sound")
class Dog(Animal):
    def sound(self):
        print("Bark")
class Cat(Animal):
    def sound(self):
        print("Meow")
Dog().sound()
Cat().sound()
```
The method name is the same, but the output is different.
## Operator Overloading
Special methods define how operators work with objects.
Examples: `__add__` for `+`, `__eq__` for `==`, and `__str__` for `str()`.
# 4. Abstraction
Abstraction means hiding unnecessary implementation details.
Python provides the `abc` module for abstract classes.
```python
from abc import ABC, abstractmethod
class Payment(ABC):
    @abstractmethod
    def pay(self, amount):
        pass
class UpiPayment(Payment):
    def pay(self, amount):
        print("Paid using UPI")
class CardPayment(Payment):
    def pay(self, amount):
        print("Paid using Card")
UpiPayment().pay(500)
CardPayment().pay(1000)
```


# References
1. Python Documentation - Classes and Objects
   https://docs.python.org/3/tutorial/classes.html
2. Python Documentation - Method Resolution Order
   https://docs.python.org/3/howto/mro.html
3. Python Documentation - Abstract Base Classes
   https://docs.python.org/3/library/abc.html