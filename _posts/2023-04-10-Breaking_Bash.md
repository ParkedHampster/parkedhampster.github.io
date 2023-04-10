---
layout: post
title:  "Breaking Bash - Using Bash in Jupyter Notebooks"
categories: jupyter python bash datascience
date:   2023-04-10 19:33:00 -0500
published: true
pin: true
---

# Bashing something something pun

Jupyter notebooks have a lot of capabilities and
applications. One of the less document uses is the
ability to use Bash in-line. While this power is easy
enough to overlook, it's also easy enough to
misunderstand.

Learning, looking, and reading about things related to
the bash commands available in Jupyter Notebooks, I see
a lot of people using it like this:

```python
!ls
```

```bash
file1.csv   file2.csv   file3.csv
```

```python
pd.read_csv('./file1.csv')
pd.read_csv('./file2.csv')
pd.read_csv('./file3.csv')
```

This works well enough, I guess, but what if we add a
fourth file in?

```py
!ls
```

```bash
file1.csv file2.csv file3.csv file4.csv
```
<!--
We would have to add in another file read for this to
be usable, and if we aren't sure about the structure
for the files then that's not necessarily a problem,
but if these always have the same structure - say a
series of top 10 songs that's always generated the
same way every day - you can dynamically take these
files in. -->

One thing that's incredibly nice about the outputs of
the bash commands, they can be saved off! For example,
if we want to get a list of files in a folder and save
it off to index later, we can!

```py
ls = !ls
print(ls)
```

```bash
['file1.csv','file2.csv','file3.csv']
```

This is great for saving off the file names, but what
can we use them for if they're saved into python?

We can pass them through!

This is honestly one of the features of the JN bash
that I had the most trouble finding, but you _can_ in
fact push variables into the bash arguments, just wrap
them in `{}` and they'll pass the data through!

The unfortunate part is that you can't do calculations
in-line, but you can just do the calculations off in
the previous line and have them saved to a variable
just as well.

Let's take a look at a realistic example real quick.

Say we want to render images in a notebook and we know
we want to be able to add or remove images as we work
through our processes. Here, we'll include the `Image`
function from `IPython.display`

Let's assume this is what our project directory looks
like:
```
Project_Folder
├── breaking_bash.ipynb
├── pets
|   ├── cats
|   |   └── ... <- pictures of cats
|   └── dogs
|       └── ... <- pictures of dogs
└── src
```

```python
working_dir = './pets/'
_list_dir = !ls {working_dir}
_list_dir
```

```bash
['cats','dogs']
```

Here, we look into the pets folder in our current
directory and get a list of the folder names within.

We can see from the output that the result we've saved
off is a list of folder names that we have within the
pets directory. Since we can pass in arguments to bash,
that means we have a really simple way to go into these
folders and save each of the files off into different
variables or within a dictionary.

For our purposes, we'll do the latter.

We can create a simple for loop that goes through each
of the directories that we have our images in and run
them all through `Image()` to generate a simple image
dictionary (or just display them in-line).

```py
container = {}

for _dir in _list_dir:
    # we'll create a variable that holds our directory
    # name for going through the loop
    subdir = f'{working_dir}/{_dir}'
    
    # we'll create a new array that is the list of ALL
    # files that are in the subdirectory we have active
    # (e.g. ./pets/cats) so we can iterate in the next
    # step
    files = !ls {subdir}

    # we can set the container's keys to the names of
    # the folders so that it's as easy to navigate or
    # to recall as the file structure was in the first
    # place
    container[_dir] = {}

    for file in files:
        # each file in _dir is called, rendered, and
        # saved into the dictionary with its file
        # name as the key (and also displayed)
        im = Image(f'{subdir}/{file}')
        container[_dir][file] = im
        display(im)
```

Great! This grabs all of our pictures and gives them to
us in a way that's easily accessible through python.
That's really nice and convenient, but the real gem is
that this process is _**DYNAMIC**_.

If we put a new folder into this structure, we don't
have to explicitly tell Python to look in that folder
and get the new images there. This is more of a benefit
with using variables than anything, but being able to
investigate folder contents through bash dynamically
and pulling those into Python through functions that
would otherwise need to be given string literals is
something that can easily save some time. _ESPECIALLY_
if the folder structure is used in a logical way.

A solid example of this would be data that is stored in
for train test splits. If all train data is in a
`./train/` folder, it's easy enough to include that as
a prepended folder - but what if you're adding new info
into a model to see how how well it can be trained on
new data? Looking at the pictures explanation from
above, if all the model is trained on is cats and dogs
and we want to add in a new category that our model
investigates, like rats, we wouldn't necessarily want
to have to go through and manually add that folder into
our model's data. There are a lot of places that string
would need to be added, and saving it off as a new key
would let us call all of the same processes on that
model in a much more Pythonic way.

If you have any familiarity with bash, you might be
seeing a world of possibilities opening up here.
Effectively, all commands that return an output are
usable here. `grep`, `awk`, `sed`... There are a world
of possibilities. Need to create several Pandas data
frames that are stored as different types? `grep -e
".csv"` lets you only select the csv files for your
`pd.read_csv()` function. Have some tsvs as well?
`grep -e ".tsv"` in combination with `pd.read_csv(...,
sep='\t')` will let you read those in. Using bash
commands in combination with `sys` is an untapped gold
mine of potential <sub>(and maybe a little jank)</sub>.

As I mentioned before, this functionality's
documentation seems to be extremely limited. I haven't
even been able to see another post that mentions the
fact that output can be saved to a variable. As a
result, there is a ton that may be possible. Piped
commands, custom shell scripts, it all feels limitless.
While the application of this could be minimal, I may
have covered every single use case for this
functionality in a few-hundred word post, it's entirely
possible that this extensibility and direct access to
the filesystem may be what a lot of functions need.

This may not be the most portable of pieces, but with
almost no fail, new tools are always going to make old
problems more interesting to approach.