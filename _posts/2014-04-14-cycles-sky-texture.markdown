---
layout: post
title:  "Cycles' Sky Texture"
date:   2014-04-14 16:01:13
categories: jekyll rendering cycles
---

[Cycles][cycles], Blender's internal GI renderer, provides a [Sky
Texture][sky_texture] node to draw a background sky, using the
__Preetham__ or __Hosek/Wilkie__ model.

We have already a [Blender][blender] scene in our [repository][repo],
which we can use to test this node. After loading the scene into
Blender we can switch to camera 4 (__cam4__) and to the __Cycles__
render engine:

<img src="/assets/switch_to_cam4.png" alt="Switch to camera 4" width="294"
class="img-thumbnail"/>
<img src="/assets/switch_to_cycles.png" alt="Switch to cycles" width="158"
class="img-thumbnail"/>

To be able to create a __Sky Texture__ node we have to go to the world
settings and press the __Use Nodes__ button:

<img src="/assets/cycles_world_use_nodes.png" alt="Cycles world use
nodes" width="305" class="img-thumbnail"/>
<img src="/assets/cycles_sky_texture_node.png" alt="Cycles sky texture
node" width="290" class="img-thumbnail"/>

After that we will be able to select the __Sky Texture__ as color
input. What's not so easy is to adjust the __vector__ so the sky
matches our __sun__ light. Luckily someone provides a [sun position
extension][sun_position] to do exactly that.

After installing the extension we should be able to enable the
extension's user interface under the __Sun Position__ world settings
and match __Radiance's__ [gensky] settings:

{% highlight tcsh %}
# gensky 3 20 10 -a 40 -o 98 -m 105
# Local solar time: 10.34
# Solar altitude and azimuth: 43.3 -35.4
# Ground ambient level: 14.6

void light solar
0
0
3 6.73e+06 6.73e+06 6.73e+06

solar source sun
0
0
4 0.421409 -0.593560 0.685639 0.5
...
{% endhighlight %}

So the first two parameters of the __gensky__ command refer to the
date (__March 20th__). The local time (__10:00am__) is the third
parameter, and __latitude__ and __longitude__ follow (be aware of the
__negative__ value for the longitude setting).

<img src="/assets/sun_position_extension_gui.png" alt="GUI for the sky
texture node" width="360" class="img-thumbnail"/>

To double check that we set the __UTC zone__ correctly we can compare
the vector values from Radiance's __source__ statement (see above)
with the sun location in Blender's user interface (select the sun and
check the transform settings). Because we did set up a __constraint__
between the sun and the __compass__ object, which resides in the
origin, those values should match if we normalize the resulting vector
(between the sun and the compass locations).

<img src="/assets/sun_location_check.png" alt="Check the sun location"
width="352" class="img-thumbnail"/>

Obviously we would have to adjust all the materials in the scene as
well to render with Cycles and compare against other GI renderers.

[cycles]:       http://wiki.blender.org/index.php/Doc:2.6/Manual/Render/Cycles
[blender]:      http://www.blender.org
[repo]:         https://github.com/wahn/export_multi
[sky_texture]:  http://wiki.blender.org/index.php/Doc:2.6/Manual/Render/Cycles/Nodes/Textures#Sky_Texture
[sun_position]: http://wiki.blender.org/index.php/Extensions:2.6/Py/Scripts/3D_interaction/Sun_Position
[gensky]:       http://radsite.lbl.gov/radiance/man_html/gensky.1.html