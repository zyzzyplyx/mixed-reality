---
title: Manual Unity Environment Set-up
description: Manual process for configuring a new Unity project for building mixed reality apps.
author: trferrel
ms.author: trferrel
ms.date: 11/19/2018
ms.topic: article
keywords: Unity, mixed reality, development, getting started, new project, porting, capability, camera, simulation, emulation, documentation
---

# Manual Unity environment set-up

It is recommended to utilize **[Mixed Reality Toolkit for Unity](https://github.com/Microsoft/MixedRealityToolkit-Unity)** to automate the initialzation of a new Windows Mixed Reality project in Unity. However, if MRTK cannot be used or for other reasons, this guide will walkthrough the necessary configurations to create Windows Mixed Reality apps in Unity. There are a small set of Unity settings you'll need to change for Windows Mixed Reality, broken down into two categories: per-project and per-scene.

### Per-project settings

To target Windows Mixed Reality, you first need to set your Unity project to export as a Universal Windows Platform app:
1. Select **File > Build Settings...**
2. Select **Universal Windows Platform** in the Platform list and click **Switch Platform**
3. Set **SDK** to **Universal 10**
4. Set **Target device** to **Any Device** to support immersive headsets or switch to **HoloLens**
5. Set **Build Type** to **D3D**
6. Set **UWP SDK** to **Latest installed**

We then need to let Unity know that the app we are trying to export should create an [immersive view](app-views.md) instead of a 2D view. We do that by enabling "Virtual Reality Supported":
1. From the **Build Settings...** window, open **Player Settings...**
2. Select the **Settings for Universal Windows Platform** tab
3. Expand the **XR Settings** group
4. In the **XR Settings** section, check the **Virtual Reality Supported** checkbox to add the **Virtual Reality Devices** list.
5. In the **XR Settings** group, confirm that **"Windows Mixed Reality"** is listed as a supported device. (this may appear as "Windows Holographic" in older versions of Unity)

Your app can now do basic holographic rendering and spatial input. To go further and take advantage of certain functionality, your app must declare the appropriate capabilities in its manifest. The manifest declarations can be made in Unity so they are included in every subsequent project export. The setting are found in **Player Settings > Settings for Universal Windows Platform > Publishing Settings > Capabilities**. The applicable capabilities for enabling commonly-used Unity APIs for Mixed Reality are:

|  Capability  |  APIs requiring capability | 
|----------|----------|
|  SpatialPerception  |  SurfaceObserver (access to [spatial mapping](spatial-mapping.md) meshes on HoloLens)&mdash;*No capability needed for general spatial tracking of the headset* | 
|  WebCam  |  PhotoCapture and VideoCapture | 
|  PicturesLibrary / VideosLibrary  |  PhotoCapture or VideoCapture, respectively (when storing the captured content) | 
|  Microphone  |  VideoCapture (when capturing audio), DictationRecognizer, GrammarRecognizer, and KeywordRecognizer | 
|  InternetClient  |  DictationRecognizer (and to use the Unity Profiler) | 

**Unity quality settings**

![Unity quality settings](images/unityqualitysettings-350px.png)<br>
*Unity quality settings*

HoloLens has a mobile-class GPU. If your app is targeting HoloLens, you'll want the quality settings tuned for fastest performance to ensure we maintain full framerate:
1. Select **Edit > Project Settings > Quality**
2. Select the **dropdown** under the **Windows Store** logo and select **Fastest**. You'll know the setting is applied correctly when the box in the Windows Store column and **Fastest** row is green.

### Per-scene settings

**Unity camera settings**

![Unity camera settings](images/unitycamerasettings.png)<br>
*Unity camera settings*

Once you enable the "Virtual Reality Supported" checkbox, the [Unity Camera](camera-in-unity.md) component handles [head tracking and stereoscopic rendering](rendering.md). There is no need to replace it with a custom camera to do this.

If your app is targeting HoloLens specifically, there are a few settings that need to be changed to optimize for the device's transparent displays, so your app will show through to the physical world:
1. In the **Hierarchy**, select the **Main Camera**
2. In the **Inspector** panel, set the transform **position** to **0, 0, 0** so the location of the users head starts at the Unity world origin.
3. Change **Clear Flags** to **Solid Color**.
4. Change the **Background** color to **RGBA 0,0,0,0**. Black renders as transparent in HoloLens.
5. Change **Clipping Planes - Near** to the [HoloLens recommended](camera-in-unity.md#clip-planes) 0.85 (meters).

If you delete and create a new camera, make sure your camera is **Tagged** as **MainCamera**.

## Adding mixed reality capabilities and inputs

Once you've configured your project as described above, standard Unity game objects (such as the camera) will light up immediately for a **seated-scale experience**, with the camera's position updated automatically as the user moves their head through the world.

Adding support for Windows Mixed Reality features such as [spatial stages](coordinate-systems.md#spatial-coordinate-systems), [gestures, motion controllers](gestures-and-motion-controllers-in-unity.md) or [voice input](voice-input-in-unity.md) is achieved using APIs built directly into Unity.

Your first step should be to review the [experience scales](coordinate-systems.md) that your app can target:
* If you're looking to build an **orientation-only** or **seated-scale experience**, you'll need to set Unity's tracking space type to [Stationary](coordinate-systems-in-unity.md#building-an-orientation-only-or-seated-scale-experience).
* If you're looking to build a **standing-scale** or **room-scale experience**, you'll need to ensure Unity's tracking space type is successfully set to [RoomScale](coordinate-systems-in-unity.md#building-an-orientation-only-or-seated-scale-experience).
* If you're looking to build a **world-scale** experience on HoloLens, letting users roam beyond 5 meters, you'll need to use the [WorldAnchor](coordinate-systems-in-unity.md#building-a-world-scale-experience) component.