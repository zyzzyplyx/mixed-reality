---
title: Ensure Optimal User Experience
description: Key principles for optimal user experience for mixed reality apps
author: trferrel
ms.author: trferrel
ms.date: 10/11/2018
ms.topic: article
keywords: Windows Mixed Reality, Mixed Reality, Virtual Reality, VR, MR
---

# Critical Concepts to Ensure Optimal User Experience

## Frame Rate

*Frame rate*, also known as FPS (Frames per second), is **one of the most critical indicators** that determines how optimal your application’s user experience will be.

### What is Frame Rate?
Frame rate is a metric equal to the number of times per second your app can present an image to the user in their device. 

<span style="color:red">{Show gif here, 1 FPS, 5 FPS, 10 FPS, 30 FPS, 60 FPS}</span>

Developers need to ensure their applications meet the target framerate that is determined by the type of device running the app. See the table below for frame rates on your targeted device endpoint.

| Platform | Target Frame Rate |
|----------|-------------------|
| [HoloLens](hololens-hardware-details.md) | 60 FPS |
| [Windows Mixed Reality Ultra PCs](immersive-headset-hardware-details.md) | 90 FPS |
| [Windows Mixed Reality PCs](immersive-headset-hardware-details.md) | 60 FPS |

### Why does frame rate matter?

Applications that do not meet the target value for their device endpoint will suffer the following symptoms. The severity of the problems below are correlated as worse with lower frame rate values and less impactful (WC) with higher frame rate values. 

#### Device Pose Tracking 
Your application will receive less accurate positioning and orientation for where the user is located and looking in their environment.
<span style="color:red">Finish section</span>

#### Late Stage Reprojection <span style="color:red">(Hyperlink needed)</span>
Rendering an image for your application takes time. With a target frame rate of 60 FPS, this equates to about 16 milliseconds. A user’s head will move and rotate slightly during this time interval. Thus, the point of view that your application used to render your scene will be slightly “off”. To draw your scene so it appears correct, Windows Mixed Reality devices will perform Late-Stage Reprojection (LSR) <span style="color:red">(Hyperlink needed)</span> to adjust your rendered image for the user’s new head position and orientation. This reprojection algorithm is an approximation though.

Lower frame rates means your application is taking longer to draw an image for the user. For example, at 40 FPS, your application will now takes 25 milliseconds to process a frame. This greater disparity will result in LSR being less accurate and thus, negatively impacting your user’s experience

#### Hologram Stability
For holograms to appear stable in the world, each image presented to the user must have the holograms drawn in the correct spot. 
<span style="color:red">Finish section</span>

#### Motion sickness

>[!NOTE]
> <span style="color:red">Download this app to see this symptoms for yourself (WC)</span>

### How can I measure my application’s Frame Rate?
It is vital that developers and creators keep track of their framerate throughout the entire development process. Thus, we recommend adding a visual counter to your application during environment set-up.

<span style="color:red">{GIF: Show example of FPS indicator in head locked display}</span>

>[!NOTE]
> One can also view their app's framerate via the Developer Portal <span style="color:red">WebB blah blah blah. </span>

>[!IMPORTANT]
> Please read the following articles for details on how to setup a performant development environment and more advanced performance recommendations
>
>* [Unity development overview](unity-development-overview.md)
>* [Advanced Performance Recommendations](advanced-performance-recommendations.md)

## Hologram Stability
As noted above, applications with low frame rates will suffer from unstable and incorrectly drawn holograms. However, there are other important settings and components to be cognizant of during development to ensure optimal tracking and display of your scene(WC). This is particularly important on HoloLens. 

### Late-Stage Reprojection (WC?) (Immersive Headsets & Hololens)
As described above, Windows Mixed reality devices have hardware that adjusts the rendered image from your app to account for the discrepancy between the predicted head position and the actual head position. 

Deep dive link 
	RS1 API vs 
	How to see in WebB

### Anchoring (HoloLens Only)

- World Anchors
- Spatial Anchors?

## See Also