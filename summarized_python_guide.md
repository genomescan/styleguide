# Summarized python guideline

1. Function names and parameters should be as clear as possible
2. Docstrings should be a single line explaining the meaning of a function or class. If input parameters are unclear you can explain them as well. Avoid explaining the type in the doctring, this should be clear from the provided typing (4). Try to avoid AI to generate doctrings, keep it consise and to the point
```

def create_chees_factory(flavor: str, size: int) -> CheeseFactory:
    """Creates a CheesFactory instance based on the provided flavor

    size: size in square meters
    """
    pass

```
3. Classes and functions should have a single responsibility and not have side-effects
4. Try to use typing as much as possibe for input and output arguments. This can be either standard typing or with the help of the Typing module.
```
# standard
def delete_keys(dct: dict[str, str], keys: list[str]):
    pass


from typing import List, Dict


# with typing module
def delete_keys2(dct: Dict[str, str], keys: List[str]):
   pass

```
5. Functions or classes within modules that are not supposed to be accessed outside the context of the class of module should be prefixed with an underscore. If a function should really not be used, because it permanently alters the state of the module for instance and you dont want somebody accidentally calling it, use a double underscore
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
