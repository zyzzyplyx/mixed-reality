---
title: Advanced Performance Recommendations
description: Advanced topics and details on optimizing performance for Windows Mixed Reality Apps
author: trferrel
ms.author: trferrel
ms.date: 10/11/2018
ms.topic: article
keywords: Windows Mixed Reality, Mixed Reality, Virtual Reality, VR, MR, Performance, Optimization, CPU, GPU
---

# Advanced Performance Recommendations

This article deep dives into topics to optimize performance of your Mixed Reality app to ensure you hit target frame rate. User experience can be greatly degraded if your application does not run at optimal frame rate.

>[!NOTE]
> Read [Critical Concepts to Ensure Optimal User Experience](ensure-optimal-user-experience.md) to get further information on what is frame rate and it's significance to your app development. 
>
> <span style="color:red"> Reference MRTK start here/FPS display to calculate frame rate </span>

| Platform | Target Frame Rate |
|----------|-------------------|
| [HoloLens](hololens-hardware-details.md) | 60 FPS |
| [Windows Mixed Reality Ultra PCs](immersive-headset-hardware-details.md) | 90 FPS |
| [Windows Mixed Reality PCs](immersive-headset-hardware-details.md) | 60 FPS |

# Understanding Performance Bottlenecks

If your app has an underperforming framerate, the first step is to analyze and understand where your application is computationally intensive. There are two primary processors responsible for the work to render your scene: the CPU and the GPU. Each of these two components handle different operations and stages of your Mixed Reality app. There are three key places where bottlenecks may occur.

<span style="color:red"> Useful to have diagram here </span>

1. App Thread - CPU
    * This thread is responsible for your app logic. This includes processing input, animations, physics, and other app logic/state
2. Render Thread - CPU to GPU
    * This thread is responsible for submitting your draw calls to the GPU. When your app wants to render an object such as a cube or model, this thread sends a request to the GPU, which has an architecture optimized for rendering, to perform these operations. 
3. GPU
    * This processor most commonly handles the graphics pipeline of your application to transform 3D data (models, textures, etc) into pixels and ultimately produce a 2D image to submit to your device's screen

## How to Analyze Your Application

<span style="color:red"> Need to outline how to determine bottle neck, brief intros to useful tools</span>

Make note that most Mixed Reality apps are GPU-bounded, particularly true on HoloLens but every application is different

## How to Fix Your Application

Point out that you should use blah tools to determine where your application is performing expensive computations but here is a list of best practices and useful insights. 

**Table of Contents:**
* [CPU](#CPU-Performance-Recommendations)
* [CPU to GPU](#CPU-to-GPU-Performance-Recommendations)
* [GPU](#GPU-Performance-Recommendations)
* [Memory](#Memory-Recommendations)
* <span style="color:red"> Additional References/links/etc
* <span style="color:red">Power => Make separate page

### CPU Performance Recommendations

	Focus on tight loops => Update or other repeating functions 
o	Update Manager instead of many update calls
	General Scripting
o	Avoid LINQ
o	Avoid Lambdas
o	Avoid Expensive calls 
	GetCOmponent, FindObjectOfType, RaycastAll, look at my code list
o	Cache References 
o	Boxing, passing structs & copying vs refs
	Animation
o	Disable idle animations by disabling animator components…
	Physics
o	Control of operations per second (look up unity quality)

### CPU-to-GPU Performance Recommendations
	Static Batching
	Dynamic Batching
	Single pass instanced rendering

### GPU Performance Recommendations

o	Bandwidth vs Fill rate?
	Full screen passes
o	MSAA/HDR
	Reduce Poly count
	Shaders
o	Move operations from Pixel to Vertex
o	Explain how to get shader outputs

### Memory Recommendations
	Avoid Allocations
	Object Pooling
	Garbage Collections

## See Also
<span style="color:red"> Insert links here </span>