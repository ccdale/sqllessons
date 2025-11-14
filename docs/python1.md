## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. Accessing Data with Python
1. [ORM with SQLAlchemy](orm.md)
1. [Using the ORM](usingtheorm.md)

# Accessing Data with Python

Python has support for sqlite3 in the standard library. This means that you can
use Python to interact with a SQLite database without needing to install any
additional libraries.


## Typical Workflow

* Connect to the database
* Construct an SQL statement by concatenating strings
* Execute the SQL statement
* Fetch the results
* Close the connection

Take a look at the file [db.py](../jestbd/db.py) for a simple implementation of
a hand-written SQL class to interact with a SQLite database. It is simple but
not very user-friendly. You'll still have to construct SQL statements in the
code before passing them in - see [test_db.py](../tests/test_db.py) for what I
mean.  There are better ways to do this so we won't spend time on this class.

## Object Oriented Programming

As python is an object-oriented language there are many, many libraries
dedicated to making it easier to interact with databases. We will be using the
`sqlalchemy` library to interact with our SQLite database in an object oriented
way. The main feature of `sqlalchemy` is the `session` object which allows you
to interact with the database in a more pythonic way using it's Object
Relational Mapping (ORM) features (gobbledegook I know, but hopefully, all will
become clear).

## ORM

Object Relational Mapping (ORM) is a programming technique for converting data
in tabular form (i.e. that from a database, csv files etc.) into objects that
can be used in code (i.e. python dictionaries, lists, etc.). This allows you to
interact with a database in a more programmatical way.

## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. Accessing Data with Python
1. [ORM with SQLAlchemy](orm.md)
1. [Using the ORM](usingtheorm.md)
