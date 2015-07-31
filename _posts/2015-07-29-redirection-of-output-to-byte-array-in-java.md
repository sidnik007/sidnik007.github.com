---
layout: post
category : lessons
tags : [redirection, java, System.out]
---
{% include JB/setup %}

This tutorial is meant for redirecting the console output which is printed when `System.out.println("Your text to be printed")` to a byte array.


### What could be the need of redirection
Redirecting into an byte array could be neccessary for testing purpose, especially when it comes to legacy code. **Legacy code is the code that does not have test.** And when you are in agile environment, it is a serious crime to change the code without having a test for it. In case of legacy code, what we follow is Characterization test, where the existing output is stored in memory, and once you change a part of code, check if it matches to the stored output. In case of agile, the changes should be small, and the test run should be frequent. Also we follow the TDD approach which says that no production code is to be written unless you write a test that fails, and write only the code that will pass the test. So even in case of Legacy code, TDD approach could be followed. Suppose that the code that you are going to test prints some data on the console, then first store the expected data into a string, and then redirect the output to a byte array and compare the expected String with byte array data whenever to verify the changes. 

### How to redirect
If you have a look at `System` class, (<https://docs.oracle.com/javase/specs/jls/se8/html/jls-17.html#jls-17.5.4>) `out` is the standard output stream  and is of type `PrintStream`. In simple terms this `out` decides where the data is to be redirected. By default the data is redirected to the console, and by changing the the destination it could be redirected to the stream that you want. For that purpose `setOut` method is used, which takes `PrintStream` as a paramater.

**NOTE** Though the `out` is `final`, and according to java when a field is declared `final`, it has to be assigned in declaration or in constructor. But in case of out though `final`, and no constructor is present, still it has a `setOut` method, which contradicts what `final` does. The reason here is because`setOut` if native method and `final` does not work on native methods.

First we need to keep the value of default output stream as a backup. And for that we create a variable of type `PrintStream` and assign it the value of `System.out`.

{% highlight java %}
PrintStream oldOut = System.out;
{% endhighlight %}

Now `oldOut` has the value of default output stream. So the next task is to create a `ByteArrayOutputStream` where the data could be saved.

{% highlight java %}
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
{% endhighlight %}

Now we could use the `setOut` method to set the output stream to byte array.

{% highlight java %}
System.setOut(new PrintStream(outputStream));
{% endhighlight %}

Now after this whatever you print using `System.out.println`, would not be visible on console, and will be redirected and saved in the byte array.

{% highlight java %}
System.out.println("Print the data that you want to redirect");
{% endhighlight %}

Once done with redirecting and saving, we need to set the out to default output stream. The value of default output stream is in `oldOut`.

{% highlight java %}
System.setOut(oldOut);
{% endhighlight %}

And then you are done! Now your byte array has the data that was redirected from the console.
