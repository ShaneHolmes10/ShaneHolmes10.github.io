---
layout: page
title: 3D Renderer
description: CPU Software Rasterizer built from first principles in C++
img: assets/img/3d-renderer-thumbnail.gif
importance: 3
category: work
related_posts: false
---

## Overview

A CPU-based 3D rendering engine built entirely from scratch in C++. No GPU acceleration, no OpenGL, no rendering libraries — every stage of the pipeline from 3D transformations through triangle rasterization is implemented manually. The goal was to understand graphics programming fundamentals from first principles rather than relying on abstractions.

The full source is available on [GitHub](https://github.com/ShaneHolmes10/3DRenderer).

---

## Demo

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/3DRender_demo_video.mp4" class="img-fluid rounded z-depth-1" controls=false autoplay=true loop=true muted=true %}
    </div>
</div>
<div class="caption">
    The video above shows a demo, where the camera is being moved around using the WASD keys. The arrow keys control the angle of the camera, which is what's causing the jitter.
</div>

---

## The Pipeline

The renderer implements the full standard graphics pipeline manually:

**Model space → World space → Camera space → Screen space**

Each transformation is handled explicitly using 4×4 matrices, giving full visibility into what graphics APIs like OpenGL normally abstract away.

---

## Key Systems

**Scene Graph**
A hierarchical parent-child entity system with automatic world matrix propagation. Entities can be nested, and transforms cascade correctly down the hierarchy — the same fundamental architecture used in game engines like Unity.

**Triangle Rasterization**
Triangles are rasterized using edge functions and barycentric coordinates, with per-vertex color interpolation across the triangle surface. No library handles this — it's computed per-pixel.

**Camera System**
The camera mounts to entities in the scene graph, enabling camera rigs and orbital views. Handles the full world-to-screen transformation pipeline.

**Transform System**
Position, rotation (Euler angles), and scale are encapsulated into a 4×4 transformation matrix with automatic recomputation on change.

**OBJ File Loading**
Supports standard Wavefront OBJ files as well as a custom extended format (`cobj`) with per-vertex color data.

**Multithreaded Display**
The viewport runs on a separate thread using SFML for windowing, decoupling the rendering pipeline from the display update loop.

---

## Technical Stack

| Component | Technology |
|---|---|
| Language | C++ |
| Build system | CMake |
| Linear algebra | Eigen |
| Windowing / display | SFML  |
| Unit testing | CppUnitLite |
| Platform | Linux |

---

## Takeaways

Building a rasterizer from scratch forced me to confront every assumption that high-level graphics APIs normally hide — how coordinate spaces relate to each other, why barycentric coordinates work for interpolation, what a transformation matrix is actually doing. It gave me genuine intuition for 3D graphics rather than just knowing how to call the right functions. It was also a good exercise in structuring a C++ project with testing, documentation, and build management.

---

*Dec 2025 – Present · Personal Project*