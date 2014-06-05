<!--

http://stackoverflow.com/questions/364802/generator-comprehension
https://wiki.python.org/moin/Generators
http://stackoverflow.com/questions/4260280/python-if-else-in-list-comprehension

nested list comprehensions:
http://stackoverflow.com/questions/8049798/understanding-nested-list-comprehension

Formatting:
http://stackoverflow.com/questions/5809059/line-continuation-for-list-comprehensions-or-generator-expressions-in-python

Elegant code:
http://ask.slashdot.org/story/14/03/26/0116227/ask-slashdot-what-do-you-consider-elegant-code
-->



# List Comprehensions and<br>Generator Expressions<br><p>Jóan Petur Petersen</p>

---

# Presentations Layout

* List comprehensions basics
* Generator expressions
* List comprehensions continued

---

# List Comprehensions

---

# List Comprehensions Why?


<br>
List comprehensions:

* Provide a syntax making a list from list
* Simpler code
    * Maybe one line
    * More readable
    * Easier to understand


---

# Simple example

Traditional code:

    !python,linenos=no
    old_list = range(4)
    new_list = []
    for n in old_list:
        new_list.append(n**2)

with a list comprehension this can be rewritten as:

    !python,linenos=no
    old_list = range(4)
    new_list = [n**2 for n in old_list]

---

# List comprehension syntax

a simplification of the full syntax:

    !python,linenos=no
    [expression for variable in iterable]
    
* `expression` describes how the elements in the new list are formed.
* `variable` the name of the variable that represent each element in
`iterable`.
* iterate over items in `iterable`.

---

# Tuple Unpacking

The `variable` part of the list comprehension does not have to be a single variable.

    !python,linenos=no
    >>> d = {'kyle': 3, 'stan': 5, 'eric': 0, 'kenny': 7}
    >>> d.items()
    [('stan', 5), ('kenny', 7), ('kyle', 3), ('eric', 0)]
    >>> ["{0} = {1}".format(k, v) for k, v in d.items()]
    ['stan = 5', 'kenny = 7', 'kyle = 3', 'eric = 0']

---

# List Comprehension Filter

an optional filter can be also be added to the list comprehension:

    !python,linenos=no
    [expression for variable in iterable if condition]

The equivalent code:

    !python,linenos=no
    new_list = []
    for variable in iterable:
        if condition:
            new_list.append(expression)

---

# Filter example

    !python,linenos=no
    >>> d = {'kyle': 3, 'stan': 5, 'eric': 0, 'kenny': 7}
    [('stan', 5), ('kenny', 7), ('kyle', 3), ('eric', 0)]
    >>> ["%s = %d" % (k, v) for k, v in d.items() if v>0]
    ['stan = 5', 'kenny = 7', 'kyle = 3']

---

# Not to be confused with...

The filter `if` is only for selecting elements - `else` can not be used.

An ternary conditional expression 

    !python,linenos=no
    a if test else b

can however be used in the `expression` part of the list comprehension.

---

# Conditional Expression Example

    !python,linenos=no
    >>> d = {'kyle': 3, 'stan': 5, 'eric': 0, 'kenny': 7}
    [('stan', 5), ('kenny', 7), ('kyle', 3), ('eric', 0)]
    >>> ["{0} = 5".format(k) k if (v==0) else "{0} = {1}" % (k, v) \
        for k, v in d.items()]
    ['stan = 5', 'kenny = 7', 'kyle = 3', 'eric = 5']


---

# Equivalent notations

* `map(f, x)` is equivalent to `[f(xi) for xi in x]`
* `filter(f, x)` is equivalent to `[xi for xi in x if f(xi)]`
 
---

# A Note of Warning

Python versions 2.x the list comprehensions scope leaks into the
enclosing scope:

    !python,linenos=no
    >>> [i for i in range(3)]
    [0, 1, 2]
    >>> i
    2

This is not the case for Python 3.x:

    !python,linenos=no
    >>> [i for i in range(3)]
    [0, 1, 2]
    >>> i
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    NameError: name 'i' is not defined


---

# Generator expressions

---

# Generator Expressions

* Similar to list comprehensions, but generate data on demand.
* Don't return a new list, but an iterator.
* Easy to create.

Syntax:

    !python,linenos=no
    (expression for variable in iterable if condition)


---

# Example

    !python,linenos=no
    >>> [x**2 for x in xrange(4)]
    [0, 1, 4, 9]
    >>> g = (x**2 for x in xrange(4))
    >>> g
    <generator object <genexpr> at 0x2ca7d8>
    >>> g.next()  # In python 3.x use g.__next__()
    0
    >>> g.next()
    1

Note we use `xrange(4)`.

---

# Notation Exception

There is a special exception where a generator expression does not have to be enclosed in parenthesis

    !python,linenos=no
    >>> sum(x**2 for x in range(4))
    14

---
# Drawbacks of Generator Expressions

Drawbacks:

* Generators cannot be added to a list.
* Don't have a lenght.
* Can not be sliced or indexed.
* Not very flexible - use a generator pattern instead or a stateful
method that yields.
 

---

# Dictionary and Set Comprehensions 

Dictionary and set comprehensions are also available:

Examples:

    !python,linenos=no
    >>> {x for x in range(4)}
    set([0, 1, 2, 3])
    >>> {x:"A"*x for x in range(4)}
    {0: '', 1: 'A', 2: 'AA', 3: 'AAA'}


---

# List Comprehensions Continued

---
                      
# Nested Loops

Syntax taken from Python 2.7 reference:

    comprehension ::=  expression comp_for
    comp_for      ::=  "for" target_list "in" or_test [comp_iter]
    comp_iter     ::=  comp_for | comp_if
    comp_if       ::=  "if" expression_nocond [comp_iter]


---

# Nested Loops

A list comprehension of this form:

    !python,linenos=no
    res = [expression for i1 in L1 if cond1 for i2 in L2 if cond2 ...]

is equivalent to:

    !python,linenos=no
    res = []
    for i1 in L1:
        if cond1:
            for i2 in L2:
                if cond2:
                    ...
                    res.append(expression)

---

# Nested Loops
<!--
https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions
-->

    !python,linenos=no
    >>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
    [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

is equivalent to:

    !python,linenos=no
    >>> combs = []
    >>> for x in [1,2,3]:
    ...     for y in [3,1,4]:
    ...         if x != y:
    ...             combs.append((x, y))
    ...
    >>> combs
    [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

---

# Nested Loops

The nested loops can also iterate over variables from the outer loops.

Example:

    !python,linenos=no
    >>> L = [[1,2,3],[2,3],[1]]
    >>> res = []
    >>> for vec in L:
    ...     if len(vec)>2:
    ...         for e in vec:
    ...             res.append(e*2)
    ...
    >>> res
    [2, 4, 6]

which equivalent to:

    !python,linenos=no
    >>> L = [[1,2,3],[2,3],[1]]
    >>> [e*2 for vec in L if len(vec)>2 for e in vec]
    [2, 4, 6]

---

# Nested List Comprehensions

Nest a list comprehension in the expression.

    !python,linenos=no
    >>> L = [[1,2,3],[2,3],[1]]
    >>> [[i*2 for i in vec] for vec in L if len(vec)>2]
    [[2, 4, 6]]


---

# Long list comprehensions

<!-- http://docs.python-guide.org/en/latest/writing/style/ -->

What happend when list comprehensions become long?

PEP8 predates list comprehensions.

    !python,linenos=no
    [expression 
     for variable
     in iterable
    ]


Try to avoid two disjoint statements the same line.

---

# References

* PEP 202 List Comprehensions http://www.python.org/dev/peps/pep-0202/
    * List Comprehensions where itroduced in Python 2.0
* PEP 255 Simple Generators http://www.python.org/dev/peps/pep-0255/
* PEP 289 Generator Expressions http://legacy.python.org/dev/peps/pep-0289/
* PEP 274 Dict Comprehensions http://legacy.python.org/dev/peps/pep-0274/
    * Dictionary and set comprehensions were introduced in Python 2.7

* http://python-history.blogspot.dk/2010/06/from-list-comprehensions-to-generator.html

