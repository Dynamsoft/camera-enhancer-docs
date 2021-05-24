---
layout: default-layout
title: Dynamsoft Camera Enhancer - Android API references - Zoom & Focus Setting
description: This is the documentation - Android API references - Zoom & Focus Setting page of Dynamsoft Camera Enhancer.
keywords:  Camera Enhancer, Focus, Zoom, Android API Reference
needAutoGenerateSidebar: true
breadcrumbText: Android Zoom and Focus
---

# Android API Reference - Zoom & Focus Setting

- [`setAutoFocusPosition`](#setautofocusposition)
- [`setManualFocusPosition`](#setautofocusposition)
- [`setFocalLength`](#setfocallength)
- [`enableDCEAutoFocus`](#enabledceautofocus)
- [`getEnabledDCEAutoFocusStatus`](#enabledceautofocus)
- [`enableDefaultAutoFocus`](#enabledefaultautofocus)
- [`getEnabledDefaultAutoFocusStatus`](#enabledefaultautofocus)
- [`enableRegularAutoFocus`](#enableregularautofocus)
- [`getEnabledRegularAutoFocusStatus`](#enableregularautofocus)
- [`setRegularAutoFocusParam`](#setregularautofocusparam)
- [`enableAutoFocusOnSharpnessChange`](#enableautofocusonsharpnesschange)
- [`getEnabledAutoFocusOnSharpnessChangeStatus`](#enableautofocusonsharpnesschange)
- [`enableAutoZoom`](#enableautozoom)
- [`getEnabledAutoZoomStatus`](#enableautozoom)
- [`setZoomFactor`](#setzoomfactor)

## setAutoFocusPosition

Set the position that you want to autofocus. This setting will replace the default focus value and always focus on the set point.

```java
    mCamera.setAutoFocusPosition(0.5,0.6);
```

## setManualFocusPosition

Set the manual focus position. This position only effects once when this API is called.

```java
    mCamera.setManualFocusPosition(0.5,0.6);
```

## setFocalLength

Set the focal length (float). The range of focal length is from 0 to 10. The value doesn't mean a distance but a percentage. If a user sets `setFocalLength(5);`, it means the focal length will be 50% of the maximum focal length of the camera. Please note, If this API is called to set a focal length, the focal length will be fixed. All other autofocus settings will be disabled. To quit this fixed focal length mode, please set the focal length into -1.

To enter the fixed focal length mode:

```java
    mCamera.setFocalLength(8.5);
```

To quit:

```java
    mCamera.setFocalLength(-1);
```

## enableDCEAutoFocus

This API is designed to turn on DCE autofocus mode, which is specially designed and different from the system default autofocus mode. DCE autofocus and the default autofocus can work together at the same time without any conflict. The above focus settings are also available for controlling system default autofocus.

To turn on DCE autofocus mode:

```java
    mCamera.enableDCEAutoFocus(true);
```

To get status (on/off) of DCE autofocus mode:

```java
    boolean x = mCamera.getEnabledDCEAutoFocusStatus();
```

## enableDefaultAutoFocus

This API is designed for controlling the system default autofocus. To turn off default autofocus mode:

```java
    mCamera.enableDefaultAutoFocus(false);
```

To get status (on/off) of Default autofocus mode:

```java
    boolean x = mCamera.getEnabledDefaultAutoFocusStatus();
```

## enableRegularAutoFocus

Regular autofocus is an advanced setting that enables the camera to autofocus for every 3 seconds. It is compiled in DCE autofocus. When DCE autofocus is enabled, regular autofocus is activated as well. To turn off the regular autofocus mode:

```java
    mCameraEnhancer.enableRegularAutoFocus(false);
```

To get status (on/off) of regular autofocus mode:

```java
    boolean x = mCamera.getEnabledRegularAutoFocusStatus();
```

## setRegularAutoFocusParam

There are focus interval time and focus terminate time for users to set in regular autofocus mode. Please use `setRegularAutoFocusParam` to make these settings.

```java
    // Set focus interval = 3000 and focus terminate time = 500.
    mCamera.setRegularAutoFocusParam(3000, 500);
```

## enableAutoFocusOnSharpnessChange

This API is another advanced setting that enabled the camera to autofocus when sharpness change is detected between contiguous frames. The same with regular autofocus, this focus mode is also enabled by default when DCE autofocus is enabled. To turn off camera autofocus when sharpness changes:

```java
    mCameraEnhancer.enableAutoFocusOnSharpnessChange(false);
```

To get the status (on/off) of this autofocus mode:

```java
    boolean x = mCamera.getEnabledAutoFocusOnSharpnessChangeStatus();
```

## enableAutoZoom

DCE autozoom mode can be enabled if the user is using DCE to enhance decode performance. The autozoom mode is base on the decoding region predicted algorithm. In DCE autozoom mode, If the lastest decoded frame is predicted to contain a barcode but fail on decoding, DCE will control the camera to zoom in to approach the barcode region.
To enable autozoom mode:

```java
    mCamera.enableAutoZoom(true);
```

To check the status (on/off) of autozoom mode:

```java
    boolean x = mCamera.getEnabledAutoZoomStatus();
```

## setZoomFactor

Set the zoom factor (float).

```java
    mCamera.setZoomFactor(1.5);
```
