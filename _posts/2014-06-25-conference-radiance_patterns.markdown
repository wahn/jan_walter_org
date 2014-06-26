---
layout: post
title:  "Conference Room: How to Create Radiance Patterns for Other Renderers"
date:   2014-06-25 16:18:43
categories: jekyll rendering radiance
---

The [Conference Room][conf-room] is a very famous scene, originally
created for and rendered by [Radiance][radiance]. The room does (or at
least did) exist in reality and was painstakingly measured and
re-created in a text editor ([vi][vi]). See __README__ file and
credits in the [repository][repo], where I keep my test scenes in
various formats:

<img src="/assets/conference_room_radiance_shaft.png" alt="Original
scene rendered by Radiance" width="650" class="img-thumbnail"/>

What's missing in the current state of my
[multi-exporter][multi_exporter] is the ability to re-create the __3D
textures__ or __patterns__ being used in the scene. If you compare
with the [Luxrender][luxrender] rendering below, you will notice that
the ceiling, the red chairs, the table, the podium, and other objects
in the scene use patterns to modify the color of a material based on
the position of the geometry in space.

<img src="/assets/conference_room_luxrender_shaft.png" alt="Exported
scene rendered by Luxrender" width="650" class="img-thumbnail"/>

So, let's have a look at one of those __patterns__ by using the
[reference manual][ref_manual] of Radiance and trying to understand
how those patterns modify existing __materials__.

First of all we have to patch the checked in versions of __*.rad__
files to end up with something Radiance can render, we provide a
__Makefile__ for that:

{% highlight tcsh %}
cd ~/git/github/export_multi/04_conference_room/rad/
make apply_patches
{% endhighlight %}

After that we can open the file __table.rad__ and see how the materials
and patterns are applied:

{% highlight tcsh %}
# description of a small table of the conference room
# measures are in inches

void brightfunc xwood_pat
4 xgrain woodpat.cal -s .4
0
1                0.6

xwood_pat plastic table_mat
0
0
5 .29 .15 .13 .005 .1

!genprism table_mat table ...
...
{% endhighlight %}

The call to __genprism__ (see [list of all Radiance
programs][gen_rad]) generates the geometry for the table. The next
string (__table_mat__) defines the material name, and the __table__
string names the geometry itself. In the [reference
manual][ref_manual] we find the description of the __plastic__
materials being used:

{% highlight tcsh %}
mod plastic id
0
0
5 red green blue spec rough
{% endhighlight %}

Plastic is a material with uncolored highlights. It is given by its
__RGB__ reflectance, its fraction of __specularity__, and its
__roughness__ value. The basic format of the scene description starts
with a __modifier__ (in this case __xwood_pat__), the __type__ (of
e.g. a __material__), and an __identifier__. Basically the plastic
material color gets modified by another element, called
__brightfunc__, which is a pre-defined __pattern__:

{% highlight tcsh %}
mod brightfunc id
2+ refl funcfile transform
0
n A1 A2 .. An
{% endhighlight %}

All the magic happens in the __funcfile__ called __woodpat.cal__:

{% highlight tcsh %}
{
	Wood grain pattern:

	A1 - magnitude (0 to 1)
}

xgrain = woodgrain(Ring(Py,Pz));	{ along x axis }
ygrain = woodgrain(Ring(Px,Pz));	{ along y axis }
zgrain = woodgrain(Ring(Px,Py));	{ along z axis }

woodgrain(r) = hermite(.6-A1/2,.6+A1/2,2,.5,2*tri(r,.5));

Ring(a,b) = sqrt( 25 + sq(mod(a,50)-25) + sq(mod(b,50)-25)) +
		  7 * fnoise3(Px/40,Py/40,Pz/40) ;
{% endhighlight %}

This is like writing a shader for other renderers. There are
predefined names for the position a ray has hit on the geometry we try
to shade, like __Px__, __Py__, and __Pz__, and __functions__ which are
__implemented in C__, like __hermite__, __tri__, __sqrt__, __sq__, and
__fnoise3__. __A1__ is a shader parameter handed in by the scene
description, in this case the value is 0.6. __Ring__ and __woodgrain__
are entirely defined within the __woodpat.cal__ file, and __xgrain__,
__ygrain__, and __zgrain__ are variations, which can be chosen from
via the scene description format, in our case we use __xgrain__.

For fun I implemented an [Arnold][arnold] shader which creates the
same pattern, the C functions mentioned above were directly taken from
the Radiance source code and are omitted here, but some code fragmets
for Arnold are shown (to see how it translates from the
__woodpat.cal__ file to Arnold's __C/C++ API__):

{% highlight c %}
#include <ai.h>
#include <cstring>

#define XGRAIN 0
#define YGRAIN 1
#define ZGRAIN 2

static const char* enum_direction[] = {
  "xgrain", "ygrain", "zgrain", NULL
};

AI_SHADER_NODE_EXPORT_METHODS(RadWoodpatMethods)

enum RadWoodpatParams
{
  p_Kd_color,
  p_magnitude,
  p_direction,
  p_scale,
  p_rotation
};

node_parameters
{
  AiParameterRGB("Kd_color", 1.0f, 1.0f, 1.0f);
  AiParameterFLT("magnitude", 1.0f);
  AiParameterEnum("direction", XGRAIN, enum_direction);
  AiParameterPNT("scale", 1.0f, 1.0f, 1.0f);
  AiParameterPNT("rotation", 0.0f, 0.0f, 0.0f);
}

...

///////////////////////////////////////////////////////////////////////////////
// code from Radiance /////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////////////////

...

// Ring(a,b) = (sqrt(25 + sq(mod(a,50)-25) + sq(mod(b,50)-25)) + 7 *
// fnoise3(Px/40,Py/40,Pz/40));
double
Ring(double a, double b, double Px, double Py, double Pz) {

  double a2 = (a - floor(a/50.0) * 50.0) - 25.0;
  double b2 = (b - floor(b/50.0) * 50.0) - 25.0;
  double p[3] = { Px/40.0, Py/40.0, Pz/40.0 };
  return (sqrt(25.0 + (a2*a2) + (b2*b2)) +
          7.0 * fnoise3(p));
}

// woodgrain(r) = hermite(.6-A1/2,.6+A1/2,2,.5,2*tri(r,.5));
double
woodgrain(double r, float magnitude) {
  double A1 = (double) magnitude;
  // from rayinit.cal:
  // mod(n,d) : n - floor(n/d)*d;
  double m = (r - 0.5) - floor((r - 0.5) / 1.0)*1.0;
  // tri(n,d) : abs( d - mod(n-d,2*d) );
  double tri = (0.5 - m);
  // abs(x) : if( x, x, -x );
  if (tri < 0.0) tri = -tri;
  return hermite(0.6 - A1 / 2.0, 0.6 + A1 / 2.0, 2.0, 0.5, 2.0 * tri);
}

///////////////////////////////////////////////////////////////////////////////
// code from Radiance /////////////////////////////////////////////////////////
///////////////////////////////////////////////////////////////////////////////

shader_evaluate
{
  AtColor Kd_color = AiShaderEvalParamRGB(p_Kd_color);
  float magnitude = AiShaderEvalParamFlt(p_magnitude);
  const int direction = AiShaderEvalParamEnum(p_direction);
  AtPoint scale = AiShaderEvalParamPnt(p_scale);
  AtPoint rotation = AiShaderEvalParamPnt(p_rotation);
  AtPoint P = sg->P;
  AtPoint Ps = P;
  Ps.x = Ps.x / scale.x;
  Ps.y = Ps.y / scale.y;
  Ps.z = Ps.z / scale.z;
  // see woodpat.cal (e.g. export_multi/04_conference_room/rad/woodpat.cal)
  double grain = 1.0;
  double ring = 0.0;
  switch (direction) {
  case XGRAIN:
    ring = Ring(Ps.y, Ps.z, Ps.x, Ps.y, Ps.z);
    grain = woodgrain(ring, magnitude); // magnitude = A1
    break;
  case YGRAIN:
    ring = Ring(Ps.x, Ps.z, Ps.x, Ps.y, Ps.z);
    grain = woodgrain(ring, magnitude); // magnitude = A1
    break;
  case ZGRAIN:
    ring = Ring(Ps.x, Ps.y, Ps.x, Ps.y, Ps.z);
    grain = woodgrain(ring, magnitude); // magnitude = A1
    break;
  default:
    AiMsgError("[rad_woodpat] direction has to be in [\"%s\", \"%s\", \"%s\"].",
               enum_direction[0], enum_direction[1], enum_direction[2]);
    break;
  }
  sg->out.RGB = Kd_color * grain;
}
...
{% endhighlight %}

Here is the table top rendered in Radiance's interactive viewer
__rvu__ on the left and the __Arnold__ shader on the right:

<img src="/assets/rad_woodpat.png" alt="Pattern rendered in Radiance
vs. Arnold" width="650" class="img-thumbnail"/>

After compiling the shader you can use __kick__ (the Arnold
executable) to get infos about the shader parameters and types, or
which strings an __ENUM__ parameters accepts:

{% highlight tcsh %}
% kick -info rad_woodpat
node:         rad_woodpat
type:         shader
output:       RGB
parameters:   6
filename:     ./rad_woodpat.so
version:      4.2.0.5

Type          Name                              Default
------------  --------------------------------  --------------------------------
RGB           Kd_color                          1, 1, 1
FLOAT         magnitude                         1
ENUM          direction                         xgrain
POINT         scale                             1, 1, 1
POINT         rotation                          0, 0, 0
STRING        name                              

% kick -info rad_woodpat.direction
node: rad_woodpat
param: direction
type: ENUM
default: xgrain
enum values: xgrain ygrain zgrain 
{% endhighlight %}

But how does that pattern (or the Arnold shader) get applied in an
__.ass__ (Arnold's scene description) file? Here is a snippet of an
example file:

{% highlight tcsh %}
rad_woodpat
{
 name rad_woodpat
 Kd_color 0.289999992 0.150000006 0.129999995
 magnitude 0.6
 direction "xgrain"
 scale .4 .4 .4
 rotation 0 0 0
}

standard
{
 name MAtable_mat
 Kd 0.995000005
 Kd_color rad_woodpat
 ##Kd_color 0.289999992 0.150000006 0.129999995
 Ks 0.00499999989
 Ks_color 1 1 1
 specular_roughness 0.100000001
 Kr 0.00499999989
 Kr_color 1 1 1
}
{% endhighlight %}

Basically, instead of specifying three RGB values for the __standard__
shader parameter __Kd_color__, we link to the new shader called
__rad_woodpat__ which uses the original __Kd_color__ values as one of
the input parameters and modifies the color based on the calculated
(monochromatic) pattern.

[conf-room]:          https://www.janwalter.org/renderforum/index.php?topic=15.0
[radiance]:           http://radsite.lbl.gov/radiance
[vi]:                 https://en.wikipedia.org/wiki/Vi
[repo]:               https://github.com/wahn/export_multi/tree/master/04_conference_room
[multi_exporter]:     https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[luxrender]:          http://www.luxrender.net/en_GB/index
[ref_manual]:         http://radsite.lbl.gov/radiance/refer/ray.html#Index
[gen_rad]:            http://radsite.lbl.gov/radiance/whatis.html
[arnold]:             http://www.solidangle.com
