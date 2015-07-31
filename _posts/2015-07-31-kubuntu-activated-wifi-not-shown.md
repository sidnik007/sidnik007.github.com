---
layout: post
category : lessons
tags : [kubuntu, wifi]
---
{% include JB/setup %}

Some devices do not show the list of activated wifi, This solution could probably help in that case.

On the console type following commands

{% highlight bash %}
sudo ifconfig wlan0 down
sudo dhclient -r wlan0
sudo ifconfig wlan0 up
sudo dhclient wlan0
{% endhighlight %}

This should show the available wifi.


