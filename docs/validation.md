## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. Data Validation
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)
1. [Using the ORM](usingtheorm.md)

# Data Validation

It is important that the data that is stored in the database is valid. This
means that the data is in the correct format and type and that it is within the
expected bounds. You need to protect your database from bad data including
incorrect data, [SQL injection
attacks](https://en.wikipedia.org/wiki/SQL_injection), and other malicious data.

i.e. if you are storing a number then it should be a number, not a string. If
you are storing an email address, then it should be in the correct format.
Strings should be checked for length and content.

In SQLite, you can use the `CHECK` constraint to ensure that the data is valid.
On fully featured databases, you can use triggers and stored procedures to
check.

However, the best way to ensure that the data is valid is to validate it before
it is inserted into the database. This can be done in the application code and
that is what we will do here.

## Data Validation in Python

Referring back to the JSON data for an AWS Account you'll see that there is very
little validation that will be required; by the time the data has been saved to
this JSON format it will have been validated by the AWS API. We will, however,
ensure that the data is in the correct format before we save it to the database,
and that it is within the expected bounds.

### Data Validation Functions

We will create a number of functions to validate the data. These functions will
all return a boolean value, `True` if the data is valid and `False` if it is
not.

We will use this `^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$` regular
expression to detect and validate an email address:


| RE | Description |
|----|-------------|
| ^  | Start of string |
| [a-zA-Z0-9_.+-]+ | One or more of lower/upper case letters, numbers, underscore, dot, a plus sign and hyphen |
| @ | The at symbol |
| [a-zA-Z0-9-]+ | One or more of lower/upper case letters, numbers and hyphen |
| \\. | A dot |
| [a-zA-Z0-9-.]+ | One or more of lower/upper case letters, numbers, dot and hyphen |
| $ | End of string |


```python
def validate_string(value: str, min_length: int, max_length: int) -> bool:
    """A generic string validator"""
    if isinstance(value, str):
        strlen = len(value)
        if min_length <= strlen and strlen <= max_length:
            return True
    return False

def validate_awsaccountid(value: str) -> bool:
    """Validate an AWS Account ID

    An AWS Account ID is a 12 digit number stored as a string
    """
    if validate_string(value, 12, 12) and value.idigit():
        return True
    return False

def validate_email(value: str) -> bool:
    """Validate an email address

    This is a simple email address validator. It is not perfect but it will
    catch most invalid email addresses. It utilises regular expressions to check
    the validity of the email address.

    """

    import re

    emailre = re.compile(r'^[a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$')
    if validate_string(value, 5, 255) and emailre.match(value):
        return True
    return False
```

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



## Contents

1. [Index / README](../README.md)
1. [The findaccount Problem](docs/thefindaccountprob.md)
1. [Data Organisation](dataorg.md)
1. [Relationships](relationships.md)
1. Data Validation
1. [Accessing Data with Python](python1.md)
1. [ORM with SQLAlchemy](orm.md)
1. [Using the ORM](usingtheorm.md)

