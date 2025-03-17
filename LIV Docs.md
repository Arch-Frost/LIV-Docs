# LIV SDK Documentation for Integration with Unity

## Introduction

Here is how I integrated LIV SDK into Unity for Mixed Reality Capture. First of all, check the official documentation for LIV for the most detail. This is just the process that worked for me.

- [LIV SDK official documentation](https://docs.liv.tv/)
- [LIV SDK Support for Unity YouTube Tutorial](https://youtu.be/FSXfNXu0mT0)

## Downloading the LIV SDK

The LIV SDKs can be downloaded through the [LIV Developer Portal](https://dev.liv.tv/).

1. Register at [dev.liv.tv](https://dev.liv.tv/) with your work email<br>
   <span style="display: flex; justify-content:center;">![LIV Dev Portal Register](/Images/Download1.png)</span>
2. Create your account, using your business contact<br>
   <span style="display: flex; justify-content:center;">![LIV Dev Portal Account Details](/Images/Download2.png)</span>
3. Create your game record with LIV<br>
   <span style="display: flex; justify-content:center;">![LIV SDK Create Game](/Images/Download3.png)</span>
4. Select your game and navigate to Downloads<br>
   <span style="display: flex; justify-content:center;">![LIV SDK Download Page](/Images/Download4.png)</span>
5. Agree to the Terms of Service & Privacy Policy
6. Generate your app specific SDK by selecting Prepare.<br>
   **Note:** If you create a new game you’ll need a new SDK. Select Prepare
   <span style="display: flex; justify-content:center;">![LIV SDK Download Prepare](/Images/Download6.png)</span>
7. Download the SDK and you’re ready to move onto installation<br>
   <span style="display: flex; justify-content:center;">![LIV SDK Download](/Images/Download7.png)</span>

## Unity SDK Installation and Configuration

I'll add the LIV SDK to a **VR Core** template project in Unity for demonstration. The Unity Editor version I'm using is **2022.3.11f1**. The setup should be similar to this for other Unity versions as well.

1. Import the LIV SDK into your project. _(Assets > Import > Custom Package)_<br>
   <span style="display: flex; justify-content:center;">![LIV SDK Import](/Images/Config1.png)</span>

2. You can add the LIV SDK component anywhere in the project, but I add it as a parent of the **XR Origin (XR Rig)** prefab, so I don't have to add it to every scene manually. So I create an **Empty Child** to the root of **_XR Origin (XR Rig)_** prefab and add the **LIV script** to it.<br>
   <span style="display: flex; justify-content:center;">![XR Origin Prefab Hierarchy](/Images/Config2.png)</span><br>
   <span style="display: flex; justify-content:center;">![LIV Object Components](/Images/Config3.png)</span>

3. Next, we need to set the **HMD Camera** and the **XR Origin**. We will set the **HMD Camera** to the **Main Camera** in the **XR Rig**. We need to set the **XR Origin** component to the parent of **Main Camera** i.e. **Camera Offset** in this case, and **NOT** the root parent of the prefab i.e. **XR Rig**<br>
   <span style="display: flex; justify-content:center;">![XR Origin Prefab Hierarchy](/Images/Config4.png)</span><br>
   <span style="display: flex; justify-content:center;">![LIV Object Components](/Images/Config5.png)</span>

4. Now what we need to understand is that when we pass the Main Camera as the HMD Camera, LIV clones the camera and uses it for Mixed Reality Capture. But this might not be desirable if you have some components or some player specific code that should only run on the player controller i.e. XR Rig, and not on the LIV Camera. So it is generally a better practice to create a separate prefab for LIV camera, and remove any components that you might not want in the LIV camera. To do this:

   1. Drag the **Main Camera** to any folder in your Assets directory. This creates a prefab of the **Main Camera** GameObject:<br>
      <span style="display: flex; justify-content:center;">![Main Camera Prefab](/Images/Config6.png)</span><br>
      <span style="display: flex; justify-content:center;">![Main Camera Prefab in Project Window](/Images/Config7.png)</span>

   2. In the Hierarcy Window, select the **Main Camera** prefab and unpack it completely _(Right-Click > Prefab > Unpack Completely)_<br>
      <span style="display: flex; justify-content:center;">![Main Camera Unpack Completely](/Images/Config8.png)</span>

   3. Rename the **Main Camera** Prefab to anything, _LIV Camera Prefab_ in my case, and remove any unnecessary components or code from it. Like setting the **Target Eye** in **Camera Output** to **None** instead of **Both**, removing **AudioListeners**, etc.<br>
      <span style="display: flex; justify-content:center;">![LIV Camera Prefab Properties](/Images/Config9.png)</span>

   4. Now we can use it in the **_MR Camera Prefab_** field in the **LIV** Component:<br>
      <span style="display: flex; justify-content:center;">![MR Camera Prefab](/Images/Config10.png)</span>

5. In the Spectator Layer Mask and Mask, you need to select all the layers that will be visible in your Virtual Camera and MR Passthrough respectively. You can just exclude the layers that you don't want to be visible in your LIV camera here<br>
   <span style="display: flex; justify-content:center;">![Spectator Layer Mask](/Images/Config10.png)</span>

6. If you are using URP, you need to click _Open Settings_ under LIV properties, and then **Switch to Universal Rendering Pipeline**. To confirm it is setup, go to _(File > Build Settings... > Player Settings...)_ and scroll down till you see _Scripting Define Symbols_ and check whether it contains **_LIV_UNIVERSAL_RENDER_**. If not, then add it manually using the + icon.<br>
   <span style="display: flex; justify-content:center;">![URP Setup](/Images/Config11.png)</span><br>
   <span style="display: flex; justify-content:center;">![URP Setup](/Images/Config12.png)</span><br>
   <span style="display: flex; justify-content:center;">![URP Setup](/Images/Config13.png)</span>

7. On thing to note is that **LIV** works with these runtimes:

   - SteamVR (OpenVR API)
   - OpenXR (When provided by SteamVR)

   So you need to use either SteamVR API while building the project in Unity, or enable OpenXR under plugin providers in _(Edit > Project Settings > XR Plug-in Management)_
   <span style="display: flex; justify-content:center;">![Plugin Providers](/Images/Config14.png)</span>

That is all for the integrating LIV SDK into Unity. For additional information, look at the [official documentation for LIV SDK](https://docs.liv.tv/). Onto testing the integration now.

## LIV Installation and Setup

First thing you need to do is install these applications from Steam.

- [SteamVR](https://store.steampowered.com/app/250820/SteamVR/)
- [LIV App](https://store.steampowered.com/app/755540/LIV/)

Now, start _SteamVR_ and set the OpenXR runtime to SteamVR by going to _(Settings > OpenXR > Set SteamVR as OpenXR Runtime)_
<span style="display: flex; justify-content:center;">![OpenXR Runtime](/Images/Setup1.png)</span>

Now put on your the headset and connect through Meta Link (in case of Oculus headsets).

After that, launch LIV through Steam.
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup2.png)</span>

Go to General Setting, and click on **INSTALL** under _LIV SteamVR Driver_
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup3.png)</span>

After that, go to _Virtual Cameras & Avatars_ and click **_START VR CAMERAS & AVATARS_**
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup4.png)</span>

Go to _(Capture > Manual)_ and select the Executable of your game if you are running a build, or select _Unity.exe_ if you're running it through Unity in **Play Mode**
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup5.png)</span>

Go to _Camera_ and click on **Add** to create a new camera profile.
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup6.png)</span>

Set the Type to _Video Camera_, select your camera device under _Device_ (DroidCam Video in my case), and select the resolution you want.
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup7.png)</span>

Go to _Keying_ and select the color you want to **Chroma Key**, its threshold, smoothness and de-spill strength.
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup8.png)</span>

Next, go to _Calibration_ and click on **Begin Calibration**
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup9.png)</span>

Select _Desktop_ an now put on your VR headset
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup10.png)</span>

Now you are in calibration mode for the camera. Remeber that since we do not have a tracker for the camera. We cannot move the camera, and every time we move the camera we have to re-calibrate the camera.
<span style="display: flex; justify-content:center;">![LIV App](/Images/Setup11.png)</span>

In the **LIV Output** window, you will be able to see the markers. You have to follow the inscructions on screen corner and place your controller as close to the camera as possible, and then press the trigger button.
<span style="display: flex; justify-content:center;">![Tracking Marker](/Images/Marker1.png)</span>
Then for the second marker, move back a bit, then go to the corner on the screen shown in the instructions and align your controller with the marker and press the trigger button.
<span style="display: flex; justify-content:center;">![Tracking Marker](/Images/Marker2.png)</span>
Similarly for the third time, align the controller with the marker and press the trigger button.
<span style="display: flex; justify-content:center;">![Tracking Marker](/Images/Marker3.png)</span>

After that, you will see virtual controllers that are aligned to the controllers from the camera video, and follow the real controlllers.
<span style="display: flex; justify-content:center;">![Controller Sync](/Images/Calibrated1.png)</span>

Yuo can also adjust the latency of the virtul controller movement to make sure they are in sync with the controllers in the video feed. You can also make adjustment to the X, Y and Z position and rotation of the controller manually from within the LIV app in the headset if you want to make micro adjustments to the controller position.

Now you can save the camera settings and LIV is now ready to record the content from the game.
<span style="display: flex; justify-content:center;">![Controller Sync](/Images/Calibrated2.png)</span>