+++
title = "Is string formatting in Python safe to use or not?"
date = "2023-03-02"
draft = false
tags = ['python']
+++

The mighty and powerful f-strings. Well, they are only a sugar syntax on string.format, but whatever. Here I will look at one of the usage scenarios that you should not commit - taking format string from the user. Well, I had to do it in my project, so...

<!--more-->

#### Easy, what is an f-string? 
Literal string prefixed with *f* with expression inside curly braces.

```python
name = 'Pawel'
print(f"My name is {name}")
```

#### Next level, f-strings:
* are evaluated at runtime
* are parsed into literal strings and expressions
* use the same format specifier mini-language as *str.format*
* has full access to local and global variables
* can run any valid Python expression, including function and method calls (ast.parse etc.)
* can use lambdas.. game over.

#### Security issues?

Some prominent Python community personalities-celebrities observed that The F's could be misused. That is a fact. There are a few ways to do it:
* denial of service attack by a large string size
* using F's in logging in wrong way ([read me][floggin])
* accessing attributes within an object, especially if it holds some kind of secrets (web dev)
* in general, user-supplied format string could inject unwanted code

Try that in your interpreter:
```python
f"Goodbye! {exit()}"
$ BOOM

# on the other hand this will not work when using string.format

"{exit()}".format(exit()=1) # SyntaxError
"{exit()}".format(exit=1) # KeyError
"{exit()}".format(foo="bar") # KeyError
```

So is this a problem? 

Yes and no. Depends on the context and place where it is executed.

```python
In [35]: f"{self.__init__}"
NameError: name 'self' is not defined

In [36]: "{self.__init__}".format()
KeyError: 'self'
```

#### My application

I had a service that would read configuration files created by another team, and the clue was that they could put an array in yaml in such a form: *key: *"{value}"*, where *{value}* would be used later for dynamic lookup in some data. Thanks to that I would not have to know the keys and values beforehand in my code.

```bash
# conf.yaml
some-data:
 - key: "{value}"
 - foo: "{another} {self.secret} {exit()} {locals()}"
```

```python
#app.py
from string import Formatter

formatter = Formatter()

resolved_fields_dict = dict()
external_data_source = dict(foo=1)

user_format_string = "{foo} {bar}" # This comes from conf.yaml

# Extract field from format string
fields = [field for _, field, _, _ in formatter.parse(user_format_string) if field]

# In: fields
# Out: ['foo', 'bar']

# Update my dict from external data if exists, otherwise use the default
for field in fields:
    resolved_fields_dict[field] = external_data_source.get(field, "Not Found")

# In: resolved_fields_dict
# Out: {'foo': 1, 'bar': 'Not Found'}

# Finally return
return user_format_string.format(**resolved_fields_dict)
# Out: '1 Not Found'
```

And while I am not afraid the other team would want to hack me, they could inject by mistake some bad input and make my Python throw various errors.

#### Solution

I used a customised string.Formatter in yaml config parser with overrided *get_field* method. This is based on an example found in [stackoverflow][fsafe]:

```python
from string import Formatter

class SafeFormatter(Formatter):

    def get_field(self, field_name, args, kwargs):
        if '.' in field_name or '[' in field_name:
            raise InsecureStringFormatError('Invalid format string.')
        return super().get_field(field_name,args,kwargs)

formatter = SafeFormatter()
try:
    formatter.format("{sys.exit()"}, num=1, id='hello')
except InsecureStringFormatError:
    print("Hack the world")
```

Now we can catch and handle bad input early. 

#### Conclusions 

* Do not take format string from the user, and if you have to then do not use f-strings
* string.Formatter could be used to create custom validations

---

Resources:

* [PEP 498 – Literal String Interpolation][pep0498]
* [Discourage logging f-strings due to security considerations][floggin]
* [[SO] Can Python's string .format() be made safe for untrusted format strings][fsafe]

[pep0498]: https://peps.python.org/pep-0498/
[floggin]: https://github.com/python/cpython/issues/90358
[fsafe]: https://stackoverflow.com/questions/15356649/can-pythons-string-format-be-made-safe-for-untrusted-format-strings