---
layout: post
title:  "New GitHub Repository to Tweak Scenes from export_multi.py"
date:   2014-04-04 11:13:37
categories: jekyll github repo export_multi
---

There is a new [repository][repo] on [GitHub][github], which purpose
it is to use the [multi-exporter][multi_exporter] for
[Blender][blender] and check in (and tweak) various scenes step by
step.

1. Provide [Radiance][radiance] scene (__.rad__ files) as reference

2. Import Radiance files into [Blender][blender] and optimize it
before saving __.blend__ files

3. Export to various file formats using the
[multi-exporter][multi_exporter] add-on.

4. Use the __estimate settings__ button of [Luxrender][luxrender]
(__.lxs__ files) to find good camera settings for [Maxwell][maxwell].

5. Render a __reference__ image with __Maxwell__ (__.mxs__ files),
probably separating several light sources to see their individual
impact on the final image.

6. Tweak the [Arnold][arnold] __.ass__ files step by step following
the [noise workflow][noise_workflow] and looking at the __AOVs__ (AOV
= arbitrary output variable).

7. Check in the resulting images (into a __img__ sub-folder) and
comment on the __renderer version__ and the __render time__.

<img src="/assets/01_simple_room.png" alt="Sub-directories for a
example scene" width="650" class="img-thumbnail"/>

Posts about individual scenes will follow soon ...

[github]:         https://github.com
[repo]:           https://github.com/wahn/export_multi
[multi_exporter]: https://bitbucket.org/wahn/blender-add-ons/wiki/Home
[blender]:        http://www.blender.org
[radiance]:       http://radsite.lbl.gov/radiance
[luxrender]:      http://www.luxrender.net/en_GB/index
[maxwell]:        http://www.maxwellrender.com
[arnold]:         http://www.solidangle.com
[noise_workflow]: https://support.solidangle.com/display/mayatut/Removing+Noise+Workflow