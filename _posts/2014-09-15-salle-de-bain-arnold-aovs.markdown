---
layout: post
title:  "Salle de bain: Tweaking Arnold Options and Samples"
date:   2014-09-15 10:41:35
categories: jekyll rendering arnold
---

Let's talk about __optimizing__ [Arnold's][arnold] __render settings__
to bring down rendering time considerable (at least if you compare
vs. renderers like [Luxrender][luxrender], [Indigo][indigo], or
[Maxwell][maxwell]). Basically there are three things you should know
about:

1. __AOVs__: __AOV__ stands for <u>A</u>rbitrary <u>O</u>utput
<u>V</u>ariables and is explained for example in Pixar's
[RenderMan][renderman] [application notes][application-notes].

2. The recommended __noise reduction workflow__ (see image below and
[link][noise-workflow] to Solid Angle's support documentation).

3. Why using a __mesh_light__ node might be preferable over using the
__emission__ parameters of e.g. the __standard__ material shader.

There is another [post][post] about __AOVs__, but basically it allows
you to split the different contributions to the beauty image into
several parts:

{% highlight tcsh %}
options
{
...
 outputs 7 1 STRING

  "RGBA RGBA filter driver" 
  "direct_diffuse RGB filter driver_direct_diffuse" 
  "direct_specular RGB filter driver_direct_specular" 
  "reflection RGB filter driver_reflection" 
  "refraction RGB filter driver_refraction" 
  "indirect_diffuse RGB filter driver_indirect_diffuse" 
  "indirect_specular RGB filter driver_indirect_specular"
...
}
...
gaussian_filter
{
 name filter
}

driver_exr
{
 name driver
 filename "arnold.exr"
}

driver_exr
{
 name driver_direct_diffuse
 filename "arnold_aov_direct_diffuse.exr"
}
...
{% endhighlight %}

Theoretically you could write to a single [OpenExr][openexr] file by
specifying exactly the same name for (and path to) a filename
(e.g. _"arnold.exr"_) within the different __driver_exr__ nodes, but I
prefer to write to separate files.

As mentioned before, there is a more complex [noise reduction
workflow][noise-workflow] you should look at, but for now lets focus
on a simplified version:

<img src="/assets/arnold_aov_noise_reduction.png" alt="Why is it worth
using mesh_lights (instead of standard material's emission)."
width="960" class="img-thumbnail"/>

Basically you try to go step by step from a __noisy__ image on the
upper left to a __noise free__ (or optimized) version on the lower
right. There are decisions to make by looking at the resulting AOV
images (e.g. the __indirect diffuse__) and knowing what parameters to
optimize. For example the first and last decision in the simplified
workflow above tell you to increase either the diffuse or specular
samples. They are part of the global __options__ node:

{% highlight tcsh %}
options
{
...
 GI_diffuse_depth 0
 GI_glossy_depth 0
...
}
{% endhighlight %}

First we are only interested in __direct lighting__, which means that
we don't allow any diffuse or specular (glossy) bouncing by setting
the parameters above to zero. This is the resulting image after
optimizing the individual light samples:

<img src="/assets/salle_de_bain_arnold_direct_lighting.png"
alt="Direct lighting: No bounces for diffuse or specular (glossy)."
width="960" class="img-thumbnail"/>

To be able to tweak sample settings in the scene above we have to use
a __mesh_light__ node for each light emitting geometry. The reason for
that is, that for example the __standard__ material (which could be
used alternatively) has emission settings, but does __not__ provide a
__samples__ parameter.

{% highlight tcsh %}
...
polymesh
{
 name MEmirror_emit
...
 shader "MAmirror_emit_mat"
 opaque on
}
...
mesh_light
{
 name MAmirror_emit_light
 mesh "MEmirror_emit"
 color 1 1 1
 intensity 14
 samples 2
}
...
{% endhighlight %}

So what I did first was to turn each __mesh_light__ node off (by
setting __intensity__ to zero), except one (which I intended to find
good sample values for). Comparing with a renderer (like
[Luxrender][luxrender]) which supports __light groups__ might help in
this case. I ended up with these values by raising the intensities and
samples for each light source independently (this is the output of
__diff__, which I stored as a __.patch__ file in the
[repository][repo]):

{% highlight diff %}
9c9
<  AA_samples 5
---
>  AA_samples 3
...
501285,501286c501285,501286
<  intensity 14
<  samples 2
---
>  intensity 1400 # 14
>  samples 6
675489,675490c675489,675490
<  intensity 10
<  samples 2
---
>  intensity 200 # 10
>  samples 4
675525,675526c675525,675526
<  intensity 4.5
<  samples 2
---
>  intensity 450 # 4.5
>  samples 4
675559,675560c675559,675560
<  intensity 2
<  samples 2
---
>  intensity 200 # 2
>  samples 3
675593,675594c675593,675594
<  intensity 14
<  samples 2
---
>  intensity 1400 # 14
>  samples 5
{% endhighlight %}

Having found good parameter values for each individual __mesh_light__
the next step would be to allow bouncing of the diffuse materials (to
have proper __global illumination__) but to limit the number of times
light bounces off diffuse materials to something reasonable (again,
comparing e.g. against [Luxrender][luxrender] might help). I decided
to let the diffuse lighting bounce __twice__ (otherwise some shadow
areas might be too dark).

{% highlight tcsh %}
options
{
...
 GI_diffuse_depth 2
...
 GI_diffuse_samples 15
...
}
{% endhighlight %}

To find a good value for __GI\_diffuse\_samples__ I started low
(e.g. __three__) and step by step increased that value until I was
satisfied. I use [imf_diff][imf_diff] from [mental ray][mental-ray] to
compare the results from a previous render to the increased quality
(using __false colors__ to emphasize the differences):

<img src="/assets/salle_de_bain_arnold_diff_diffuse_samples.jpg"
alt="Use imf_diff to see what changed (for diffuse) from one run to
the next."  width="960" class="img-thumbnail"/>

So far we didn't allow the specular materials to bounce light:

{% highlight tcsh %}
options
{
...
 GI_diffuse_depth 2
 GI_glossy_depth 0
...
}
{% endhighlight %}

And here is the result after tweaking the __diffuse lighting__:

<img src="/assets/salle_de_bain_arnold_diffuse_sampling.png"
alt="Optimize diffuse settings first." width="960"
class="img-thumbnail"/>

The final task was to find good values for __GI\_glossy\_samples__ and
using [imf_diff][imf_diff] again to watch the differences while doing
so:

<img src="/assets/salle_de_bain_arnold_diff_specular_samples.jpg"
alt="Use imf_diff to see what changed (for specular) from one run to
the next."  width="960" class="img-thumbnail"/>

You can for example write to different OpenExr files for the
__indirect specular AOV__ and compare different
__GI\_glossy\_samples__ values until you are satisfied:

<img src="/assets/salle_de_bain_arnold_specular_sampling.png"
alt="Optimize the specular settings next." width="960"
class="img-thumbnail"/>

The options parameters I ended up with are:

{% highlight tcsh %}
options
{
...
 AA_samples 3
...
 GI_diffuse_depth 2
 GI_glossy_depth 1
...
 GI_diffuse_samples 15
...
 GI_glossy_samples 9
...
}
{% endhighlight %}

An additional trick I use is to render 4 different runs of the same
scene with different seeds and combine the resulting images into an
even less noisy image (because the different seeds lead to different
noise patterns and get averaged). The rendering gets automated by
Makefiles (like this):

{% highlight basemake %}
# usage:
# make clobber
# make -j 4

ARNOLD = /usr/local/Arnold
KICK = $(ARNOLD)/bin/kick

all: \
salle_de_bain_seed_1.exr \
salle_de_bain_seed_2.exr \
salle_de_bain_seed_3.exr \
salle_de_bain_seed_4.exr

# patch

salle_de_bain_patched.ass: salle_de_bain.ass
	patch salle_de_bain.ass -i salle_de_bain.patch -o salle_de_bain_patched.ass

# 4 seeds

salle_de_bain_seed_1.exr: salle_de_bain_patched.ass
	$(KICK) -dp -dw -i salle_de_bain_patched.ass -o salle_de_bain_seed_1.exr \
-set options.AA_seed 1 -v 6

salle_de_bain_seed_2.exr: salle_de_bain_patched.ass
	$(KICK) -dp -dw -i salle_de_bain_patched.ass -o salle_de_bain_seed_2.exr \
-set options.AA_seed 2 -v 6

salle_de_bain_seed_3.exr: salle_de_bain_patched.ass
	$(KICK) -dp -dw -i salle_de_bain_patched.ass -o salle_de_bain_seed_3.exr \
-set options.AA_seed 3 -v 6

salle_de_bain_seed_4.exr: salle_de_bain_patched.ass
	$(KICK) -dp -dw -i salle_de_bain_patched.ass -o salle_de_bain_seed_4.exr \
-set options.AA_seed 4 -v 6

# TODO: combine 4 seeds into beauty
{% endhighlight %}

On my Linux machine that __4 images__ rendered in less than __15
hours__ in 1920x1080 resolution, whereas the same quality might take
double (or even longer) for other renderers. A single seed alone
(which might be already good enough quality-wise) renders with
__Arnold__ in less than __3 hours__.

Feel free to __download__ the scene and make your own tests:

[Repository][repo]:

* __Name:__ [Salle de bain][salle-de-bain]
* __Author:__ [nacimus][nacimus]

[arnold]:            https://www.solidangle.com/arnold
[luxrender]:         http://www.luxrender.net/en_GB/index
[indigo]:            http://www.indigorenderer.com
[maxwell]:           http://www.maxwellrender.com/
[renderman]:         http://renderman.pixar.com/view/about-renderman
[application-notes]: http://renderman.pixar.com/view/Appnote24
[noise-workflow]:    https://support.solidangle.com/display/mayatut/Removing+Noise+Workflow
[post]:              https://www.janwalter.org/jekyll/rendering/arnold/2014/06/03/gallery-day-lighting-arnold.html
[openexr]:           http://openexr.com
[repo]:              https://github.com/wahn/export_multi/tree/master/09_salle_de_bain/ass
[imf_diff]:          http://docs.autodesk.com/MENTALRAY/2012/CHS/mental%20ray%203.9%20Help/files/manual/node251.html
[mental-ray]:        http://www.nvidia-arc.com/mentalray.html
[salle-de-bain]:     http://www.blendswap.com/blends/view/73937
[nacimus]:           http://www.blendswap.com/users/view/nacimus
