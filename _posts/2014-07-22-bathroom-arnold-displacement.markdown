---
layout: post
title:  "Bathroom: Using noise in a displacement shader for Arnold"
date:   2014-07-22 16:05:44
categories: jekyll rendering arnold
---

The __Creston Bathroom Model__ was created by __Greg Ward__, the
original author of [Radiance][radiance] as part of a bathroom
renovation project. Different materials and paints were used to give
the bathroom various appearances, but we will focus on an Arnold
rendering task regarding the shower door used in that scene. Here are
two different perspectives where you can see parts of the shower door,
rendered in __Radiance__:

<p class="text-center"><img src="/assets/bathroom_radiance_mir.png"
alt="Shower door visible in mirror" width="300"
class="img-thumbnail"/> <img src="/assets/bathroom_radiance_stall.png"
alt="Shower for on the right side" width="300"
class="img-thumbnail"/></p>

Let's first isolate the door to render a simpler scene with
__Radiance__. The lighting isn't that great, but we can see how a
__simple polygon__ is used to __reflect light__ from a spherical light
source as well as __scattering__ the __colors__ of geometry from
behind the door:

<p class="text-center"><img
src="/assets/shower_glass_door_displacement_radiance.png"
alt="Simplified Radiance scene with the shower door" width="346"
class="img-thumbnail"/></p>

Here is the part of the Radiance file which describes the __polygon__
and the __material__ being used:

{% highlight tcsh %}
...
void texfunc shdoor_texture
4 .5*noise3a(5*Px,5*Py,5*Pz) .5*noise3b(5*Px,5*Py,5*Pz)
	.5*noise3c(5*Px,5*Py,5*Pz) .
0
0

shdoor_texture glass shdoor_mat
0
0
3 .9 .9 .9
...
shdoor_mat polygon door
0
0
12
	0	0	.2
	0	0	56
	0	33	56
	0	33	.2
...
{% endhighlight %}

This __noise functions__ can actually easily be translated into an
[Arnold][arnold] shader. I will not show the source code for the
shader here, but the output of __kick -info rad_noiseABC__:

{% highlight tcsh %}
% kick -info rad_noiseABC
node:         rad_noiseABC
type:         shader
output:       VECTOR
parameters:   7
filename:     ./rad_noiseABC.so
version:      4.2.0.6

Type          Name                              Default
------------  --------------------------------  --------------------------------
FLOAT         magnitudeA                        1
FLOAT         magnitudeB                        1
FLOAT         magnitudeC                        1
BOOL          use_Pref_matrix                   true
VECTOR        scale                             1, 1, 1
POINT         rotation                          0, 0, 0
STRING        name                              
{% endhighlight %}

For all the Radiance __patterns__ we have seen so far, we always
modified an __input color__ and used the __standard__ shader for the
actual material definition and rendering. So, instead of returning
__RGB__ values, we return a __VECTOR__, which can be used for
__displacements__.

{% highlight tcsh %}
...
polymesh
{
 name MEshower_door
...
 subdiv_type "catclark"
 subdiv_iterations 4
 disp_map "MAshdoor_disp"
 disp_padding 0.0125
 shader "MAshdoor_mat"
 opaque off
}

rad_noiseABC
{
 name MAshdoor_disp
 magnitudeA 0.00127 # 0.05 * 0.0254 (inch to meters)
 magnitudeB 0.00127 # 0.05 * 0.0254 (inch to meters)
 magnitudeC 0.00127 # 0.05 * 0.0254 (inch to meters)
 use_Pref_matrix false
 scale 0.127 0.127 0.127 # 5 * 0.0254 (inch to meters)
 rotation 0 0 0
}

standard
{
 name MAshdoor_mat
 Kd 0
 Kd_color 0.899999976 0.899999976 0.899999976
 Ks 0
 Ks_color 1 1 1
 specular_roughness 0.5
 Kr 0.0799999982
 Kr_color 1 1 1
 Kt 0.920000017
 Kt_color 1 1 1
 IOR 1.51999998
}
...
{% endhighlight %}

From the partial __.ass__ file (Arnold's scene description) above you
can see how a __polymesh__ can be converted into a
[Catmull–Clark][catmull–clark] __subdivision surface__, by simply
adding values for __subdiv_type__ and __subdiv_iterations__. So, let's
see how the number of polygons created by the subdivison scheme will
influence the rendering of the displacement:

<p class="text-center"><img
src="/assets/shower_glass_door_displacement_arnold01.png"
alt="Starting with a low polygon count and slowly increasing it"
height="512" class="img-thumbnail"/></p>

You can see how the __polygon count__ influences the resulting
displacement, even though it's exactly the same shader with the same
parameters. This behaviour of __Arnold__ is very different from the
__Radiance__ renderer, so let's increase the number of polygons even
more:

<p class="text-center"><img
src="/assets/shower_glass_door_displacement_arnold02.png"
alt="Increasing the polygon count even more"
height="512" class="img-thumbnail"/></p>

I'm not showing the resulting polygons here (from the Catmull–Clark
subdivision scheme), but I figured out that I get the best results for
this noise based displacement shader, if I have a evenly distributed
numer of polygons with approximately the same size. This can be seen
best within [Blender][blender], by applying a __subdivison modifier__:

<p class="text-center"><img
src="/assets/shower_glass_door_displacement_blender.png"
alt="Screenshot from Blender to visualize the subdivision polygons"
height="500" class="img-thumbnail"/></p>

Don't forget to __remove__ the subdivison modifier before exporting
because the __subdivison__ steps __should be done by__ Arnold, the
__renderer__, not by Blender. But you can get a feeling for the
distribution and amount of polygons by using temporarily a modifier
before exporting the control mesh. Here the results of the
displacement shader rendered by Arnold:

<p class="text-center"><img src="/assets/bathroom_arnold_mir.png"
alt="Shower door visible in mirror" width="300"
class="img-thumbnail"/> <img src="/assets/bathroom_arnold_stall.png"
alt="Shower for on the right side" width="300"
class="img-thumbnail"/></p>

As usual the scene files are available in a [repository][repository].

[radiance]:      https://en.wikipedia.org/wiki/Radiance_%28software%29
[arnold]:        https://www.solidangle.com/arnold
[catmull–clark]: https://en.wikipedia.org/wiki/Catmull%E2%80%93Clark_subdivision_surface
[blender]:       http://www.blender.org
[repository]:    https://github.com/wahn/export_multi/tree/master/05_bathroom
