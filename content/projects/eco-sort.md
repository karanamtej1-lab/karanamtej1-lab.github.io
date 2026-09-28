---
title: "Eco-Sort"
date: 2026-09-27
summary: "Point a camera at a piece of trash and it tells you Recycling, Compost, or Landfill, live in the browser."
tags: ["javascript", "ai", "web"]
stack: ["JavaScript", "HTML / CSS", "TensorFlow.js", "Teachable Machine", "getUserMedia"]
links:
  - name: "Live demo"
    url: "https://karanamtej1-lab.github.io/eco-sort/"
  - name: "Source on GitHub"
    url: "https://github.com/karanamtej1-lab/eco-sort"
---

## Overview

Hold an item up to your phone or laptop camera and Eco-Sort names the bin it
goes in: Recycling, Compost, or Landfill. It runs entirely on your device. No
image ever leaves the browser.

## How it works

- I trained an image classifier in Google Teachable Machine and load it with
  TensorFlow.js. The first prediction compiles WebGL shaders, so the app runs
  a warm-up pass on a blank canvas at load time. The first real frame is
  instant.
- The camera code and the model code are separate files that talk through two
  events, `camera:started` and `camera:stopped`. Neither reaches into the
  other.
- It classifies 8 frames a second, not 60. A label can't change faster than a
  person can read it, so anything more just burns battery.
- Each frame is blended into a moving average. One odd frame can't flip the
  answer, and the label settles in about half a second.
- The model always picks one of three bins, even when the camera sees nothing.
  Below 60% confidence it says "Not sure" instead of guessing, which is what
  stops an empty desk from reading as Landfill.
- On phones it asks for the rear camera, since you point it at trash, not your
  face.

## What I learned

The model was the easy part. Making its output trustworthy took the work:
smoothing, a confidence floor, and honest error messages for every way a
camera can fail (blocked, missing, busy, not on HTTPS).

## Status

Live. Plain HTML, CSS, and JavaScript with no build step.
