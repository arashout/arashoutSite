---
layout: post
title: Accidental Static Class Variables
permalink: posts/accidental-static-class-variables
date: 2018-03-20 00:00:00 +0000
tags: 
- Python
---

Using default argument parameters on the `__init__` method on your class will create shared class variables instead of instance variables as you might expect!
It is best demonstrated through some code snippets below:

1. Normal/Expected behavior when we use immutable primitives as default parameters

```python3
class Foo:
    def __init__(self, bar = 3):
        self.bar = bar

f1 = Foo()
print(f1.bar) # 3
f2 = Foo(4)
print(f2.bar) # 4
print(f1.bar) # 3

```

2. Un-intuitive behavior when we use mutable structures as default parameters

```python3
class Foo:
    def __init__(self, bar_list = []):
        self.bar_list = bar_list

f1 = Foo([2, 2] )
print(f1.bar_list) # [2, 2]
f1.bar.append(4)
print(f1.bar) # [2, 2, 4]
f2 = Foo()
print(f2.bar) # [2, 2, 4] <- Wait what? Should this not be []
```

So it turns that putting [mutable default parameters in the class constructor is not a good idea](https://stackoverflow.com/questions/4841782/python-constructor-and-default-value)

The reason behind this is the Python interpreter reads through your class definition only once.        
Which means that the SAME mutable default parameter will be used by reference every time you instantiate a new instance of your class.   