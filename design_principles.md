# Code design principles

1. ***Function names and parameters should be as clear as possible.*** Try and avoid i and j use instead index1 and index2 for instance
2. ***Docstrings should be a single line explaining the meaning of a function or class.*** If input parameters are unclear you can explain them as well. Avoid explaining the type in the doctring, this should be clear from the provided typing (4). Try to avoid AI to generate doctrings, keep it consise and to the point
```

def create_chees_factory(flavor: str, size: int) -> CheeseFactory:
    """Creates a CheesFactory instance based on the provided flavor

    size: size in square meters
    """
    ...

```
3. ***Classes and functions should have a single responsibility as much as possible.*** This makes them more extensible and easier to understand. To illustrate this we have the example below; Here we have a protocol for sending an email, we provide the email handler with an instance of our email class and it will send then store the email. Our email_handler does not have a single responsibility in this case but that can be unavoidable at times. Our three classes; Email, EmailSender and EmailStorer all do have a single responsibility. Each of these Clases can now easily be subclassed or extended in the future. You might want to add a function to also store the emails as text files on the file system, this can now easily be added to the EmailStorer class.
```

class Email:
    """Creates an email
    """
    ...


class EmailSender:
    """Sends provided emails
    """
    ...


class EmailStorer:
    """Store provided emails in a database
    """
    ...


def email_handler(email: Email):
    # this function has multiple responsibilities, but that is unavoidable at times
    sender = EmailSender()
    storer = EmailStorer()
    sender.send(email)
    storer.store(email)

```
4. ***Use typing as much as possibe for input and output arguments.*** This can be either standard typing or with the help of the Typing module. The typing module is more flexible and has a number of things you can use for more complex situations
```
# standard
def delete_keys(dct: dict[str, str], keys: list[str]):
    ...


from typing import List, Dict


# with typing module
def delete_keys2(dct: Dict[str, str], keys: List[str]):
   ...

Addable = TypeVar['Addable']

# use of generic type -> this function takes and returns the same type. The type itself can be dynamic
def sum(x: Addable, y: Addable) -> Addable:
    ...

```
5. ***Functions or classes within modules that are not part of the API of that module should be protected or private.*** In python this can be (sort of) accomplished with the help of one or 2 starting underscores.
```

def create_butter() -> Butter:
    # both these functions are not important to the person trying to create butter
    # they are prefaced with an underscore to indicate that they should not be used outside this module
    clump_of_butter = _churn_milk()
    return _package_butter(clump_of_butter)


def __dont_use_me():
    # this function should never be called from outside this module
    pass

```
6. ***Keep code readable by avoiding 1 line syntax*** Try to avoid the use of list comprehensions, multiple statements on a single line (using ;) or unindented if statements. The exceptions are tertiary statements and simple one loop list comprehensions.
```

# this list comprehension is fine
_100_range = range(0, 100)
list_x = [val for val in _100_range]

# this is not fine
_100_range_again =  range(0, 100)
list_y = [inner_val for val in _100_range_again for inner_val in range(val)]


# this tertiary operation is fine
input_name = None
name = input_name if input_name is not None else ''

# or this one
name = input_name or ''

```
7. ***Avoid reusing variables*** Meaning that you don't overwrite an existing variable with something new because you wont need the old variable anymore. This makes code hard and error prone to refactor
```

row = ["a", "b", "c"]
print(row)
row_matrix = [["g", "b", "j"],
              ["n", "h", "k"], 
              ["m", "l", "s"]]

# do not do this! We now overwrite the old row variable. Either rename the row above or change the name in the loop
for row in row_matrix:
    ...


# Also avoid this scenario. We have row defined in the outer scope, and now we overwrite it within the function
def create_row() -> List[str]:
    row = ['t']
    return row


# finally avoid this scenario. Here we overwrite a builtin method
list = []

```
