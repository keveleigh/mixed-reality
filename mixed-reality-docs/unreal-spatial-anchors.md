---
title: Spatial Anchors in Unreal
description: Guide to using spatial anchors in Unreal
author: hferrone
ms.author: v-haferr
ms.date: 06/10/2020
ms.topic: article
ms.localizationpriority: high
keywords: Unreal, Unreal Engine 4, UE4, HoloLens, HoloLens 2, mixed reality, development, features, documentation, guides, holograms, spatial anchors
---
# Spatial Anchors in Unreal

## Overview

Spatial anchors are used to save holograms in real-world space between application sessions.  These get surfaced through Unreal as **ARPin**s and saved in the HoloLens’ anchor store, which is loaded in future sessions. 

## Checking the anchor store

Before saving or loading anchors, you need to check if the anchor store is ready.  Calling any of the HoloLens anchor functions before the anchor store is ready will not succeed.  

![Spatial Anchors Store Ready](images/unreal-spatialanchors-store-ready.PNG)

## Saving anchors

Once the application has a component that needs to be pinned to the world, it can be saved to the anchor store with the following sequence: 

![Spatial Anchors Save](images/unreal-spatialanchors-save.PNG)

Breaking this down:
1. Spawn an actor at a known location.
2. Create an **ARPin** with that location and a name based on the actor’s class. 
3. Add the actor to the **ARPin** and save the pin to the HoloLens anchor store.  
    * The anchor name you choose must be unique, which in this example is the current timestamp. 

4. If the anchor is successfully saved to the anchor store, you can be inspect it in the HoloLens device portal under **System > Map manager > Anchor Files Saved On Device**. 

## Loading anchors

When an application starts, you can use the following blueprint to restore components to their anchor locations:

![Spatial Anchors Load](images/unreal-spatialanchors-load.PNG)

Breaking this down:
1. Iterate over all of the anchors in the anchor store. 
2. Spawn an actor at identity.
3. Pin that actor to the **ARPin** from the anchor store.  

    * It's important to spawn the actor at identity since the anchor is responsible for repositioning the hologram in the world based on where it was saved. Any transform added here will add an offset to the anchor. 

The anchor ID is also queried so that different actors can be spawned depending on the anchor’s saved name. 

## Removing anchors 

When you're done with an anchor you can clear individual anchors or the entire anchor store with the **Remove ARPin from WMRAnchor Store** and **Remove All ARPins from WMRAnchor Store** components.

![Spatial Anchors Remove](images/unreal-spatialanchors-remove.PNG)

> [!NOTE]
> Bear in mind that Spatial Anchors are still in Beta, so be sure to check back for updated information and features.

## See also
* [Spatial anchors](spatial-anchors.md)
* [Coordinate systems](coordinate-systems.md)
