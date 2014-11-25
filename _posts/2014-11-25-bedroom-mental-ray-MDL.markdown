---
layout: post
title:  "Bedroom: Mental Ray using MDL"
date:   2014-11-25 18:00:04
categories: jekyll rendering mental_ray
---

After adjusting the [export_multi.py][multi-exporter] script to export
__light emitting objects__ and a __sun/sky__ system, we can
render the __bedroom scene__ with [mental ray][mental-ray] using
[MDL][mdl] materials (see NVIDIA's technical documentation about their
__Material Definition Language__):

<img src="/assets/bedroom_mental_ray01.png" alt="Bedroom rendered by
mental ray (using MDL)." width="600" class="img-thumbnail"/>

Unfortunately the scenes are getting too big for the
[repository][repo], so I provide download links in various (renderer
specific) file formats [here][download-scenes].

Let's just have a quick look at parts of the resulting __bedroom.mi__
file ([mental ray][mental-ray] specific scene description):

{% highlight tcsh %}
...
# sun
shader "sun_sky" "mdl::base::sun_and_sky" (
  "on" on,
  "multiplier" 0.025,
  "rgb_unit_conversion" 0.00666667 0.00666667 0.00666667,
...
  "physically_scaled_sun" on
)
...
{% endhighlight %}

The __intensity__ of the __sun__ light can be controlled by adjusting
the __rgb\_unit\_conversion__ color parameter.

{% highlight tcsh %}
...
# Lamp_Bulb_mat
shader "Lamp_Bulb_mat_mat" "physical_light" (
  "color" 50000.0 50000.0 50000.0
)
...
{% endhighlight %}

The __intensity__ of the __light bulb__ is dependent on the __color__
of the __physical_light__ shader. Later this light emitter will be
replaced by it's MDL counterpart, but for now that's how it works.

Credits
-------

* __Challenge #21: The Bedroom__
* Modeled by __David Vacek__, design by __David Tousek__.
* from [http://www.3drender.com/challenges][3drender_com]
* some textures from [http://www.cgtextures.com][cgtextures_com]

[multi-exporter]:  https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[mental-ray]:      http://www.nvidia-arc.com/mentalray.html
[mdl]:             http://www.nvidia-arc.com/products/iray/technical-documentation.html
[repo]:            https://github.com/wahn/export_multi/tree/master/11_bedroom
[download-scenes]: https://www.janwalter.org/download
[3drender_com]:    http://www.3drender.com/challenges
[cgtextures_com]:  http://www.cgtextures.com
