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
4. ***Use typing as much as possibe for input and output arguments.*** This can be either standard typing or with the help of the Typing module.
```
# standard
def delete_keys(dct: dict[str, str], keys: list[str]):
    ...


from typing import List, Dict


# with typing module
def delete_keys2(dct: Dict[str, str], keys: List[str]):
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
