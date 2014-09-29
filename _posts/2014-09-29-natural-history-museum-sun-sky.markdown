---
layout: post
title:  "Natural History Museum: Why are Sun & Sky Simulations so Different?"
date:   2014-09-29 12:08:27
categories: jekyll rendering maxwell luxrender
---

Now that we are dealing with more complex [scenes][repo] (regarding
geometry) it might be worth to investigate, why the __colors__ can get
__very different__ for two renderers using more or less the same
simple materials and a __sun & sky simulation__.

I ran a test with [Maxwell][maxwell] over a weekend and let it render
on my laptop (a __MacBook Pro__ - 2.6 GHz Intel Core i7 - Retina,
15-inch, Late 2013) for about __55 hours__:

<img src="/assets/natural_history_museum_maxwell.png" alt="New scene
'Natural History Museum' rendered by Maxwell."  width="650"
class="img-thumbnail"/>

At the same time I was rendering two images of the same scene with
[Luxrender][luxrender] on my __Linux__ desktop machine (Centos release
6.5 on a 8 CPU x86_64 Intel):

<img src="/assets/natural_history_museum_luxrender.png" alt="New scene
'Natural History Museum' rendered by Luxrender."  width="650"
class="img-thumbnail"/>

Both Luxrender images rendered at the same time for about 110 hours,
so each image rendered about __55 hours__ if it would have gotten all
CPUs.

Let's have a look at the histograms of both images (the __left__ one
belongs to __Maxwell__, the __right__ one to __Luxrender__):

<p class="text-center"><img
src="/assets/natural_history_museum_maxwell_histogram.png"
alt="Histogram for Maxwell" width="320" class="img-thumbnail"/> <img
src="/assets/natural_history_museum_luxrender_histogram.png"
alt="Histogram for Luxrender" width="320" class="img-thumbnail"/></p>

I'm giving no answer here, I just want to raise the __question__:

__Why are sun & sky simulations so different?__

In this scene there is a single light source and the
[multi-exporter][multi_exporter] translates a single Blender lamp of
type [Sun][sun-blender] into whatever sun & sky simulation can be used
for the renderer in question. So far I didn't pay much attention to
additional parameters, but I made sure I get the __sun direction__
right. So, hopefully, in a future post I can shed some more light onto
the topic, why we end up with so different colors, and how we can fix
that in the exporter source code.

Credits
-------

* __Challenge #17: Natural History__
* Modeled by __Alvaro Luna Bautista__ and __Joel Andersdon__.
* from [http://www.3drender.com/challenges][3drender_com]

[repo]:           https://github.com/wahn/export_multi/tree/master/10_natural_history_museum
[maxwell]:        http://www.maxwellrender.com
[luxrender]:      http://www.luxrender.net/en_GB/index
[multi_exporter]: https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[sun-blender]:    http://wiki.blender.org/index.php/Doc:2.6/Manual/Lighting/Lamps/Sun
[3drender_com]:   http://www.3drender.com/challenges
