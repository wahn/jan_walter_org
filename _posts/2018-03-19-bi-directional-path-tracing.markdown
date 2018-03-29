---
layout: post
title:  "Bi-directional Path Tracing"
date:   2018-03-19 11:11:00
categories: jekyll rendering pbrt
---

[Version 0.3.0][releases] of my [Rust][rust] based [PBRT][pbrt]
implementation can render with [bi-directional path tracing][bdpt]
(BDPT) now:

<p class="text-center"><img src="/assets/rs_pbrt_bidir_01.png"
alt="Comparison between uni- and bi-directional path tracing"
width="740" class="img-thumbnail"/></p>

On the left the same scene was rendered with uni-directional path
tracing and the resulting image contains much more noise, on the right
bi-directional path tracing was used (with otherwise identical
settings). Rays leaving the scene without hitting geometry render
black right now (see windows). This should be fixed for future
releases.

Here is another scene which profits from using BDPT:

<p class="text-center"><img
src="/assets/art_gallery_pbrt_rust_07n.png" alt="" width="500"
class="img-thumbnail"/></p>

Version 0.3.0 consists of **105** public **structs**, **9 enums**,
**19 traits**, and **138** public **functions**:

```shell
> pwd
/mill3d/users/jan/git/github/rs_pbrt/src
> rg -trust "^pub struct" | wc
    105     423    4739
> rg -trust "^pub enum" | wc
      9      36     371
> rg -trust "^pub trait" | wc
     19      80     802
> rg -trust "^pub fn" | wc
    138    1061   10024
```

Here are the **traits**:

```shell
> rg -trust "^pub trait" 
core/microfacet.rs
12:pub trait MicrofacetDistribution {

core/texture.rs
17:pub trait TextureMapping2D {
77:pub trait Texture<T> {

core/lightdistrib.rs
21:pub trait LightDistribution {

core/interaction.rs
27:pub trait Interaction {

core/shape.rs
16:pub trait Shape {

core/integrator.rs
20:pub trait SamplerIntegrator {

core/primitive.rs
15:pub trait Primitive {

core/light.rs
23:pub trait Light {
113:pub trait AreaLight: Light {

core/sampler.rs
12:pub trait Sampler: SamplerClone {
36:pub trait PixelSampler: Sampler {
39:pub trait GlobalSampler: Sampler {
42:pub trait SamplerClone {

core/material.rs
20:pub trait Material {

core/reflection.rs
253:pub trait Bxdf {
307:pub trait Fresnel {

core/filter.rs
11:pub trait Filter {

core/camera.rs
16:pub trait Camera {
```

The next major release will hopefully implement [Metropolis Light
Transport][mlt] (MLT). I already created an [issue][issue] about it.

[releases]: https://github.com/wahn/rs_pbrt/releases
[rust]:     https://www.rust-lang.org/en-US
[pbrt]:     http://www.pbrt.org
[bdpt]:     https://en.wikipedia.org/wiki/Path_tracing#Bidirectional_path_tracing
[mlt]:      https://en.wikipedia.org/wiki/Metropolis_light_transport
[issue]:    https://github.com/wahn/rs_pbrt/issues/43
