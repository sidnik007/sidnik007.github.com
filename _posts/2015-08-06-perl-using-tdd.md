---
layout: post
category : lessons
tags : [tdd, perl]
---
{% include JB/setup %}

This tutorial will explain how tdd could be used in case of perl. We would be developing a program that returns a string indicating if a number is prime or not prime. The test uses `Test::Simple`, an extremely simple and basic module for writing test. Please refer (<http://perldoc.perl.org/Test/Simple.html>) for details. There is another module for complicated test, known as `Test::More`, but that will be covered in other blog. For now we will ony focus on `Test::Simple`.

### Prerequiste
The readers should be aware of TDD, i.e. Test Driven Development.

### TDD and three laws of TDD
Before we start, it is important to revise the three laws of Test Driven Development, which are as follows
1. You are not allowed to write any production code unless it is to make a failing unit test pass.
2. You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
3. You are not allowed to write any more production code than is sufficient to pass the one failing unit test.


### Enough of talk, time for some action
Roll up the sleeves and code along, will be much helpful then reading only :)

Let's call our file as PrimeNumberKata.t (What is Kata, will add a blog about it, but for now it is not going to affect your understanding anyway). We start with the `shebang`

{% highlight perl %}
#!/usr/bin/perl
{% endhighlight %}
