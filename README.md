# WEEDO X40 Community Firmware
![image](http://www.weedo.ltd/wp-content/uploads/2021/04/970x300-ABanner1.jpg)

## Summary: Copied from Legodev/WEEDOX40firmware because could not control visibility of original repository. 
This is the repository that contains the community version firmware for the WEEDO X40 3D Printer. This firmware cannot be updated with the TF card.  To go back to the official firmware, load X40firmware_factory.bin

The framework of the firmware is based on the Marlin 2.0.x version. 
We fixed some bugs in the dual x carriage modules.

The user interaction and network communication modules are developed by WEEDO3D.

Binary file is in **WEEDOX40_firmware\.pio\build\stm32_vet6\firmware.bin**

Major differences from the official firmware:
- Improved hotend temp overshoot by increasing PID_FUNCTIONAL_RANGE from 10 to 30.  You will also need change the PID gains in EEPROM after flashing, run gcode:

```
M301 E0 P8 I0.37 D40
M301 E1 P8 I0.37 D40
M500
```

- Default baudrate set to 2000000 from 115200 to help with serial port bottlenecking when using Octoprint.
- G2/G3 implementation is updated from a newer version of Marlin which fixes a bug that results in incorrect move speeds.  This is needed to make it work well with Arc Welder.
- Z-axis offset adjustment resolution increased from 0.1mm to 0.02mm.
- Extruder will brush both sides of the nozzle before printing.
- Extruder nozzle will hover over home position while heating up instead of hovering over the part.
- Added M922 and M923 commands to control the purging (M922 is usefull is you want to use a purgetower without using the wipers of the X40).
    * M922 S0/S1 - Turn automatic filament extrusion Off/On (S1 is the default behavior of the original Printer)
    * M923 S0/S1 - Turn multiple nozzle wipes Off/On (S0 is default for Weedo, S1 is default FW) 
- Relaxed tolerances for temperature hysteresis.
- Changed linear advance K from 0.22 to 0.56.

## Compile requirements

- Download and install [VSCode](https://code.visualstudio.com/)
- Search and install PlatformIO IDE from store marketplace
- Search and install ST STM32 embedded platform from PIO Home
- Copy framework-arduinoststm32-maple@99.99.99 from /buildroot/lib to PlatformIO library location (user)/.platformio/packages/
- Install the USB driver from /buildroot/driver/CH341SER.ZIP

## Upload firmware

X40 uses a customized bootloader, which requires a customized download program for firmware update.  

The Windows version of the download program WEEDOIAP.exe is located in the /buildroot/upload/ directory. The macos version is still under development.

Lanuch the WEEDOIAP.exe, open the firmware.bin file from build directory, choice the com port, then click the update.

![image](http://www.weedo.ltd/wp-content/uploads/2021/04/weedoiap.png)

X40 will restart to enter the firmware update mode. Wait for about 1 minute, the printer will automatically restart after the firmware update is completed.

![image](http://www.weedo.ltd/wp-content/uploads/2021/04/iap.jpg)



## The difference between the community version firmware and the official version firmware

The official firmware has the function of upgrading the firmware via TF card. Use the IAP function in the control menu to read the flash.wfm file on the TF card to upgrade the firmware.

The community version firmware does not support the wfm format, so it does not support firmware upgrade via TF card.



