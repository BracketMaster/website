# Matlab's Very Real Problems

[Here](https://www.mathworks.com/products/matlab/matlab-vs-python.html), Mathworks discusses
reasons to choose Matlab over Python.

 > Function names and signatures are familiar and memorable, making them as 
 > easy to write as they are to read.

Try again Mathworks. Matlab's function signatures don't even support multiple outputs like some other nice languages I could name: 
**cough** Python **cough**...

 > The matrix-based MATLAB language lets you express math directly. 
 > Linear algebra in MATLAB is intuitive and concise. 
 > The same is true for data analytics, signal and image processing,
 > control design, and other applications.

This is somewhat superficial...
I guess you need 7 extra character in Python to declare an array in Python.
After the initial work of 7 characters, everything between Python in Matlab
linear algebra wise is more or less comparable...

```Python
#initial setup, done once
from numpy import array

#array(...) is 7 characters long, not including the "..."
my_matrix = array([[1,2,3],[4,5,6]])
```

 > The desktop environment is tuned for iterative engineering 
 > and scientific workflows.

Is this why Matlab STILL doesn't support code browsing in the IDE?

Mathwork's last argument reads something like this and sums up the rest
of their arguments:

 > Engineers are not computer scientists - so we made computing easy with a *natural* language...

What??!!

First of all - electrical engineers **WERE** the first computer scientists.
As well as mathematicians, chemists, and biologists...
Computer Science didn't nominally exist until the late 60s or early 70's
depending on who you ask.

What Mathworks is really trying to hide is that Matlab is just a very dated
programming language, and even when it was young - Matlab simply ignored many standard
programming practices of the era.

# Matlab Doesn't Have Types
OK - that is not entirely true - but types were added as an afterthought.
The following code snippet will highlight the problem...

```matlab
>> a = [1 2 3 4; 5 6 7 8]

a =

     1     2     3     4
     5     6     7     8

>> a(1,1)

ans =

     1

>> a(1,2)

ans =

     2
```

Ok... So we use parentheses to index into array elements. So, what if we want
to get the size of array a, does Matlab return the result to me as an array?

If so - I should be able to index into elements of that array right?

```matlab
>> size(a)

ans =

     2     4

>> size(a)(1)
Error: Indexing with parentheses '()' must appear as the last operation of a
valid indexing expression.
```
**NOPE** Not even close!!! And literally every other programming language I can think
of from the top of my head supports this... C, Python,...

So in modern programming languages, to resolve an operator on an object,
the ``type`` erm these days ``class`` of the object is evaluated before looking up
the implementation of the operator for the class.

Matlab's behavior here leads me to believe ``size`` is a rather old function - (1984 anyone?),
and as soon as Matlab's parser sees size, it chokes because ``()`` after ``size`` probably isn't part
of the original Matlab grammar.

Let me just take a moment to let off steam about the fact that Matlab is one-indexed instead
of zero-indexed unlike literally **EVERY** other language in existance.

OK... technically - the spiritual parent of Matlab, Fortran, is one-indexed by default,
but, you can change that in Fortran - not so in Matlab.

Anyways - back to business - so it turns out that we can actually index into the result 
returned by the size function...

```matlab
>> my_size = size(a)

my_size =

     2     4

>> my_size(1)

ans =

     2
```

# I Thought You Said that Wasn't Possible...
So yes - it is possible - otherwise many of Mathwork's *engineers* would have a fit(it turns out to be quite important to be able determine matrix dimensions in Linear Algebra).

So what's the problem here? ``size(a)(1)`` *should* work. Basically, it boils down to
the sad fact that Matlab has a spaghetti implementation of a type system.

Similar idiosyncrasies plague the implementation of Matlab's classes, import behaviors
and more.


# Python Syntax - just easier as expected...
```python
>>> from numpy import array
>>> a = array([[1,2,3,4],[5,6,7,8]])
>>> a
array([[1, 2, 3, 4],
       [5, 6, 7, 8]])
>>> a.shape
(2, 4)
>>> a.shape[0]
2
>>> a.shape[1]
4
```
 And the good thing about numpy is that the default backend is C if you're worried about speed.
 Nvidia provides an implementation of numpy built for CUDA GPUs and even Intel provides
 an implementation that tales advantage of x86 AVX2.

Python can do virtually everything that Matlab can do just as fast and in some cases
even faster, especially in Machine Learning with Google and Facebook pouring millions into
Python C++ ML backends. Mathworks simply doesn't have that bandwidth.

 But of course Mathworks didn't mention this in their article. I'll just assume they forgot :)
