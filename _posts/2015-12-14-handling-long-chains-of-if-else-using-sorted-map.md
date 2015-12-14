---
layout: post
category : lessons
tags : [redirection, java, System.out]
---
{% include JB/setup %}

In this blog I will show you how to handle a long chain of `if-else` using sorted map. To know why `if-else` chains shoud be replaced please refer to my previous blog. 

### When to use sorted map when replacing `if-else`?
There are mutiple ways to handle chains of `if-else`, and which to use when completely depends on the situation. Sorted map could be used when the `if` checks are for greater/less then or equal to `if (x <= 10)`. In this case using enums would not be possible. Even the normal map would not be of any use. We need to store the data in sorted map only.

### How to use sorted map instead of `if-else`?
Let us consider following example of InterestRateCalculator class.

{% highlight java %}
public class InterestRateCalculator {
    public int getInterest(final int amount){
        if (amount <= 1000)
            return 1;
        if (amount <= 2000)
            return 2;
        if (amount <= 3000)
            return 3;
        return 0;
    }
}
{% endhighlight %}

Also find below the test class to run the above program.

{% highlight java %}
public class InterestRateCalculatorTest {

    private InterestRateCalculator interestRateCalculator;

    @Before
    public void givenInterestRateCalculator() throws Exception {
        interestRateCalculator = new InterestRateCalculator();
    }

    @Test
    public void whenAmountIsLessThen1000_thenInterestReturnedIsOnePercent(){
        Assert.assertEquals(1, interestRateCalculator.getInterest(1000));
    }

    @Test
    public void whenAmountIsLessThen2000_thenInterestReturnedIsTwoPercent(){
        Assert.assertEquals(2, interestRateCalculator.getInterest(2000));
    }

    @Test
    public void whenAmountIsLessThen3000_thenInterestReturnedIsThreePercent(){
        Assert.assertEquals(3, interestRateCalculator.getInterest(3000));
    }
}

{% endhighlight %}


Now in the InterestRateCalculator class, if new slab is to be added, then we would need to modify the `if-else` and add another condition. Rather then that we could use a sorted map as shown below.


{% highlight java %}
public class InterestRateCalculator {

    private final SortedMap<Integer, Integer> map = new TreeMap<>();

    public InterestRateCalculator() {
        map.put(1000, 1);
        map.put(2000, 2);
        map.put(3000, 3);
    }

    public int getInterest(final int amount){
        for (final int amountLimit : map.keySet()){
            if (amount <= amountLimit) {
                return map.get(amountLimit);
            }
        }
        return 0;
    }
}
{% endhighlight %}

For am adding the data in the map using a constructor. You could use a new method or class, and add the data in the map. This will keep the core logic safe, and will not be touched whenever you add a new amount slab. Hence OCP is followed.