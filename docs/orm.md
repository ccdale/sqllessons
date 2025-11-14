## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. ORM with SQLAlchemy
1. [Using the ORM](usingtheorm.md)

# ORM with SQLAlchemy

SQLAlchemy is an Object Relational Mapper (ORM) that allows you to interact with
a database using Python objects. It is a powerful tool that can simplify the
need to write SQL by hand.

## The Model

You define the tables of a database by creating classes that inherit
from the `Base` class of SQLAlchemy. This `Base` class is a template for
creating tables in the database. `declarative_base()` is a function that returns
the `Base` class.

```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class Account(Base):
    __tablename__ = 'accounts'

    id = Column(String, primary_key)
    name = Column(String)
    shortname = Column(String, nullable=True)
    md5 = Column(String, nullable=False)
    accountownerid = Column(Integer, ForeignKey('people.id'), nullable=True)
    ltownerid = Column(Integer, ForeignKey('people.id'), nullable=True)
    payerid = Column(Integer, ForeignKey('payers.id'), nullable=False)
    environmentid = Column(Integer, ForeignKey('environments.id'), nullable=False)

    __repr__(self):
        return f'<Account(id={self.id}, name={self.name})>'


class People(Base):
    __tablename__ = 'people'

    id = Column(Integer, primary_key)
    email = Column(String)

    __repr__(self):
        return f'<People(id={self.id}, email={self.email})>'
```

If you examine the definition of the Accounts Model, you will see that it has
some items that are not in the table definition. These are the relationships and
are declared as ForeignKeys.

When we use this we'll want to know if any of the source data (from AWS) has
changed or not. So we don't have to check every single account and relationship
we can use a hash of the data to quickly compare the data from the last time.
This is the `md5` field.

As some of the data is not necessary to be present and because of the single
relationship foreign keys we need to write a `create` function to give
sqlalchemy hints on how the table is constructed and related to others.

```python

class Account(Base):
    __tablename__ = 'accounts'

    id = Column(String, primary_key)
    name = Column(String)
    shortname = Column(String, nullable=True)
    md5 = Column(String, nullable=False)
    accountownerid = Column(Integer, ForeignKey('people.id'), nullable=True)
    ltownerid = Column(Integer, ForeignKey('people.id'), nullable=True)
    payerid = Column(Integer, ForeignKey('payers.id'), nullable=False)
    environmentid = Column(Integer, ForeignKey('environments.id'), nullable=False)

    def __repr__(self):
        return f'<Account(id={self.id}, name={self.name})>'

    def create(self, name, accountid, md5,
                shortname=None, ltowner=None, accountowner=None,
                payerid=None, environmentid=None):
        self.name = name
        self.id = accountid
        self.md5 = md5
        self.shortname = shortname
        self.ltownerid = ltowner
        self.accountownerid = accountowner
        self.payerid = payerid
        self.environmentid = environmentid

```



## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. ORM with SQLAlchemy
1. [Using the ORM](usingtheorm.md)
