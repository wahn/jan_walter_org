---
layout: post
title:  "Conference Room: First Results for Rust version of PBRT"
date:   2017-09-14 10:50:00
categories: jekyll rendering pbrt
---

We have seen the [Conference Room][conf-room] scene before (in other
posts). It was originally created for [Radiance][radiance]. The room
does (or at least did) exist in reality and was painstakingly measured
and re-created in a text editor ([vi][vi]). See __README__ file and
credits in the [repository][repo], where I keep my test scenes in
various formats.

Here are the first results for the [Rust version][rs_pbrt] of the [PBRT][pbrt]
renderer:

<p class="text-center"><img
src="/assets/conference_room_pbrt_rust_current.png" alt="Conference
Room rendered by Rust version of PBRT (camera 1)."  width="740"
class="img-thumbnail"/></p>

<p class="text-center"><img
src="/assets/conference_room_pbrt_rust_door1.png" alt="Conference Room
rendered by Rust version of PBRT (camera 2)."  width="740"
class="img-thumbnail"/></p>

<p class="text-center"><img
src="/assets/conference_room_pbrt_rust_door2y.png" alt="Conference
Room rendered by Rust version of PBRT (camera 3)."  width="740"
class="img-thumbnail"/></p>

<p class="text-center"><img
src="/assets/conference_room_pbrt_rust_shaft.png" alt="Conference Room
rendered by Rust version of PBRT (camera 4)."  width="740"
class="img-thumbnail"/></p>

<p class="text-center"><img
src="/assets/conference_room_pbrt_rust_xY.png" alt="Conference Room
rendered by Rust version of PBRT (camera 5)."  width="740"
class="img-thumbnail"/></p>

One last thing to mention is that right now the **C++ version** and
the Rust version **differ** a bit in rendering the **plastic
chairs**. Here the C++ version of the first image:

<p class="text-center"><img
src="/assets/conference_room_pbrt_cpp_current.png" alt="Conference Room
rendered by Rust version of PBRT (camera 5)."  width="740"
class="img-thumbnail"/></p>

And the resulting difference image (create via **imf_diff**):

<p class="text-center"><img
src="/assets/conference_room_pbrt_cpp_vs_rust_diff.jpg"
alt="Difference between the Rust and C++ implementation."  width="740"
class="img-thumbnail"/></p>

The difference is most likely caused by the **roughness** value (being
different from zero) in the plastic material of some parts being used
for the chairs.

```rib
  # black_mat
  MakeNamedMaterial "black_mat"
    "string type" [ "plastic" ]
    "color Kd" [ 0.05 0.05 0.05 ]
    "color Ks" [ 0.01 0.01 0.01 ]
    "float roughness" [ 0.1 ]
```

This will be fixed in the next [release][release].

The PBRT file (including all cameras) for this scene can be found in
the [download][download] section.

[conf-room]: https://www.janwalter.org/renderforum/index.php?topic=15.0
[radiance]:  http://radsite.lbl.gov/radiance
[vi]:        https://en.wikipedia.org/wiki/Vi
[repo]:      https://github.com/wahn/export_multi/tree/master/04_conference_room
[rs_pbrt]:   https://github.com/wahn/rs_pbrt
[pbrt]:      http://www.pbrt.org
[release]:   https://github.com/wahn/rs_pbrt/wiki/Release-Notes
[download]:  https://www.janwalter.org/download
