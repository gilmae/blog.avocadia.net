---
layout: post
title: "Random Mrandelbot"
date: 2016-12-09 21:45
comments: true
categories: projects coding
featured:
   image: /images/posts/20161209/mandelbrot.jpg
precis: In which I seek how to seek the edge.
published: true
---

![Cover of Oh! Pascal! Turbo Pascal 6.0, by Doug Cooper](/images/posts/20161209/516T559WW3L._SX258_BO1,204,203,200_.jpg)

The text book for my CS101 class at the University of Queensland (1). Within this textbook was a problem set that started with rendering the Mandelbrot set, and then delved into Julia Set variants, and assorted fripperies hanging off the sides. It probably wasn’t the first time I had encountered the Mandelbrot Set render - that would have been a science show on Chaos Theory, of all things - but it stuck with me for years. Every now and then I would go back and build something to render it.

I mean, it’s not like it is a particularly difficult algorithm.

[@randommandelbot](https://twitter.com/randommandelbot) is a twitter bot I wrote in early 2016. The bot randomly generates a rendering of the [Mandelbrot set](https://en.m.wikipedia.org/wiki/Mandelbrot_set). A centre point on the real and imaginary axis is generated and a zoom level. If the result passes a rudimentary boringness filter, it is added to a queue and eventually tweeted. Most do not pass the boringness filter. They are zooms into a zone with no variation, either in the main bulbs or out near the edges of the set. The best images are found very close to the event horizon.  So how to find them?

I took a really naive approach at first with a crude histogram. I had a tool that scanned the entire image and summed the total red, green, and blue values for each pixel, clumped into 16 rows. It was kind of effective although albeit allowing some very marginal renders through.

![A very boring render of the mandelbrot set](/images/posts/20161209/Cxr4GVPUUAAUgk3.jpg)

Because while to me it is boring, to my crude filter it is good enough.

![Output of a histogram of the previous image](/images/posts/20161209/Screen%20Shot%202016-11-20%20at%205.38.06%20pm.png)

I then moved onto using the `identify -verbose` tool that comes with `imagemagick` which seemed like it would be a better choice. Why wouldn’t it be, it wasn’t a crude tool I smashed out like a [Stone Adze](https://primitivetechnology.wordpress.com/2015/07/18/stone-adze/)  (2)  However it turned out it would let have let that smoggy sunset image pass as well. And it got much worse when I started playing with random gradients.

![Another boring render of the Mandelbrot set, looks like TV static](/images/posts/20161209/CzIkvD1VEAMQ82D.jpg)

Clearly I need a better approach. Too many false positives getting through. So wasteful as well. Only 5% of the renders actually pass the test, so to ensure I can keep to the schedule I have to be attempting a render every minute. That also requires me to keep the bounds of the randomisation at a fairly low zoom level or I risk too many deep zooms deep in the black hole or too far out.

### Finding the edges?
I *had* been considering some sort of ray casting solution to find edges of the Mandelbrot set. Cast rays from a random spot on the edge until they either strike the edge of the image or they strike the Mandelbrot set. Then I went to my thinking room (the shower) and my brain laughed at me, called me an idiot and said “Why not something like a [Flood fill algorithm](https://en.wikipedia.org/wiki/Flood_fill) to find **all** of the edges."" Using this I should be able to find the entire edge in any image I have and then randomly pick one of those edge points

1. Generate a plot in memory
2. Find a spot in the plot that is not part of the Mandelbrot set and then recursively visit the neighbouring pixels. If that pixel has already been visited, exhaust that particular search and continue. If the pixel found is part of the Mandelbrot set then record it, but also exhaust immediately.
3. When all threads exhaust, you're left with a record of all the points in the plot that are the edge of the mandelbrot set. Randomly pick one of those recorded points and a random increase in zoom.
4. Iterate until the entire image is the Mandelbrot set, or none of it is. I don't think it is possible to do the later and I am unsure of the former, although I have some renders that are a distinction without a difference.

In practice what I am doing now is actually just doing 20 plots with randomised increases in zoom (x2-6), and discarding anything with a zoom less than x50. I can do this three times a day and end up with about as many images than I need to post every 30 minutes, based on a rule of thumb of 16 images per run being at greater than x50 zoom. Which I am sure you will agree is much better than running it 1440 times a day.

The real benefit is the imagery produced by being able to zoom so much further into the set. In practical terms most images produced by the previous method were at 1 to three orders of magnitude of zoom. Anything greater than that required the centre point to very close to the edge of the set which was unlikely. Now I get images like this.


![Mandelbrot set render, centred at 4.010655E-01 -3.261615E-01i, at zoom 10000](/images/posts/20161209/mb_4.010655E-01_-3.261615E-01_1E+04.jpeg "4.010655E-01 -3.261615E-01i, at zoom 10000")
![Mandelbrot set render, centred at 4.010655E-01 -3.261615E-01i, at zoom 1000000 1000000](/images/posts/20161209/mb_4.010655E-01_-3.261615E-01_1E+06.jpg "4.010655E-01 -3.261615E-01i, at zoom 1000000")
![Mandelbrot set render, centred at -6.01590531831553421e-01 + 4.25746251751493388e-01i at zoom 51306602770.68678](/images/posts/20161209/C0ofWqlUcAEEhXv.jpg "-6.01590531831553421e-01 + 4.25746251751493388e-01i at zoom 51306602770.68678")

### Source
* [Mandelbrot set renderer](https://github.com/gilmae/mandelbrot).
* [Twitter Bot](https://github.com/gilmae/mRandelbot).

----

1) I’ve become a little obsessed with taking a look at this book again. So much so I am paying money (~$16) to get it via an interlibrary loan. From the University of the South Pacific. In Fiji.

2) Seriously though, go watch that. That guy is something else.
