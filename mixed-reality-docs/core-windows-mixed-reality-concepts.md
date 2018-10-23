---
title: Core Windows Mixed Reality Concepts
description: Core understanding of Windows Mixed Reality devices and Platform
author: trferrel
ms.author: trferrel
ms.date: 10/11/2018
ms.topic: article
keywords: Windows Mixed Reality, Mixed Reality, Virtual Reality, VR, MR
---

# Core Windows Mixed Reality Concepts

<span style="color:red">insert intro speel</span>


## Understanding Reality

<span style="color:red">How to make caveats for immersive vs Hololens</span>

Windows Mixed Reality allows developers to build 3D spatial applications by primarily understanding the user's physical space. In particular, the Windows Mixed Reality devices track the user's pose in their environment. A pose is a position and orientation that defines where the user is and where they are looking in your spatial application.

<span style="color:red">INSERT GIF of camera visualization of pose/devicein space (side-by-side of real person?)</span>

WMR builds camera pose (position, orientation)

How does WMR figure out your position
Inside-out tracking
-> WMR build map of the world
    -> Constantly learning & adjusting to reality
    -> WMR looks to identify visual features in it's environment
-> Builds map using tracking cameras 
---> Link to deep dive

## Understanding Interactions

Windows Mixed Reality headsets use innovative hardware and software to provide a platform for your app to understand the user's physical space. Mixed Reality apps offer new interaction models compared to traditional application design. Below is a list of core mixed reality concepts that are essential to understand.

| Feature | [HoloLens](hololens-hardware-details.md) | [Immersive Headsets](immersive-headset-hardware-details.md) | Demonstration |
|-----|-----|-----|-----|
| **Gaze** <br> Windows Mixed Reality devices can track where a user is located in their physical space and in what orientation they are looking. Gaze is powerful tool to allow the user to interact with your application scene. <br> [More About Gaze](gaze.md) <br> [Gaze Design](gaze-targeting.md) <br> [Gaze in Development](gaze-in-unity.md) | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |
| **Gestures** <br> Blah blah blah  <br> [Concept Deep Dive](gaze.md) <br> [Design Deep Dive](gaze-targeting.md) <br> [Development Deep Dive](gaze-in-unity.md) | ✔️ |  | ![Gesture Example](images/bloom-giphy.gif) |
| **Motion Controllers** <br> <br> [Concept Deep Dive](gaze.md) <br> [Design Deep Dive](gaze-targeting.md) <br> [Development Deep Dive](gaze-in-unity.md) |  | ✔️ | ![test-gif](test/test-gif.gif) |
| **Voice Input** <br>  <br> [Concept Deep Dive](gaze.md) <br> [Design Deep Dive](gaze-targeting.md) <br> [Development Deep Dive](gaze-in-unity.md) | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |
| **Spatial Audio** <br>  <br> [Concept Deep Dive](gaze.md) <br> [Gaze targeting](gaze-targeting.md) <br> [Development Deep Dive](gaze-in-unity.md) | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |

## Understanding Your App Runtime

Mixed Reality apps behave very similarly to game runtime dynamics. Your application will process input, update current state and perform app-specific computations, and then render an image to be presented to the user. This process will then loop continuously for the lifetime of your program.

<span style="color:red">
Game loop diagram
Process Input -> Update Game -> Render -> Present -> Loop
</span>

### Pipeline

The goal of your application every iteration in the loop above is ultimately to calculate an image to present to the user. In order to do this correctly, the WMR platform has to provide accurate input information to your app to ensure your scene is rendered correctly. Further, the WMR platform has to track various inputs and make them available to your application.

<span style="color:red">
Diagram of lifetime of an app
 </span>

Tracking Cameras Input
-> Hands/gestures
-> Motion Controllers

Late-stage reprojection:
-> Link to deep dive
