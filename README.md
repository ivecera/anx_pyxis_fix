# ANXCamera fixes for Mi 9 Lite Magisk Module

## Description
Various features (48 mpix, portrat mode...) of ANXCamera are broken on custom ROMs on Mi 9 Lite (aka pyxis).
This module goal is fix this issues so one can use __unmodified__ ANXCamera on this device with all features.

_Verified on Evolution-X using ANXCamera 182 & 184_

### What works
- 48 MPix (aka Ultrapixel) mode
- Ultrawide sensor
- RAW capture in Pro mode
- Portrait mode for back camera

### What does NOT work (yet)
- Portrait mode for front camera

## Technical details
The MiuiCamera (a base for ANXCamera) uses different mapping of camera IDs than AOSP. While AOSP generates
sequence 0...11 for camera IDs on pyxis but the MiUi uses { 0, 1, 21, 20, 62, 60, 63, 61, 91, 90, 100, 101 }.

The problem is that ANXCamera uses hardcoded values for certain sensors. So camera ID for ultrawide sensors needs to be 21, for Tele 20 etc. Without proper mapping user usually gets "Can't connect to camera" error message because the ANXCamera tries to open camera with non-existent camera ID.

The mapping on MIUI is driven by the property `camera.xiaomi.remapid`

    pyxis:/ $ getprop camera.xiaomi.remapid
    0 1 21 20 62 60 63 61 91 90 100 101

This proprietary mapping is done (or the property is used) in the file `android.hardware.camera.provider@2.4-legacy.so` but only in its MiUi variant not the AOSP one. So to provide this mapping you need to take this file from MiUi.
