---
layout: post
title:  "Walrus Operator"
date:   2023-03-20 16:31:45 -0500
categories: datascience blogs python
tags: python walrus operator
published: true
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

[RealPython.com has a really good explanation about this here.](https://realpython.com/python-walrus-operator/#walrus-operator-syntax)[^fn3]
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
One of the most common use cases[^fn4]
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

## ~~In~~comprehensible!
Frequently, Pandas data frames or dictionaries will need to be
mapped over. List comprehensions are, generally, pretty compact
and great in their own right - but what if you need to use the
same operation over and over again? What if you need to use
each instance in the next iteration?

This is where a walrus-ified variable can truly come in handy.

We're going to look at COVID data from India that covers the 
time period from January 30th, 2020 to October 31st, 2021.
This dataset is available [here](https://www.kaggle.com/datasets/mdahmadjami/covid19-india?resource=download)[^fn5]
 via Kaggle. The next steps will
also assume that pandas has been imported as `pd`.

The dataset we're taking a look at *does* have a column for new
cases, but what if it didn't? Here, we'll go ahead and re-calculate
new cases based on the cumulative number of cases. We'll go ahead
and create a new column for the calculated data as well.

```python
covid_daily_in = pd.read_csv('./COVID-19_India_Data.csv')
prev_day = 0
covid_daily_in['calculated_cases'] = [
        abs(prev_day - (prev_day := cases)) for
        cases in covid_daily_in['cum_cases']
    ]
```

| Date	        | Total Cases	| New Cases	| Calculated Cases |
|---------------|-----------|-----------|-------------------|
| 2020-01-30	| 1	        | 1	        | 1                 |
| 2020-01-31	| 1	        | 0	        | 0                 |
| 2020-02-01	| 1	        | 0	        | 0                 |
| 2020-02-02	| 2	        | 1	        | 1                 |
| 2020-02-03	| 3	        | 1	        | 1                 |
| ...	        | ...	    | ...	    | ...               |
| 2021-10-27	| 34231030	| 16351 	| 16351             |
| 2021-10-28	| 34245337	| 14307	    | 14307             |
| 2021-10-29	| 34259552	| 14215	    | 14215             |
| 2021-10-30	| 34272492	| 12940	    | 12940             |
| 2021-10-31	| 34285399	| 12907	    | 12907             |

The `new_cases` and `calculated_cases` values line up, which means
that (in theory) the line worked!

There's a little bit of wonkiness going on to get this functional
because of the order that the assignment happens, but it works out
well and doesn't have to call a previous cell or enumerate on every
line.

An alternative way to have this written out would be:
```python
calculated_cases = [covid_daily_in['cum_cases'].iloc[0]]
for i ,_case in enumerate(covid_daily_in['cum_cases'][1:]):
    calculated_cases.append(
        _case - covid_daily_in['cum_cases'].iloc[i]
    )
covid_daily_in['calculated_cases'] = calculated_cases
```

Not only is the first result _cleaner_, it's _**faster**_.

Here are the results of running the magic function `%%timeit` in
a notebook:

For the list comprehension: \
`30.3 µs ± 60.3 ns per loop (mean ± std. dev. of 7 runs, 10000 loops each)`

For the for loop: \
`1700 µs ± 4470 ns per loop (mean ± std. dev. of 7 runs, 1000 loops each)`

_**That's 56 times faster**_.

A lot of that speed comes from not having to re-reference the data
frame.

## Wrapping Things Up

There's a lot more that can be done with the walrus operator,
it has a lot of uses and a lot of *mis*uses. Always make sure
you're fitting in with the style guides outlined in PEP where
possible. Not all space reduction in Python is created equal.

If you're looking for more information on the walrus operator
and good ways to use it, check out the RealPython post that
was mentioned above and below. There's also a good post on
[mathspp.com about when to, and when not to, use the walrus operator](https://mathspp.com/blog/pydonts/assignment-expressions-and-the-walrus-operator)[^fn6]

Additionally, if you want to run the code outlined in this
post, you can find a Jupyter notebook with all of the references
available
[here](/assets/2023-03-14/walrus_interactible.ipynb).
\(you'll need 
[this](/assets/2023-03-14/COVID-19_India_Data.csv)
.csv file as well if you can't get it from kaggle, they'll need
to be put in the same folder).


<br/>
## References
[^fn1]: [Python version docs](https://www.python.org/doc/versions/)
[^fn2]: [Python 3.8 release notes](https://docs.python.org/release/3.8.0/whatsnew/3.8.html)
[^fn3]: [RealPython.com walrus operator syntax](https://realpython.com/python-walrus-operator/#walrus-operator-syntax)
[^fn4]: In my experience <sup>this does not count as a real reference</sup>
[^fn5]: [Covid-19 Cases for India, Kaggle](https://www.kaggle.com/datasets/mdahmadjami/covid19-india?resource=download)
[^fn6]: [Mathspp.com / PyDon't](https://mathspp.com/blog/pydonts/assignment-expressions-and-the-walrus-operator)