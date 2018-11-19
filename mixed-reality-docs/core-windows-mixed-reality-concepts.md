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

## Understanding reality

Windows Mixed Reality allows developers to build 3D spatial applications using innovative hardware and software to understand the user's physical space. Every device has multiple hardware sensor components that provide data of the physical environment which the platform analyzes to surface valuable information to the application.

### Head Tracking

In particular, the Windows Mixed Reality devices track the user's pose in their environment. A pose is a position and orientation that defines where the user is and where they are looking in your spatial application.

<span style="color:red">INSERT GIF of camera visualization of pose/device in space (side-by-side of real person?)</span>

In order to calculate an individual's pose, Windows Mixed Reality devices use **inside-out tracking technology**. Every device has multiple hardware sensor components that provide data of the physical environment which the platform analyzes to perform inside-out tracking as a user moves and rotates through their space. WMR devices are constantly scanning and learning about the physical environment as the user moves and looks from different vantage points.

### Spatial Mapping

Using data collected by various sensors, HoloLens can build a polygon mesh representative of the physical surfaces in the current environment. This can then be utilized for placing objects (i.e on a wall), occluding holograms (i.e a character behind a couch), and drive other important simulation models such as physics and path finding. The surface reconstruction of the physical space or also known as the spatial map, is built gradually over time as the HoloLens learns more about the environment. The HoloLens can gather new information about the environment if users move and view a space from multiple vantage points. Thus, it is common for HoloLens applications to involve an environment scanning experience so the user fully maps out their physical space before the core application scenarios begin. 

![Spatial Mapping](images/surfacemapping.png)

## Understanding Interactions

 Mixed Reality apps offer new interaction models compared to traditional application design. This section will introduce some of these new mixed reality input concepts. In particular, these interaction models can become exceedingly powerful when leveraged together.

### [Gaze](gaze.md)

Since devices can understand where a user is positioned and in which direction they are looking, developers can leverage gaze to interact with their application. By drawing a line from the user's head position along the line of their view, a developer can identify which holograms and UI the user is currently gazing. Gaze generally acts the *cursor* for the user in the Mixed Reality world. Gaze in conjunction with other input and/or app information can lead to powerful interactions. For example, a user may be target a hologram with their gaze and speak "Hide that". With gaze, your application is able to know which hologram the user is referencing with their command.

![test-gif](images/useriscamera-640px.jpg)

<span style="color:red"> Insert pic</span>

### [Gestures](gestures.md) and [Motion controllers](motion-controllers.md)

Users want to touch and nimbly control their Mixed Reality environment. Gestures and motion controllers are the primary means for users to interact with their *hands* in your application. Which you as a developer leverage depends on the device endpoint being targeted. Gestures are used for HoloLens devices and Motion Controllers function in Immersive Headsets. 

The platform recognizes a predefined set of hand gestures and Motion Controller buttons. However, developers are also given the capability to track the position of each. This *hand position* can be by the user to control UI or other interactions such as dragging a selected through space.

![Bloom gesture](images/bloom-giphy.gif)

![Motion Controllers](images/winmr-ck-1080x1080-350px.jpg)

### [Voice Input](voice-input.md)
The Windows Mixed Reality platform can understand simple verbal commands and keywords. Analogous to Gestures and Motion Controllers, Voice is another form of core input enabling users to take action in their Mixed World. For some scenarios though, voice is a more efficient means of input and communicating intent. For example, when combined with gaze, users can more easily delete a hologram using Voice then trying to target and select a button via gestures/motion controllers. 

![Voice Input](images/voice.gif)

### [Spatial Audio](spatial-sound.md)

To ensure a true spatial experience, the Windows Mixed Reality platform simulates 3D sound allowing users to understand direction and distance with the virtual holograms and UI in their space. Spatial Audio conveys the sense of 3D position for holograms and activity in a holographic application. For example, a developer can redirect a user's attention to look backward by playing audio behind them to draw their attention.

![Spatial Audio](images/spatialaudio.gif)

## Understanding Your App Runtime

Mixed Reality applications behave very similarly to game runtime dynamics. Your application will process input, update current state and perform app-specific computations, and then render an image to be presented to the user. This process will then loop continuously for the lifetime of your program.

The ultimate goal of your application every iteration in this loop is to calculate an image to present to the user. In order to do this correctly, the Windows Mixed Reality platform provides accurate input information to your app to ensure your scene is rendered correctly. Further, the Windows Mixed Reality platform has to track various inputs and make them available to your application.

The end-to-end process of creating an image though takes time. If an application runs at 60 frames-per-second, which means 60 images are rendered and presented to the user per second, then an application is finishing an image every 16.6 milliseconds. 

<span style="color:red">Need to talk about head pose prediction & LSR</span>
<span style="color:red">Links to where to read more about this</span>

![Lifetime of a frame](images/lifetime-of-a-frame.jpg)

## Concepts for review

| Concept | [HoloLens](hololens-hardware-details.md) | [Immersive Headsets](immersive-headset-hardware-details.md) | Demonstration |
|-----|-----|-----|-----|
| **Head Tracking** <br> Windows Mixed Reality devices can track where a user is located in their physical space and in what orientation they are looking. <br><span style="color:red"> HEAD TRACKING LINK HERE </span> | ✔️ | ✔️ | ![test-gif](test/test-gif.gif) |
| **[Spatial Mapping](spatial-mapping.md)** <br> sdfsdf <br> [Spatial Mapping design](spatial-mapping-design.md) \| [Spatial mapping in Development](spatial-mapping-in-unity.md) | ✔️ |   | ![Spatial Mapping](images/surfacereconstruction.jpg) |
| **Spatial Understanding** <br>  <br>  | ✔️ |   | <span style="color:red">INSERT</span> |
| **[Gaze](gaze.md)** <br> The user's pose, or where they are gazing, can be a powerful tool to allow for richer input and interact with your application scene. <br> [Gaze Design](gaze-targeting.md) \| [Gaze Development](gaze-in-unity.md) | ✔️ | ✔️ | ![test-gif](images/useriscamera-640px.jpg) |
| **[Gestures](gestures.md)** <br> Hand gestures allow HoloLens users a model to drive action and interaction in an application<br> [Gestures Development](gestures-and-motion-controllers-in-unity.md) | ✔️ |  | ![Gesture Example](images/bloom-giphy.gif) |
| **[Motion Controllers](motion-controllers.md)** <br> Motion Controllers are the complement for gestures on Hololens to immersive headsets. Users leverage motion controllers as a tool to "touch" and interact with objects <br> [Motion Controllers Development](gestures-and-motion-controllers-in-unity.md) |  | ✔️ | ![Motion Controllers](images/winmr-ck-1080x1080-350px.jpg) |
| **[Voice Input](voice-input.md)** <br> The Windows Mixed Reality platform can recognize voice keywords to enable users to directly command holograms and activity in an app.<br> [Voice design](voice-design.md) \| [Voice Input Development](voice-input-in-unity.md) | ✔️ | ✔️ | ![Voice Input](images/voice.gif) |
| **[Spatial Audio](spatial-sound.md)** <br> Windows Mixed Reality simulates 3D sound to ensure users gain dimension and depth in their Mixed Reality world. <br> [Spatial Sound Design](spatial-sound-design.md) \| [Spatial Sound Development](spatial-sound-in-unity.md) | ✔️ | ✔️ | ![Spatial Audio](images/spatialaudio.gif) |