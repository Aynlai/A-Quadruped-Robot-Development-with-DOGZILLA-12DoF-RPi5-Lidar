# Raspberry Pi Camera Setting

## 1. Get the device number of the camera

open the terminal of the Raspberry Pi and enter the following command:
```
ls /dev/video*
```

if only one camera is connected, generally the camera device is video0

If there is no camera device number, please plug and unplug the camera's USB port again.

## 2. Test Camera

By default, the location of the camera driver file is located at:

```
/home/pi/DOGZILLA/app_dogzilla/camera_dogzilla.py
```

Since the camera display requires desktop display, please connect the screen or log in using VNC viewer and open the terminal of the Raspberry Pi and enter the following:

```
python3 /home/pi/DOGZILLIA/app_dogzilla/camera_dogzilla.py
```

