---
layout: post
title:  "Conference Room: Results for Arnold showing Radiance Patterns"
date:   2014-07-04 12:09:43
categories: jekyll rendering arnold
---

The [Conference Room][conf-room] is a very famous scene, originally
created for and rendered by [Radiance][radiance]. The room does (or at
least did) exist in reality and was painstakingly measured and
re-created in a text editor ([vi][vi]). See __README__ file and
credits in the [repository][repo], where I keep my test scenes in
various formats. Here are the (current) results for [Arnold][arnold]:

<img src="/assets/conference_room_arnold_current.png" alt="Camera
'current' rendered by Arnold" width="650" class="img-thumbnail"/>

<img src="/assets/conference_room_arnold_door1.png" alt="Camera
'door1' rendered by Arnold" width="650" class="img-thumbnail"/>

<img src="/assets/conference_room_arnold_door2y.png" alt="Camera
'door2y' rendered by Arnold" width="650" class="img-thumbnail"/>

<img src="/assets/conference_room_arnold_shaft.png" alt="Camera
'shaft' rendered by Arnold" width="650" class="img-thumbnail"/>

<img src="/assets/conference_room_arnold_xY.png" alt="Camera 'xY'
rendered by Arnold" width="650" class="img-thumbnail"/>

To render the __Radiance patterns__ (basically 3D textures based on a
position in space and some parameters), I wrote __Arnold shaders__,
which are currently __not__ publicly available, but checked in into a
[shader repository][shader-repository].

The pinboard (visible in the third and forth picture) uses for example
two (pattern) shaders, called __rad_dirt__ and __rad_bwave__ and this
is how it looks like in an .ass file:

{% highlight tcsh %}
...
polymesh
{
 name MEpinboard
...
 shader "MAwhite_mat"
 opaque on
 declare Pref_matrix constant MATRIX
 Pref_matrix 
 1 0 0 0
 0 1 0 0
 0 0 1 0
 0 0 0 1
}

rad_dirt
{
 name MAwhite_pat1
 Kd_color 0.515999973 0.449999988 0.340000004
 dirtiness 0.4
 scale 0.0009144 0.0009144 0.0009144 # 0.003 * 0.3048 (feet to meters)
 rotation 0 0 0
}

rad_bwave
{
 name MAwhite_pat2
 Kd_color MAwhite_pat1
 scale 0.006096 0.006096 0.006096 # 0.02 * 0.3048 (feet to meters)
 rotation 90 0 0
}

standard
{
 name MAwhite_mat
 Kd 1
 Kd_color MAwhite_pat2 # 0.515999973 0.449999988 0.340000004
 Ks 0
 Ks_color 1 1 1
}
...
{% endhighlight %}

The parameters of __rad_dirt__ are:

{% highlight tcsh %}
node:         rad_dirt
type:         shader
output:       RGB
parameters:   6
filename:     ./rad_dirt.so
version:      4.2.0.5

Type          Name                              Default
------------  --------------------------------  --------------------------------
RGB           Kd_color                          1, 1, 1
FLOAT         dirtiness                         0.5
BOOL          use_Pref_matrix                   true
VECTOR        scale                             1, 1, 1
POINT         rotation                          0, 0, 0
STRING        name                              
{% endhighlight %}

For __rad_bwave__ they look like this:

{% highlight tcsh %}
node:         rad_bwave
type:         shader
output:       RGB
parameters:   5
filename:     ./rad_bwave.so
version:      4.2.0.5

Type          Name                              Default
------------  --------------------------------  --------------------------------
RGB           Kd_color                          1, 1, 1
BOOL          use_Pref_matrix                   true
VECTOR        scale                             1, 1, 1
POINT         rotation                          0, 0, 0
STRING        name                              
{% endhighlight %}

Basically all patterns manipulate the color (parameter __Kd_color__)
of e.g. a standard shader and you can plug several patterns together
before delivering the final color to the material shader. All patterns
allow to __scale__ and apply __rotation__ angles (in degrees).

{% highlight c %}
...
shader_evaluate
{
  AtColor Kd_color = AiShaderEvalParamRGB(p_Kd_color);
...
  bool use_Pref_matrix = AiShaderEvalParamBool(p_use_Pref_matrix);
  AtVector scale = AiShaderEvalParamVec(p_scale);
  AtPoint rotation = AiShaderEvalParamPnt(p_rotation);
  AtPoint P = sg->Po;
...
  // apply matrices
...
  if (use_Pref_matrix && AiUDataGetMatrix("Pref_matrix", &userMat)) {
    AiM4Mult(mat1, mat0, userMat);
  } else {
    AiM4Mult(mat1, mat0, sg->M);
  }
...
  AtPoint Pt = P;
  AiM4PointByMatrixMult(&Pt, mat5, &P);
  // see bwave.cal (e.g. export_multi/04_conference_room/rad/bwave.cal)
  sg->out.RGB = Kd_color * bwave(Pt.x, Pt.y, Pt.z);
}
...
{% endhighlight %}

Each pattern works more or less the same way (except calling different
functions for the actual pattern generation). It retrieves from
Arnold's __shader globals__ the shading point in object-space
(__Po__), calculates __transformation matrices__ for the scale and
rotation, and either applies a __user matrix__ (based on the boolean
parameter __use\_Pref\_matrix__ and the existence of such a matrix
called __Pref_matrix__), or applies the __local-to-world matrix
transform__ to calculate world coordinates, before using those
transformed coordinates (__Pt__) for further calculations.

The __user matrix__ allows for example to texture all the chairs in
the same __rest position__ (therefore the name __Pref__ - see [Pixar's
Primitive Variables][pref]), but instead of defining the geometry in
terms of a rest position (which can be used for deforming geometry), I
decided to use a simple user matrix, which should involve less user
data to store.

{% highlight tcsh %}
...
rad_upholstery
{
 name MAcloth1_pat
 Kd_color 0.980000019 0.182999998 0.165000007
 scale 0.0381 0.0381 0.0381 # 0.125 * 0.3048 (feet to meters)
 rotation 0 -45 0
}

standard
{
 name MAcloth1_mat
 Kd 1
 Kd_color MAcloth1_pat # 0.980000019 0.182999998 0.165000007
 Ks 0
 Ks_color 1 1 1
}
...
polymesh
{
 name MEseat
...
 matrix
 -0.082034491 -0.0144648636 0 0
 -6.32279284e-10 3.5858414e-09 0.0833000019 0
 -0.0144648636 0.082034491 -3.64115871e-09 0
 3.03261352 3.5615592 0 1
 shader "MAcloth1_mat"
 opaque on
 declare Pref_matrix constant MATRIX
 Pref_matrix 
 1 0 0 0
 0 -4.37113883e-08 1 0
 0 -1 -4.37113883e-08 0
 0 5.2578001 0 1
}
...
...
polymesh
{
 name MEseat_un2609
...
 matrix
 -0.0215596184 0.0804616362 0 0
 3.51708973e-09 9.42400824e-10 0.0833000019 0
 0.0804616362 0.0215596184 -3.64115871e-09 0
 3.11262894 4.03192377 0 1
 shader "MAcloth1_mat"
 opaque on
 declare Pref_matrix constant MATRIX
 Pref_matrix 
 1 0 0 0
 0 -4.37113883e-08 1 0
 0 -1 -4.37113883e-08 0
 0 5.2578001 0 1
}
...
{% endhighlight %}

Notice in the example above how the matrices differ for both seats,
but the __Pref_matrix__ defines the space the chair was textured in.

{% highlight tcsh %}
rad_speck
{
 name MAcurtain_pat
 Kd_color 0.600000024 0.600000024 0.600000024
 brightness 0.2
 use_Pref_matrix false
 scale 0.006096 0.006096 0.006096 # 0.02 * 0.3048 (feet to meters)
 rotation 0 0 0
}
{% endhighlight %}

There exists a shader parameter called __use\_Pref\_matrix__ in case
you want to use the matrix of the primitive (e.g. of a __polymesh__)
instead of the __user matrix__, which is in this case ignored.

[conf-room]:          https://www.janwalter.org/renderforum/index.php?topic=15.0
[radiance]:           http://radsite.lbl.gov/radiance
[vi]:                 https://en.wikipedia.org/wiki/Vi
[repo]:               https://github.com/wahn/export_multi/tree/master/04_conference_room
[arnold]:             http://www.solidangle.com
[shader-repository]:  https://bitbucket.org/wahn/arnold_shaders/wiki/Home
[pref]:               http://renderman.pixar.com/view/Appnote22
[multi_exporter]:     https://bitbucket.org/wahn/blender-add-ons/wiki/Home
