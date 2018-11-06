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

#### Hologram Stability

For holograms to appear stable in the world, each image presented to the user must have the holograms drawn in the correct spot. Rendering an image for your application takes time. With a target frame rate of 60 FPS, this equates to about 16 milliseconds. At 30FPS, this delay doubles to ~33 ms and effectively the same image is shown twice to the user, compared to rendering at 60 FPS. This longer delay can result in uneven motion and double images of holograms which will manifest to the user as judder.

Further, a user’s head is constantly moving and rotating over time, even if every so slightly. The user's head position will be different before and after rendering a frame. The Windows Mixed Reality platform will make predictions of where the user's view will be at the end of the frame. Additionally, the platform will perform Late-Stage Reprojection (LSR) <span style="color:red">(Hyperlink needed)</span> which adjusts the rendered image to account for the discrepancy between the predicted head position and the actual head position.

The error for predicting final head position will be greater for longer rendering times. The image adjustment process of LSR also works best for small discrepencies in point of view. The reprojection will be less effective for lower framerates. 

Lower framerate applications thus will have less effective positioning and stability of holograms being rendered to the user, resulting in a negative user experience. 

#### Motion sickness

Unstable holograms and inaccurate position tracking will give misleading rendered results to the user relative to their actual movements. This can induce motion sickness. 

<span style="color:red">Finish section</span>

>[!NOTE]
> <span style="color:red">Download this app to see this symptoms for yourself (WC)</span>

### How can I measure my application’s frame rate?
It is vital that developers and creators keep track of their framerate throughout the entire development process. Thus, we recommend adding a visual counter to your application during environment set-up. 

A framerate counter can easily be enabled in Unity with [Mixed Reality Toolkit for Unity](https://github.com/Microsoft/MixedRealityToolkit-Unity). 

1) Search in your *Project Window* for **"FPSDisplay.prefab"**
2) Drag this prefab into your scene

<span style="color:red">{GIF: Show example of FPS indicator in head locked display}</span>

>[!NOTE]
> One can also view their app's framerate via the **System Performance** page of **[Device Portal](using-the-windows-device-portal.md)**. The *Device Portal* lets you configure and manage your device remotely over Wi-Fi or USB via a web browser page. Setup via instructions [here](using-the-windows-device-portal.md). Once enabled, on the left hand-side, look for the tab **Performance/System Performance**. The frame rate section on this page will track current frame rate and graph over time.

>[!IMPORTANT]
> Please read the following articles for details on how to setup a performant development environment and more advanced performance recommendations
>
>* [Unity development overview](unity-development-overview.md)
>* [Advanced Performance Recommendations](advanced-performance-recommendations.md)

## Hologram Stability

As noted in the previous section, applications with low frame rates will suffer from unstable and incorrectly drawn holograms. However, there are other important settings and components to be cognizant of during development to ensure optimal tracking and display of your scene

Unlike traditional programming, holographic applications require various configurations to be properly set in order to ensure a smooth and stable user experience. Failure to set up your application appropriately may result in experience with accuracy misalignment, jitter, judder, drift, jumpiness, etc. This is particularly more problematic on HoloLens.

<span style="color:red"> { INSERT GIF OF SWIMMING OR OTHER BAD EXPERIENCE } </span>

- [Consistent 60 FPS](#why-does-frame-rate-matter?)
- [Late-Stage Reprojection (Immersive Headsets & Hololens)](#late-stage-reprojection-immersive-headsets--hololens)
- [Anchoring (HoloLens Only)](#anchoring-hololens-only)
- [Device Calibration](#device-calibration)
- [Interpupillary Distance (IPD)](#interpupillary-distance-ipd)

### Late-Stage Reprojection (Immersive Headsets & Hololens)
As mentioned in the Frame Rate section above, Windows Mixed reality devices has hardware that adjusts the rendered image from your app to account for the discrepancy between the predicted head position and the actual head position.

To help the platform best stabilize holograms, developers can share their application's depth buffer with Windows. 

- For **[Immersive Headsets](immersive-headset-hardware-details.md)**, the platform will utilize the depth buffer to perform per-pixel depth-based reprojection. 
- For **[HoloLens](hololens-hardware-details.md)**, the depth buffer can be used to find a [focus point](focus-point-in-unity.md) to optimize hologram stability through a stabilization plane. 

A developer can **[enable depth buffer sharing in Unity](camera-in-unity.md#sharing-your-depth-buffers-with-windows)** if:
1) The device endpoint is running [Windows 10 Fall Creators Update](release-notes-october-2017.md)
2) You are using [Unity 2017.3](https://unity3d.com/unity/whats-new/unity-2017.3.0) or later

Otherwise, for HoloLens, **[set the focus point in Unity](focus-point-in-unity.md)** manually using the
*[UnityEngine.XR.WSA.HolographicSettings.SetFocusPointForFrame()](https://docs.unity3d.com/ScriptReference/XR.WSA.HolographicSettings.SetFocusPointForFrame.html)* API in Unity.

If you are writing an application with DirectX, read [Rendering in DirectX](rendering-in-directx.md#set-the-focus-point-for-image-stabilization) for guidance on sharing the depth buffer or setting the focus point.

>[!NOTE]
> The [Windows Device Portal](using-the-windows-device-portal.md) can be used to debug and validate that the stabilization plane is being set correctly on Hololens.
> 1) Go to **Views** > **3D View** via the tabs on the left-hand side
> 2) Enable the **View Options** > **Show Stabilization Plane** checkbox
> 3) Confirm the rendered plane in the main window tracks correctly with your gaze

### Anchoring (HoloLens Only)

HoloLens is constantly scanning and reasoning the current environment via <span style="color:red"> head tracking</span>. The device keeps track of multiple [coordinate systems](coordinate-systems.md) and their relationship to one another based on prominent features identified in the physical space. The platform abstracts much of these calculations so that developers only have to worry about the [coordinate system in Unity](coordinate-systems-in-unity.md). 

>[!NOTE]
> The initial position of the user in the physical world when a Unity holographic app starts will become the origin (i.e <0,0,0\>) in the Unity coordinate system. Further, the user's initial head orientation will become Unity's world Z-forward(+) > axis , which will correspondingly make the X-axis orthogonal to the initial orientation.
> Note though, the Y-axis, or "Up" > axis, in Unity will be aligned with gravity using the device's inertial measurement unit (IMU).

**[Anchoring](spatial-anchors.md)** is a special concept whereby the developer can indicate to the HoloLens that a specific spot in space is of interest to their app experience. If a hologram should be _**anchored**_ to a particular point in physical space, you can attach a [WorldAnchor](https://docs.unity3d.com/2017.4/Documentation/ScriptReference/XR.WSA.WorldAnchor.html) component in Unity to the GameObject that should remain stable at one position. This will allow the platform to take over control of this GameObject's transform to ensure accurate positioning during app runtime.

To learn more about how Anchor's work and best practices when using them, please read [Spatial anchors](spatial-anchors.md).

>[!NOTE]
> Spatial Anchors and World Anchors are one in the same. Unity describes an Anchor as a [WorldAnchor](https://docs.unity3d.com/2017.4/Documentation/ScriptReference/XR.WSA.WorldAnchor.html) component while the exact concept is known as a [Spatial Anchor](https://docs.microsoft.com/en-us/uwp/api/windows.perception.spatial.spatialanchor) in the Windows Mixed Reality API. 
> 
> If writing a holographic app in DirectX, [read this guidance](coordinate-systems-in-directx.md#place-holograms-in-the-world-using-spatial-anchors) on using the [Spatial Anchor](https://docs.microsoft.com/en-us/uwp/api/windows.perception.spatial.spatialanchor) class.

### Device Calibration

<span style="color:red"> CREATE CONTENT </span>

### Interpupillary Distance (IPD)

<span style="color:red"> Need graphic here 

Explain what IPD is. how to set it in WebB or via API
</span>

## See Also
- [Hologram stability](hologram-stability.md)
- [Unity development overview](unity-development-overview.md)
- [Advanced Performance Recommendations](advanced-performance-recommendations.md)
- [Mixed Reality Toolkit for Unity](https://github.com/Microsoft/MixedRealityToolkit-Unity)