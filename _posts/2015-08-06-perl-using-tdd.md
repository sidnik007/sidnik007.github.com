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

The plan here is to have different test and source file. Let's call our test file as PrimeNumberTest.t We start with the `shebang` and adding the modules


{% highlight perl %}
#!/usr/bin/perl

use Test::Simple tests => 1;
{% endhighlight %}

We use the `Test::Simple` module, and it needs to have a _plan_, which is provided by  `test => 1`. Basically this test plan consists of the number of test that you have. It has to be pre-defined so the test does not exit in between without completing all the test if something goes wrong. For instance if you have 20 test and the script causes you to exit after test number 10, then what we see is 10 successful assertions indicating the test has passed, which could not be the case. So keep this test number updated with the actual number of test.

Now we add the first test. Remember we have not created our source file and we would not create it even unless we are forced to do so by the test.

{% highlight perl %}
#!/usr/bin/perl

use Test::Simple tests => 1;

ok(isPrime(0) eq "Not prime", "Is zero prime");
{% endhighlight %}

`ok()` is the basic unit of Perl testing. It is used to test the given test case. It will print if the test is "ok" or "not ok" to indicate if the test passed of failed. The format of `ok()` is 

{% highlight perl %}
ok(actual testcase, testname)
{% endhighlight %}

In our case isPrime(0) is the function that we would be testing, and should return a string that should match "Not prime", depending on which our test will decide whether it passed or failed. Also we mention the test name as "Is zero prime" to help us understand what the testcase is doing. Remember keep the test, function names descriptive, rather then adding comment. 

Now run the test, using the following command

{% highlight perl %}
perl PrimeNumberTest.t
{% endhighlight %}

You will get the following error

{% highlight perl %}
1..1
Undefined subroutine &main::isPrime called at PrimeNumberTest.t line 5.
 Looks like your test exited with 255 before it could output anything.
{% endhighlight %}

This is the time to add the source file and function. Let the source file be called PrimeNumber.pm, and add following code in it

{% highlight perl %}
#!/usr/bin/perl

sub isPrime {
    $num = $_[0];
    return "Not prime";
}

1
{% endhighlight %}

Note that we are just returning a string that is expected by the test, rather then having some logic to find the number is prime. According to TDD, strive for the simplest solution.

Also in the test file that is PrimeNumberTest.t add the `use` command to use the PrimeNumber.pm as module. For those who don't know what `use` command does, it is similar as `import` command.
The test file should look as follows

{% highlight perl %}
#!/usr/bin/perl

use Test::Simple tests => 1;
use PrimeNumber;

ok(isPrime(0) eq "Not prime", "Is zero prime");
{% endhighlight %}

Run the test again and you will see following output.

{% highlight perl %}
1..1
ok 1 - Is zero prime
{% endhighlight %}

We see the test has passed. Consider the second test case, lets say for number 1. Number 1 is also not prime, so if we add the test case, this production code will be sufficient to pass this test as well. Hence we directly go for next scenerio, that is number 2, which is prime. 

{% highlight perl %}
#!/usr/bin/perl

use Test::Simple tests => 1;

ok(isPrime(0) eq "Not prime", "Is zero prime");
ok(isPrime(2) eq "Prime", "Is two prime");
{% endhighlight %}

After running the test you will see the following failure

{% highlight perl %}
1..1
ok 1 - Is zero prime
not ok 2 - Is two prime
#   Failed test 'Is two prime'
#   at PrimeNumberTest.t line 7.
# Looks like you planned 1 test but ran 2.
# Looks like you failed 1 test of 2 run.
{% endhighlight %}

Now again we go for a simplest solution. 

{% highlight perl %}
#!/usr/bin/perl

sub isPrime {
    $num = $_[0];
    if($num < 2) {
        return "Not prime";
    } else {
        return "Prime";
    }
}
1
{% endhighlight %}

That should pass your test. 
