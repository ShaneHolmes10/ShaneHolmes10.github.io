---
layout: page
title: Hand Robot
description: Commissioned Project — Fully Articulated Robotic Hand
img: assets/img/hand-robot-thumbnail.jpg
importance: 2
category: work
---

## Overview

A commissioned project to design and fabricate a fully articulated robotic hand with individual finger control and an accompanying arm. I owned the entire development pipeline — from mechanical design through embedded systems to the final software control interface.

---

## The Challenge

Building a robotic hand that feels natural to control means solving problems at every layer of the stack simultaneously — the mechanical structure has to allow the right range of motion, the servo circuits have to be precise enough to act on it, and the control interface has to make all of it accessible in real time. Every layer is a dependency of the one above it.

---

## What I Built

I designed and fabricated the hand and arm from the ground up:

- Iteratively modeled the mechanical structure in **CAD** with fully custom 3D printed components
- Engineered **servo control circuits** on an Arduino platform for individual finger articulation
- Developed a **desktop application in C++** for real-time control of the hand

The result was a full-stack robotics demonstration — mechanical design, embedded systems, and user interface all built and integrated by a single developer.

---

## See It In Action

{% include video.liquid path="https://www.youtube.com/embed/uCIiBNb3MNI" class="img-fluid rounded z-depth-1" %}

---

## Technical Stack

| Layer | Technology |
|---|---|
| Mechanical design | CAD, 3D printing |
| Low-level control | Arduino |
| Actuation | Servo control systems |
| Software | C++ desktop application |

---

## Takeaways

This project was an exercise in full-stack ownership. Designing something physical that has to be precisely controlled by software forces you to think about the whole system at once — a constraint in the mechanical design has ripple effects on the firmware, which has ripple effects on the UI. Iterating across all three layers simultaneously is a different kind of problem solving than working in a single domain.

---

*Nov 2020 – Feb 2021 · Commissioned Project*