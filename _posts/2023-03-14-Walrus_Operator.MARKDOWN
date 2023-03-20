2533 152nd Ave NE Suite 14JK---
layout: post
title:  "Walrus Operator"
date:   2023-03-14 16:31:45 -0500
categories: datascience blogs python
tags: python walrus operator
published: false
pin: true
---

# Using an Arctic Tank with a Data Snake
## What IS a Walrus?


![Public domain image of tow walruses sitting on rocks](/assets/2023-03-14/walrus-pixnio.jpg)
<sup>[USFWS](https://www.fws.gov/) via [PIXNIO](https://pixnio.com/fauna-animals/walrus/walrus-pair-on-rocky-beach), licensed under [Creative Commons, CC0](https://pixnio.com/creative-commons-license)</sup>

Data science marine biologists rejoice! In-line assignment is possible as of Python 3.8[^fn1] (released in Oct. 2019)[^fn2]. The walrus operator (`:=`) sets a variable to a value, just like `=` - but importantly - **IT RETURNS A VALUE**.

This may not seem very useful, and often times, it isn't. However, this means that variable assignment can be done _in line_.

First, let's take a look at how a situation where this might not be helpful.

```python
(walrus_name := "Frank")

>>>'Frank'
```
Great! Now we have successfully named our walrus
variable Frank.
Running this is a Python shell would return `'Frank'`.
If we just passed in the variable assignment, it would return
nothing - but it would still result in our `walrus_name`
variable being 'Frank'. So not much is really different.

As a matter of fact, this seems _less_ convenient, as we
have to wrap the walrus operator in parentheses. Why is
that?

Effectively, this has a lot to do with how the Python
Software Foundation approaches Python's usability. It's the
same reason the walrus operator is useful in the first place,
you can't perform an assignment during an operation.

[RealPython.com has a really good explanation about this here.](https://realpython.com/python-walrus-operator/#walrus-operator-syntax)[^fnwalrus-syntax]
Additionally, their entire page goes into a lot more detail on
the operator itself, if you have a chance to really dig
into it.

## Okay... but how is this _**useful**_?
### f'' Strings
The walrus operator becomes useful when you want to set a
variable and then _immediately_ use it.
Take `f''` strings, for example.

Normally, when using an f string to display the results of a
function, you would need to set the variable before the string
and call for that variable within brackets.

```python
walrus_name = "Frank"
print(f'My favorite walrus is named {walrus_name}!')

>>>'My favorite walrus is named Frank!'
```

This isn't too bad when you're using a single string or you
have all of the variables already defined, since we set Frank
our `walrus_name` already.
But if have new assignments to make, there's not much of a
reason for us to break it out.

```python
walrus_name = "Frank"
walrus_last_name = "Ocean"
walrus_full_name = walrus_name + ' ' + walrus_last_name
print(f'''Have you met the greatest of the walruses, {walrus_full_name}?''')

>>>'Have you met the greatest of the walruses, Frank Ocean?'
```

That's not the worst to look at, but there's a lot of
assignment happening in multiple lines.
What if we made *ALL* of this fit into walrus format?

```python
print(f'''Have you met the greatest of the walruses,{
    (walrus_full_name := (walrus_name := "Frank") +
    " " +
    (walrus_last_name := "Ocean")
    )
}?''')

>>>'Have you met the greatest of the walruses, Frank Ocean?'
```

Okay, maybe this is a bit clunky. But we can find a middle
ground.

```python
walrus_name = "Frank"
walrus_last_name = "Ocean"
print(f'''Have you met the greatest of the walruses, {
    (walrus_full_name := walrus_name + " " + walrus_last_name)
}''')

>>>'Have you met the greatest of the walruses, Frank Ocean?'
```

This is nice. We didn't need the `walrus_full_name` until we
got to this point. There aren't a million assignments in a
single line, and everything returns as expected.

We aren't just talking about walrus names here, though.
There's ___real___ and ___important___ data to be investigated
here!

<!--
### **100%** Probability <sub>of me using this because it's fun</sub>

Statistics has a LOT of math. 
-->

### Simpl`if`ying some things...
One of the most common use cases<sup>[^fn3]</sup>
of walruses in the wild is in a conditional statement.

Let's say that we want to print some number (`n`) if the square
of that number is greater than or equal to 50. Normally, we
would write n*\*2 both *in* the if statement and in the code
within, like so.
```python
if(n**2 >= 50):
    print(n**2)
```

We *could* save off the number beforehand and then perform our
checks after it's defined as such:
```python
check = n**2
if(check >= 50):
    print(check)
```
but that's not necessary.
```python
if((check:=n**2) >= 50):
    print(check)
```

This is nice and all, but it only reduces the code a little
bit and doesn't increase readability all that much.

Where this shorthand *really* shines for conditionals is in
loops. *Particularly* loops with inputs.

Take for example...
```python
user_input = input("What's your favorite arctic mammal? > ")

while user_input.lower() != 'walrus':
    print(f'{user_input} is WRONG!')
    user_input = input("What's your favorite arctic mammal? > ")
```

The `user_input` needs to be defined twice, including once
outside of the loop.

How would this look with a walrus operator?

```python
while (user_input := input("What's your favorite arctic mammal? > ").lower()) != 'walrus':
    print(f'{user_input} is WRONG!')
```
That's quite a bit cleaner!

It's great that we can clean a lot of this up, but we should
probably drop the pun for a little bit and move on to some
more real-world oriented applications.

## Navigating With a `.map`



## ~~In~~comprehensible!



<br/><br/>
## References
[^fn1]: [https://www.python.org/doc/versions/](https://www.python.org/doc/versions/)
[^fn2]: [https://docs.python.org/release/3.8.0/whatsnew/3.8.html](https://docs.python.org/release/3.8.0/whatsnew/3.8.html)
[^fn3]: In my experience
[^fnwalrus-syntax]: [https://realpython.com/python-walrus-operator/#walrus-operator-syntax](https://realpython.com/python-walrus-operator/#walrus-operator-syntax)