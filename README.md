# Just Enough SQL to be <strike>Dangerous</strike> Useful

A few lessons in basic SQL to get you started.

We will learn the basic SQL statements for CRUD (Create, Retrieve, Update,
Delete) operations:

* SELECT - retrieve data
* INSERT - insert data
* UPDATE - change data
* DELETE - delete data

We'll be building a database of AWS Account Information to work with and explore
a few of the ways of accessing and manipulating the data from the CLI, a GUI and
python.

## Prerequisites

* Common sense
* Logical understanding
* A text editor (VS Code, PyCharm, vim, nano, notepad++ etc. but not emacs)
* SQLite installed on your machine
    * Windows: search google
    * OSX: already installed with the OS
* DB Browser for SQLite
    * Windows: search google
    * OSX: `brew install db-browser-for-sqlite`
* Python 3.11 or higher
    * Windows: search google
    * OSX: already installed with the OS
* GNU Make
    * Windows: I have absolutely no idea (yet), also, I'm not sure if it's
      necessary, we can probably just run the commands manually.
    * OSX: `brew install make`
        * Note: you may need to install the Xcode Command Line Tools first and
          add the `gnubin` directory to your path.
* A terminal
    * Windows: try `cmd` or `powershell`, `warp` preferred
    * OSX: `Terminal.app` or `iTerm2`


## Contents

1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](docs/dataorg.md)
1. [Relationships](docs/relationships.md)
1. [Data Validation](docs/validation.md)
1. [Accessing Data with Python](docs/python1.md)
1. [ORM with SQLAlchemy](docs/orm.md)
1. [Using the ORM](usingtheorm.md)
