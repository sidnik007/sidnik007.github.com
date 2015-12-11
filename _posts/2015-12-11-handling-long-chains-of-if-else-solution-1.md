---
layout: post
category : lessons
tags : [redirection, java, System.out]
---
{% include JB/setup %}

In this blog I will show different ways of handling the long chains of `if-else`. 

### Why care about the long chains of `if-else` in the first place?
A code should always follow **OCP**, i.e **Open Close Principle**. The code should be open for extension, but close for modification. Which means that whenever there is a new requirement, it should be possible to extend the code rather than modifying the existing one. And so long chains of `if-else` voilate OCP, as if you have to add another condition, you have to modidy the existing code, rather then extending it. 

### How could I avoid `if-else`?
There are multiple ways to handle the long chains of `if-else`, here I will show one way using enums.

We will take a factory design pattern as an example. Consider the following example where the PizzaContentFactory returns contents of types of Pizza. An interface PizzaContent with only one method 'printPizzaContent()' will be implemented by different Pizza classes.

{% highlight java %}
public interface PizzaContent {
    void printPizzaContent();
}
{% endhighlight %}

{% highlight java %}
public class CheesePizzaContent implements PizzaContent {
    @Override
    public void printPizzaContent() {
        System.out.println("Contains Cheese");
    }
}
{% endhighlight %}

{% highlight java %}
public class SpicyPineapplePizzaContent implements PizzaContent {
    @Override
    public void printPizzaContent() {
        System.out.println("Contains Spicy Pineapple");
    }
}
{% endhighlight %}

{% highlight java %}
public class ChickenPizzaContent implements PizzaContent {
    @Override
    public void printPizzaContent() {
        System.out.println("Contains Chicken");
    }
}
{% endhighlight %}

Now here comes the class of interest.

{% highlight java %}
public class PizzaContentFactory {
    public PizzaContent getPizzaName(String type){
        if (type == "Cheese")
            return new CheesePizzaContent();
        if (type == "Spicy")
            return new SpicyPineapplePizzaContent();
        if (type == "Chicken")
            return new ChickenPizzaContent();
        return null;
    }
}
{% endhighlight %}

Also find the demo class needed to run the above program to see if is working

{% highlight java %}
public class PrintPizzaContentDemo {
    public static void main(String[] args) {
        PizzaContentFactory getPizzaContent = new PizzaContentFactory();
        PizzaContent cheesePizzaContent = getPizzaContent.getPizzaName("Cheese");
        cheesePizzaContent.printPizzaContent();
        PizzaContent chickenPizzaContent = getPizzaContent.getPizzaName("Chicken");
        chickenPizzaContent.printPizzaContent();
        PizzaContent spicyPineapplePizzaContent = getPizzaContent.getPizzaName("Spicy");
        spicyPineapplePizzaContent.printPizzaContent();
    }
}
{% endhighlight %}

When you run the above program, it should print following

{% highlight java %}
Contains Cheese
Contains Chicken
Contains Spicy Pineapple
{% endhighlight %}

We would be modifying the 'PizzaContentFactory' class. Right now if a new pizza content has to be added, then we have to modify the existing `if-else` in the code. So we use enums instead.

Find below the enum class which contains the types of pizzas.

{% highlight java %}
public enum PizzaContentTypes {
    CHEESE {
        @Override
        PizzaContent returnPizzaObject() {
            return new CheesePizzaContent();
        }
    }, CHICKEN {
        @Override
        PizzaContent returnPizzaObject() {
            return new ChickenPizzaContent();
        }
    }, SPICY {
        @Override
        PizzaContent returnPizzaObject() {
            return new SpicyPineapplePizzaContent();
        }
    };

    abstract PizzaContent returnPizzaObject();
}
{% endhighlight %}

This ofcourse will also change the 'PizzaContentFactory' class.

{% highlight java %}
public class PizzaContentFactory {
    public PizzaContent getPizzaName(PizzaContentTypes type){
        return type.returnPizzaObject();
    }
}
{% endhighlight %}

And the demo class to run the program will be as follows.

{% highlight java %}
public class PrintPizzaContentDemo {
    public static void main(String[] args) {
        PizzaContentFactory getPizzaContent = new PizzaContentFactory();
        PizzaContent cheesePizzaContent = getPizzaContent.getPizzaName(PizzaContentTypes.CHEESE);
        cheesePizzaContent.printPizzaContent();
        PizzaContent chickenPizzaContent = getPizzaContent.getPizzaName(PizzaContentTypes.CHICKEN);
        chickenPizzaContent.printPizzaContent();
        PizzaContent spicyPineapplePizzaContent = getPizzaContent.getPizzaName(PizzaContentTypes.SPICY);
        spicyPineapplePizzaContent.printPizzaContent();
    }
}
{% endhighlight %}

### But still when a new type is introduced, the enum has to be changed. Is that not modification?
Yes! Enum changes everytime you add some pizza type. But there is a difference, and that is you are adding data, and not modifying the core logic. So the core logic stays untouched, data gets added, and everybody lives happily ever after!