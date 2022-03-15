---
layout: post
title:  "[blend_info] Camera"
date:   2022-03-14 14:30:00
categories: jekyll blend_info blender camera
---

# Blender 3.1.0

```cpp
static float rna_Camera_angle_get(PointerRNA *ptr)
{
  Camera *cam = (Camera *)ptr->owner_id;
  float sensor = BKE_camera_sensor_size(cam->sensor_fit,
					cam->sensor_x,
					cam->sensor_y);
  return focallength_to_fov(cam->lens, sensor);
}
```

## Camera.sensor\_fit

```
# Auto
$ .\target\release\blend_info -n Camera.sensor_fit untitled.blend
Camera.sensor_fit = 0
# Horizontal
$ .\target\release\blend_info -n Camera.sensor_fit untitled.blend
Camera.sensor_fit = 1
# Vertical
$ .\target\release\blend_info -n Camera.sensor_fit untitled.blend
Camera.sensor_fit = 2
```

## Camera.sensor\_[xy]

```
$ .\target\release\blend_info -n Camera.sensor_x untitled.blend
Camera.sensor_x = 36_f32
$ .\target\release\blend_info -n Camera.sensor_y untitled.blend
Camera.sensor_y = 24_f32
```

## BKE\_camera\_sensor\_size

```cpp
float BKE_camera_sensor_size(int sensor_fit, float sensor_x, float sensor_y)
{
  /* sensor size used to fit to. for auto, sensor_x is both x and y. */
  if (sensor_fit == CAMERA_SENSOR_FIT_VERT) {
    return sensor_y;
  }

  return sensor_x;
}
```

## focallength\_to\_fov

```cpp
/* lens/angle conversion (radians) */
float focallength_to_fov(float focal_length, float sensor)
{
  return 2.0f * atanf((sensor / 2.0f) / focal_length);
}
```
