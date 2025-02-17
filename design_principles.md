# Code design principles
This document outlines a number of design principles for readable and extendable code. Most principles apply to any language, but some are more python/non-typed language specific. These are guidelines and not rules that should be strictly enforced. If you are making a quick script you don't have to think about these guidelines, but if that script later grows out to something bigger then you should revisit and refactor it. Lastly, don't be afraid to refactor code, almost nobody can write the perfect code on the first go. Also the extra time it takes to rewrite is often saved in the future when you inevitably have to extend it.

1. ***Function names and parameters should be as clear as possible.*** Names should be descriptive of what it contains. Not of the type. Single letter variables should be avoided, an exception can be an i indicating in index in a loop. 
```

# use of a single character in a simple (6!!) comprehension is fine
name_sample_map = {s.name: s for s in samples}

# we use i here for index which is fine too, but instead of k and v we use name and sample here to make it clearer
#  what the key and value are from this map
for i, (name, sample) in enumerate(name_map):
    ...

```
2. ***Docstrings should start with a single line explaining the meaning of a function or class.*** If you need more space you can always add a double enter with a larger explanation. The main point is that users should be able to read quickly what a function is about. If you cannot explain it in one line the function might be too long (3). If input parameters need more clarification you can explain them after the explantion with a variable colon explanation. Avoid explaining the type in the doctring, this should be clear from the provided typing (4). Try to avoid AI to generate doctrings, keep it consise and to the point. 
```

def create_chees_factory(flavor: str, size: int) -> CheeseFactory:
    """Creates a CheesFactory instance based on the provided flavor

    This factory produces different cheeses but will not cover all options. If a provided option is not
    present a NotImplementedError is raised

    size: size in square meters
    """
    ...

```
3. ***Classes and functions should have a single responsibility as much as possible.*** This makes them more extensible and easier to understand. To illustrate this we have the example below; Here we have a protocol for sending an email, we provide the email handler with an instance of our email class and it will send then store the email. Our email_handler does not have a single responsibility in this case but that can be unavoidable at times. Our three classes; Email, EmailSender and EmailStorer all do have a single responsibility. Each of these Clases can now easily be subclassed or extended in the future. You might want to add a function to also store the emails as text files on the file system, this can now easily be added to the EmailStorer class. When a function is longer then what fits on your screen it often has more then a single responsibility and should be split up.
```

# class example
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
5. ***Functions or classes within modules that are not part of the API of that module should be protected or private.*** This prevents other developers from using code not meant to be used outside the intended scope. In python this can be (sort of) accomplished with the help of one or 2 starting underscores.
```

def create_butter() -> Butter:
    # both these functions are not important to the person trying to create butter
    # they are prefaced with an underscore to indicate that they should not be used outside this module
    clump_of_butter = _churn_milk()
    return _package_butter(clump_of_butter)

...


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
7. ***Avoid reusing variables*** Meaning that you don't overwrite an existing variable with something new because you wont need the old variable anymore. This makes code hard to read and error prone to refactor. This also goes for builtin functions or variables in a higher scope. You can make an exception if you need to reuse a very memory heavy already allocated variable, since it can speed up code
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
8. ***Do not duplicate code*** It is a very easy way to introduce bugs at some future point by forgetting to change both parts. Duplicating a couple lines is fine, but more than that means that you will have to extract a function and call that instead.
```

# Here we duplicate a piece of code twice with the only change being the command that we input
process = subprocess.run(['ls', '-lha'], stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
text = process.stdout.decode("utf-8")
if process.returncode != 0:
    raise RuntimeError(f"Failed command with message: {text}")

process2 = subprocess.run(['du', '-sh'], stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
text2 = process.stdout.decode("utf-8")
if process.returncode != 0:
    raise RuntimeError(f"Failed command with message: {text2}")


# instead it makes more sense to extract a function from this like so

def run_command(command: List[str]) -> str:
    process = subprocess.run(command, stdout=subprocess.PIPE, stderr=subprocess.STDOUT)
    text = process.stdout.decode("utf-8")
    if process.returncode != 0:
        raise RuntimeError(f"Failed command with message: {text}")
    return text

# then we can run it like this
run_command(["ls", "-lha"])
run_command(["du", "-sh"])

```
9. ***Use a consistent code style*** For python this would simply be the [pep 8 styleguide](https://peps.python.org/pep-0008/). This means variable names in snake case (everything\_with\_underscores) and class names in camel case (EachWordStartsWithACapital).
10. ***Make use of a command line parser packages*** For each language agree on a command line parser package that preferably resembles other languages in syntax. This allows people to get familiar with a certain way of doing command line interfaces
