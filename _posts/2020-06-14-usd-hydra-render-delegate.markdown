---
layout: post
title:  "USD Hydra Render Delegate"
date:   2020-06-14 11:11:00
categories: jekyll rendering usd hydra
---

# USD

What is USD? Pixar's **U**niversal **S**cene **D**escription
(USD). Read more about it [here][usd]. The following short
descriptions are taken from there.

## Hydra

**Hydra** is the imaging framework that ships as part of the USD
distribution.

## USDView

The USD front-end to Hydra is used in **usdview** and the third-party
plugins included with the USD distribution, and is meant to provide a
"ground truth" geometry rendering of any scene composed of prims
conforming to the [UsdGeom schemas][usdgeom]. It also provides fast preview and
animation streaming for USD scenes.

<div id='i-bGVUjh8NU' class="youtube"><a
href='https://www.youtube.com/watch?v=i-bGVUjh8NU&feature=youtu.be'><img
src='/assets/usd_hydra_prman_youtube.png' height="740"
class="img-thumbnail"/></a></div>

Click on the image above to get redirected to a **YouTube video**
which shows [RenderMan][prman] being used through Hydra (within usdview).

### 3rd Party Renderers

The distribution also ships with a simple [Embree][embree]-based path tracer to
serve as an example for creating more backends.

Lets just say that compiling the USD [source code][usdgithub] and it's
dependencies from scratch isn't that easy, but feel free to follow the
[Advanced Build Configuration][building] and find out for
yourself. **Hint**: Try without Python first, if that works **add
Python** support, so you end up at least with **usdview**. After that
try adding [Embree][embree].

I started writing a **skeleton** for any renderer, which means that
there is a library, which compiles and links against USD, and some
standalone executable for a minimal hydra application, as described
[here][hdembree] (for [Embree][embree]), except that I took out all
**Embree** related code and replaced it by [PBRT][pbrt]:

<p class="text-center"><img src="/assets/usdview_hydra_renderer.png"
alt="USDView and other renderers like PBRT and Arnold." width="740"
class="img-thumbnail"/></p>

I also managed to compile the [Arnold][arnold] version of their [USD
components][arnold-usd] as you can see in the screenshot above. Here
is a video about that (or locally [here][usdview_arnold]):

<div id='tVd45QV4PkU' class="youtube"><a
href='https://www.youtube.com/watch?v=tVd45QV4PkU&feature=youtu.be'><img
src='/assets/usdview_arnold.png' height="740"
class="img-thumbnail"/></a></div>

Let's see if I find time in the future to keep working on this. I
would like to build such a bridge to my own Rust based PBRT
implementation, [rs-pbrt][rs-pbrt]. The first step would be to use the
**C++** code and fill in the bits and pieces for the skeleton to
actually render something using the PBRT **API** (which might has to
be adjusted slightly to do that). After that I would do the same
adjustments for the **Rust** version of the API and use foreign
function interfaces (**FFI**) to make C++ and Rust talk to each
other. Maybe [safer_ffi][safer-ffi] can help with that?

[usd]:            https://graphics.pixar.com/usd/docs/index.html
[usdgeom]:        https://graphics.pixar.com/usd/docs/api/usd_geom_page_front.html
[prman]:          https://rmanwiki.pixar.com
[embree]:         https://www.embree.org
[usdgithub]:      https://github.com/PixarAnimationStudios/USD
[building]:       https://github.com/PixarAnimationStudios/USD/blob/master/BUILDING.md
[hdembree]:       https://graphics.pixar.com/usd/docs/api/hd_embree_page_front.html
[pbrt]:           https://www.pbrt.org
[arnold]:         https://www.arnoldrenderer.com
[arnold-usd]:     https://github.com/Autodesk/arnold-usd
[rs-pbrt]:        https://www.rs-pbrt.org
[safer-ffi]:      https://github.com/getditto/safer_ffi
[usdview_arnold]: https://www.janwalter.org/Videos/usdview_arnold.webm
