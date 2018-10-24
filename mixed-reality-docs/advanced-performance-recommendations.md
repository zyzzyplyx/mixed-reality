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

This article deep dives into topics to optimize performance of your Mixed Reality app to ensure you hit target frame rate. User experience can be greatly degraded if your application does not run at optimal frame rate. For review, the performant framerate values for each target platform are listed below.

>[!NOTE]
> If your application is not meeting frame rate as outlined below, before reviewing these advanced recommendations, ensure you > have set-up your development environment to get easy performance wins. 
>
> - [Critical Concepts to Ensure Optimal User Experience](ensure-optimal-user-experience.md)
> - [Unity development overview](unity-development-overview.md)
>
> Further, for direction on how to measure your application's framerate, please review
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

There are a list of tools that allow you as a developer to understand the performance profile of your Mixed Reality application. These will enable you to both target where you have bottlenecks and how they are manifesting themselves to debug them. Most HoloLens applications will be GPU bounded.

There is one simple test to quickly determine if you are likely GPU bounded or CPU bounded in your application. If you decrease the resolution of the render target output, there are less pixels to calculate and thus, less work the GPU needs to perform to render an image.

Thus, after decreasing rendering resolution, if:
1) Application framerate **increases**, then you are likely **GPU Bounded**
1) Application framerate **decreases**, then you are likely **CPU Bounded**

Unity provides the ability to easily modify the render target resolution of your application at runtime through the *[XRSettings.renderViewportScale](https://docs.unity3d.com/ScriptReference/XR.XRSettings-renderViewportScale.html)* property. NOTE: The final image presented on device has a fixed resolution. The platform will sample the lower resolution output to build a higher resolution image for rendering on displays. 

```CS
UnityEngine.XR.XRSettings.renderScale = 0.7f;
```

List of tools
<span style="color:red"> Need to outline how to determine bottle neck and fill rate and stuff, brief intros to useful per tools</span>

To figure this out, you may try decreasing the viewport scaling factor. If that improves your framerate, then you are likely GPU bound. Conversely, if decreasing the viewport does not improve your FPS, then you are likely CPU bound. Viewport scaling (dynamic resolution scaling) is the practice of rendering your image to a smaller render target then your output device can display, and sampling from those pixels to display your final image. It trades visual fidelity for speed. Windows Mixed Reality devices support viewport scaling at a platform level. This means if you set the viewport to be smaller (in Unity: UnityEngine.XR.XRSettings.renderViewportScale = 0.7f) Unity will inform the platform it is rendering to a smaller section of the render target, and the platform will composite its display from that smaller section of the render target.

>[!NOTE]
> Make note that most Mixed Reality apps are GPU-bounded when rendering pixels. This does not apply to every application though and thus it is recommended to use the tools & techniques above to get to ground-truth for your particular app. 

## How to Fix Your Application

Point out that you should use <span style="color:red"> blah </span> tools to determine where your application is performing expensive computations but here is a list of best practices and useful insights. 

<span style="color:red"> Need to link to unity development overview to ensure environment set up correctly </span>

**Table of Contents:**
* [CPU](#CPU-Performance-Recommendations)
* [CPU to GPU](#CPU-to-GPU-Performance-Recommendations)
* [GPU](#GPU-Performance-Recommendations)
* [Memory](#Memory-Recommendations)
* <span style="color:red"> Startup Performance?
* <span style="color:red"> Additional References/links/etc
* <span style="color:red">Power => Make separate page

### CPU Performance Recommendations

#### Cache References

It is best practice to cache references to all relevant components and GameObjects at initialization. This is because repeating function calls such as *[GetComponent\<T>()](https://docs.unity3d.com/ScriptReference/GameObject.GetComponent.html)* are significantly more expensive relative to the memory cost to store a pointer. This also applies to to the very, regularly used [Camera.main](https://docs.unity3d.com/ScriptReference/Camera-main.html). *Camera.main* actually just uses *[FindGameObjectsWithTag()](https://docs.unity3d.com/ScriptReference/GameObject.FindGameObjectsWithTag.html)* underneath which expensively searches your scene graph for a camera object with the *"MainCamera"* tag.

```CS
using UnityEngine;
using System.Collections;

public class ExampleClass : MonoBehaviour
{
    private Camera cam;
    private CustomComponent comp;

    void Start() 
    {
        cam = Camera.main;
        comp = GetComponent<CustomComponent>();
    }

    void Update()
    {
        // Good
        this.transform.position = cam.transform.position + cam.transform.forward * 10.0f;

        // Bad
        this.transform.position = Camera.main.transform.position + Camera.main.transform.forward * 10.0f;

        // Good
        comp.DoSomethingAwesome();

        // Bad
        GetComponent<CustomComponent>().DoSomethingAwesome();
    }
}
```

>[!NOTE] Avoid GetComponent(string) <br/>
> When using *[GetComponent()](https://docs.unity3d.com/ScriptReference/GameObject.GetComponent.html)*, there are a handful of different overloads. It is important to always use the Type based implementations and never the string-based searching overload. Searching by string in your scene is significantly more costly than searching by Type. <br/>
> (Good) Component GetComponent(Type type) <br/>
> (Good) T GetComponent\<T>() <br/>
> (Bad) Component GetComponent(string)> <br/>

#### Avoid Expensive Operations

1) **Avoid use of [LINQ](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/getting-started-with-linq)**

    Although LINQ can be very clean and easy to read and write, it generally requires much more computation and particularly memory allocation than writing the operation out manually.

    ```CS
    // Example Code
    using System.Linq;

    List<int> data = new List<int>();
    data.Any(x => x > 10);

    var result = from x in data
                 where x > 10
                 select x;
    ```

2) **Common Unity APIs**

    Certain Unity APIs, although useful, can be very expensive to execute. Most of these involve searching your entire scene graph for some matching list of GameObjects. These operations can generally be avoided by caching references or implementing a manager component for the GameObjects in question to track the references at runtime.

        GameObject.SendMessage()
        GameObject.BroadcastMessage()
        UnityEngine.Object.Find()
        UnityEngine.Object.FindWithTag()
        UnityEngine.Object.FindObjectOfType()
        UnityEngine.Object.FindObjectsOfType()
        UnityEngine.Object.FindGameObjectsWithTag()
        UnityEngine.Object.FindGameObjectsWithTag()

>[!NOTE]
> *[SendMessage()](https://docs.unity3d.com/ScriptReference/GameObject.SendMessage.html)* and *[BroadcastMessage()](https://docs.unity3d.com/ScriptReference/GameObject.BroadcastMessage.html)* should be eliminated at all costs. These functions can be on the order of 1000x slower than direct function calls.

3) **Beware of Boxing**

    A common case for that is passing structs to a method that takes an interface as a parameter. Instead, make the method take the concrete type (by ref) so that it can be passed without allocation.

    <span style="color:red"> CLEAN HERE </span>

#### Repeating Code Paths

Any repeating Unity callbacks function that are executed many times per second and/or frame should be written very carefully. Any expensive operations here will have huge and consistent impact on performance. 

1) **Empty Callback Functions**

    Although the code below may seem innocent to leave in your application, especially since every Unity script auto-initializes with this code block, these empty callbacks can actually become very expensive. Unity operates back and forth over an unmanaged/managed code boundary, between UnityEngine code and your application code. Context switching over this bridge is fairly expensive even if there is nothing to execute. This becomes especially problematic if your app has 100's of GameObjects with components that have empty repeating Unity callbacks.

    ```CS
    void Update()
    {
    }
    ```

>[!NOTE]
> Update() is the most common manifestation of this performance issue but other repeating Unity callbacks such as the following can be equally as bad if not worse: FixedUpdate(), LateUpdate(), OnPostRender", OnPreRender(), OnRenderImage(), etc. 

<span style="color:red"> Note?
If you have a lot of objects in the scene and/or scripts that do heavy processing, avoid having Update functions on every object in your scene. Instead, have just a few higher level "manager" objects with Update functions that calls into any other objects that need attention. The specifics of using this approach is highly application-dependent, but you can often skip large numbers of objects at once using higher level logic. For example, AI logic code probably doesn't need to update every single frame, so you could instead store them all in an array and have a higher level manager object update only a small number of them per fr
</span>

2) **Common, Expensive Operations**

    The following Unity APIs are common operations almost every Holographic App. These can still readily be used to create an application with a constant 60 FPS but they need to be managed wisely.

    a) Generally it is good practice to have a dedicated Singleton class to handle your gaze Raycast into the scene and then re-use this result in all other scene components, instead of making repeated and essentially identical Raycast operations by each component.

        UnityEngine.Physics.Raycast()
        UnityEngine.Physics.RaycastAll()

    b) Avoid GetComponent() operations by [caching references](#cache-references) in Start() or Awake()

        UnityEngine.Object.GetComponent()

    c) It is good practice to instantiate all objects, if possible, at initialization and use [object pooling](#object-pooling) to recycle and re-use GameObjects throughout runtime of your application

        UnityEngine.Object.Instantiate()

<span style="color:red"> Note?
Avoid interfaces and virtual methods in hot code such as inner loops. Interfaces in particular are much slower than a direct call. Virtual functions are not as bad, but still significantly slower than a direct call.
</span>

#### Miscellaneous

1) **Physics**

    a) Generally, easiest way to improve physics is to limit the amount of time spent on Physics or the number of iterations per second. Of course, this will reduce simulation accuracy. See [TimeManager](https://docs.unity3d.com/Manual/class-TimeManager.html) in Unity

    b) The type of colliders in Unity have widely different performance characteristics. The order below lists the most performant colliders to least performant colliders from left to right. It is most important to avoid Mesh Colliders which are substantially more expensive than the primitive colliders.

        Sphere < Capsule < Box <<< Mesh (Convex) < Mesh (non-Convex)

    [Unity Physics Best Practices](https://unity3d.com/learn/tutorials/topics/physics/physics-best-practices)

2) **Animations**

    a) Simplify animations

    b) Disable idle animations by disabling the Animator component (disabling the game object won't have the same effect). Avoid design patterns where an animator sits in a loop setting a value to the same thing. There is considerable overhead for this technique, with no effect on the application.

<span style="color:red"> FINISH ANIMATION SECTION</span>

3) **Complex Algorithms**

    If your application is using complex algorithms such as inverse kinematics, path finding, etc, look to find a simpler approach or adjust relevant settings for their performance

### CPU-to-GPU Performance Recommendations

Generally, CPU-to-GPU performance comes down to the **draw calls** submitted to the graphics card. To improve performance, draw calls need to be strategically **a) reduced** or **b) restructured** for optimal results. Since draw calls themselves are resource-intensive, reducing them will reduce overall work required. Further, state changes between draw calls requires costly validation and translation steps in the graphics driver and thus, restructuring of your application's draw calls to limit state changes(i.e different materials, etc) can boost performance.

Unity has a great article that gives an overview and dives into batching draw calls for their platform.
- [Unity Draw Call Batching](https://docs.unity3d.com/Manual/DrawCallBatching.html)

#### Single Pass Instanced Rendering

Single Pass Instanced Rendering in Unity allows XR applications draw calls for each eye to be reduced down to one instanced draw call. Due to cache coherency between two draw calls, there is also some performance improvement on the GPU as well. 

To enable this feature, open **Player Settings** (go to Edit > Project Settings > Player). In Player Settings, navigate to **XR Settings** at the bottom, check the **Virtual Reality Supported** box, then select **Single Pass Instanced (Preview)** from the Stereo Rendering Method drop-down menu.

<span style="color:red"> Can we copy & credit images from unity?
https://blogs.unity3d.com/2017/11/21/how-to-maximize-ar-and-vr-performance-with-advanced-stereo-rendering/>
</span>

Read the following articles from Unity for details with this rendering approach.
- [How to maximize AR and VR performance with advanced stereo rendering](https://blogs.unity3d.com/2017/11/21/how-to-maximize-ar-and-vr-performance-with-advanced-stereo-rendering/)
- [Single Pass Instancing](https://docs.unity3d.com/Manual/SinglePassInstancing.html) 

>[!NOTE]
> One common issue with Single Pass Instanced Rendering occurs if developers already have existing custom shaders not written for instancing. After enabling this feature, developers may notice some GameObjects only render in one eye. This is because the associated custom shaders do not have the appropriate properties for instancing 
>
> See [Single Pass Stereo Rendering for HoloLens](https://docs.unity3d.com/Manual/SinglePassStereoRenderingHoloLens.html) from Unity for how to address this problem

#### Static Batching

Static Batching works for most Renderer objects in Unity that **1) share the same material** and **2) are all marked as *Static*** (Select an object in Unity and click the checkbox in the top right of the inspector). GameObjects marked as *Static* cannot be moved throughout your application's runtime. Thus, static batching can be difficult to leverage on HoloLens where virtually every object needs to be placed, moved, scaled, etc. For immersive headsets, static batching can dramatically reduce draw calls and thus improve performance.

Read *Static Batching* under [Draw Call Batching in Unity](https://docs.unity3d.com/Manual/DrawCallBatching.html) for more details.

#### Dynamic Batching

Since it is problematic to mark objects as *Static* for HoloLens development, dynamic batching can be a great tool to compensate for this lacking feature. Of course, it is can also be useful on immersive headsets as well. Dynamic batching in Unity can be difficult though to enable because GameObjects must **a) share the same Material** and **b) meet a long list of other criteria**. 

Read *Dynamic Batching* under [Draw Call Batching in Unity](https://docs.unity3d.com/Manual/DrawCallBatching.html) for the full list. Most commonly, GameObjects become invalid to be batched dynamically because the associated mesh data can be no more than 300 vertices.

#### Other Techniques

Batching can only occur if multiple GameObjects are able to share the same material. Typically this will be blocked by the need for GameObjects to have a unique texture for their respective Material. It is common to combine Textures into one big Texture, a method known as [Texture Atlasing](https://en.wikipedia.org/wiki/Texture_atlas).

Further, it is generally preferable to combine meshes into one GameObject where possible and reasonable. Each Renderer in Unity will have it's associated draw call(s) versus submitting a combined mesh under one Renderer. 

>[!NOTE] 
> Modifying properties of Renderer.material at runtime will create a copy of the Material and thus potentially break batching. Use Renderer.sharedMaterial to modify shared material properties across GameObjects.

### GPU Performance Recommendations

#### Understand Bandwidth vs Fill Rate

Talk about stuff*

#### General

- Reduce poly count -> LOD System
- Limit overdraw
- MSAA
- Post Effects
- Sub-pixel triangles are very expensive.
- HoloLens => Avoid Geometry, Hull and Compute Shaders.
- Single pass Instanced Rendering

#### Shaders

How to get shader stats ouptut
Use bilinear whenever possible.
such as trying to rearrange expressions to use the mad intrinsics in order to do a multiply and an add at the same time.
And of course, precalculate as much as you can on the CPU and pass as constants to the material. Whenever that isn't possible, do as much as you can in the vertex shader. Even for things that vary per-pixel you can sometimes get away with doing parts of the calculation in the vertex shader.

### Memory Recommendations

#### Avoid Allocations
sdfsdf

#### Object Pooling
sdfsdf

#### Garbage Collections

### Startup Performance

You should consider starting your app with a smaller scene, then using SceneManager.LoadSceneAsync to load the rest of the scene. This allows your app to get to an interactive state as fast as possible. Be aware that there may be a large CPU spike while the new scene is being activated and that any rendered content might stutter or hitch. One way to work around this is to set the AsyncOperation.allowSceneActivation property to false on the scene being loaded, wait for the scene to load, clear the screen to black, and then set back to true to complete the scene activation.

Remember that while the startup scene is loading the holographic splash screen will be displayed to the user.

## See Also
<span style="color:red"> Insert links here </span>