---
layout: page
title: Interactive Demo
description: Example of an embedded interactive JavaScript element
img: assets/img/demo-thumbnail.png
importance: 4
category: work
---

## Overview

This is an example of an interactive element embedded directly in a project page.

---

## Interactive Demo

Move the slider to change the value:

<div style="margin: 2rem 0; padding: 1.5rem; border: 1px solid #ddd; border-radius: 6px; text-align: center;">
    <input 
        type="range" 
        id="demo-slider" 
        min="0" 
        max="100" 
        value="50" 
        style="width: 80%; cursor: pointer;"
    />
    <p style="font-size: 2rem; margin-top: 1rem; font-weight: bold;">
        Value: <span id="demo-value">50</span>
    </p>
</div>

<script>
    const slider = document.getElementById('demo-slider');
    const display = document.getElementById('demo-value');

    slider.addEventListener('input', function() {
        display.textContent = this.value;
    });
</script>

---

## What this demonstrates

Any standard HTML and JavaScript can be embedded directly in a Jekyll markdown file like this.
The markdown content renders as normal, and the interactive element sits inline alongside it.
This means things like sliders, canvases, buttons, and visualizations can all live
directly on a project page without any special configuration.