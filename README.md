# ANXCamera fixes for Mi 9 Lite Magisk Module

## Description
Various features (48 mpix, portrat mode...) of ANXCamera are broken on custom ROMs on Mi 9 Lite (aka pyxis).
This module goal is fix this issues so one can use __unmodified__ ANXCamera on this device with all features.

_Verified on Evolution-X 4.6 (with enforcing SELinux) using ANXCamera 180, 182, 184, 185 & 190_

### What works
- 48 MPix (aka Ultrapixel) mode
- Ultrawide sensor
- Portrait mode for back camera
- RAW capture in Pro mode (see below how to enable)
- Portrait mode for front camera (see below how to fix)

### What does NOT work (yet)
_Let me know_

## Miscellaneous
***You would need to install [ANXCamera Pro](https://play.google.com/store/apps/details?id=com.aeonax.camerapro&hl=en_US) app***
1) How to enable RAW support
   * Start **ANXCamera Pro**
   * Search for ***RAW*** or ***c_r_i_m_m*** and mark it ***TRUE***
2) How to fix front camera portrait crash
   * Start **ANXCamera Pro**
   * Search for ***Portrait Lighting Front/Crash*** or ***s_p_l_f*** and mark it ***FALSE***

## Technical details
The MiuiCamera (a base for ANXCamera) uses different mapping of camera IDs than AOSP. While AOSP generates
sequence 0...11 for camera IDs on pyxis but the MiUi uses { 0, 1, 21, 20, 62, 60, 63, 61, 91, 90, 100, 101 }.

The problem is that ANXCamera uses hardcoded values for certain sensors. So camera ID for ultrawide sensors needs to be 21, for Tele 20 etc. Without proper mapping user usually gets "Can't connect to camera" error message because the ANXCamera tries to open camera with non-existent camera ID.

The mapping on MIUI is driven by the property `camera.xiaomi.remapid`

    pyxis:/ $ getprop camera.xiaomi.remapid
    0 1 21 20 62 60 63 61 91 90 100 101

This proprietary mapping is done (or the property is used) in the file `android.hardware.camera.provider@2.4-legacy.so` but only in its MiUi variant not the AOSP one. So to provide this mapping you need to take this file from MiUi.
