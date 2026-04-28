---
layout: page
title: Mock Rover
description: FSI Capstone Project — NASA Space Grant Consortium
img: assets/img/fsi-rover-thumbnail.jpg
importance: 1
category: work
---

## Overview

During my senior capstone project with the **Florida Space Institute (FSI)** and the **NASA Space Grant Consortium**, I inherited a non-functional mock rover from previous student teams — no documentation, scattered components, unknown state. Over the course of the year I reverse-engineered what existed and rebuilt it into a fully operational remote-controlled vehicle.

---

## The Challenge

The rover arrived with zero documentation. Previous teams had left behind a collection of components with no wiring diagrams, no codebase, and no record of what had or hadn't worked. The first task was simply figuring out what we had before any rebuilding could begin.

---

## What I Built

I led the technical integration of the embedded control systems from the ground up, designing a **Raspberry Pi-based architecture** that coordinates:

- Four AC motors via rebuilt motor controller systems
- A stereoscopic camera feed for real-time remote video streaming
- A wireless joystick command interface
- 3D-printed structural components

The final system achieved:
- **~100ft wireless range** for remote video and joystick control
- **300lb payload capacity**
- **5mph traverse speed**

---

## See It In Action

<iframe
  src="https://www.youtube.com/embed/waXdsutvG5M"
  class="w-100 rounded z-depth-1"
  style="aspect-ratio: 16/9; border: none;"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

---

## Technical Stack

| Layer | Technology |
|---|---|
| Primary compute | Raspberry Pi |
| Low-level control | Arduino Uno |
| Software | Python, C++ |
| Actuation | AC motors, motor controllers |
| Sensing | Stereoscopic cameras |
| Communication | Wireless command interface |
| Fabrication | 3D printing |

---

## Takeaways

This project was as much about systems thinking and debugging under uncertainty as it was about writing code. Inheriting a broken system with no documentation forces you to reason carefully about every component before touching anything — a skill that transfers directly to any complex engineering environment.

---

*Sep 2022 – May 2023 · Florida Space Institute · NASA Space Grant Consortium*