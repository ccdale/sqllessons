## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)
1. Object Oriented Programming with Python
1. [Using the ORM](usingtheorm.md)

# Object Oriented Programming with Python

The dry definition of Object Oriented Programming (OOP) is that it is a way of
visualising data and functions as single, encapsulated units.

An object in Python is an instance of a class. A class is a blueprint for an
object. A class is defined using the `class` keyword. The class definition is a
block of code that defines the attributes and methods of the class. The
attributes are the data that the object holds. The methods are the functions
that the object can perform.

Here is an example of a simple class definition:

```python
# the class keyword introduces an object template or blueprint and names it.
class Dog:
    # the __init__ method is a special method that is called when an object is created.
    # it is used to initialise the object's attributes (data).
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

    # a method is a function that is defined inside a class.
    # it can be called on an object of the class by using the dot operator.
    def bark(self):
        print("Woof!")
```

The `Dog` class has two attributes, `name` and `breed`, and one method, `bark`.
It can be used thus:

```python
# create an instance of the Dog class
my_dog = Dog("Fido", "Labrador")
# make it bark
my_dog.bark()
```

What makes OOP so useful is that it allows us to model real-world entities in
code. Also, we can create multiple instances of a class, each with its own data.

In the example above, we created an instance of the `Dog` class called `my_dog`
that was named "Fido" and was a Labrador. We could create another instance of
the class named "Fred" and make it a Poodle.

```python
another_dog = Dog("Fred", "Poodle")
# this will return True
assert another_dog.name == "Fred"
# this will return False
assert another_dog == my_dog
# True again
assert isinstance(another_dog, Dog)
```

We can also create sub-classes for objects that are closely related but have
some differences. For example we can create a Vehicle class and then create a
Car class that inherits from Vehicle.

```python
# Base class for all vehicles
class Vehicle:
    def __init__(self, number_of_wheels, powered_by, colour):
        self.number_of_wheels = number_of_wheels
        self.powered_by = powered_by
        self.colour = colour

    def goBabe(self):
        print("Vroom!")

# Sub-class of Vehicle
class PetrolCar(Vehicle):
    def __init__(self, colour):
        # call the __init__ method of the base class
        super().__init__(4, "petrol engine", colour)

class ElectricCar(Vehicle):
    def __init__(self, colour):
        super().__init__(4, "electric motor", colour)

    def goBabe(self):
        print("Whine!")

class Bicycle(Vehicle):
    def __init__(self, colour):
        super().__init__(2, "human power", colour)

    def goBabe(self):
        print("Puff!")
```

As you can see the petrol car object doesn't have a `goBabe` method so will use
the default one from the base class.

All objects created from these classes will have the `number_of_wheels`,
`powered_by` and `colour` attributes.

We could have gone even deeper by creating a `Car` class that inherits from
`Vehicle` and then creating `PetrolCar` and `ElectricCar` classes that inherit
from `Car`, however, sub-classes of sub-classes can get complicated and just
does my head in, so I avoid them if I can.

We'll now carry on with the findaccount problem and create our database.

## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)
1. Object Oriented Programming with Python
1. [Using the ORM](usingtheorm.md)
