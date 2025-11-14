## Contents

1. [Index / README](../README.md)
1. The findaccount Problem
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)

# The findaccount Problem

If we look at the steps needed to find any specific account in the SSO system,
we see it is a simple but laborious process. We need to:

* download the gzipped JSON file for each payer
* unzip each file
* combine each file into a single python dictionary
* search both the Id and the Name fields for the account we are looking for
  stepping through each account in the dictionary


If we had a database of accounts, we could simply query the database for the
required account with a single SQL statement. This would be much faster as the
database engine is optimised for searching and retrieving data.

```sql
SELECT * FROM account WHERE account_id LIKE '%1234%' OR account_name LIKE '%myaccount%';
```


## Contents

1. [Index / README](../README.md)
1. The findaccount Problem
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. [Data Validation](validation.md)
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)
