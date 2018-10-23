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

<span style="color:red">
1) How to make caveats for immersive vs Hololens <br>
2) Of course, review wording accurate
</span>

Windows Mixed Reality allows developers to build 3D spatial applications by primarily understanding the user's physical space by using innovative hardware and software. In particular, the Windows Mixed Reality devices track the user's pose in their environment. A pose is a position and orientation that defines where the user is and where they are looking in your spatial application.

<span style="color:red">INSERT GIF of camera visualization of pose/device in space (side-by-side of real person?)</span>

In order to calculate an individual's pose, Windows Mixed Reality devices use **inside-out tracking technology**. Every device has multiple hardware sensor components that provide data of the physical environment which the platform analyzes to perform inside-out tracking as a user moves and rotates through their space. WMR devices are constantly scanning and learning about the physical environment as the user moves and looks from different vantage points.

<span style="color:red">
Further, the platform is able to provide additional feature capabilities to an application beyond pose by processing information about the current space. 
- Review Spatial mapping, spatial understanding, boundary
- Can also understanding new modes of input, tie into below
</span>

| Feature | [HoloLens](hololens-hardware-details.md) | [Immersive Headsets](immersive-headset-hardware-details.md) | Demonstration |
|-----|-----|-----|-----|
| **Head Tracking** <br> Windows Mixed Reality devices can track where a user is located in their physical space and in what orientation they are looking. <br><span style="color:red"> HEAD TRACKING LINK HERE </span> | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |
| **Spatial Mapping** <br> sdfsdf <br> [More about Spatial mapping](spatial-mapping.md) <br> [Spatial Mapping design](spatial-mapping-design.md) <br> [Spatial mapping in Development](spatial-mapping-in-unity.md) | ✔️ |   | ![Spatial Mapping](images/surfacereconstruction.jpg) |
| **Spatial Understanding** <br>  <br>  | ✔️ |   | ![test-gif](test/test-gif.gif) |
| **Boundary** <br>  <br> |  | ✔️ | ![test-gif](test/test-gif.gif) |

## Understanding Interactions

 Mixed Reality apps offer new interaction models compared to traditional application design. Below is a list of core mixed reality concepts that are essential to understand.

[Hardware accessories](hardware-accessories.md)

| Feature | [HoloLens](hololens-hardware-details.md) | [Immersive Headsets](immersive-headset-hardware-details.md) | Demonstration |
|-----|-----|-----|-----|
| **[Gaze](gaze.md)** <br> The user's pose, or where they are gazing, can be a powerful tool to allow for richer input and interact with your application scene. <br> [Gaze Design](gaze-targeting.md) \| [Gaze Development](gaze-in-unity.md) | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |
| **[Gestures](gestures.md)** <br> Blah blah blah <br> [Gestures Development](gestures-and-motion-controllers-in-unity.md) | ✔️ |  | ![Gesture Example](images/bloom-giphy.gif) |
| **[Motion Controllers](motion-controllers.md)** <br> Blah blah blah <br> [Motion Controllers Development](gestures-and-motion-controllers-in-unity.md) |  | ✔️ | ![test-gif](test/test-gif.gif) |
| **[Voice Input](voice-input.md)** <br> Blah blah blah <br> [Voice design](voice-design.md) \| [Voice Input Development](voice-input-in-unity.md) | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |
| **[Spatial Audio](spatial-sound.md)** <br> blah blah blah <br> [Spatial Sound Design](spatial-sound-design.md) \| [Spatial Sound Development](spatial-sound-in-unity.md) | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |

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

[App model](app-model.md)
[App views](app-views.md)