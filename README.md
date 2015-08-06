[![Build Status](https://scrutinizer-ci.com/g/vasili-v/sherbet/badges/build.png?b=master)](https://scrutinizer-ci.com/g/vasili-v/sherbet/build-status/master) [![Code Coverage](https://scrutinizer-ci.com/g/vasili-v/sherbet/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/vasili-v/sherbet/?branch=master)
# sherbet
Sherbet library automates word inflection to support plurals

The library is a superset for curly braces formatting. Any format suitable for str.format also works for sherbet. Additionally the library supports automatic plural froms.

Default substitution:
```python
>>> sherbet.sweeten("{x} file{x_plural}.", x=1)
"1 file."
>>> sherbet.sweeten("{x} file{x_plural}.", x=5)
"5 files."
```

Simple substitution for plural:
```python
>>> sherbet.sweeten("{x} box{x_plural<es>}.", x=1)
"1 file."
>>> sherbet.sweeten("{x} box{x_plural<es>}.", x=5)
"5 files."
```

Alternative substitution:
```python
>>> sherbet.sweeten("{x} file{x_plural} ha{x_plural<ve|s>} been deleted.", x=1)
"1 file has been deleted."
>>> sherbet.sweeten("{x} file{x_plural} ha{x_plural<ve|s>} been deleted.", x=5)
"5 files have been deleted."
```
Note that option for plural form goes first. Also options delemitted by "<|>" shouldn't contain symbols "|" and ">". No escaping supproted.

Substitution for tuples, sets and lists:
```python
>>> sherbet.sweeten("{x} file{x_plural} ha{x_plural<ve|s>} been deleted.", x=["one"])
"one file has been deleted."
>>> sherbet.sweeten("{x} file{x_plural} ha{x_plural<ve|s>} been deleted.", x=["one", "two", "three"])
'"one", "two" and "three" files have been deleted.'
>>> sherbet.sweeten("{x!r} file{x_plural} ha{x_plural<ve|s>} been deleted.", x=["one", "two", "three"])
"['one', 'two', 'three'] files have been deleted."
```

More examples:
```python
>>> sherbet.sweeten("{} file{0_plural} ha{0_plural<ve|s>} been deleted.", ["one"])
"one file has been deleted."
>>> sherbet.sweeten("{} file{0_plural} ha{0_plural<ve|s>} been deleted. Again: {0}!", ["one", "two", "three"])
'"one", "two" and "three" files have been deleted. Again: "one", "two" and "three"!'
>>> sherbet.sweeten("{x!s: >5s} file{x_plural: <5s} ha{x_plural<ve|s>} been deleted.", x=10)
"   10 files     have been deleted."
```

And more:
```python
>>> class MyBaseException(Exception):
...     def __init__(self, **kwargs):
...         super(MyBaseException, self).__init__(sherbet.sweeten(self.template, **kwargs))
...
>>> class MyException(MyBaseException):
...     template = "{n} error{n_plural} ha{n_plural<ve|s>} been detected."
...
>>> raise MyException(n=5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
__main__.MyException: 5 errors have been detected.
```
