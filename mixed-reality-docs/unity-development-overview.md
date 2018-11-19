---
title: Unity development overview
description: Getting started building mixed reality apps in Unity.
author: thetuvix
ms.author: alexturn
ms.date: 03/21/2018
ms.topic: article
keywords: Unity, mixed reality, development, getting started, new project, porting, capability, camera, simulation, emulation, documentation
---

# Unity development overview

The fastest path to building a [mixed reality app](app-views.md) is with [Unity](http://aka.ms/HoloLensUnity). We recommend you take the time to explore the [Unity tutorials](https://unity3d.com/learn/tutorials). If you need assets, Unity has a comprehensive [Asset Store](https://www.assetstore.unity3d.com/). Once you have built up a basic understanding of Unity, you can visit the [Mixed Reality Academy](academy.md) to learn the specifics of mixed reality development with Unity. Be sure to visit the [Unity Mixed Reality forums](http://forum.unity3d.com/forums/hololens.102/) to engage with the rest of the community building mixed reality apps in Unity and find solutions to problems you might run into.

## Porting an existing Unity app to Windows Mixed Reality

If you have an existing Unity project that you're porting to Windows Mixed Reality, follow along with the [Unity porting guide](porting-guides.md) to get started.

## Configuring a new Unity project for Windows Mixed Reality

To get started building mixed reality apps with Unity, first [install the tools](install-the-tools.md).

After installing the necessary tools, the easiest way to set-up a new Windows Mixed Reality project in Unity is to utilize the **[Mixed Reality Toolkit for Unity](https://github.com/Microsoft/MixedRealityToolkit-Unity)**. 

After importing the *.unitypackage for MRTK into your project, you will find a new *"Mixed Reality Toolkit"* tab at the top of Unity. Under this tab, a developer has two options to allow MRTK to auto-setup your Unity environment. The "Apply Mixed Reality Project SettingsS" tool will modify project-wide configurations while the "Mixed Reality Scene Settings" will need to be utilized per scene. These tools will enable XR in Unity, initialize MRTK components such as input manager, modify the camera GameObject to function correctly for mixed reality, and more.

If so desired, [this guide](manual-unity-environment-set-up.md) will also walkthrough the manual process for configuring a Unity project for Windows Mixed Reality.

- **Mixed Reality Toolkit** > **Configure** > **Apply Mixed Reality Project Settings**
- **Mixed Reality Toolkit** > **Configure** > **Apply Mixed Reality Scene Settings**

![MRTK Unity Configure](images/mrtk-configure-unity.png)

After MRTK has automated the process to correctly configure your Unity project & scenes to run Windows Mixed Reality, there are three other important settings that are highly recommended.

### Framerate counter

It is vital that developers and creators keep track of their framerate throughout the entire development process. Thus, we recommend adding a visual counter to your application during environment set-up.

A framerate counter can easily be enabled in Unity with the [Mixed Reality Toolkit for Unity](https://github.com/Microsoft/MixedRealityToolkit-Unity).

1) Import the MRTK *.unitypackage into your project
2) Search in your *Project Window* for **"FPSDisplay.prefab"**
3) Drag this prefab into your scene

To learn more about framerate, see [Critical Concepts to Ensure Optimal User Experience](ensure-optimal-user-experience.md)

### Single Pass Instanced Rendering

Single Pass Instanced Rendering in Unity allows XR applications to reduce and optimizes the draw operations needed for each eye. Turning this feature ON will optimize the rendering pipeline in Unity to save on both CPU and GPU performance for Mixed Reality projects.

Read the following articles from Unity for details with this rendering approach.
- [How to maximize AR and VR performance with advanced stereo rendering](https://blogs.unity3d.com/2017/11/21/how-to-maximize-ar-and-vr-performance-with-advanced-stereo-rendering/)
- [Single Pass Instancing](https://docs.unity3d.com/Manual/SinglePassInstancing.html) 

To enable this feature in your Unity Project
1)  Open **Player Settings** (go to **Edit** > **Project Settings** > Player > **Universal Windows Platform tab** > **XR Settings**)
2) Select **Single Pass Instanced (Preview)** from the **Stereo Rendering Method** drop-down menu (**Virtual Reality Supported** should already be checked)

>[!NOTE]
> One common issue with Single Pass Instanced Rendering occurs if developers already have existing custom shaders not written for instancing. After enabling this feature, developers may notice some GameObjects only render in one eye. This is because the associated custom shaders do not have the appropriate properties for instancing.
>
> See [Single Pass Stereo Rendering for HoloLens](https://docs.unity3d.com/Manual/SinglePassStereoRenderingHoloLens.html) from Unity for how to address this problem

### Enable Depth Buffer Sharing

The depth buffer for each frame of your application contains depth data for all the scene geometry currently in view from the camera when the application is running. Each pixel of a depth buffer provides a distance to the closest geometry along a ray from that pixel. Sharing this data with the Windows Mixed Reality platform can enhance user experience by improving hologram stability.

To set whether your Unity app will provide a depth buffer to Windows:

1)  Open **Player Settings** (go to **Edit** > **Project Settings** > Player > **Universal Windows Platform tab** > **XR Settings**)
2) Expand the **Windows Mixed Reality SDK** item under **Virtual Reality SDKs**.
3) Check the **Enable Depth Buffer Sharing** box. 

>[!NOTE]
> The **Depth Format** next to the **Enable Depth Buffer Sharing** check box determines the level of precision for each pixel in your depth buffer. There are two options available: 16-bit and 24-bit. The 16-bit option will generally improve performance for your application but the lower fidelity may result in [Z-fighting](https://en.wikipedia.org/wiki/Z-fighting) between geometry in your scene. Z-fighting occurs when two pieces of geometry have similar or identical z-values in the depth buffer and the system cannot accurately determine which object is actually closer to the user. This generally results in a flickering and pixelated blending effect between the two pieces of geometry. If an application experiences [Z-fighting](https://en.wikipedia.org/wiki/Z-fighting), two options to resolve this issue are:
> 1) Reduce the difference in the near & far clipping plane values on the [Unity camera](https://docs.unity3d.com/Manual/class-Camera.html). Narrowing this range will allow greater precision while still using the 16-bit configuration
> 2) Switch from 16-bit depth buffer format to 24-bit depth buffer format

## Optimal Windows Mixed Reality user experience

There are many features that the Windows Mixed Reality platform provides to build powerful, interactive holographic apps. However, there are some critical concepts to understand to ensure user's have an optimal experience. Of most important note is understanding the significance of application framerate and the necessary tools to ensure stable holograms. It is recommended to review the topics below. 

- [Critical Concepts to Ensure Optimal User Experience](ensure-optimal-user-experience.md)
- [Performance Recommendations](advanced-performance-recommendations.md)

## Windows Mixed Reality features in Unity

To get started learning how to use Windows Mixed Reality platforms features & functionality, it is recommended to review the [Mixed Reality Academy](academy.md). The Academy has a rich list of tutorials and walkthroughs for how to get started incorporating key features into your holographic application.

Further, developers can learn more on the core building blocks for mixed reality apps in Unity via the dedicated sections below.
* [Camera](camera-in-unity.md)
* [Coordinate systems](coordinate-systems-in-unity.md)
* [Gaze](gaze-in-unity.md)
* [Gestures and motion controllers](gestures-and-motion-controllers-in-unity.md)
* [Voice input](voice-input-in-unity.md)
* [Persistence](persistence-in-unity.md)
* [Spatial sound](spatial-sound-in-unity.md)
* [Spatial mapping](spatial-mapping-in-unity.md)

There are other key features that many mixed reality apps will want to use, which are also exposed to Unity apps:
* [Shared experiences](shared-experiences-in-unity.md)
* [Locatable camera](locatable-camera-in-unity.md)
* [Focus point](focus-point-in-unity.md)
* [Tracking loss](tracking-loss-in-unity.md)
* [Keyboard](keyboard-input-in-unity.md)

## Running your Unity project on a real or simulated device

Once you've got your holographic Unity project ready to test out, your next step is to [export and build a Unity Visual Studio solution](exporting-and-building-a-unity-visual-studio-solution.md).

With that Visual Studio solution in hand, you can then run your app in one of three ways, using either a real or simulated device:
* [Deploy to a real HoloLens or Windows Mixed Reality immersive headset](using-visual-studio.md)
* [Deploy to the HoloLens emulator](using-the-hololens-emulator.md)
* [Deploy to the Windows Mixed Reality immersive headset simulator](using-the-windows-mixed-reality-simulator.md)

## Unity documentation

In addition to this documentation available on the Windows Dev Center, Unity installs documentation for Windows Mixed Reality functionality alongside the Unity Editor. The Unity provided documentation includes two separate sections:
1. **Unity scripting reference**
    * This section of the documentation contains details of the scripting API that Unity provides.
    * Accessible from the Unity Editor through **Help > Scripting Reference**
2. **Unity manual**
    * This manual is designed to help you learn how to use Unity, from basic to advanced techniques.
    * Accessible from the Unity Editor through **Help > Manual**

## See also
* [MR Basics 100: Getting started with Unity](holograms-100.md)
* [Recommended settings for Unity](recommended-settings-for-unity.md)
* [Performance Recommendations](advanced-performance-recommendations.md)
* [Exporting and building a Unity Visual Studio solution](exporting-and-building-a-unity-visual-studio-solution.md)
* [Using the Windows namespace with Unity apps for HoloLens](using-the-windows-namespace-with-unity-apps-for-hololens.md)
* [Best practices for working with Unity and Visual Studio](best-practices-for-working-with-unity-and-visual-studio.md)
* [Unity Play Mode](unity-play-mode.md)
* [Porting guides](porting-guides.md)
