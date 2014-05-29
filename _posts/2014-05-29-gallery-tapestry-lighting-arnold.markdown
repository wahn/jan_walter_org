---
layout: post
title:  "Gallery: Tapestry Lighting"
date:   2014-05-29 19:40:00
categories: jekyll rendering arnold
---

In chapter 3.5 of the __Rendering with Radiance__ [book][book] an art
gallery with a small lobby entrance and a showroom is described and
rendered with [Radiance][radiance]. One piece within the showroom is a
__tapestry glass panel__, which is shown below:

<p class="text-center"><img src="/assets/art_gallery_radiance_04.png"
alt="Simple room" width="500" class="img-thumbnail"/></p>

In the original Radiance scene the glass panels do not have a
thickness, they are simply made by __flat polygons__:

{% highlight tcsh %}
...
dk_red_g polygon  l1
0
0
12	-3 	-2	-2
	3	-2	-2
	3	-2	-.5
	-3	-2	-.5
...
{% endhighlight %}

The modified Radiance scene in the GitHub [repository][repo] uses
volumes with a __thickness__ (in this case 0.009 units):

{% highlight tcsh %}
...
!genprism dk_red_g l1 4 \
 -3  -2 \
  3  -2 \
	3  -.5 \
 -3  -.5 \
 -l 0 0 .009 | xform -rx 90 -t 0 -2 0
...
{% endhighlight %}

As you can see in the image below the __glass panels__ are arranged in
front of a white plaster and __lit by 4 lights__ to cast __colored
shadows__. Three lights are arranged in a row a little bit further
away and a single light in front of the glass panels:

<p class="text-center"><img src="/assets/art_gallery_tapestry_blender.png"
alt="Simple room" width="800" class="img-thumbnail"/></p>

Each light is modelled as a cylinder with a closed top as the housing
and a __light emitting disk__ with the normals pointing downwards:

<p class="text-center"><img
src="/assets/art_gallery_typeS4T_blender.png" alt="Simple room"
width="500" class="img-thumbnail"/></p>

To render efficiently in [Arnold][arnold] only a few anti-aliasing
samples were used (__AA_samples__), nodes for the sun&sky simulation
(nodes __physical_sky__ and __skydome_light__) were removed, no
diffuse light bouncing was allowed (__GI\_diffuse\_depth__), and all
other light emitters were turned off, by setting their __standard__
shader __emission__ values to zero:

{% highlight tcsh %}
...
options
{
...
 AA_samples 3
 AA_seed 1
...
 # background "sun_sky"
...
 GI_diffuse_depth 0
...
}
...
polymesh
{
 name MEtypeS4T.d
...
}

mesh_light
{
 name LAtypeS4T_light1
 mesh MEtypeS4T.d
 color 1 1 1
 intensity 5
 samples 3
}
...
standard
{
...
 emission 0 # 1
...
}
{% endhighlight %}

The light emitting disks used already a __polymesh__ instead of a
__disk__ primitive, but to control the __samples__ for each light we
had to use a __mesh_light__ instead of the __standard__ shader with
__emission__.

<p class="text-center"><img src="/assets/art_gallery_arnold_04.png"
alt="Simple room" width="500" class="img-thumbnail"/></p>

To achieve a good quality (and a relative short render time) for the
500x500 image shown above we used another trick by rendering __four
images__ in 2000x2000 resolution with different seed values
(__AA_seed__), e.g. values from 1 to 4, and mixing the resulting
images before scaling the final image down to 500x500. Here is the
relevant part of a __Makefile__ to achieve this:

{% highlight tcsh %}
...
# art_gallery_04

art_gallery_04.ass: art_gallery_04.patch
	patch art_gallery.ass -i art_gallery_04.patch -o art_gallery_04.ass

art_gallery_04.exr: \
art_gallery_04_seed_1.exr \
art_gallery_04_seed_2.exr \
art_gallery_04_seed_3.exr \
art_gallery_04_seed_4.exr
	echo "TODO: combine seed images into a single one"

art_gallery_04_seed_1.exr: art_gallery_04.ass
	$(KICK) -dp -dw -i art_gallery_04.ass -o art_gallery_04_seed_1.exr \
-set options.AA_seed 1 -r 2000 2000 -v 0

art_gallery_04_seed_2.exr: art_gallery_04.ass
	$(KICK) -dp -dw -i art_gallery_04.ass -o art_gallery_04_seed_2.exr \
-set options.AA_seed 2 -r 2000 2000 -v 0

art_gallery_04_seed_3.exr: art_gallery_04.ass
	$(KICK) -dp -dw -i art_gallery_04.ass -o art_gallery_04_seed_3.exr \
-set options.AA_seed 3 -r 2000 2000 -v 0

art_gallery_04_seed_4.exr: art_gallery_04.ass
	$(KICK) -dp -dw -i art_gallery_04.ass -o art_gallery_04_seed_4.exr \
-set options.AA_seed 4 -r 2000 2000 -v 0
...
{% endhighlight %}

Notice the __syntax__ for overwriting the __resolution__ and one of
the parameter values (__AA_seed__) in the __options__ node from the
command line.

[book]:     http://radsite.lbl.gov/radiance/book/index.html
[radiance]: http://radsite.lbl.gov/radiance
[repo]:     https://github.com/wahn/export_multi
[arnold]:   http://www.solidangle.com
