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

To improve the image above I added a couple of things (by hand for
now). First, the part where both bubbles overlap in pixel space is too
dark. This can be improved by raising the __ray depth__ (which wasn't
specified at all in the above example):

{% highlight tcsh %}
...
SurfaceIntegrator "bidirectional"
	"integer eyedepth" [16]
	"integer lightdepth" [16]
	"integer lightraycount" [1]
	"string lightpathstrategy" ["auto"]
	"string lightstrategy" ["auto"]
WorldBegin
...
{% endhighlight %}

Similar to the __Cycles__ node graph I use some noise to perturb the
__thinfilm__ nano meter setting:

{% highlight tcsh %}
...
Texture "Material.002::Distorted Noise Texture" "float" "blender_distortednoise"
	"string noisebasis" ["original_perlin"]
	"string type" ["original_perlin"]
	"float noisesize" [0.25]
	"float distamount" [0.37]
	"float nabla" [0.025]
	"float bright" [1.0]
	"float contrast" [1.0]

Texture "Material.002::Scale" "float" "scale"
	"texture tex1" ["Material.002::Distorted Noise Texture"]
	"float tex2" [384.0] # 2 * 192

MakeNamedMaterial "Material"
  "string type" [ "glass" ]
	"bool architectural" ["false"]
  "float index" [ 1.33 ]
  "float filmindex" [ 1.33 ]
  "float film" ["Material.002::Scale"] # [ 192.0 ]
  "color Kr" [ 0.8 0.8 0.8 ]
  "color Kt" [ 1.0 1.0 1.0 ]
...
{% endhighlight %}

That make's it look a bit more like the __Cycles__ counterpart:

<img src="/assets/bubbles_luxrender_noise_base_film_nano.png"
alt="Noise based film nano variations" width="650"
class="img-thumbnail"/>

I have problems using the same bubble model with __Arnold__ using the
__standard__ shader, because I end up with internal reflections and
refractions, but I will post an update in case I find a solution.

[thinfilmdoc]:    http://splicer.tv/?page_id=238 
[blenderartists]: http://blenderartists.org
[forum1]:         http://blenderartists.org/forum/showthread.php?258289-How-to-make-a-bubble-material-LuxRender-and-Cycles
[forum2]:         http://blenderartists.org/forum/showthread.php?257518-Cycles-Wavelength-Nanometer-to-Color-NodeGroup
[luxglass]:       http://www.luxrender.net/wiki/LuxRender_Materials_Glass