---
layout: post
title:  "Classroom: How to Use Radiance's Falsecolor"
date:   2015-10-02 16:18:00
categories: jekyll rendering radiance
---

On the [Blender][blender] web site you will find some [demo
files][demo-files]. One of them is a classroom scene by __Christophe
Seux__ and looks like this:

<p class="text-center"><img src="/assets/classroom_cycles.png"
alt="Classroom rendered by Cycles." width="740"
class="img-thumbnail"/></p>

I'm in the process of changing the scene a bit to be able to use my
own [exporter][exporter] and render with various renderers (from the
same Blender scene). The current state of the classroom scene can be
seen here (rendered by [Indigo][indigo]):

<p class="text-center"><img src="/assets/classroom_indigo.png"
alt="Current state of the classroom scene rendered by Indigo." width="740"
class="img-thumbnail"/></p>

If you turn the light layer for the sky off, so only the sun is
illuminating the scene, the result will look like this:

<p class="text-center"><img src="/assets/classroom_indigo_sun.png"
alt="Only the sun contribution for the classroom scene rendered by
Indigo." width="740" class="img-thumbnail"/></p>

I modified the latest Blender release a bit so that my local
executable dumps a __binary file__ to disk with __triangle
information__ while you start rendering with [Cycles][cycles],
Blender's global illumination renderer. As an exercise to learn the
new programming language [Rust][rust] I parse the binary file and convert the
triangle information to the [Radiance][radiance] file format. Right
now I don't deal with materials, but I easily can make some triangles
emit light or setup a sun and sky simulation for [Radiance][radiance]:

<p class="text-center"><img src="/assets/classroom_radiance.png"
alt="Unfinished classroom scene rendered by Radiance without
materials." width="740" class="img-thumbnail"/></p>

All I want to talk about in this blog is how that scene can be used to
create a __falsecolor__ image via Radiance:

<p class="text-center"><img src="/assets/classroom_radiance_lux.png"
alt="Unfinished classroom scene rendered by Radiance using
falsecolor." width="740" class="img-thumbnail"/></p>

First of all you need kind of a __makefile__ for Radiance, called
__classroom.rif__, which I created by hand:

{% highlight tcsh %}
ZONE= I -5.7 4.55 -4.8 3.6 -.1 3.2
scene= classroom.rad
scene= classroom_gensky.rad
VAR= L
DET= H
QUAL= H
AMB= classroom.amb
INDIRECT= 1
view= -vp 2.57252 -4.45 1.095 -vd -1.92564 7.70325 0.32097 -vh 65.238
RESOLUTION= 1920 1080
{% endhighlight %}

The first line describes a bounding box for the scene and tells the
renderer that it's an __interior scene__ (via the capitalized __I__
before the x-, y-, and z-dimensions). The __classroom.rad__ file was
created via my __Rust__ program. The __classroom_gensky.rad__ file I
created via a command line tool called __gensky__:

{% highlight tcsh %}
gensky -ang 28.8 -84.5
{% endhighlight %}

The camera view point (__-vp__), direction (__-vd__), and horizontal
field of view (__-vh__) can be calculated from the Blender camera.

The first __Radiance__ image (without the __falsecolor__ contour
lines) can be rendered with the first two lines of the following
command lines:

{% highlight tcsh %}
rad -n classroom.rif > classroom.sh
sh classroom.sh
# see second rpict line in classroom.sh
rpict -vp ... -i classroom.oct > classroom_irr.unf
pfilt -m .25 -x /3 -y /3 classroom_irr.unf > classroom_irr.hdr
falsecolor -i classroom_irr.hdr -p classroom_1.hdr -cl -n 15 -s 10000 -l Lux > classroom_lux.hdr
{% endhighlight %}

The __rpict__ line is a modified version of the one found in
__classroom.sh__ and basically renames the output file to
__classroom_irr.unf__. But it also specifies __one__ extra parameter
__-i__, which makes Radiance compute __irradiance__ rather than
radiance values, as can be found in the
[documentation][documentation]. The __falsecolor__ command finally
combines the two images (with irradiance and radiance values) into a
single __HDR__ (high dynamic range) file, providing a legend via the
__-l__ [option][option] in a range from 0 to 10000 __Lux__. The
__−cl__ option asks for contour lines, and the __−n__ option can be
used to change the number of contour lines.

All files needed to render with Radiance can be downloaded from
[here][here] or from the [download][download] section, where things
get updated and marked by the date.

[blender]:       https://www.blender.org
[demo-files]:    https://www.blender.org/download/demo-files
[exporter]:      https://bitbucket.org/wahn/blender-add-ons/wiki/io_scene_multi
[indigo]:        http://www.indigorenderer.com
[cycles]:        https://www.blender.org/manual/render/cycles/index.html
[rust]:          https://www.rust-lang.org
[radiance]:      http://radsite.lbl.gov/radiance
[documentation]: http://radsite.lbl.gov/radiance/man_html/rpict.1.html
[option]:        http://radsite.lbl.gov/radiance/man_html/falsecolor.1.html
[here]:          https://www.janwalter.org/Download/Scenes/classroom_rad.tar.gz
[download]:      https://www.janwalter.org/download