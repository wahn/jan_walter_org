---
layout: post
title:  "Cryptomatte vs. OpenEXR/Id"
date:   2016-05-05 13:30:00
categories: jekyll development openexrid
---

## Cryptomatte

The first time I have heard about **Cryptomatte** was last year just
after [SIGGRAPH][siggraph]. I think [Psyop][psyop] published a
[SIGGRAPH][siggraph] poster last year, but it disappeared from their
web site. Anyway there is still a video available on **Vimeo**:

<div id='136954966' class="vimeo"><a
href='https://vimeo.com/136954966'><img
src='/assets/cryptomatte.png' width="740"
class="img-thumbnail"/></a></div>

### Update

**Jonah Friedman** just provided a [link][jonahfriedman] for the
  [SIGGRAPH][siggraph] poster.

## OpenEXR/Id

Recently it was brought to my attention that [Guerilla
Render][guerilla] (version 1.4.0b35) published a similar idea and made
their implementation available to the public by making it [open
source][openexrid]:

<div id='W_ltvSMrwnQ' class="youtube"><a
href='https://youtu.be/W_ltvSMrwnQ'><img
src='/assets/openexrid.png' width="740"
class="img-thumbnail"/></a></div>

## Arnold

So basically that left us with a [Nuke][nuke] plugin which (thanks to
[OpenFX][openeffects]) can be used for [Natron][natron] as well. We
also found some example files which we immediately could try with the
plugins. The next step was to develop an [Arnold][arnold] **output
driver** to create images like that ourselves during rendering. Here
some very early screenshots (from interacting with those images within
**Nuke**):

<p class="text-center"><img
src="/assets/mill_driver_deepexrid_shape_node_names.png" alt="."
width="740" class="img-thumbnail"/></p>

I used the **alWriteIds** shader from Anders Langlands
[alShaders][alshaders] with one of my **.ass** test scene files
(Arnold's native scene description file format):

{% highlight tcsh %}
...
options
{
...
 outputs 11 1 STRING
...
  "id.shapeName POINTER filter shape_names" 
  "id.shapeName1 FLOAT filter shape_names" 
  "id.shapeName2 FLOAT filter shape_names" 
...
}
...
driver_deepexrid
{
 name shape_names
 filename "cafe_scene1.exrid" 
}
...
alWriteIds
{
 name MAred_porcelain_ids
 passthrough MAred_porcelain
 string "Shape Node Name" 
}

standard
{
 name MAred_porcelain
...
}
...
{% endhighlight %}

In your favourite text editor you can search and replace all `Shape
Node Name` strings from the previous example by `Shader Node Name` and
the resulting image will contain **shader names** instead (of **shape
names**):

<p class="text-center"><img
src="/assets/mill_driver_deepexrid_shader_node_names.png" alt="."
width="740" class="img-thumbnail"/></p>

I hope one or the other will be a **standard** soon, so more renderers
will support writing out IDs in such a way that it can be used in
(various) compositing applications.

[siggraph]:      http://www.siggraph.org
[psyop]:         http://www.psyop.tv
[guerilla]:      http://guerillarender.com/
[openexrid]:     https://github.com/MercenariesEngineering/openexrid
[nuke]:          https://www.thefoundry.co.uk/products/nuke
[natron]:        http://natron.fr/features
[openeffects]:   http://openeffects.org
[arnold]:        https://www.solidangle.com
[alshaders]:     http://anderslanglands.com/alshaders
[jonahfriedman]: http://www.jonahfriedman.com/?p=2494
