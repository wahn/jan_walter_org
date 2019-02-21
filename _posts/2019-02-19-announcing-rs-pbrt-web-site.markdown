---
layout: post
title:  "Announcing new Web Site for rs-pbrt"
date:   2019-02-19 13:15:00
categories: jekyll rendering pbrt
---

# New Website

On November 14th I started working on a new [website][rs_pbrt] about
my **port** of an existing (C++ code) implementation of a **physically
based renderer** to [Rust][rust], a new programming language I was
interested in learning. As the [about page][about] tells you there is

1. a [book][book] to learn from about the theory of **Physically Based
   Rendering**

2. an [accompanying website][pbrt] which provides further resources
   and links e.g. to the [C++ source code][cpp_sources] or a number of
   interesting [scenes][scenes]

3. an [online edition][online_book] of the same book, so there is no
   need to buy the book, but I do recommend it

4. the [source code][rust_sources] of the **Rust** implementation,
   which isn't finished yet, but does implement most of the algorithms
   already and can be used for rendering

5. the [online documentation][rs_pbrt_doc] of the aforementioned
   **Rust** implementation, which can also be found [on this
   website][updated_docs] (updated inbetween releases)

## Blog

Whereas this blog is more about rendering in general and comparing
different commercial and open source renderers, the [blog][blog] on
the new website is about [releases][releases] and other topics around
the Rust executable (called `rs_pbrt`) and how to use it. So far you
can find:

- [Getting Started][getting_started]: Tells you where to get the
  source code from, how to compile it, or create a local copy of the
  documentation, render your first image, or where to find [more
  scenes][more_scenes] (because `rs_pbrt` isn't 100% compatible to the
  C++ counter part yet).

- Some [Blender][blender] related posts (see [tag][blender_tag]),
  which tell you more about the current state of **exporters**, which
  allow you to create scenes in Blender, and render them with
  `rs_pbrt`.

- [Release Notes][releases]: So far I have written about three
  releases, but in the future there will be more.
  
## Features

I try to show an **image** for each release, which shows a new
feature, which was added to the latest `rs_pbrt` executable and most
likely there is a **scene file** which allows you to render that image
yourself, after you have downloaded that scene and compiled the latest
Rust source code into an executable. Which raises the question:
**Which feature are already implemented?**

### Stochastic Progressive Photon Mapping (SPPM)

The [last release][v0-5-1] added **Stochastic Progressive Photon
Mapping** (SPPM), which gets activated through an `Integrator "sppm"`
line in the [scene file][sppm_scene]:

```sh
> grep sppm caustic-glass.pbrt
Integrator "sppm" "integer numiterations" [10000] "float radius" .075
```

<p class="text-center"><img
src="/assets/caustic_glass_pbrt_rust_sppm.png" alt="Caustics created
by rippled glass." width="740" class="img-thumbnail"/></p>

As described [here][caustic-glass] you can write out a `pbrt.png` file
every iteration (or whatever you specify as argument):

```sh
> grep imagewritefrequency caustic-glass.pbrt
    "integer imagewritefrequency" [1]
```

### Subsurface Scattering (SSS)

<p class="text-center"><img src="/assets/sss_dragon.png"
alt="Subsurface Scattering (SSS)." width="740"
class="img-thumbnail"/></p>

See also the [v0-5-0][v0-5-0] relase notes.

### Other Integrators

#### Ambient Occlusion (AO)

<p class="text-center"><img
src="https://www.janwalter.org/doc/img/cornell_box_pbrt_rust_ao.png"
alt="Ambient Occlusion (AO)." class="img-thumbnail"/></p>

#### Direct Lighting

<p class="text-center"><img
src="https://www.janwalter.org/doc/img/cornell_box_pbrt_rust_directlighting.png"
alt="Direct Lighting." class="img-thumbnail"/></p>

#### Path Tracing

<p class="text-center"><img
src="https://www.janwalter.org/doc/img/cornell_box_pbrt_rust_path.png"
alt="Path Tracing." class="img-thumbnail"/></p>

#### Bidirectional Path Tracing (BDPT)

<p class="text-center"><img
src="https://www.janwalter.org/doc/img/art_gallery_pbrt_rust_bdpt.png"
alt="Bidirectional Path Tracing (BDPT)." class="img-thumbnail"/></p>

#### Metropolis Light Transport (MLT)

<p class="text-center"><img
src="https://www.janwalter.org/doc/img/volume_caustic_pbrt_rust_mlt.png"
alt="Metropolis Light Transport (MLT)." width="512"
class="img-thumbnail"/></p>

### Other Features

There are other features, like **motion blur**, **depth of field**,
[realistic cameras][realistic_cam] etc. but those will be most likely
documented with better example scenes and written about on the
aforementioned [website][rs_pbrt] and/or [blog][blog].

[rs_pbrt]:         https://www.rs-pbrt.org
[rust]:            https://www.rust-lang.org
[about]:           https://www.rs-pbrt.org/about
[book]:            https://www.elsevier.com/books/physically-based-rendering/pharr/978-0-12-800645-0
[pbrt]:            http://www.pbrt.org
[cpp_sources]:     https://github.com/mmp/pbrt-v3
[scenes]:          https://www.pbrt.org/scenes-v3.html
[online_book]:     http://www.pbr-book.org
[rust_sources]:    https://github.com/wahn/rs_pbrt
[rs_pbrt_doc]:     https://www.rs-pbrt.org/doc/crates/pbrt/index.html
[updated_docs]:    https://www.janwalter.org/doc/rust/pbrt/index.html
[blog]:            https://www.rs-pbrt.org/blog
[releases]:        https://www.rs-pbrt.org/tags/release
[getting_started]: https://www.rs-pbrt.org/blog/2018-11-16-getting-started
[more_scenes]:     https://gitlab.com/jdb-walter/rs-pbrt-test-scenes/wikis/home
[blender]:         https://www.blender.org
[blender_tag]:     https://www.rs-pbrt.org/tags/blender
[v0-5-0]:          https://www.rs-pbrt.org/blog/2019-01-16-v0-5-0-release-notes
[v0-5-1]:          https://www.rs-pbrt.org/blog/2019-02-19-v0-5-1-release-notes
[caustic-glass]:   https://gitlab.com/jdb-walter/rs-pbrt-test-scenes/wikis/pbrt-caustic-glass
[sppm_scene]:      https://www.janwalter.org/Download/Scenes/caustic_glass.tar.gz
[realistic_cam]:   http://www.pbr-book.org/3ed-2018/Camera_Models/Realistic_Cameras.html
