---
title: QR codes in Unreal
description: A guide to using QR codes in Unreal
author: hferrone
ms.author: v-haferr
ms.date: 06/10/2020
ms.topic: article
ms.localizationpriority: high
keywords: Unreal, Unreal Engine 4, UE4, HoloLens, HoloLens 2, mixed reality, development, features, documentation, guides, holograms, qr codes
---
# QR codes in Unreal

## Overview

The HoloLens 2 can see QR codes in world space using the webcam, which renders them as holograms using a coordinate system at each code's real-world position.  In addition to single QR codes, HoloLens 2 can also render holograms in the same location on multiple devices to create a shared experience. Make sure you're following the best practices for adding QR codes to your applications:

- Quiet zones
- Lighting and backdrop
- Size, distance, and angular position

Pay special attention to the [environment considerations](environment-considerations-for-hololens.md) when QR codes are being placed in your app. You can find more information on each of these topics and instructions on how to download the required NuGet package in the main [QR code tracking](qr-code-tracking.md) document. 

## Enabling QR detection
Since the HoloLens 2 needs to use the webcam to see QR codes, you'll need to enable it in the project settings:
- Open **Edit > Project Settings**, scroll to the **Platforms** section and click **HoloLens**.
    + Expand the **Capabilities** section and check **Webcam**.  

You'll also need to opt into QR code tracking by [adding an ARSessionConfig asset](https://docs.microsoft.com/windows/mixed-reality/unreal-uxt-ch3#adding-the-session-asset).

## Setting up a tracked image

QR codes are surfaced through Unreal’s AR tracked geometry system as a tracked image. To get this working, you'll need to:
1. Create a Blueprint and add an **ARTrackableNotify** component.

![QR AR Trackable Notify](images/unreal-spatialmapping-artrackablenotify.PNG)

2. Select **ARTrackableNotify** and expand the **Events** section in the **Details** panel. 

![QR Events](images/unreal-spatialmapping-events.PNG)

3. Click **+** next to **On Add Tracked Geometry** to add the node to the Event Graph.
    - You can find the full list of events in the [UARTrackableNotify](https://docs.unrealengine.com/API/Runtime/AugmentedReality/UARTrackableNotifyComponent/index.html) component API. 

![QR Render Example](images/unreal-qr-codes-tracked-geometry.png)

## Using a tracked image
The Event Graph in the following image shows the **OnUpdateTrackedImage** event being used to render a point in the center of a QR code and print out its data. 

![QR Render Example](images/unreal-qr-render.PNG)

Here's what's going on:
1. First, the tracked image is cast to an **ARTrackedQRCode** to check that the current updated image is a QR code.  
2. The encoded data is retrieved from the **QRCode** variable. You can get the top-left of the QR code from the location of **GetLocalToWorldTransform** and the dimensions with **GetEstimateSize**. 

You can also [get the coordinate system for a QR code](https://docs.microsoft.com/windows/mixed-reality/qr-code-tracking#getting-the-coordinate-system-for-a-qr-code) in code.

## Finding the unique ID
Every QR code has a unique guid ID, which you can find by:
- Dragging and dropping the **As ARTracked QRCode**  pin and searching for **Get Unique ID**.

![QR Guid](images/unreal-qr-guid.PNG)

There's a lot going on behind the scenes with QR codes, so this isn't the end of the road. Be sure to check out the following links for more details on what's under the hood.

## See also
* [Spatial mapping](spatial-mapping.md)
* [Holograms](hologram.md)
* [Coordinate systems](coordinate-systems.md)