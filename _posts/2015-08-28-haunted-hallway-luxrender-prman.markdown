---
layout: post
title:  "Haunted Hallway - Luxrender and PRMan"
date:   2015-08-28 14:30:00
categories: jekyll rendering luxrender
---

In a previous blog post from __25th of August 2015__ we talked about
the __Haunted Hallway__ scene already and showed images rendered with
[Blender][blender]'s global illumination renderer
[Cycles][cycles]. Today I would like to talk about
[Luxrender][luxrender] and [Pixar][Pixar]'s RenderMan implementation
[PRMan][prman20] (version 20).

Let's start with __Luxrender__. On the [forum][forum] I asked for help
how to achieve [god rays][crepuscular] as in the __Cycles__
renderer. I got a response and the resulting file renders like this:

<p class="text-center"><img
src="/assets/hallway_luxrender_volumetric.png" alt="Using Luxrender's
'VolumeIntegrator' for god rays." width="740"
class="img-thumbnail"/></p>

Right now my own exporter does not support volumes (yet), so let's
summarize what was added to the exported __.lxs__ file by hand. 

{% highlight tcsh %}
# Camera
LookAt ...
...
Film "fleximage"
...
  "integer outlierrejection_k" [ 1 ] #changed
...
VolumeIntegrator "single" # new
Accelerator "qbvh" # new
{% endhighlight %}

I don't know why the __outlierrejection\_k__ value changed, but we
need a __VolumeIntegrator__ for sure.

{% highlight tcsh %}
...
WorldBegin
  MakeNamedVolume "dusty air" "homogeneous" # new
	"float fresnel" [1.000000000000000]
	"color g" [0.00000000 0.00000000 0.00000000]
	"color sigma_a" [0.00000000 0.00000000 0.00000000]
	"color sigma_s" [0.00853225 0.00853225 0.00853225]
  MakeNamedMaterial "dusty air"	# new
	"string type" ["null"]
...
{% endhighlight %}

The volume __dusty air__ will be used later. You can raise or lower
the color __sigma\_s__ values for the volume to get more or less
__scattering__.

{% highlight tcsh %}
...
  AttributeBegin # "dust box" # new
  Transform [...]
  NamedMaterial "dusty air"
  Interior "dusty air"
  Shape "mesh" ... "string name" ["dust box"]
  AttributeEnd
...
{% endhighlight %}

A cube called __dust box__, big enough to contain all objects in the
hallway, was added to the scene. Everything inside is filled with
__dusty air__ (therefore the __Interior__).

{% highlight tcsh %}
...
  # light_bulb
  AttributeBegin
    Transform [...]
    NamedMaterial "light_bulb"
    LightGroup "light_bulb"
    Exterior  "dusty air" # new
    AreaLightSource "area" ...
    Shape "mesh" ...
  AttributeEnd
...
{% endhighlight %}

The volume __dusty air__ is used as an __Exterior__ for the __light
bulb__.

{% highlight tcsh %}
...
  Exterior "dusty air" # new
WorldEnd
{% endhighlight %}

The volume __dusty air__ is used as an __Exterior__ for the __camera__
(which is defined before __WorldBegin__).

What's interesting in regard of finding good values for the light
sources for other renderers is the ability of __Luxrender__ to use
__light groups__. While the renderer is rendering you can turn those
light groups on or off to see the effect of a single light, here the
light bulb, emitting __red__ light:

<p class="text-center"><img
src="/assets/hallway_luxrender_volumetric_red.png" alt="Using
Luxrender's light groups to separate red light from the light bulb."
width="740" class="img-thumbnail"/></p>

Here the daylight system (sun and sky), emitting __green__ light,
which is unusual for a daylight system, but you can do that:

<p class="text-center"><img
src="/assets/hallway_luxrender_volumetric_green.png" alt="Using
Luxrender's light groups to separate green light from the daylight
system."  width="740" class="img-thumbnail"/></p>

Actually I defined the colors while running Luxrender and stored the
settings (tonemapping values and light group colors) in a file called
__hallway.ini__:

{% highlight tcsh %}
[tonemapping]
...
[lightgroup_Sun]
...
LG_scaleRed=0.380392156862745
LG_scaleGreen=1
LG_scaleBlue=0.349019607843137
...
[lightgroup_light_bulb]
...
LG_scaleRed=1
LG_scaleGreen=0.172549019607843
LG_scaleBlue=0.101960784313725
...
{% endhighlight %}

You can load that file (during rendering) via __File->Load Panel Settings...__.

Let's have a look at the result we got from Pixar's __PRMan 20.0__
renderer:

<p class="text-center"><img src="/assets/hallway_prman_volumetric.png"
alt="Using Pixar's 'PxrVolume' for god rays." width="740"
class="img-thumbnail"/></p>

There are two Blender files for this scene in the [download][download]
section (via the __Cycles__ link). Beside the changes regarding using
a volume for the god rays I replaced the sun light (using
__PxrEnvDayLight__) by a spot light. This was mainly done for
__Cycles__ which currently can't use a daylight system for volumetric
lighting, but I kept this change for __RenderMan__ as well. So I
replaced this piece of the RIB file:

{% highlight tcsh %}
...
  # Sun
  Attribute "identifier" "string name" ["Sun"]
  AttributeBegin
    AreaLightSource "PxrEnvDayLight" "Sun"
      "float intensity" [0.001]
      "vector direction" [...]
      "float haziness" [2.0]
      "color sunTint" [0.382 1.0 0.34]
    Sides 1
    Opacity [1 1 1]
    Bxdf "PxrLightEmission" "visibleToCamera"
    Rotate 180 0 1 0 # right handed
    Scale -1 1 1 # right handed
    Geometry "envsphere"
  AttributeEnd
...
{% endhighlight %}

The direction of the spot light is pretty similar (moving the light
along the same vector), but the resulting bright spot on the floor is
still not exactly the same (because a spot light has a __position__):

{% highlight tcsh %}
...
  # Spot
  AttributeBegin
    Attribute "identifier" "string name" ["Spot"]
    Transform [
      -0.9844222068786621 -0.17582131922245026 -1.4901162970204496e-08 0.0
      0.13712269067764282 -0.7677489519119263 0.6259064078330994 0.0
      -0.11004770547151566 0.6161562204360962 0.7798981070518494 0.0
      -12.677900314331055 126.31094360351562 120.90076446533203 1.0
    ]
    AreaLightSource "PxrStdAreaLight" "Spot"
      "string rman__Shape" ["spot"]
      "float exposure" [22.0]
      "color lightColor" [0.3819217383861542 1.0 0.3401939868927002]
      "float coneAngle" [10.0]
      "float penumbraAngle" [0.0]
    Bxdf "PxrLightEmission" "Spot_emit"
    Disk 0 0.5 360
  AttributeEnd
...
{% endhighlight %}

Now let's talk about the god rays and how to define a volume to create
those. The volume contains the __whole scene__, including camera's and
lights (in this case the spot light outside the building), not just
geometry:

{% highlight tcsh %}
...
WorldBegin
  AttributeBegin
    Identity
    Bxdf "PxrVolume" "volume"
      "color diffuseColor" [0.8 0.8 0.8]
      "float densityFloat" [0.01]
      "int multiScatter" [1]
    Volume "box"
      [...] # bounding box for the whole scene
      [0 0 0]
  AttributeEnd
...
WorldEnd
{% endhighlight %}

Nothing else is needed for PRMan to make the light scatter in a
__homogeneous volume__ (using a single density value).

Credits
-------

* __Challenge #8: Haunted Hallway__
* Modeled by __Dan Wade__, concept by __Gary Tonge__.
* from [http://www.3drender.com/challenges][3drender_com]

[blender]:        https://www.blender.org/
[cycles]:         https://www.blender.org/manual/render/cycles/index.html
[luxrender]:      http://www.luxrender.net/en_GB/index
[Pixar]:          http://www.pixar.com
[prman20]:        http://renderman.pixar.com/view/renderman20
[forum]:          http://www.luxrender.net/forum/viewtopic.php?f=14&t=12348
[crepuscular]:    https://en.wikipedia.org/wiki/Crepuscular_rays
[download]:       https://www.janwalter.org/download
[3drender_com]:   http://www.3drender.com/challenges
