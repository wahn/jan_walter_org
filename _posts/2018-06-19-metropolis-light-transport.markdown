---
layout: post
title:  "Metropolis Light Transport (MLT)"
date:   2018-06-19 13:30:00
categories: jekyll rendering pbrt
---

[Version 0.3.2][releases] of my [Rust][rust] based [PBRT][pbrt]
implementation can render with [Metropolis Light Transport][mlt] (MLT)
now:

<p class="text-center"><img
src="/assets/living-room-2_pbrt_rust_mlt.png" alt="Living Room
rendered via MLT." width="740" height="540"
class="img-thumbnail"/></p>

I also fixed a [bug][issue] for [Bi-directional Path Tracing][bdpt]
(see previous [blog][blog]):

<p class="text-center"><img
src="/assets/living-room-2_pbrt_rust_bdpt.png" alt="Living Room
rendered via bi-directional path tracing." width="740" height="540"
class="img-thumbnail"/></p>

The difference between both algorithms is hard to see, but with some
false-color representation you can see that the main difference is in
some specular highlights at the rim of some objects:

<p class="text-center"><img
src="/assets/living-room-2_pbrt_rust_diff.png" alt="The difference
between both algorithms." width="740" class="img-thumbnail"/></p>

To see a higher resolution of the image above follow this
[link][commit] and __right-click->View Image__. For high-res versions
of the other two images just follow this [link][other_scenes] and
click either __living-room-2\_pbrt\_rust\_mlt.png__ or
__living-room-2\_pbrt\_rust\_bdpt.png__.

Here is another scene which profits from using MLT:

<p class="text-center"><img src="/assets/volume-caustic.png"
alt="Volumetric caustics." width="740" height="740"
class="img-thumbnail"/></p>

Version 0.3.2 consists of **114** public **structs**, **9 enums**,
**21 traits**, and **143** public **functions**:

```shell
> pwd
/mill3d/users/jan/git/github/rs_pbrt/src
> rg -trust "^pub struct" | wc
    114     459    5143
> rg -trust "^pub enum" | wc
      9      36     371
> rg -trust "^pub trait" | wc
     21      88     879
> rg -trust "^pub fn" | wc
    143    1089   10238
```

Here are the **traits**:

```shell
> rg -trust "^pub trait" 
core/reflection.rs
253:pub trait Bxdf {
307:pub trait Fresnel {

core/primitive.rs
16:pub trait Primitive {

core/filter.rs
11:pub trait Filter {

core/camera.rs
16:pub trait Camera {

core/microfacet.rs
12:pub trait MicrofacetDistribution {

core/medium.rs
266:pub trait PhaseFunction {
271:pub trait Medium {

core/light.rs
26:pub trait Light {
126:pub trait AreaLight: Light {

core/texture.rs
17:pub trait TextureMapping2D {
77:pub trait Texture<T> {

core/interaction.rs
28:pub trait Interaction {

core/integrator.rs
23:pub trait SamplerIntegrator {

core/shape.rs
16:pub trait Shape {

core/material.rs
20:pub trait Material {

core/lightdistrib.rs
22:pub trait LightDistribution {

core/sampler.rs
12:pub trait Sampler: SamplerClone {
36:pub trait PixelSampler: Sampler {}
38:pub trait GlobalSampler: Sampler {}
40:pub trait SamplerClone {
```

The next major release will probably either re-write the parser to use
the latest [pest][pest] crate or experiment with other __parser
generators__ available for Rust.

[releases]:     https://github.com/wahn/rs_pbrt/releases
[rust]:         https://www.rust-lang.org
[pbrt]:         http://www.pbrt.org
[mlt]:          https://en.wikipedia.org/wiki/Metropolis_light_transport
[bdpt]:         https://en.wikipedia.org/wiki/Path_tracing#Bidirectional_path_tracing
[issue]:        https://github.com/wahn/rs_pbrt/issues/50
[blog]:         https://www.janwalter.org/jekyll/rendering/pbrt/2018/03/19/bi-directional-path-tracing.html
[commit]:       https://github.com/wahn/export_multi/commit/fdece53735a88b33e7cb8dc5471d7ad24807ee03
[other_scenes]: https://github.com/wahn/export_multi/tree/master/other_scenes/img
[pest]:         https://crates.io/crates/pest
