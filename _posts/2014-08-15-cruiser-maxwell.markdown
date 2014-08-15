---
layout: post
title:  "The Cruiser: The Scene and a First Image Rendered by Maxwell"
date:   2014-08-14 11:48:08
categories: jekyll rendering maxwell
---

This model was created by __Saba Rofchaei__ and __Greg Ward__ under
contract from the U.S. Navy. It is an H-shaped deck space with three
lighting configurations (see __README__ file and credits in the
[repository][repo]). Here is a floor plan for the geometry visible in
various camera perspectives:

<img src="/assets/07_cruiser_plan.png" alt="Floor plan for the cruiser
scene" width="650" class="img-thumbnail"/>

<img src="/assets/07_cruiser_bow_in_elev.png" alt="Orthograhpic view
for bow side (inwards)" width="650" class="img-thumbnail"/>

<img src="/assets/07_cruiser_bow_out_elev.png" alt="Orthograhpic view
for bow side (outwards)" width="650" class="img-thumbnail"/>

<img src="/assets/07_cruiser_stern_in_elev.png" alt="Orthograhpic view
for stern side (inwards)" width="650" class="img-thumbnail"/>

<img src="/assets/07_cruiser_stern_out_elev.png" alt="Orthograhpic
view for stern side (outwards)" width="650" class="img-thumbnail"/>

The images above were rendered with the interactive
[Radiance][radiance] viewer __rvu__ (see [man page][man_page]) and
there is a __Makefile__ with a rule __view_all__ which can be used to
render even more camera perspectives of the same scene. This assumes
that you have Radiance installed on your computer.

{% highlight tcsh %}
% cd 07_cruiser/rad
% make view_all
{% endhighlight %}

The import into [Blender][blender] was not fully automated, but I step
by step imported geometry into Blender, cleaned up (e.g. by combining
several meshes into a single mesh), and slowly assembled the full
scene by mirroring geometry from one side of the ship to the other.

There are still __textures missing__ and the text used on signs
etc. has to be either modelled or replaced by textures. Anyway, here
is a rendering done with [Maxwell][maxwell] using [depth of
field][wikipedia]:

<img src="/assets/07_cruiser_maxwell_port_bow_stairs.png" alt="First
image rendered by Maxwell" width="650" class="img-thumbnail"/>

[repo]:      https://github.com/wahn/export_multi/tree/master/07_cruiser
[radiance]:  https://en.wikipedia.org/wiki/Radiance_%28software%29
[man_page]:  http://radsite.lbl.gov/radiance/man_html/rvu.1.html
[blender]:   http://www.blender.org
[maxwell]:   http://www.maxwellrender.com
[wikipedia]: https://en.wikipedia.org/wiki/Depth_of_field