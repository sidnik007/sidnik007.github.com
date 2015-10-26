---
layout: post
category : lessons
tags : [aspect ratio, width, height]
---
{% include JB/setup %}

The images if not in correct aspect ratio on website, could look stretched. Following is the solution for the issue. You will need some maths, a calculator, and some patience.

Basically the image is always represented as width x height. On Linux you can use `file` command to view the image size.

{% highlight bash %}
shinchan@office:~$ file image.jpg 
image.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, progressive, precision 8, 3362x2241, frames 3
{% endhighlight %}

So what exactly is the aspect ratio? It is just ratio of width/height.

If the aspect ratio of the image does not match with the aspect ratio of viewport, then the image looks stretched.

To solve the problem, first calculate the aspect ratio of the viewport. Just divide width by height and you have it. Now check the aspect ratio of the image. As you are reading this post I am assuming both aspect ratio do not match. So now we have to change the aspect ratio of the image. 

As you already have seen, the aspect ratio is the division of width and height. So we need to change rather width or height of the image, that means we have to cut the image.

So now we have to calculate by what amount the image should be cut. We could keep height constant and change the width or we could keep the width constant and change the height. 

If width is constant then multiply the height of image by aspect ratio of the viewport, and you will get the new height.

If height is constant then divide the width of image by aspect ratio of the viewport, and you will get the new width.

Once you have the new height or width, On Linux you can use `pnmcut` command and cut the image so that it scales down to the new width and height. Now that image could be used in viewport.

Fine fine no need to thank me ;)

