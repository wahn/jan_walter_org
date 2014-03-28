---
layout: post
title:  "Making Bubbles with Cycles and Luxrender"
date:   2014-03-28 15:43:33
categories: jekyll rendering cycles
---

On the __HtoA__ (Houdini to Arnold) mailing list I found a new
__Arnold shader__, which can create a thin film interference, which is
common in soap bubbles. See [documentation][thinfilmdoc] and example
renderings there.

I was interested if someone has tried to render soap bubbles in
__Cycles__ (the GI renderer within __Blender__) and found two forum
entries on [blenderartists.org][blenderartists]:

* [How to make a bubble material - LuxRender and Cycles][forum1]
* [Cycles - Wavelength Nanometer to Color NodeGroup][forum2]

Here is a little scene I created using the __NanometerColor__ node
group:

<img src="/assets/bubbles_cycles.png" alt="Rendered with Cycles"
width="650" class="img-thumbnail"/>

I modified my Blender __multi-exporter__ to detect __HDR lighting__
within Blender and the first renderer to pick that up is
__Luxrender__. I haven't used the __thin film__ parameter of the
[Luxrender glass material][luxglass] yet, but the HDR lighting works
fine now:

<img src="/assets/bubbles_luxrender.png" alt="Rendered with Luxrender"
width="650" class="img-thumbnail"/>

I have problems using the same bubble model with __Arnold__ using the
__standard__ shader, because I end up with internal reflections and
refractions, but I will post an update in case I find a solution.

[thinfilmdoc]:    http://splicer.tv/?page_id=238 
[blenderartists]: http://blenderartists.org
[forum1]:         http://blenderartists.org/forum/showthread.php?258289-How-to-make-a-bubble-material-LuxRender-and-Cycles
[forum2]:         http://blenderartists.org/forum/showthread.php?257518-Cycles-Wavelength-Nanometer-to-Color-NodeGroup
[luxglass]:       http://www.luxrender.net/wiki/LuxRender_Materials_Glass