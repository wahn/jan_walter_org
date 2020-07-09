---
layout: post
title:  "Denoising with Arnold"
date:   2020-07-08 10:00:00
categories: jekyll rendering denoising arnold 
---

# Denoising

There is not so much information out there how to denoise an image
with the [Arnold][arnold] renderer. You probably find pages like
[Denoising a Room Interior][interior] via Google search. Feel free to
read the information provided and try to apply this to **MtoA** (Maya
to Arnold). I will not talk about any **DCC** ([Digital Content][dc]
Creation) tool specific solutions here. I rather give an example file
in Arnold's scene description file format ([.ass][ass]).

## Blender

If you want to see a denoiser in action you can use the open source
DCC [Blender][blender] and download one of the example files I talked
about in the past:

- [Render Comparison on GitHub][render_comparison1]
- [Render Comparison][render_comparison2]

The [Cornell Box][cornell-box] scene used in this simple example
should be prepared for [Cycles][cycles] and you can watch tiles being
render if you press **F12**. You will notice that the tiles at the
edges are still noisy until all surrounding tiles got rendered. This
is when the denoiser kicks in and makes the tiles less noisy (or noise
free).

## Arnold

But now let's look at Arnold and two images (within Blender):

<p class="text-center"><img src="/assets/arnold_noice.png"
alt="Arnold." width="740" class="img-thumbnail"/></p>

(right click in your browser to see a larger version by selecting
**"view image"** or **"save image as..."**.)

On the upper right side you see an image with [AOVs][aovs] (Arbitrary
Output Variables). In Blender they call this AOV (or pass)
**Combined** (see below and to the right of the image). Most other
DCCs call it **beauty pass**. Basically there are several images
stored within a single [OpenEXR][openexr] image. And the lower two
images show you the **diffuse indirect** pass on the left, and the
**variance** pass on the right. They are all part (beside other
passes) of the same rendered image (called *arnold.exr*).

On the upper left you see the denoised image (called *noice.exr*). But
how did we render those two images via Arnold?

### export_multi

I started to collect simple scenes for my render comparisons in a
GitHub repository called [export_multi][export_multi]. The directory
for the **Cornell Box** contains a sub directory for [.ass][ass]
files:

```shell
# where are we?
$ pwd
/home/jan/git/github/export_multi/00_cornell_box/ass
# what's there?
$ ls -lag
total 24
drwxr-xr-x  2 jan 4096 Jul  9 10:51 .
drwxr-xr-x 14 jan 4096 Apr  1 10:49 ..
-rw-r--r--  1 jan 5116 Jul  8 13:06 cornell_box.ass
-rw-r--r--  1 jan 1865 Jul  8 13:07 cornell_box_denoise.patch
-rw-r--r--  1 jan 1037 Jul  8 12:29 Makefile
```

For testing the denoiser I modified an already existing *cornell_box.ass* file:

```diff
diff --git a/00_cornell_box/ass/cornell_box.ass b/00_cornell_box/ass/cornell_box.ass
index f144aa42..49f0f84d 100644
--- a/00_cornell_box/ass/cornell_box.ass
+++ b/00_cornell_box/ass/cornell_box.ass
@@ -9,13 +9,13 @@ options
  AA_samples 5
  outputs 8 1 STRING
   "RGBA RGBA filter driver"
-  "emission RGB filter driver_emission"
-  "diffuse_direct RGB filter driver_diffuse_direct"
-  "specular_direct RGB filter driver_specular_direct"
-  "transmission_direct RGB filter driver_transmission_direct"
-  "transmission_indirect RGB filter driver_transmission_indirect"
-  "diffuse_indirect RGB filter driver_diffuse_indirect"
-  "specular_indirect RGB filter driver_specular_indirect"
+  "emission RGB filter driver"
+  "diffuse_direct RGB filter driver"
+  "diffuse_indirect RGB filter driver"
+  "RGB RGB variance_filter driver variance"
+  "diffuse_albedo RGB filter driver"
+  "Z FLOAT filter driver"
+  "N VECTOR filter driver"
  xres 500
  yres 500
  bucket_size 32
@@ -55,50 +55,14 @@ gaussian_filter
  name filter
 }
 
-driver_exr
-{
- name driver
- filename "arnold.exr"
-}
-
-driver_exr
-{
- name driver_emission
- filename "arnold_aov_emission.exr"
-}
-
-driver_exr
-{
- name driver_diffuse_direct
- filename "arnold_aov_diffuse_direct.exr"
-}
-
-driver_exr
+variance_filter
 {
- name driver_specular_direct
- filename "arnold_aov_specular_direct.exr"
+ name variance_filter
+ filter_weights gaussian
 }

 driver_exr
 {
- name driver_transmission_direct
- filename "arnold_aov_transmission_direct.exr"
-}
-
-driver_exr
-{
- name driver_transmission_indirect
- filename "arnold_aov_transmission_indirect.exr"
-}
-
-driver_exr
-{
- name driver_diffuse_indirect
- filename "arnold_aov_diffuse_indirect.exr"
-}
-
-driver_exr
-{
- name driver_specular_indirect
- filename "arnold_aov_specular_indirect.exr"
+ name driver
+ filename "arnold.exr"
 }

 polymesh
```

For convenience I provide three files [here][cornell_box_denoise]:

```shell
$ unzip -l cornell_box_denoise.zip
Archive:  cornell_box_denoise.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     4411  2020-07-09 10:55   cornell_box_denoise.ass
  8442067  2020-07-09 10:56   cornell_box_denoise_arnold.exr
  2517479  2020-07-09 10:56   cornell_box_denoise_noice.exr
---------                     -------
 10963957                     3 files
```

This is the patched *cornell\_box\_denoise.ass* Arnold file for
rendering and the two resulting images. The full procedure to create
those images yourself is (look at the **Makefile** in the
[export_multi][export_multi] folder mentioned above):

```shell
$ kick -dp -dw -i cornell_box_denoise.ass -v 6 ...
# replace the '...' above by the following three options
# -set MALight_light.intensity 1 
# -set MALight_light.samples 3 
# -set options.GI_diffuse_samples 1
00:00:00    82MB         | log started Thu Jul  9 11:05:26 2020
00:00:00    82MB         | Arnold 6.0.3.1 [93beeb25] ...
...
00:00:04    93MB         | Arnold shutdown
# that rendered arnold.exr with several AOVs
# now let's do the denoising:
$ noice -i arnold.exr -o noice.exr
noice 6.0.3.1 [93beeb25] - the Arnold denoiser
Using 28 threads.
Loading images...
Loading file "arnold.exr".
Using feature AOV 'diffuse_albedo' with filter 'gaussian_filter'
Using feature AOV 'N' with filter 'gaussian_filter'
Using feature AOV 'Z' with filter 'gaussian_filter'
Working with 1 frame at 500x500
Will denoise AOV "RGBA", using associated variance
   Output file will be "noice.exr"
Start denoising (patch radius 3, search radius 9, variance 0.5)
Denoising RGBA
Finished denoising
Saving image noice.exr (500 x 500 x 4)
```

Both executables *kick* and *noice* should ship with your **Arnold
installation**. For those of you who do not have an Arnold license,
you can still inspect both *.exr* files (e.g. via Blender) and look at
the AOVs.

[arnold]:              https://www.arnoldrenderer.com
[interior]:            https://docs.arnoldrenderer.com/display/A5AFMUG/Denoising+a+Room+Interior
[dc]:                  https://en.wikipedia.org/wiki/Digital_content
[ass]:                 https://docs.arnoldrenderer.com/pages/viewpage.action?pageId=36110427
[blender]:             https://www.blender.org
[render_comparison1]:  https://www.janwalter.org/jekyll/github/rendering/2020/03/26/github-render_comparison.html
[render_comparison2]:  https://www.janwalter.org/jekyll/download/rendering/2020/05/24/github-render_comparison.html
[cornell-box]:         https://www.janwalter.org/Download/Scenes/cornell_box_blend.tar.gz
[cycles]:              https://www.cycles-renderer.org
[aovs]:                https://docs.arnoldrenderer.com/display/A5AFMUG/AOVs
[openexr]:             http://www.openexr.org
[export_multi]:        https://github.com/wahn/export_multi
[cornell_box_denoise]: https://www.janwalter.org/Download/Scenes/cornell_box_denoise.zip
