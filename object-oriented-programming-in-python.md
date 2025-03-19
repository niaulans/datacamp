## [Object-Oriented Programming in Python](https://app.datacamp.com/learn/courses/object-oriented-programming-in-python)

OOP Fundamentals
------------------------------------------------------------------------
Procedural programming:
- Code as a sequence of steps 
- Great for data analysis

Object-oriented programming:
- Code as interactions of objects
- Great for building frameworks and tools
- Reusable and maintainable code

Object as data structures:
Object = state + behavior

Encapsulation:
- Bundling data with code operating on it 

Classes as blueprints:
- Class: blueprint for objects outlining possible states and behaviors

Objects in Python:
- Everything in Python is an object
- Every object has a class
- Use type() to check the class of an object

Attributes and methods:
State -> attributes
Behavior -> methods

import numpy as np
a = np.array([1, 2, 3])
a.shape -> attribute
a.sum() -> method
a.reshape(1, 3) -> method

object = attributes + methods
attributes -> variables -> obj.attribute,
methods -> functions -> obj.method()

dir(obj) -> list of attributes and methods of an object

class -> blueprint
object -> instance of a class

------------------------------------------------------------------------
# Print the mystery employee's name
print(mystery.name)

# Print the mystery employee's salary
print(mystery.salary)

# Give the mystery employee a raise of $2500
mystery.give_raise(2500)

# Print the salary again
print(mystery.salary)

------------------------------------------------------------------------
Class anatomy: attributes and methods
A basic class:

class Customer: -> class name
    pass

c1 = Customer() -> object of the class
c2 = Customer() -> object of the class

class Customer:
    
    def identity(self, name): -> method
        print(f"I am Customer {name}")

cust = Customer()
cust.identity("Nia")
-> I am Customer Nia

What is self?
- Classes are templates, how to refer data of a particular object?
- self is a stand-in for a particular object used in class definition
- should be the first argument of any method

Add an attribute to class:

class Customer:
    def set_name(self, new_name):
        self.name = new_name
    
cust = Customer() -> .name attribute is not defined
cust.set_name("Nia") -> .name attribute is defined
print(cust.name) 
-> Nia

------------------------------------------------------------------------
class Employee:
    def set_name(self, new_name):
        self.name = new_name

    def set_salary(self, new_salary):
        self.salary = new_salary 
  
emp = Employee()
emp.set_name('Korel Rossi')
emp.set_salary(50000)

# Print the salary attribute of emp
print(emp.salary)

# Increase salary of emp by 1500
emp.salary = emp.salary + 1500

# Print the salary attribute of emp again
print(emp.salary)

------------------------------------------------------------------------
class Employee:
    def set_name(self, new_name):
        self.name = new_name

    def set_salary(self, new_salary):
        self.salary = new_salary 

    # Add a give_raise() method with raise amount as a parameter
    def give_raise(self, amount):
        self.salary = self.salary + amount


emp = Employee()
emp.set_name('Korel Rossi')
emp.set_salary(50000)

print(emp.salary)
emp.give_raise(1500)
print(emp.salary)
------------------------------------------------------------------------
class Employee:
    def set_name(self, new_name):
        self.name = new_name

    def set_salary(self, new_salary):
        self.salary = new_salary 

    def give_raise(self, amount):
        self.salary = self.salary + amount

    # Add monthly_salary method that returns 1/12th of salary attribute
    def monthly_salary(self):
        return self.salary / 12

    
emp = Employee()
emp.set_name('Korel Rossi')
emp.set_salary(50000)

# Get monthly salary of emp and assign to mon_sal
mon_sal = emp.monthly_salary()

# Print mon_sal
print(mon_sal)
------------------------------------------------------------------------
Class anatomy: the __init__ constructor

Constructor:
- Add data to object when creating it
- Constructor __init__() method is called every time an object is created.

class Customer:
    def __init__(self, name):
        self.name = name
        print("The __init__ method was called")

cust = Customer("Lara")
print(cust.name)
-> The __init__ method was called
-> Lara

Attributes in methods vs. constructor:
- Attributes can be added in methods
- Constructor is a good place to add attributes

Best practices:
- Initializes attributes in __init__()
- Naming :
    - CamelCase for class names
    - lower_snake_case for method and attribute names
- Keep self as self in methods
- Use docstrings to document classes

------------------------------------------------------------------------
class Employee:
  
    def __init__(self, name, salary=0):
        self.name = name
        # Modify code below to check if salary is positive
        if salary > 0:
            self.salary = salary
        else:
            self.salary = 0
            print("Invalid salary!")
   
   # ...Other methods omitted for brevity ...
      
emp = Employee("Korel Rossi", -1000)
print(emp.name)
print(emp.salary)

------------------------------------------------------------------------
# Write the class Point as outlined in the instructions
import numpy as np

class Point:
    """ A point on a 2D plane 

    Attributes
    ----------
    x : float, default 0.0. The x coordinate of the point        
    y : float, default 0.0. The y coordinate of the point
    """

    def __init__(self, x=0.0, y=0.0):
        self.x = x
        self.y = y

    def distance_to_origin(self):
        return np.sqrt((self.x**2) + (self.y**2))

    def reflect(self, axis):
        if axis == "x":
            self.y = - self.y
        elif axis == "y":
            self.x = - self.x
        else:
            print("The argument axis only accepts values 'x' and 'y'!")

pt = Point(x=3.0)
pt.reflect("y")
print((pt.x, pt.y))
pt.y = 4.0
print(pt.distance_to_origin())
------------------------------------------------------------------------
Inheritance and Polymorphism

Core principles of OOP:
- Inheritance: Extending functionality of existing code
- Polymorphism: Creating a unified interface
- Encapsulation: Bundling of data and methods   

Instance-level data:
- Attributes are instance-level data
- Each object has its own copy of attributes

class Employee:
    def __init__(self, name, salary=30000):
        self.name = name
        self.salary = salary

emp1 = Employee("Amar Howard", 30000)
emp2 = Employee("Carolyn Ramirez", 35000)

name, salary -> instance-level data
------------------------------------------------------------------------
Class-level data:
- Data shared among all instances of a class 
- Define class attributes in the class body

class MyClass:
    # Class attribute
    CLASS_ATTR_NAME = value

"Global variables" for the class

class Employee:
    MIN_SALARY = 30000 --> no self
    def __init__(self, name, salary):
        self.name = name
        if salary >= Employee.MIN_SALARY:
            self.salary = salary
        else:
            self.salary = Employee.MIN_SALARY

emp1 = Employee("Amar Howard", 30000)
emp1.MIN_SALARY
-> 30000

Why use class attributes?
- Global constants related to the class
- Minimal/maximal values for attributes
- Commonly used values and constants, e.g PI

------------------------------------------------------------------------
Class methods:
- Methods are already "shared" among all instances
- Class methods can't use instance-level data
- Use @classmethod decorator

class MyClass:

    @classmethod
    def my_class_method(cls, arg1, arg2): -> cls is the class itself
        pass
    
MyClass.my_class_method(arg1, arg2)

class Employee:
    MIN_SALARY = 30000

    def __init__(self, name, salary):
        self.name = name
        if salary >= Employee.MIN_SALARY:
            self.salary = salary
        else:
            self.salary = Employee.MIN_SALARY

    @classmethod
    def from_file(cls, filename):
    with open(filename, 'r') as f:
        name = f.readline()
    return cls(name)

emp = Employee.from_file('employee_data.txt')
print(emp.name)
-> Amar Howard

------------------------------------------------------------------------
# Create a Player class
class Player:
    MAX_POSITION = 10

    def __init__(self, position=0):
        self.position = position

# Print Player.MAX_POSITION       
print(Player.MAX_POSITION)

# Create a player p and print its MAX_POSITITON
p = Player()
print(p.MAX_POSITION)

------------------------------------------------------------------------
class Player:
    MAX_POSITION = 10
    
    def __init__(self):
        self.position = 0

    # Add a move() method with steps parameter
    def move(self, steps):
        if self.position + steps < Player.MAX_POSITION:
            self.position += steps
        else:
            self.position = Player.MAX_POSITION
       
    # This method provides a rudimentary visualization in the console    
    def draw(self):
        drawing = "-" * self.position + "|" +"-"*(Player.MAX_POSITION - self.position)
        print(drawing)

p = Player(); p.draw()
p.move(4); p.draw()
p.move(5); p.draw()
p.move(3); p.draw()

------------------------------------------------------------------------
# Create Players p1 and p2
p1 = Player()
p2 = Player()

print("MAX_SPEED of p1 and p2 before assignment:")
# Print p1.MAX_SPEED and p2.MAX_SPEED
print(p1.MAX_SPEED) -> 3
print(p2.MAX_SPEED) -> 3

# Assign 7 to p1.MAX_SPEED
p1.MAX_SPEED = 7

print("MAX_SPEED of p1 and p2 after assignment:")
# Print p1.MAX_SPEED and p2.MAX_SPEED
print(p1.MAX_SPEED) -> 7
print(p2.MAX_SPEED) -> 3

print("MAX_SPEED of Player:")
# Print Player.MAX_SPEED
print(Player.MAX_SPEED) -> 3

------------------------------------------------------------------------
class BetterDate:    
    # Constructor
    def __init__(self, year, month, day):
      # Recall that Python allows multiple variable assignments in one line
      self.year, self.month, self.day = year, month, day
    
    # Define a class method from_str
    @classmethod
    def from_str(cls, datestr):
        # Split the string at "-" and convert each part to integer
        parts = datestr.split("-")
        year, month, day = int(parts[0]), int(parts[1]), int(parts[2])
        # Return the class instance
        return cls(year, month, day)
        
bd = BetterDate.from_str('2020-04-30')   
print(bd.year)
print(bd.month)
print(bd.day)

------------------------------------------------------------------------
# import datetime from datetime
from datetime import datetime

class BetterDate:
    def __init__(self, year, month, day):
      self.year, self.month, self.day = year, month, day
      
    @classmethod
    def from_str(cls, datestr):
        year, month, day = map(int, datestr.split("-"))
        return cls(year, month, day)
      
    # Define a class method from_datetime accepting a datetime object
    @classmethod
    def from_datetime(cls, datetime):
      year = datetime.year
      month = datetime.month
      day = datetime.day
      return cls(year, month, day)
      
# You should be able to run the code below with no errors: 
today = datetime.today()     
bd = BetterDate.from_datetime(today)   
print(bd.year)
print(bd.month)
print(bd.day)

------------------------------------------------------------------------
Class inheritance:

DRY -> Don't Repeat Yourself
New class functionality = Old class functionality + extra

class MyChild(MyParent):
    pass

class bankAccount:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        self.balance -= amount

class SavingsAccount(BankAccount):
    pass

- Child class has all of the parent's attributes and methods
- isinstance() checks if an object is an instance of a class

------------------------------------------------------------------------
class Employee:
  MIN_SALARY = 30000    

  def __init__(self, name, salary=MIN_SALARY):
      self.name = name
      if salary >= Employee.MIN_SALARY:
        self.salary = salary
      else:
        self.salary = Employee.MIN_SALARY
        
  def give_raise(self, amount):
      self.salary += amount      
        
# Define a new class Manager inheriting from Employee
class Manager(Employee):
  pass

# Define a Manager object
mng = Manager("Debbie Lashko", 86500)

# Print mng's name
print(mng.name)
------------------------------------------------------------------------
class Employee:
  MIN_SALARY = 30000    

  def __init__(self, name, salary=MIN_SALARY):
      self.name = name
      if salary >= Employee.MIN_SALARY:
        self.salary = salary
      else:
        self.salary = Employee.MIN_SALARY
  def give_raise(self, amount):
    self.salary += amount      
        
# MODIFY Manager class and add a display method
class Manager(Employee):
  def display(self):
    print(f"Manager {self.name}")

mng = Manager("Debbie Lashko", 86500)
print(mng.name)

# Call mng.display()
print(mng.display())

------------------------------------------------------------------------
Customizing constructors:

class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        self.balance -= amount

class SavingsAccount(BankAccount):
    def __init__(self, balance, interest_rate):
        bankAccount.__init__(self, balance) -> call the parent constructor
        self.interest_rate = interest_rate

class CheckingAccount(BankAccount):
    def __init__(self, balance, limit):
        BankAccount.__init__(self, balance)
        self.limit = limit
    def deposit(self, amount):
        self.balance += amount
    def withdraw(self, amount, fee=0):
        if fee <= self.limit:
            BankAccount.withdraw(self, amount + fee)
        else:
            BankAccount.withdraw(self, amount + self.limit)

------------------------------------------------------------------------
class Employee:
    def __init__(self, name, salary=30000):
        self.name = name
        self.salary = salary

    def give_raise(self, amount):
        self.salary += amount

        
class Manager(Employee):
    def display(self):
        print("Manager ", self.name)

    def __init__(self, name, salary=50000, project=None):
        Employee.__init__(self, name, salary)
        self.project = project

    # Add a give_raise method
    def give_raise(self, amount, bonus=1.05): --> override the parent method
        return Employee.give_raise(self, amount * bonus)
    
    
mngr = Manager("Ashta Dunbar", 78500)
mngr.give_raise(1000)
print(mngr.salary)
mngr.give_raise(2000, bonus=1.03)
print(mngr.salary)

------------------------------------------------------------------------
# Import pandas as pd
import pandas as pd

# Define LoggedDF inherited from pd.DataFrame and add the constructor
class LoggedDF(pd.DataFrame):
  
  def __init__(self, *args, **kwargs):
    pd.DataFrame.__init__(self, *args, **kwargs)
    self.created_at = datetime.today()
    
  def to_csv(self, *args, **kwargs):
    # Copy self to a temporary DataFrame
    temp = self.copy()
    
    # Create a new column filled with self.created_at
    temp["created_at"] = self.created_at
    
    # Call pd.DataFrame.to_csv on temp, passing in *args and **kwargs
    pd.DataFrame.to_csv(temp, *args, **kwargs)

------------------------------------------------------------------------
Operator overloading: comparison

Object quality:

class Customer:
    def __init__(self, name, balance):
        self.name = name
        self.balance = balance

cust1 = Customer("Lara", 2000)
cust2 = Customer("Lara", 2000)
cust1 == cust2 
-> False

Overloading __eq__():

class Customer:
    def __init__(self, id, name):
        self.id, self.name = id, name

    def __eq__(self, other):
        print("__eq__ is called")
        return (self.id == other.id) and (self.name == other.name)

cust1 = Customer(1, "Lara")
cust2 = Customer(1, "Lara")
cust1 == cust2
-> True

Other comparison operators:
== -> __eq__()
!= -> __ne__()
<= -> __le__()
>= -> __ge__()
< -> __lt__()
> -> __gt__()

__hash__() method: returns a hash value for an object

------------------------------------------------------------------------
class BankAccount:
   # MODIFY to initialize a number attribute
    def __init__(self, number, balance=0):
        self.balance = balance
        self.number = number
      
    def withdraw(self, amount):
        self.balance -= amount 
    
    # Define __eq__ that returns True if the number attributes are equal 
    def __eq__(self, other):
        return self.number == other.number   

# Create accounts and compare them       
acct1 = BankAccount(123, 1000)
acct2 = BankAccount(123, 1000)
acct3 = BankAccount(456, 1000)
print(acct1 == acct2)
print(acct1 == acct3)
-> True
-> False
------------------------------------------------------------------------
class BankAccount:
    def __init__(self, number, balance=0):
        self.number, self.balance = number, balance
      
    def withdraw(self, amount):
        self.balance -= amount 

    # MODIFY to add a check for the type()
    def __eq__(self, other):
        return (self.number == other.number) & (type(self) == type(other))

acct = BankAccount(873555333)
pn = Phone(873555333)
print(acct == pn)

-> False

------------------------------------------------------------------------
class Parent:
    def __eq__(self, other):
        print("Parent's __eq__() called")
        return True

class Child(Parent):
    def __eq__(self, other):
        print("Child's __eq__() called")
        return True

p = Parent()
c = Child()

p == c 
-> Child's __eq__() called
-> True

Child class __eq__() always called when comparing Parent and Child objects
------------------------------------------------------------------------
Operator overloading: string representation

__str__(): human-readable string representation -> informal
__repr__(): unambiguous machine-readable string representation -> formal

Implementing __str__():

class Customer:
    def __init__(self, name, balance):
        self.name, self.balance = name, balance

    def __str__(self):
        cust_str = """
        Customer:
        name: {name}
        balance: {balance}""".format(name=self.name, balance=self.balance)

        return cust_str

cust = Customer("Lara", 2000)
print(cust)
-> Customer:
   name: Lara
   balance: 2000
------------------------------------------------------------------------
Implementing __repr__():

class Customer:
    def __init__(self, name, balance):
        self.name, self.balance = name, balance

    def __repr__(self):
        return "Customer('{name}', '{balance}')".format(name=self.name, balance=self.balance)

cust = Customer("Lara", 2000)
print(cust)
-> Customer('Lara', '2000')

------------------------------------------------------------------------
class Employee:
    def __init__(self, name, salary=30000):
        self.name, self.salary = name, salary
            
    # Add the __str__() method
    def __str__(self):
        emp_str = """
        Employee name: {name}
        Employee salary: {salary}
        """.format(name=self.name, salary=self.salary)
        return emp_str

emp1 = Employee("Amar Howard", 30000)
print(emp1)
emp2 = Employee("Carolyn Ramirez", 35000)
print(emp2)

------------------------------------------------------------------------

class Employee:
    def __init__(self, name, salary=30000):
        self.name, self.salary = name, salary
      

    def __str__(self):
        s = "Employee name: {name}\nEmployee salary: {salary}".format(name=self.name, salary=self.salary)      
        return s
      
    # Add the __repr__method  
    def __repr__(self):
        s = "Employee(\"{name}\", {salary})".format(name=self.name, salary=self.salary)      
        return s      

emp1 = Employee("Amar Howard", 30000)
print(repr(emp1))
emp2 = Employee("Carolyn Ramirez", 35000)
print(repr(emp2))

------------------------------------------------------------------------
Exception handling:

a = 1
a / 0
-> ZeroDivisionError: division by zero

a = [1, 2, 3]
a[5]
-> IndexError: list index out of range

a = 1
a + "Hello"
-> TypeError: unsupported operand type(s) for +: 'int' and 'str'

a = 1
a + b
-> NameError: name 'b' is not defined

try - except - finally:
try: 
    # Some code
except ExceptionNameHere:
    # Handle exception
finally:
    # Clean up

Raise an exception:
-> Will stop the program and raise an error 

def make_list_of_ones(length):
    if length < 1:
        raise ValueError("Invalid length")
    return [1] * length

------------------------------------------------------------------------
BaseException
- Exception 
    - ArithmaticError
        - FloatingPointError
        - OverflowError
        - ZeroDivisionError
    - TypeError
    - ValueError
        - UnicodeError
            - UnicodeDecodeError
            - UnicodeEncodeError
            - UnicodeTranslateError
    - RuntimeError
        - RecursionError
        - NotImplementedError
    ...
- SystemExit

------------------------------------------------------------------------
Custom exceptions:
- Inherit from Exception or one of its subclasses

class BalanceError(Exception):
    pass

class Customer:
    def __init__(self, name, balance):
        if balance < 0:
            raise BalanceError("Balance has to be non-negative")
        else:
            self.name, self.balance = name, balance

cust = Customer("Lara", -1000)
-> BalanceError: Balance has to be non-negative

------------------------------------------------------------------------
# MODIFY the function to catch exceptions
def invert_at_index(x, ind):
    try:
        return 1/x[ind]
    except ZeroDivisionError:
        print("Cannot divide by zero!")
    except IndexError:
        print("Index out of range!")
 
a = [5,6,0,7]

# Works okay
print(invert_at_index(a, 1))

# Potential ZeroDivisionError
print(invert_at_index(a, 2))

# Potential IndexError
print(invert_at_index(a, 5))

------------------------------------------------------------------------
class SalaryError(ValueError): pass
class BonusError(SalaryError): pass

class Employee:
  MIN_SALARY = 30000
  MAX_BONUS = 5000

  def __init__(self, name, salary = 30000):
    self.name = name    
    if salary < Employee.MIN_SALARY:
      raise SalaryError("Salary is too low!")      
    self.salary = salary
    
  # Rewrite using exceptions  
  def give_bonus(self, amount):
    if amount > Employee.MAX_BONUS:
       raise BonusError("The bonus amount is too high!")  
        
    elif self.salary + amount <  Employee.MIN_SALARY:
       raise SalaryError("The salary after bonus is too low!")
      
    else:  
      self.salary += amount

------------------------------------------------------------------------
except block for a parent exception will catch child exceptions

✅ Jika kita menangkap exception parent, semua child exception juga ikut tertangkap.
✅ Urutan except penting! Tangkap child exception lebih dulu, lalu parent di akhir untuk menangkap error lainnya.

It's better to include an except block for a child exception before the block for a parent exception, otherwise the child exceptions will be always be caught in the parent block, and the except block for the child will never be executed.

------------------------------------------------------------------------
Best Practice of Class Design

Polymorphism:
- Using a unified interface to operate on objects of different classes

def batch_withdraw(list_of_accounts, amount):
    for acct in list_of_accounts:
        acct.withdraw(amount)

b, c, s = BankAccount(1000), CheckingAccount(2000), SavingsAccount(3000)
batch_withdraw([b, c, s]) -> works for all account types

Base class should be interchangeable with any of its subclasses without altering any properties of the program
------------------------------------------------------------------------
Violating LSP 

- Syntactic incompability
BankAccount.withdraw() requires a single argument, but CheckingAccount.withdraw() requires two arguments
- Subclass strengthening input conditions
BankAccount.withdraw() allows any amount to be withdrawn, but CheckingAccount.withdraw() only allows withdrawals up to a certain limit
- Subclass weakening output conditions
BankAccount.withdraw() can only leave a non-negative balance, but CheckingAccount.withdraw() can leave a negative balance

-> Changing additional attributes in subclass methods
-> Throwing additional exceptions in subclass methods

NO LSP -> NO INHERITANCE
------------------------------------------------------------------------
class Rectangle:
    def __init__(self, w,h):
      self.w, self.h = w,h
      
# Define set_h to set h       
    def set_h(self, h):
      self.h = h

# Define set_w to set w
    def set_w(self, w):
      self.w = w   
      
class Square(Rectangle):
    def __init__(self, w):
      self.w, self.h = w, w 
      
# Define set_h to set w and h 
    def set_h(self, h):
      self.h = h
      self.w = h
      
# Define set_w to set w and h 
    def set_w(self, w):
      self.w = w   
      self.h = w  

------------------------------------------------------------------------
Managing data access: private attributes

Restricting access:
- Naming conventions
- Use @property to customize access
- Overriding __getattr__ and __setattr__ methods

Naming convention: internal attributes 
obj._att_name, obj._method_name

- Starts with a single _ -> internal use/protected
- Not a part of the public API

Naming convention: private attributes
obj.__att_name, obj.__method_name

- Starts with a double __ -> private use
- Name mangling: __att_name -> _ClassName__att_name

------------------------------------------------------------------------
# Add class attributes for max number of days and months
class BetterDate:
    _MAX_DAYS = 31
    _MAX_MONTHS = 12
    
    def __init__(self, year, month, day):
        self.year, self.month, self.day = year, month, day
        
    @classmethod
    def from_str(cls, datestr):
        year, month, day = map(int, datestr.split("-"))
        return cls(year, month, day)
    
    # Add _is_valid() checking day and month values
    def _is_valid(self):
        if self.day <= BetterDate._MAX_DAYS and self.month <= BetterDate._MAX_MONTHS:
            return True
        else:
            return False
    
bd1 = BetterDate(2020, 4, 30)
print(bd1._is_valid())

bd2 = BetterDate(2020, 6, 45)
print(bd2._is_valid())

------------------------------------------------------------------------
Properties:

Changing attribute values:
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def set_name(self, name):
        self.name = name

    def set_salary(self, salary):
        self.salary = salary

    def give_raise(self, amount):
        self._salary += amount

emp = Employee("Nia", 50000)
emp.salary = emp.salary + 10000
------------------------------------------------------------------------
@property decorator:

class Employee:
    def __init__(self, name, new_salary):
    self._salary = new_salary

    @property
    def salary(self):
        return self._salary

    @salary.setter
    def salary(self, new_salary):
        if new_salary < 0:
            raise ValueError("Invalid salary")
        self._salary = new_salary

- Jika kita hanya mendefinisikan @property tanpa @setter, atribut akan menjadi read-only.

Why use @property?
- user facing: behave like attributes
- developer-facing: control attribute access


Kelebihan @property & @setter	                           Tanpa @property	              Dengan @property
Bisa mengontrol perubahan data	                            ❌ Tidak bisa	            ✅ Bisa pakai @setter
Bisa menjalankan logika saat membaca atribut	            ❌ Tidak bisa	            ✅ Bisa dengan @property
Bisa mengubah atribut jadi method tanpa mengubah kode lama	❌ Harus ubah semua kode	    ✅ Tidak perlu ubah kode
Bisa membuat atribut hanya-baca	                            ❌ Tidak bisa	            ✅ Bisa tanpa @setter

------------------------------------------------------------------------
class Customer:
    def __init__(self, name, new_bal):
        self.name = name
        if new_bal < 0:
           raise ValueError("Invalid balance!")
        self._balance = new_bal  

    # Add a decorated balance() method returning _balance        
    @property
    def balance(self):
        return self._balance

    # Add a setter balance() method
    @balance.setter
    def balance(self, new_bal):
        # Validate the parameter value
        if new_bal < 0:
           raise ValueError("Invalid balance!")
        self._balance = new_bal
        print("Setter method called")

# Create a Customer        
cust = Customer("Belinda Lutz", 2000)

# Assign 3000 to the balance property
cust.balance = 3000

# Print the balance property
print(cust.balance)

------------------------------------------------------------------------
Read-only properties:

import pandas as pd
from datetime import datetime

# MODIFY the class to use _created_at instead of created_at
class LoggedDF(pd.DataFrame):
    def __init__(self, *args, **kwargs):
        pd.DataFrame.__init__(self, *args, **kwargs)
        self._created_at = datetime.today()
    
    def to_csv(self, *args, **kwargs):
        temp = self.copy()
        temp["created_at"] = self._created_at
        pd.DataFrame.to_csv(temp, *args, **kwargs)   
    
    # Add a read-only property: _created_at
    @property 
    def created_at(self):
        return self._created_at

# Instantiate a LoggedDF called ldf
ldf = LoggedDF({"col1": [1,2], "col2":[3,4]}) 

-> Throw an error if we try to assign a value to the created_at property

------------------------------------------------------------------------
class DataShell:
    def __init__(self, inputFile):
        self.file = inputFile  
    def df_from_csv(self):
        self.df = 
(self.file)
        return self.df
data_shell = DataShell(filepath)
data_shell.df_from_csv().head(3)

------------------------------------------------------------------------
class DataShell:
    def __init__(self, filename):
        self.file = filename
    def create_datashell(self):
        self.array = 
[1:,1:]
        return self.array
      
data_obj = DataShell(filepath)
data_obj.create_datashell()

------------------------------------------------------------------------


