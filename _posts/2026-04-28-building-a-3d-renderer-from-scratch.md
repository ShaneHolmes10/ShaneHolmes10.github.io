---
layout: post
title: Hyper-fixating on 3D graphics 
date: 2026-04-28
description: The high level overview of how I got sucked into this 3D rendering rabbit hole.
tags: graphics c++
featured: true
---

So this post pertains to the 3D Render project that I've been working on. You can see it in the projects section of this website. I guess I'll begin with what drove me to start working on this in the first place, and a bit of the history of my working on this because this has been a pretty long running project of mine for a while now. 

I honestly can't even remember when the first thoughts entering my head of "how the computer renders 3D objects " I think it was Probably in my Freshman year? But it did and I guess I just assumed that it must be so wildly complicated that it would take an entire course and several months to understand it. So I just never really looked too deeply into it there were other higher priority concerns at hand. Actually I sort of found out how rendering fundamentally worked by accident. I was watching this [video](https://www.youtube.com/watch?v=hFRlnNci3Rs) of a guy who had built a computer in Minecraft and programmed it to render a rotating cube. Curb the fact that the person who built this is mildly psychotic for even thinking to do this, he explained the concept very well. It was the first time I had actually heard an explanation for how rendering worked and it struck me how wildly simple it was. 

The very core concept is that you take a bunch of 3D points in space and you project them onto a 2D surface (the screen). Quite literally all you do to achieve this 


