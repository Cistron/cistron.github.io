---
layout: splat
title: Introduction to programming with Python
---

This tutorial will provide you with a little taster introduction to programming. In small steps you will learn about basic syntax and routines and apply them to play the children's word and number game *Fizz Buzz* (more on this later).

## Why Python?

Python frequently claims a spot in the top spot of the world's most popular programming languages and [the ranks of its followers are growing rapidly](https://stackoverflow.blog/2017/09/06/incredible-growth-python/). It is a general purpose language, which feels at home almost anywhere - from running a web application to data analysis in scientific research. Consequently, a whole plethora of packages or libraries (maintained code for reuse) are available.

Python is a high level programming language; there is no need to take care of memory management or compile the source code before execution. This ease of use, combined its good code readability (some will disagree), and widespread popularity makes Python a great first programming language to learn.

<!-- This tutorial is not necessarily all about Python, as programming concepts are shared between languages. -->

## Jupyter Notebook

We will be writing code in a [Jupyter Notebook](http://jupyter.org/), quasi a code editor, code interpreter and lab-book in a browser. Notebooks are great for building code or data analysis 'stories', as you get to see the history of your input and outputs.

The following video on Youtube provides a [quick introduction to Juypter Notebooks](https://www.youtube.com/watch?v=jZ952vChhuI) and introduces the majority of the core features you need for this workshop. For deeper inmersion, have a look at a longer, more [in-depth introduction](https://www.youtube.com/watch?v=UyxuLHLzDLA) on all the extra bits and bobs ('magics') of Jupyter, which makes it special.

![Launching Jupyter on UCL cluster PC]({{ "/splats/images/jupyter_notebook.png" | absolute_url }})

In UCL cluster rooms, Jupyter comes preinstalled as part of the Anaconda Python distribution. You should be able to find and launch it by typing 'jupyter' into the start menu.

![Launching Jupyter on UCL cluster PC]({{ "/splats/images/jupyter_ucl_cluster.png" | absolute_url }})

If you like to use your personal computer, you can download the same distribution from the [Anaconda website](https://www.anaconda.com/download/); make sure to download **version 3.6** (or higher) of Python.

## FizzBuzz!?

As mentioned previously, you'll get to implement a children's game in code. [Fizz Buzz](https://en.wikipedia.org/wiki/Fizz_buzz) is often played by children sitting in a circle, counting from one upwards. However, at every multiple of 3, the player has to shout 'Fizz' and at every multiple of 5, 'Buzz'. At multiples of 5 and 3, this has to be combined into 'FizzBuzz'.

Hence, it would go along the lines of `1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz, 16, 17, Fizz, 19, Buzz, Fizz, 22, 23, Fizz, Buzz, 26, Fizz, 28, 29, FizzBuzz, 31, 32, Fizz, 34, Buzz, Fizz, ...`

Now don't snuff at this quite yet, as thousands of programmers are challenged with *FizzBuzz* in their interviews. Though seemingly easy, one can implement more and more difficult aspects of programming into the game.

## S'up, Pythonistas?

We can print strings of characters in Python by using the `print()` command. Entering

```python
print("S'up, Pythonistas?")
```

into one of the Jupyter cells and executing it with `shift+enter` (or the play button) will print the string of characters below. Please make note of the syntax - round parenthesis, no spaces, quotation marks around the phrase to be printed. 

Thanks to the Jupyter notebook, you can always edit cells and re-execute them. Make some deliberate (syntax) errors and see what error message are returned to you. Usually they provide a good clue about what went wrong.

## Help! Algebra

In order to play our game, we will have to implement a tiiiny bit of algebra and learn about variables and conditional statements.

Python uses pretty standard operators for addition (`+`), substraction (`-`), multiplication (`*`), division (`/`) and exponents (`**`). In addition you can also use the modulo opperator (`%`), which returns the remainder after division - useful if you want to figure out if one number is the multiple of another.

You can carry out just by typing numbers and operators in the Juypter cells. Hence, `5+9` returns `14`, whilst `5+9/3` returns `8.0`.

Have a play. Maths is cool!

## Variables - persistent little devils

So far, all the code you've typed won't persist. In order to recall information we can pass it between lines of codes, we need to store it in a variable.

Storage of information is carried out via the assignment operator (`=`). So if you wanted to store the result of your equation in a variable called `some_number`, you could do this as follows.

```python
some_number = 5+9/3
print(some_number)
```

Make note that a variable name (`some_number`) must not have any spaces. It cannot start with a number and there are limitations to special characters being used (go and experiment). Whole style guides have been written on the topic of naming - [here's one from Google](https://google.github.io/styleguide/pyguide.html#Naming).

<!-- PEP8, import this, pythonic -->

From now on, until you overwrite or clear the variable `some_number`, you can recall it and use it. Try printing it, or use it for more maths. What would happen if you did the following below?

```python
some_number = some_number + 1
# or - note: anything behind a hashtag is a comment and won't be interpreted
some_number = some_number + some_number
```

You can also assign text to a variable.

```python
some_text = "Brave Sir Robin ran away. Bravely ran away away. When danger reared it’s ugly head, he bravely turned his tail and fled. Brave Sir Robin turned about and gallantly he chickened out ...
```

There are many of different types of variables and data structures. Some more will be introduced later.

The function `type()` will tell you which variable type you are dealing with. `type(some_text)` should return `str` for *string* (or sequences of characters). The number variable we created earlier, will return either `int` for *integer* or `float` for *floating point number* (i.e. number has a decimal point).

## Let me check that for you

Most programming languages (and Python indeed as well) come with a range of **comparison operations**. This allows the validation of a statement into a simple **Boolean** value of `True` or `False` (this exact spelling in Python). For example:

```python
5 != 3 # not equal
5 == 5 # equal
5 > 3 # larger than
5 >= 3 # equal or larger than
5 < 3 # smaller than
5 <= 3 # equal or smaller than
```

If you want to see each of the comparisons `True` or `False` output, you have to place them in separate Juypter cells.

Test some of your own comparisons. You can also compare information stored in variables.

We could thus use a comparison to figure out whether a number was evenly divisible by another number (sounds useful for a counting game?):

```python
current_count = 3
current_count % 3 == 0
```

# What if ...

... you wanted to execute a bunch of code only under certain circumstances? 

This is where the Boolean `True` or `False` output becomes incredibly useful, as we can execute code through **conditionals** when a condition is met. *Conditionals* allow us to expand our code from just a linear set of orders to something branched and mor complex.

Most commonly you will come across the `if` statement:

```python
a_number = 10
if a_number > 5:
    print('The number is larger than 5')
```

Please take note of the formatting. The `if`-statement starts with a colon (`:`) and all code which should only be executed if the comparison (`a_number > 5` in this case) validates as `True` is indented with a tab-stop.

`if`-statements can be expanded with `elif` (else if), for other specific comparisons, and `else` clauses. The code under `else` is only executed if none of the `if` or `elif` statements are met. This allows us to cover several cases.

```python
a_number = 10
if a_number > 5:
    print('The number is larger than 5')
elif a_number < 5:
    print('The number is smaller than 5')
else:
    print('The number is 5')
```

Note that the `if` and `elif` statements are evaluated sequentially. Hence, even if more than one would evaluate as `True`, only the first would be executed. To evaluate two conditions at the same time, comparisons can be connected with `and` and `or`.

```python
a_number = 10

if a_number < 15 and a_number > 5:
    print('The number is larger than 5, but smaller than 15')

if a_number < 15 or a_number > 5:
    print('The number is either larger than 5, or smaller than 15')
```

## Things are abuzz

Now that you have learned how to use comparisons and if-statements, write your own `if` and `elif` code to evaluate a number for *Fizz Buzz*.

Hint: Remember the modulo operator (`%`) and that numbers divisible by `3` should output `Fizz`, divisible by `5` `Buzz` and divisible by `5` and `3` `FizzBuzz`. Don't forget to just print the number if it isn't a multiple of 3 or 5.

Make sure to test your code with serval input numbers (e.g. 3, 5, 15 and some non-mulitples of 3 and/or 5).

## 1, 2, ... many

Now that you have figured out how to evaluate numbers, we need to keep them coming. Testing each number by hand is tedious. This is where **loops** come in. A `for` loop will continuously step through elements in a sequence.

```python
for i in [1,2,3,4,5,6,7,8,9,10]:
    print(i)
```

The example above will print out numbers from 1 to 10. The numbers in the rectangular brackets are in Python called a **list** (another data structure besides integers, floats, strings and many others). At every iteration of the loop, the variable `i` is assigned an **item** from this list. The indented code below is the executed. This happens until we reach the last element in the list.

Note that you change the variable name of `i` to anything you chose, though `i` is usually used to refer to *iteration*.

Since a primary motivation for coding is lazyness, there's a faster way to create pure numerical lists to step through with the `range()` function.

```python
for i in range(1,11):
    print(i)
```

1 to 11?!? Well, just using `range(10)` would return the number 0 to 9. Indexing in Python (and many other programming languages) starts at 0. That means the first item in a list will be item 0 and hence range usually starts at 0, unless otherwise defined. Hence `range(10)` is interpreted as `range(0,10)`.

And why 11? When designing the end of the range, it helps to visualise it like a slicing operation. The sequence of numbers is 'sliced' just before the numbers put into the function (`0,10` and `1,11` respectively):

```
range(0,10)
|0 1 2 3 4 5 6 7 8 9|10 11
range(1,11)
 0|1 2 3 4 5 6 7 8 9 10|11
```

<!-- python can iterate of almost anything -->

We can nest more complicated code than just a `print` statement within loops; for example `if` statements.

``` python
for count in range(20):
    if count > 5:
        print('The number is larger than 5')
    elif count < 5:
        print('The number is smaller than 5')
    else:
        print('The number is 5')
```

In this case the loop will evaluate the number 0 to 19.

## Ooooh, Fizzy

Now that you know how to count up with a `for` loop, combine this with your evaluative `if` statements to play a short game of *Fizz Buzz*. Test several number ranges.

Whenever you are working on more complicated programmes, it greatly helps to conceptualise and visualise what you are trying to achieve. Below is a flowchart of a relatively simple *Fizz Buzz* algorithm.

![FizzBuzz algorithm]({{ "/splats/images/fizzbuzz_algorithm.png" | absolute_url }})

This can be done on the back of a napkin, or in this case with a drawing software (draw.io). Feel free to clone [this *Fizz Buzz* flow chart](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=fizzbuzz_algorithm.html#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D12Zj8lTX_aF-iXCBaeiRmu_NJjOiUckr9%26export%3Ddownload) and expand and adopt it for a more versatile algorithm later.

## Redundancy makes coders itch

If you followed the provided algorithm, your code will likely be very similar to the example below. `end=", "` leads to everything being printed on a single line separated by commas.

<p>
    <details><summary><strong>Careful Spoilers</strong>: Click to reveal code</summary>
    {% highlight python %}
    for i in range(1,101):
        if i%15 == 0:
            print('FizzBuzz', end=", ")
        elif i%3 == 0:
            print('Fizz', end=", ")
        elif i%5 == 0:
            print('Buzz', end=", ")
        else:
            print(i, end=", ")
    {% endhighlight %}
    </details>
</p>

Programmers are lazy, because simpler code is easier to maintain as it requires less edits.

In the example above, you'll find many print statements and even a superfluous check; `i%15 == 0` is given if both `i%3 == 0` and `i%5 == 0` evaluate as `True`.

Let's simplify it and make the code more versatile.

## How long is a piece of string?

At the beginning you experimented with a bit of basic algebra, using standard maths operators (`+-*/`) on integers or floats. We can use some of these operators on strings, where they have different effects. Where operators have different effects based on the context, this is known as **operator overloading**.

``` python
short_string = "na "
# note the whitespace is part of the string
print(short_string + short_string)
```

A plus (`+`) **concatenates** strings together, thus the return is: `na na `. Whilst an asteriks (`*`) will concatenate the string several times.

``` python
bruce = 'BATMAAAAN'
print(short_string.upper() * 16 + bruce) # .upper() converts string to UPPER CASE
print('eggs, ' + 'spam, ' * 32 + 'eggs, spam and bacon')
```

Besides these shenanigans, concatenation allows us to build up an output by adding bits to it. Hence it only has to be printed once.

``` python
count = 10
output = 'This number is '
if count > 5:
    output = output + 'larger than 5'
if count > 8:
    output += ' and larger than 8'
if count < 15:
    output += ', but smaller than 15.'
print(output)
```

Note that `+=` is a shorter notation of `output = output + ...`: take a variable, add to it and overwrite the original. (This also works for maths operations, e.g `count += 1`)

How could you rewrite your code to make use of this and thus reduce the number of `if`- and `print`-statements? Do some back-of-the-napkin algorithm design before you modify your code.

## Function over form

Say you wanted to use your code to play *Fizz Buzz* twice, but for different amount of times (counting higher). The easiest way would be to copy and paste your code and edit the `range()` function. Though now you duplicated the code and maintenance has doubled.

This is where functions come in. They make a piece of code more versatile, as they can be **called** again and again with different input parameters.

Say we wanted to convert (poxy) Fahrenheit to degrees Celsius, using the formula $$ T_C = \frac{5}{9}(T_F-32) $$.

```python
def f_to_c (fahrenheit):
    celsius = (fahrenheit - 32)*(5/9)
    return celsius
```

Note the syntax: We define (`def`) the function by giving it a name and determine any **input parameters** (`fahrenheit`, but could be any name) we want to pass on to the indented code of the function. In this case, whatever number is passed for `fahrenheit` is fed into the equation and saved in the variable `celsius`, which is `returned` by the function.

It is important to understand that the variables `celsius` and `fahrenheit` are only available within the context of this function - this concept is called **scope**. You will not be able to use either of the variables outside of the function, hence it is necessary to return a result to the 'outside'.

Just by itself, the function won't do anything. It needs to be called, by passing it an input parameter.

```python
temp_in_f = 100
temp_in_c = f_to_c(temp_in_f)
print(temp_in_c)
```

Experiment a little, maybe write the reverse function to convert Celsius to Fahrenheit (no idea why anyone would ever need this though). Then wrap your *Fizz Buzz* code in a function declaration. What parameter would you pass it?

## Fizz, Buzz, Bang, etc.

If you made it this far: congratulations. The next couple of points are more difficult challenges to make the *Fizz Buzz* function more versatile.

* So far the parameters of the game are hardwired. 'Fizz' is always called at multiples of 3 and 'Buzz' at multiples of 5. What if you wanted more flexibility?

    Hint: functions can be passed more than just one parameter. And you can even define defaults.

    ```python
    def fizz_buzz (variable_one, variable_two='default', variable_three=100):
        ...
    ```

* How about more complexity? Add another parameter, `bang`.

* Complete flexibility? Unshackle the function completely from a fixed set of parameters.

    Hint: you can also pass *lists* (`['fizz','buzz','bang']`, `[3,4,5]`) through function parameters. You will need to iterate over both input lists.

    ```python
    l = ['fizz','buzz','bang']
    print(l[0]) # acess a single item by index
    for index, item in enumerate(l):
        print('The item "{}" is at index "{}"'.format(item,index))
    ```

* Avoid having to match two input lists.

    Hint: use dictionaries, which are always key and value pairs.

    ```python
    d = {'unique key':'value','Fizz':3,'Buzz':5,'Bang':7}
    print(d['unique key']) # acess a single value by key
    for key, value in d.items(): # to iterate over a dictionary
        print('The key is "{}" and its value is "{}"'.format(key, value))
    ```

## Further (mostly free) resources

In order to get better at coding, as with any skill, you need deliberate praxis.

Please note that this is most definitely not a comprehensive list of resources (a quick internet search will kick up tons more), nor is this a ranking.

* There are tons of free introductory courses for Python, each with their own focus.
    * [Codeacademy](https://www.codecademy.com/learn/learn-python) courses run completely in a browser
    * [Data Camp](https://www.datacamp.com/courses/intro-to-python-for-data-science) aims to develop skills needed as a data scientist
    * [Code School](https://www.codeschool.com/courses/try-python) is more aimed towards web-development
* [Introduction to Research Programming (with Python)](http://rits.github-pages.ucl.ac.uk/intro-research-prog/) - by UCL Research IT Services
* Lynda, which you can acess for free through UCL, has a whole range of [Python courses from beginner to intermediate](https://www.lynda.com/Python-training-tutorials/415-0.html)
* More full-blown university-style MOOCs educating computer scientists are also widely available, e.g. from [MIT through edX](https://www.edx.org/course/introduction-computer-science-mitx-6-00-1x-11).
* (e)Books/websites:
    * [Automate the boring stuff with Python](https://automatetheboringstuff.com/)
    * [Learn Python the hard way](https://learnpythonthehardway.org/book/preface.html)

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>