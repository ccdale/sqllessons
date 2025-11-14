## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
2. [Data Organisation](dataorg.md)
3. Relationships
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)
1. [Using the ORM](usingtheorm.md)

# Relationships

What makes a relational database so powerful is the ability to define
relationships between tables. This allows us to store data in a normalised form,
which reduces redundancy and improves data integrity.

## Normalised Data

Normalised data is data that is stored in a way that reduces redundancy.

### First Normal Form (1NF)

A table is in First Normal Form if:
    * the data it holds is the *only* copy of that data


## Ignore the above

So Database Administration, like Finance is one of the worlds most boring jobs.
Therefore, the people that do it like to obfuscate their functions as much as
possible - hence 'normalisation'.

Basically, only store one copy of any particular piece of data. This will become
clear shortly when we start to look at the relationship between an AWS Account
and it's Tags.

## Relationships 2

This is a JSON representation of the experiment AWS account. Most of the
information can be easily translated into a table, but some of the Tags have
more than one piece of information in them.


```json
    {
        "Id": "674045653330",
        "Arn":
"arn:aws:organizations::123456789012:account/o-abcdef123/234567890123",
        "Email": "a.made@up.email",
        "Name": "experiment",
        "Status": "ACTIVE",
        "JoinedMethod": "INVITED",
        "JoinedTimestamp": 1,
        "Tags": [
            {
                "Key": "Manager",
                "Value": "b.made@up.email c.made@up.email d.made@up.email"
            },
            {
                "Key": "AccountGroup",
                "Value": "Dev"
            },
            {
                "Key": "ServiceName",
                "Value": "AMAZON WEB SERVICES"
            },
            {
                "Key": "CostCode",
                "Value": "1234567"
            },
            {
                "Key": "Environment",
                "Value": "Development"
            },
            {
                "Key": "Budget",
                "Value": "50"
            },
            {
                "Key": "LTOwner",
                "Value": "e.maid@up.email"
            },
            {
                "Key": "AccountBU",
                "Value": "Dev"
            },
            {
                "Key": "AccountOwner",
                "Value": "b.made@up.email"
            }
        ],
        "Owner": "b.made@up.email",
        "Managers": [
            "d.made@up.email",
            "c.made@up.email",
            "b.made@up.email"
        ],
        "Groups": [
            "Developer",
            "Readonly",
            "Administrator"
        ],
        "Number": "234567890123",
        "Payer": "Mr.Bill.Payer"
    },
```

To effectively and efficiently store the Tags that have more than one item we
can create 2 new tables, one for the data and one as a 'map' between the data
and the account data.

This will then setup a relationship between all 3 tables. If we go back to our
pseudo table creation statements from the previous lesson, we can add the new
tables in and create the relationships.

```sql
DROP TABLE if exists accounts;
CREATE TABLE accounts (
    account_number TEXT PRIMARY KEY,
    account_name TEXT
) WITHOUT ROWID;

DROP TABLE if exists people;
CREATE TABLE people (
    email TEXT PRIMARY KEY,
    name TEXT
) WITHOUT ROWID;

CREATE TABLE manmap (
    account_number TEXT,
    email TEXT,
    PRIMARY KEY (account_number, email),
    FOREIGN KEY (account_number) REFERENCES accounts(account_number),
    FOREIGN KEY (email) REFERENCES people(email)
) WITHOUT ROWID;
```

* PRIMARY KEY (account_number, email) - this is a composite primary key, which
  means that the combination of the two columns is unique and creates a fast
  searchable index using those fields.
* FOREIGN KEY (account_number) REFERENCES accounts(account_number) - this
  creates a relationship between the `manmap` table and the `accounts` table.
  It means that the `account_number` in the `manmap` table must exist in the
  `accounts` table.
* FOREIGN KEY (email) REFERENCES people(email) - this creates a relationship between
  the `manmap` table and the `people` table. It means that the `email` in the
  `manmap` table must exist in the `people` table.
* The `manmap` table is the 'map' between the `accounts` and the people in the
  SSO Mangers tag.

## Hold on a minute

You said that Normalisation was the practice of only having one copy of any
piece of data. In the schema above the email address is used in 2 tables.

We need to normalise that data further. If we remove the `WITHOUT ROWID` from
the table definition, sqlite will automatically create a rowid for each row in
the table or we can explicitly create one. We can then reference that rowid in
the manmap table.

```sql
DROP TABLE if exists accounts;
CREATE TABLE accounts (
    account_number TEXT PRIMARY KEY,
    account_name TEXT
);

DROP TABLE if exists people;
CREATE TABLE people (
    rowid INTEGER PRIMARY KEY,
    email TEXT,
    name TEXT
);

DROP TABLE if exists manmap;
CREATE TABLE manmap (
    account_number TEXT,
    person_id INTEGER,
    PRIMARY KEY (account_number, person_id),
    FOREIGN KEY (account_number) REFERENCES accounts(account_number),
    FOREIGN KEY (person_id) REFERENCES people(rowid)
);
```

## Joining the Data

Now that we have the data in 3 tables, we can join the data together to get the information we need.

This is the DBA recommended way to join the data to ensure that they keep their
jobs and obfuscate their functions as much as possible:

```sql
SELECT accounts.account_number, accounts.account_name, people.email, people.name
FROM accounts JOIN manmap ON accounts.account_number = manmap.account_number JOIN people ON manmap.person_id = people.rowid;
```

However, `JOIN` isn't always necessary for simple queries, we can use the `WHERE` clause to join the data:

```sql
SELECT
  accounts.account_number, accounts.account_name, people.email, people.name
FROM
  accounts, manmap, people
WHERE
  accounts.account_number = manmap.account_number
AND
  manmap.person_id = people.rowid;
```

So the format of the SELECT statement is:

* `SELECT` - the columns you want to return
* `FROM` - the tables you want to get the data from
* `WHERE` - the conditions that need to be met to return the data


## Aliasing the Output

When we run the above query, the output will have the column names as the table
name and the column name. This can be a bit confusing when you have multiple
tables with the same column names. Therefore, we can alias the output columns to
friendly names.

```sql
SELECT
  accounts.account_number AS account_number,
  accounts.account_name AS account_name,
  people.email AS email,
  people.name AS name
FROM
    accounts, manmap, people
WHERE
    accounts.account_number = manmap.account_number
AND
  manmap.person_id = people.rowid;
```

The `AS` keyword is used to alias the column name to a more friendly name. You
can also use the `AS` keyword to alias the table name (with `mysql` you don't
even need to use the `AS`). Depending on your use case this may be simpler or
indeed more confusing!

```sql
SELECT
  a.account_number AS account_number,
  a.account_name AS account_name,
  p.email AS email,
  p.name AS name
FROM
    accounts AS a, manmap AS s, people AS p
WHERE
    a.account_number = s.account_number
AND
  s.person_id = p.rowid;
```


## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
2. [Data Organisation](dataorg.md)
3. Relationships
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)
1. [Using the ORM](usingtheorm.md)
