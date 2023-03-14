---
layout: post
title:  "Walrus Operator"
date:   2023-03-14 16:31:45 -0500
categories: datascience blogs
tags: python walrus operator
published: false
pin: true
---

# Using an Arctic Tank with a Data Snake
## What IS a Walrus?


![Public domain image of tow walruses sitting on rocks](/assets/2023-03-14/walrus-pixnio.jpg)
<sup>[USFWS](https://www.fws.gov/) via [PIXNIO](https://pixnio.com/fauna-animals/walrus/walrus-pair-on-rocky-beach), licensed under [Creative Commons, CC0](https://pixnio.com/creative-commons-license)</sup>

Data science marine biologists rejoice! In-line assignment is possible as of Python 3.8 (released in Oct. 2019). The walrus operator (`:=`) sets a variable to a value, just like `=` - but importantly - **IT RETURNS A VALUE**.

This may not seem very useful, and often times, it isn't. However, this means that variable assignment can be done _in line_.

First, let's take a look at how a situation where this might not be helpful.

```python
(walrus_name := "Frank")
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

[RealPython.com has a really good explanation about this here.](https://realpython.com/python-walrus-operator/#walrus-operator-syntax)
Additionally, their entire page goes into a lot more detail on
the operator itself, if you have a chance to really dig
into it.