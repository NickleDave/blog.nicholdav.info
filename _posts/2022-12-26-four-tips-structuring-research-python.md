---
title: Four tips for structuring your research group's Python packages
post_url: 2022-12-26-four-tips-structuring-research-python
---

This is one of those petulant posts from someone who writes software.
A post from one of those people that describes some library they wrote as "opinionated",
as if the rest of us have been living in fear of expressing our opinions through software.

So, sorry in advance.
But I want to talk about some things I see a lot in Python packages shared by research groups.

At the highest level, there's two things I see:
one, researchers name their modules in a way that
might make sense when you're writing your own code,
but can seem very verbose or non-intuitive
when you are someone else on the outside
reading or using that code.
Two, I see a lot of code that doesn't
structure itself in a way
that makes it easier for people to read it and to use it.
And you're sharing your code because you want people to use it, right?

I think a lot of packages from research labs end up this way 
in part because researchers don't have a good 
[mental model](https://teachtogether.tech/en/#s:models) 
of the mapping from their written code to the package they import.
In other words, they don't have a clear picture 
of how Python translates 
the directory structure of their code and its internal `import` statements
into a package name with modules that can be accessed with the dot operator,
like so: `package.subpackage.module.function`.

To use a technical term, 
the code does not fully leverage its own [namespace](https://realpython.com/python-namespaces-scope/),
the literal mapping from names of things in the package 
to the functions and classes that they refer to.
If I may invoke the holy sayings of the revered masters,
["namespaces are a honking great idea"](https://inventwithpython.com/blog/2018/08/17/the-zen-of-python-explained/).
To a wizened software engineer, 
that phrase from the Zen of Python might sound 
like Python is patting itself on the back
for something that basically every modern language does.
But for many researchers that are getting their first experience designing a software library, 
it might seem surprising that I'm drawing a connection between 
the pile of files that constitues their code and an abstraction from computer science.
If it sounds surprising to you, 
maybe it's because you don't (yet) have a good [concept map](https://teachtogether.tech/en/#s:memory-concept-maps) 
of how namespaces relate to naming your modules and structuring your package.

So let me tell you a little bit about that, 
and how you can build on it to make your code much more accessible.
I don't claim to have a perfect mental model of Python packages,
but at least I have fought enough with my own code,
and read enough of others peoples' code, that I think I can say something useful here.
This post contains some things I've learned the hard way,
to hopefully save you from repeating the mistakes of past me.

I've boiled it down to four tips, that I'll explain in more detail below.

Here's the high-level version:  

1. Give your packages and modules terse, single-word names whenever possible.
2. Import modules internally, instead of importing everything *from* modules.
3. Make use of sub-packages.
4. Prefer modules with very specific names containing single functions over modules with very general names like `utils`, `helpers`, or `support` that contain many functions.

Of course, it goes without saying that these are absolute rules
that came to me in a dream where Guido Van Rossum
emerged from a burning bush and
handed them to me on a freshly-minted [PEP](https://peps.python.org/pep-0000/).
As such, you should never violate them for any reason,
unless you feel like it.
(Those last two sentences should be written in the sarcasm font.)
More seriously, you could call these rules, 
but there are sometimes good reasons to not follow them to the letter, 
like we'll see below.

One other thing to say before we get started: 
I will use some examples here inspired by real life,
but namespaces have been changed to protect the innocent.
I promise my goal here is not to name shame,
but instead to help you make your code as accessible as possible.
I also want to note this is *not* meant to be a whole tutorial,
just some pointers on how to pick module names
and how to structure your package in a way that makes your code
more immediately accessible and more readable.
For an introductory tutorial to packages and modules,
please see <https://realpython.com/python-modules-packages/>.

## An example of what *not* to do

Here's the scenario:
You are really into sharing your research code written in Python.
Great! Sharing code just makes sense to you.
Your lab is getting on board with this too.
You made a
[GitHub organization](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/about-organizations)
for the research group
(instead of making an individual profile with the lab name!
Please don't do that, and make an org page instead,
so we can see who's in your group and understand who contributes what!
For when I want to write you an email thanking you for your work,
or ask you to collaborate,
or figure out who I want to hire away from academia into the private sector 😈).

Now everybody in the lab is forking their repos to that org,
so you can collaborate and all rely on the same version of the same code.
I go to your lab's org page and check out one of the repos, and I see something like this.

```
electroymyographytoolkit
├── emg_load_inchan_data_four_channelinkls.py
├── emg_main.py
├── emg_simulate.py
├── emg_solver.py
├── emg_utils.py
└── __init__.py

```

In the words of a wise man:

> Oh No Baby! What Is You Doin???

<cite> --- [`nicknpattiwhack_`](https://knowyourmeme.com/memes/oh-no-baby-what-is-you-doin)
{: .small}

## What you should do instead

Please! Help as many people as possible actually use your code
by avoiding naming and package structures like that!
Instead you want something like this:

```
emgtoolkit
├── load.py
├── main.py
├── simulate.py
├── solvers
│   ├── brute_force.py
│   ├── entropy.py
│   ├── gibson.py
│   ├── __init__.py
│   ├── random_walk.py
│   └── shannon.py
├── timestamp.py
└── __init__.py
```

The changes I made above follow the aforementioned four tips.
Here's they are again, the short version of what you want to do:

1. Give your packages and modules terse, single-word names whenever possible.
2. Import modules internally, instead of importing everything *from* modules.
(You can't see this directly in the example above, but I'll show it below.)
3. Make use of sub-packages.
4. Prefer modules with very specific names containing single functions over modules with very general names like `utils`, `helpers`, or `support` that contain many functions.

All of these rules help you make code written with your package more readable from the *outside*,
when someone else uses it from a script.
Rules 2,3, and 4 in particular can also help make your code more readable from the *inside*,
when you revisit it
after you spent six months in the lab fighting with weird noise from your electrophysiology rig
or an IHC protocol or whatever it is that scientists do, I don't remember anymore
([laughs in post-academic]).

## Please don't name your modules `package.pkg_module`

Okay, I will briefly, just for one paragraph, get really "opinionated".
I am not yelling at you. I'm yelling *with* you,
from the perspective of you as a future user and reader of your own code.
(Borrowing a concept from physics while simultaneously breaking its laws,
we change our frame of reference and do some time travel
so that what you named your modules is *no longer* a problem for "future you" to solve.)

Here's our example package layout again:  
```
electroymyographytoolkit
├── emg_load_inchan_data_four_channelinkls.py
├── emg_main.py
├── emg_simulate.py
├── emg_solver.py
├── emg_utils.py
└── __init__.py

```

**yelling**:  
Look at this. What is this?!
No one wants to type out `package.pkg_solver`!
Why are you making me redundantly type the package name again *after I just typed it*?
Only instead you're making me *remember* and type some abbreviated version of the package name
that you dropped some vowels from arbitrarily?
*And*, **and** you're making me type an underscore,
that my hand has to go find the shift key for! Every time!!!
Yes I know that tab complete exists! This is still too much typing!!! Why do you hate me?

More calmly:  
That might seem like a weirdly specific example to you, 
but I have seen naming schemes like this more than once (hence, this post). 
I really do think it has something to do with the way other researchers are thinking about 
code while writing it and when importing it. 
They want to import a module *from* their package and know where it came from, 
so they put the package name in the module name. I guess? :shrug:
Instead of doing anything like that, 
just name your package and modules something terse from the get-go, like

```
emgtoolkit
├── load.py
├── main.py
├── simulate.py
├── solvers
│   ├── brute_force.py
│   ├── entropy.py
│   ├── gibson.py
│   ├── __init__.py
│   ├── random_walk.py
│   └── shannon.py
├── timestamp.py
└── __init__.py
```

Now in your scripts you can write out the whole package name, 
while at the same time having it be more succinct and clear what part of the 
package's namespace you're referring to.
E.g., `emgtoolkit.simulate` instead of `electromyographytoolkit.emg_simulate`.
You might have noticed some other things changed in the names, too.
Those changes relate to the other tips, that I'll say more about below.

## Please don't name your modules `package.incredibly_long_technical_name`

There is also an even more extreme form of verbose module names, that takes the form of: 

```
pkg/
  incredibly_long_technical_name_or_specific_task
```

*Sometimes* this is justified, if it's the core functionality of your package and you really need to clearly communicate to everyone in the universe what the package does. E.g., `sklearn.model_selection` has a very specific meaning, and it's hard to capture with an abbreviation, and the library is trying to capture basically several sub-fields of machine learning within this sub-package. I am okay with tab-completing this name just so it's clear to all of us what the heck we're doing. But `pkg.preprocessing_electrophysiological_data` is not a great name. Especially if your package *only* deals with electrophysiological data. You can just say `pkg.preprocess`, it's fine.

### Instead, make module names general, and move specifics to function names

Often these really long module names can be a very specific thing you need to do.
Here again you can make things more terse
by moving the more specific part to a function name,
while keeping the more general part as a module name.
For example, instead of 

```
electroymyographytoolkit
├── emg_load_inchan_data_four_channels.py
```

name the module `load`,
and name a function in that module `inchan_data_four_channels`,
so that you can type out:  

```python
>>> emgtoolkit.load.inchan_data_four_channels()
```

Your intention is still obvious. 
But you rely on attribute access to make that intention clear,
while simultaneously more succinct.
This also means you have left yourself room later 
to add other `load` functions in your module,
like 

```python
>>> emgtoolkit.load.inchan_data_accelerometer_channel()
```

or whatever.
Trust me, there *will* be a later.
Leave *space* in your namespace.

## Import modules, not functions and classes, for readability

This is less about naming and more about importing,
but it's related because I feel like this same anti-pattern arises
from someone not understanding what it is that package structure gives you.
It gives you a logical grouping that's readable 
not just from the outside *but also from the inside*.
Let me explain.

I often see something like this in an internal module:
```
# inside main.py
from pkg_solver import (
    EntropySolver,
    ShannonSolver,
    GibsonSolver,
    ...
    # 10 more lines of class imports from a single module (!)
    BruteForceSolver,
    RandomWalkSolver
)
```

Let's say this is in `emgtoolkit/main.py`.
Going back to the overall goal of this post,
let's think about how the import above will 
affect the namespace of your package, as seen from the outside.
if I am using your package,
one of the first things I'm going to do is inspect
the content of  `emgtoolkit.main`, 
e.g. by running `dir(emgtoolkit.main)` in a Python REPL, 
since that name signals to me very loudly 
that you think of this as its main interface or API.
With an import statement like the one above,
when I tab complete while working with this module,
I am going to be faced with a wall of `Solver` classes
that obscures the true function of the module.
This is not the `solver` module! That's some other module!
If this is a high-level module that you expect me to access as a user,
then please avoid polluting it with all these names.
One of the ways you can do this is by making use of sub-packages,
as I'll talk about below.

I want to make a second point here,
that is less about making external use of your code readable,
and more about making it readable *internally*, within the code base.
I know that right now while you are hacking on the code,
you feel like you have all the low-level context about what all the moving parts are doing.
But if you want to make life easier for yourself six months from now,
and easier for everyone else that has to use your code,
write in a way that *surfaces* that context.
Write in a way that makes it much more obvious where all the functions and classes come from,
leveraging the logical grouping that you spent so much time boiling down into concise module names.

Please, for your future self and everyone else, 
make the code readable by importing *just* the module [^1],
and then referring to those classes (or function or whatever you're referring to)
via attribute access, i.e. with a dot, like `solver.BruteForceSolver`.
You can get rid of many lines of imports at the top of your module
*and* help yourself out when you are reading many lines deep in your own module,
far from those imports, if you instead do this:

```
# just import one thing, don't pollute namespace
from . import solvers

...
# ~150 lines in
    if solve_strategy == "entropy"
        # ah, this is one of many solvers in this
        solver = solvers.EntropySolver()
    elif solve_strategy == "shannon"
        solver = solvers.ShannonSolver()
```

One slight drawback of this terse approach is you can end up shadowing the most intuitive variable name for a class instance with a module that has a similar name. I.e., if I rename my module `pkg_solver` -> `solver` then I won't be able to do `from . import solver` at the top and then later *also* say `solver = solver.EntropySolver()`, because the variable name `solver` representing my class instance will clobber the name in the namespace that represents my `solver` module.
This will cause Python's head to explode (and yours). In this case I prefer to either use an absolute import internally, `import pkg.solver`, or to alias internally, e.g., `from . import solver as solver_classes`, so that an *external* user *still* can write something simple like `pkg.solver`.

[^1]: See similar comments from core Python dev Brett Cannon in this post, 
      <https://snarky.ca/if-i-were-designing-imort-from-scratch/>
      in the section "You can only import modules".

{:footnotes}
* 

## You should know sub-packages exist

Again, this is less about naming, and more about structuring internals.
But it falls under the same category of "thing I see that tells me you might not yet have a mental model of how to organize your code to make it easier for you to maintain in the long run".

You should know (if you don't already) that you can make a sub-package inside your package.
You do this the same way you make the package itself, 
by adding the magical `__init__.py` file that tells Python "this directory is a package".

Here we add a `solvers` sub-package to the `emgtoolkit` package from our example.
```
emgtoolkit
├── solvers
│   ├── __init__.py
└── __init__.py
```

Here is an example in the Python docs on modules: <https://docs.python.org/3/tutorial/modules.html#packages>

Going back to our example from above, 
we can refactor our (presumably gigantic) `solver` module 
into a handful of modules inside the `solver` sub-package:

```
emgtoolkit
├── load.py
├── main.py
├── simulate.py
├── solvers
│   ├── brute_force.py
│   ├── entropy.py
│   ├── gibson.py
│   ├── __init__.py
│   ├── random_walk.py
│   └── shannon.py
├── timestamp.py
└── __init__.py
```

Then you can use imports so that, from the perspective of the user, 
they have access to the *same* modules
with classes, functions, etc., that they had before.
You do this by importing all the classes from each module 
in the sub-package inside the `__init__.py`
of that sub-package.

```
"""solver sub-package"""
# this is __init__.py
from .entropy_solver import EntropySolver
from .shannon_solver ShannonSolver
from .gibson_solver GibsonSolver
...
```

This kind of looks like the thing I told you to *not* do above.
But the difference here is that you are importing inside the `__init__.py` file,
where (ideally) you are not hiding any functions;
instead you are just doing some boring boilerplate imports here
to make your life easier somewhere else.
This lets you be more intentional about your API too.
You can have some helper functions inside the sub-package modules 
that users don't really need.
To broadcast to the world the stuff you think people need,
you import just that stuff inside your sub-package `__init__.py`

<div class="notice">
Python doesn't have a concept of "public and private members of objects",
as languages like Java do,
so there's no (good) way for you to <strong>absolutely</strong> prevent someone from
importing and accessing things.
Instead Python follows the principle of "we're all adults here",
and uses some conventions to indicate things like private and internal functions.
See 
<a href="https://peps.python.org/pep-0008/#descriptive-naming-styles">the section of PEP8 on naming</a>,
in particular, where it talks about <tt>_single_leading_underscore</tt> 
as a weak “internal use” indicator. 
</div>

The other advantage of breaking code up into many modules within a subpackage, 
from your perspective as a maintainer, 
is that you can easily focus on one little chunk of code at a time.
I *promise* that in the long run, this will help you,
especially when you need to context switch and work on multiple things at once.
*Especially* if there's some helper functions and you're not having to scroll
or jump around through 3000 lines of code to find them.

## Finally, don't name a module `package.utils` or `package.utilities`

Someone even more jaded than me has already said this:
<https://breadcrumbscollector.tech/stop-naming-your-python-modules-utils/>

The basic idea is that the concept of `utilities` is so nebulous that more and more things will end up crammed into this module, that are less and less related to each other.
This is especially likely if multiple people are developing a package, 
and they are tempted by the name `utilities` to 
add *just* one more helper or validation function 
inside that (now gigantic) module when trying to add a new feature.

Hold on, friend. Take a step back, take a deep breath. 
It's okay to add a module with just a single function. 
You can just have a module named `timestamp` with only one function `get_timestamp`, 
and have it be only 10 lines. Please believe me, it will make the codebase easier to read, 
it will make internal usage of the function easier to read, 
it will make it easier to write quick unit tests. 
You do **not** want to hold 500 lines of `utils` 
in your head when you're trying to track down a bug. 
If you don't like that your top-level namespace 
gets cluttered by these little modules, 
than pull out our sub-package trick from above 
to push those modules down a level.

So that's me saying what that other blog post said, after I said they already said it.
All I want to add here is that the same logic from above applies: 
you *want* people to use your package.
So *give* the modules *very specific* names so people know exactly what everything is doing.
It will help your potential users and it will help future you.

## Take-homes

At the risk of repeating myself, 
let me tell you again why you'd want to take these tips to heart.
You want people to use your library that you're sharing with the world.
It's amazing; people can go onto the internet 
and just *use* the tools you put all that effort into!
As a researcher, 
this is the kind of instant gratification you can't get 
when you are taking on the Herculean task 
of putting reality into a headlock with an experiment.

So make the most of it!
Give your Python packages and modules names with just 
a few letters, and make use of imports and sub-packages 
to further structure the namespace of your package 
so that it's convenient to work with.
This will also benefit you 
in the long run when you're working with your own code.

Since I'm writing for research scientists, 
I will of course end the discussion 
by claiming that this blog sets the stage 
for future research.
A good question would be, 
has anybody done any research 
supporting my claims that these tips will help 
you and others read and use your code?
If I ever finish reading 
[The Programmer's Brain](https://www.manning.com/books/the-programmers-brain),
I'll probably be able to tell you.
Stay tuned for another post. 🙂
