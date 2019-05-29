---
layout: splat
title: Introduction to programming with Python
---

This tutorial will provide you with a little taster introduction to programming. In small steps you will learn about basic syntax and routines and apply them to play the children's word and number game *Fizz Buzz* (more on this later).

## Why Python?

Python frequently claims a spot in the top spot of the world's most popular programming languages and [the ranks of its followers are growing rapidly](https://stackoverflow.blog/2017/09/06/incredible-growth-python/). It is a general purpose language, which feels at home almost anywhere - from running a web application to data analysis in scientific research. Consequently, a whole plethora of packages or libraries (maintained code for reuse) are available.

Python is a high level programming language; there is no need to take care of memory management or compile the source code before execution. This ease of use, combined its good code readability (some will disagree), and widespread popularity makes Python a great first programming language to learn.

## Jupyter Notebook

You will be working through this exercise in a [Jupyter Notebook](http://jupyter.org/), a quasi a code editor, code interpreter and lab-book in a browser. These notebooks are great for data analysis, or tutorial, as you get to see the history of your input and outputs and mix it with additional text (or instructions in this case).

The following video on Youtube provides a [quick introduction to Juypter Notebooks](https://www.youtube.com/watch?v=jZ952vChhuI) and introduces the majority of the core features you need for this workshop.

[![A quick introduction to Juypter](https://img.youtube.com/vi/jZ952vChhuI/maxresdefault.jpg)](http://www.youtube.com/watch?v=jZ952vChhuI "A quick introduction to Juypter")

For deeper inmersion, have a look at a longer, more [in-depth introduction](https://www.youtube.com/watch?v=UyxuLHLzDLA) on all the extra bits and bobs ('magics') of Jupyter, which makes it special.

In UCL cluster rooms, Jupyter comes preinstalled as part of the Anaconda Python distribution. You should be able to find and launch it by typing '`jupyter`' into the start menu.

If you like to use your personal computer, you can download the same distribution from the [Anaconda website](https://www.anaconda.com/download/); make sure to download **version 3.x** of Python.

## Downloading the Notebook

To start, [download the python1.ipynb notebook file]({{ "/splats/data/python1/python1.ipynb" | absolute_url }}), save it in an appropriate folder (the download folder is probably not ideal) and open it through the `jupyter` file browser.

## Solutions

Below are example solutions for each of the trickier exercises of this workshop. Give it your best try before you have a look at these, as most of the learning effect will come from you attempting to apply what you learned.

In programming there is always "more than one way to skin a cat", consequently, there are other possible solutions as well.

<details><summary>Things are abuzz: Click to reveal code</summary>
{% highlight python %}
a_number = int(input('Enter an integer'))
# input() allows you to interact with your programme
# and int() converts the input to an integer (otherwise it is a string)
if a_number%15 == 0:
    print('FizzBuzz')
elif a_number%3 == 0:
    print('Fizz')
elif a_number%5 == 0:
    print('Buzz')
else:
    print(a_number)
{% endhighlight %}
</details>

<details><summary>Ooooh, Fizzy: Click to reveal code</summary>
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
<!-- Note: `end=", "` in the `print` statement leads to everything being printed on a single line separated by commas. -->
</details>

<details><summary>How long is a piece of string?: Click to reveal code</summary>
{% highlight python %}
for i in range(1,101):
    output = ""
    if i%3 == 0:
        output += 'Fizz'
    if i%5 == 0:
        output += 'Buzz'
    if output == '':
        output = i
    print(output, end=', ')
{% endhighlight %}
</details>

<details><summary>Function over form: Click to reveal code</summary>
{% highlight python %}
def fizz_buzz (length=100):
    """Plays the children's game FizzBuzz up to the count of input. Default is 100"""
    for i in range(1,length+1):
        output = ""
        if i%3 == 0:
            output += 'Fizz'
        if i%5 == 0:
            output += 'Buzz'
        if output == '':
            output = i
        print(output, end=', ')
fizz_buzz(50)
{% endhighlight %}
</details>

<details><summary>Fizz, Buzz, Bang, etc. 1: Click to reveal code</summary>
{% highlight python %}
# Add input variables for FizzNumber, BuzzNumber
def fizz_buzz (fizz_number=3, buzz_number=5, length=100):
    for i in range(1,length+1):
        output = ""
        if i%fizz_number == 0:
            output += 'Fizz'
        if i%buzz_number == 0:
            output += 'Buzz'
        if output == '':
            output = i
        print(output, end=', ')
fizz_buzz(2,7,50) # default parameters overwritten
{% endhighlight %}
</details>

<details><summary>Fizz, Buzz, Bang, etc. 2: Click to reveal code</summary>
{% highlight python %}
# Make the game more complicated - FizzBuzzBang
def fizz_buzz_bang (fizz_bumber=3, buzz_number=5, bang_number=4, length=100):
    """Plays the children's game FizzBuzz up to the count of input. Default is 100"""
    for i in range(1,length+1):
        output = ""
        if i%fizz_bumber == 0:
            output += 'Fizz'
        if i%buzz_number == 0:
            output += 'Buzz'
        if i%bang_number == 0:
            output += 'Bang'
        if output == '':
            output = i
        print(output, end=', ')
fizz_buzz_bang(length=60)
{% endhighlight %}
</details>

<details><summary>Fizz, Buzz, Bang, etc. 3: Click to reveal code</summary>
{% highlight python %}
# Input list of terms and numbers
def fizz_buzz (numbers=[3,4,5],terms=['Fizz','Buzz','Bang'],length=100):
    if len(numbers) != len(terms):
        raise ValueError('illegal arguments, numbers and terms do not match up')
    for i in range(1,length+1):
        output = ""
        for index, item in enumerate(terms):
            if i%numbers[index] == 0:
                output += item
        if output == '':
            output = i
        print(output, end=', ')
fizz_buzz() # called with default parameters
{% endhighlight %}
</details>

<details><summary>Fizz, Buzz, Bang, etc. 4: Click to reveal code</summary>
{% highlight python %}
# Input terms and numbers as dictionary, so we don't have to match inherently connected input arguments
def fizz_buzz (terms_and_numbers={'Fizz':3, 'Buzz':5},length=100):
    for i in range(1,length+1):
        output = ""
        for term, number in terms_and_numbers.items():
            if i%number == 0:
                output += term
        if output == '':
            output = i
        print(output, end=', ')
fizz_buzz({'Awesome':3,'Dude':5},20)
{% endhighlight %}
</details>

## Further (mostly free) resources

In order to get better at coding, as with any skill, you need deliberate praxis.

Please note that this is most definitely not a comprehensive list of resources (a quick internet search will kick up tons more), nor is this a ranking.

* There is a growing number of free introductory courses for Python, each with their own focus.
  * [Codeacademy](https://www.codecademy.com/learn/learn-python) courses run completely in a browser
  * [Data Camp](https://www.datacamp.com/courses/intro-to-python-for-data-science) aims to develop skills needed as a data scientist
  * [Code School](https://www.codeschool.com/courses/try-python) is more aimed towards web-development
* [Introduction to Research Programming (with Python)](http://rits.github-pages.ucl.ac.uk/intro-research-prog/) - by UCL Research IT Services
* Lynda, which you can access for free through UCL, has a whole range of [Python courses from beginner to intermediate](https://www.lynda.com/Python-training-tutorials/415-0.html)
* More full-blown university-style MOOCs educating computer scientists are also widely available, e.g. from [MIT through edX](https://www.edx.org/course/introduction-computer-science-mitx-6-00-1x-11).
* (e)Books/websites:
  * [Automate the boring stuff with Python](https://automatetheboringstuff.com/)
  * [Learn Python the hard way](https://learnpythonthehardway.org/book/preface.html)
* Youtube:
  * [Sentdex](https://www.youtube.com/playlist?list=PLQVvvaa0QuDeAams7fkdcwOGBpGdHpXln)

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>