---
layout: page
title: Hand Robot
description: Commissioned Project — Fully Articulated Robotic Hand
img: assets/img/hand-robot-thumbnail.jpg
importance: 2
category: work
---

## Overview

A project to design and fabricate a fully articulated robotic hand with individual finger control and an accompanying arm. I owned the entire development pipeline — from mechanical design through embedded systems to the final software control interface. Originally this was a personal project I had started in highschool, but later I found someone who wanted to buy it. 

---

## See It In Action

<iframe
  src="https://www.youtube.com/embed/uCIiBNb3MNI"
  class="w-100 rounded z-depth-1"
  style="aspect-ratio: 16/9; border: none;"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen
></iframe>

---

## The Challenge

To begin with the 

Building a robotic hand that feels natural to control means solving problems at every layer of the stack simultaneously — the mechanical structure has to allow the right range of motion, the servo circuits have to be precise enough to act on it, and the control interface has to make all of it accessible in real time. Every layer is a dependency of the one above it.

---

## What I Built

I designed and fabricated the hand and arm from the ground up:

- Iteratively modeled the mechanical structure in **CAD** with fully custom 3D printed components
- Engineered **servo control circuits** on an Arduino platform for individual finger articulation
- Developed a **desktop application in C++** for real-time control of the hand

The hand went through two distinct design generations, and the shift between them was the most important engineering decision of the project.

### Generation 1: Monolithic Prints

I started by designing the entire palm and finger assembly as a single CAD model. Every iteration meant remodeling the whole part and reprinting the whole thing. If I wanted to change the angle of one finger, the spacing between knuckles, or the thickness of a tendon channel, I had to regenerate the full geometry and burn another multi hour print to test it.

<div class="row mt-3">
    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V1_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V1 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V1_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V1 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V1</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V2_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V2 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V2_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V2 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V2</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V3_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V3 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V3_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V3 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V3</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V4_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V4 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V4_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V4 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V4</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V5_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V5 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V5_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V5 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V5</strong></small></p>
    </div>
</div>

<div class="row mt-3">
    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V6_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V6 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V6_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V6 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V6</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V7_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V7 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V7_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V7 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V7</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V8_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V8 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V8_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V8 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V8</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;">
        <div class="d-flex flex-column">
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem 0.5rem 0 0;">
                <img src="{{ '/assets/img/robot_hand_project/V9_up.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V9 palm up">
            </div>
            <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0 0 0.5rem 0.5rem;">
                <img src="{{ '/assets/img/robot_hand_project/V9_down.jpg' | relative_url }}"
                     style="width: 100%; height: 100%; object-fit: cover;"
                     alt="V9 palm down">
            </div>
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V9</strong></small></p>
    </div>

    <div class="col-6 col-md-auto mb-4" style="flex: 0 0 20%; max-width: 20%;"></div>
</div>

This worked, but it was slow. Small changes to one feature forced rework on every other feature. The design space I could explore was bounded by the time I was willing to spend on each guess.

### Generation 2: Modular Palm Variants

For the second generation I broke the palm into a discrete component that mounted the fingers separately, and then iterated on the palm geometry independently. Each variant tested a different idea: tendon routing through internal channels, external slot guides, different finger spread angles, and different mounting geometries for the wrist.

<div class="row mt-3">
    <div class="col-6 col-md-4 mb-4">
        <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem;">
            <img src="{{ '/assets/img/robot_hand_project/V1_mod.jpg' | relative_url }}"
                 style="width: 100%; height: 100%; object-fit: cover;"
                 alt="V1 modular">
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V1</strong></small></p>
    </div>

    <div class="col-6 col-md-4 mb-4">
        <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem;">
            <img src="{{ '/assets/img/robot_hand_project/V2_mod.jpg' | relative_url }}"
                 style="width: 100%; height: 100%; object-fit: cover;"
                 alt="V2 modular">
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V2</strong></small></p>
    </div>

    <div class="col-6 col-md-4 mb-4">
        <div style="aspect-ratio: 1/1; overflow: hidden; border-radius: 0.5rem;">
            <img src="{{ '/assets/img/robot_hand_project/V3_mod.jpg' | relative_url }}"
                 style="width: 100%; height: 100%; object-fit: cover;"
                 alt="V3 modular">
        </div>
        <p class="text-center mt-2 mb-0"><small><strong>V3</strong></small></p>
    </div>
</div>

Because the fingers and wrist mount were now standardized interfaces, I could swap palm components in and out without touching the rest of the assembly. A new palm variant could be implemented in a fraction of the time of a full hand and could be tested against the existing fingers immediately. 

The modular approach changed what kinds of problems were tractable. Instead of having to reprint the entire hand every time I wanted to make a small change I would just print the small change and swap out the part. 


---

## Technical Stack

| Layer | Technology |
|---|---|
| Mechanical design | CAD, 3D printing, Sketchup |
| Low-level | Arduino, circuits |
| Actuation | Servo control systems |
| Software | C++ |

---

## Takeaways

The biggest engineering lesson came from the shift between the monolithic and modular design generations. Generation 1 worked, but every iteration cost a full reprint and a full remodel. Generation 2 was the first time I really internalized that the structure of a design determines how fast you can improve it — splitting the hand into standardized interfaces collapsed iteration time and let me explore ideas that wouldn't have been worth the cost under the old approach.

This project was also a good exercise in full stack robotics development. Owning the mechanical design, the servo circuits, and the control software end to end gave me a working sense of how decisions in one layer ripple into constraints in the next.


---

*Nov 2020 – Feb 2021 · Commissioned Project*